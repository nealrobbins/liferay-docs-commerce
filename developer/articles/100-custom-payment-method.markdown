# Creating Custom Payment Methods

@commerce@ includes a number of payment methods out of the box, but these will
not satisfy all users' needs. Fortunately, payments are highly customizable.

This tutorial covers the creation of new payment methods which can be activated
by administrators from *Settings* &rarr; *Payment Methods*. The business logic
of how you process payments or integrate your system with any particular
payment provider is outside the scope of this article.

There are three main types of payment methods:

**Offline:** @commerce@ does not process payment (for example, a buyer might
pay in cash when picking up a product that has been shipped to a local store).

**Online Standard:** Payment is processed entirely by @commerce@.

**Online Redirect:** Payment is processed online by a third party.

To create a new payment method of any of these types, follow these steps:

1.  Create a module, referencing `commerce-api` and `commerce-payment-api` in
    its build script.

2.  Create a component implementing the
    `com.liferay.commerce.payment.method.CommercePaymentMethod` interface.
    Provide whatever utility classes your component requires.

3.  Create a second component implementing the `ScreenNavigationEntry` interface to
    give your method and admin screen.

4.  Populate your admin screen with a JSP.

## Creating a Module

[Create a module](/develop/tutorials/knowledge_base/7-1/creating-modules-with-liferay-ide)
and provide it with a build script. The `build.gradle` file should look like
this:

    sourceCompatibility = "1.8"
    targetCompatibility = "1.8"

    dependencies {
        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.payment.api", version: "1.0.0"
    compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.api", version: "7.0.0"
    }

## Implementing the Payment Method Interface

First, create a component and implement the interface. In general,
implementations of `CommercePaymentMethod` work through the `processPayment` and
`completePayment` methods. `processPayment` returns a utility object
(`CommercePaymentResult` which is used to send information to the payment
engine. Once the payment is processed---either in house or by a third
party---`CommercePaymentEngine.java` calls the `completePayment` method to
provide information on the outcome.

This is example is a simple offline payment method. Since payment is not
processed online, the payment engine is not needed as the transaction is assumed
to be successful---a payment status of `PAID` is actually hard-coded into the
`completePayment` method. However, the implementation still follows the
@commerce@ standard of exchanging data between the payment method and payment
engine in order to store the result of the transaction.

Start by adding a component to your module:

    package com.liferay.commerce.payment.method.sample;

    import com.liferay.commerce.constants.CommerceOrderConstants;
    import com.liferay.commerce.constants.CommercePaymentConstants;
    import com.liferay.commerce.payment.method.CommercePaymentMethod;
    import com.liferay.commerce.payment.request.CommercePaymentRequest;
    import com.liferay.commerce.payment.result.CommercePaymentResult;
    import com.liferay.portal.kernel.language.LanguageUtil;
    import com.liferay.portal.kernel.util.ResourceBundleUtil;

    import java.util.Collections;
    import java.util.Locale;
    import java.util.ResourceBundle;

    import org.osgi.service.component.annotations.Component;

    @Component(
        immediate = true,
        property = "commerce.payment.engine.method.key=" + SampleCommercePaymentMethod.KEY,
        service = CommercePaymentMethod.class
    )
    public class SampleCommercePaymentMethod implements CommercePaymentMethod {

        public static final String KEY = "sample";

Include the `completePayment` method. This returns a `CommercePaymentResult`
object containing various data on the order. In this example, since the payment
method is offline, the object returned contains only the `OrderID`, the `PAID`
status, and boolean values indicating that payment was successful and that no
redirect to a third-party payment provider is necessary. Online payment methods
would require different parameters.

        @Override
        public CommercePaymentResult completePayment(
                CommercePaymentRequest commercePaymentRequest)
            throws Exception {

            return new CommercePaymentResult(
                null, commercePaymentRequest.getCommerceOrderId(),
                CommerceOrderConstants.PAYMENT_STATUS_PAID, false, null, null,
                Collections.emptyList(), true);
        }

The `getDescription` method provides a description of your payment method in the
language appropriate to the user's locale. Meanwhile the `getKEY` method returns
the `KEY` string specified immediately after the class declaration, and
`getName` calls `_getResourceBundle` to get the payment method's localized name.

        @Override
        public String getDescription(Locale locale) {
            return null;
        }

        @Override
        public String getKey() {
            return KEY;
        }

        @Override
        public String getName(Locale locale) {
            ResourceBundle resourceBundle = _getResourceBundle(locale);

            return LanguageUtil.get(resourceBundle, KEY);
        }

The `getPaymentType` method, in this case, indicates that the payment method is
offline. See `CommercePaymentConstants.java` for additional values. Meanwhile
`getServletPath` is used to identify the servlet that manages the payment. It is
required by the interface but does nothing in this case.

        @Override
        public int getPaymentType() {
            return CommercePaymentConstants.COMMERCE_PAYMENT_METHOD_TYPE_OFFLINE;
        }

        @Override
        public String getServletPath() {
            return null;
        }

The `isCompleteEnabled` and `isProcessPaymentEnabled` methods are not required
by the interface, but return `false` under the default implementation. Include
these methods to specify that you are using the `processPayment` and
`completePayment` methods.

        @Override
        public boolean isCompleteEnabled() {
            return true;
        }

        @Override
        public boolean isProcessPaymentEnabled() {
            return true;
        }

Include the `processPayment` method. To initiate the payment process,
`processPayment` returns a `CommercePaymentResult` object containing data on the
order. 

        @Override
        public CommercePaymentResult processPayment(
                CommercePaymentRequest commercePaymentRequest)
            throws Exception {

            return new CommercePaymentResult(
                null, commercePaymentRequest.getCommerceOrderId(),
                CommerceOrderConstants.PAYMENT_STATUS_AUTHORIZED, false, null, null,
                Collections.emptyList(), true);
        }

Finally, include `_getResourceBundle` to provide the language bundle appropriate
to the user's location.

        private ResourceBundle _getResourceBundle(Locale locale) {
            return ResourceBundleUtil.getBundle(
                "content.Language", locale, getClass());
        }

    }

One final method needed for online payments---but not necessary in the offline
example above---is a `cancelPayment` method to allow a buyer to cancel
a transaction. If you're processing payment online but in-house, this method
must include logic to set the order status to `CANCELLED`. If you're using
a third-party payment processor, then the processor's requirements will dictate
how `cancelPayment` should be written.

## Creating an Admin Screen

You need a UI for administrators to manage your payment method. Implement the
`ScreenNavigationEntry` interface to extend the admin portlet at *Site Menu*
&rarr; *Commerce* &rarr; *Settings* &rarr; *Payment Methods*.

Add a new package to your module and create a new component. Implement the
interface and include the `<CommercePaymentMethodGroupRel>` type parameter:

    package com.liferay.commerce.payment.method.sample.servlet.taglib.ui;

    import com.liferay.commerce.payment.constants.CommercePaymentScreenNavigationConstants;
    import com.liferay.commerce.payment.method.money.order.internal.SampleCommercePaymentMethod;
    import com.liferay.commerce.payment.method.money.order.internal.configuration.SampleGroupServiceConfiguration;
    import com.liferay.commerce.payment.method.money.order.internal.constants.SampleCommercePaymentEngineMethodConstants;
    import com.liferay.commerce.payment.model.CommercePaymentMethodGroupRel;
    import com.liferay.frontend.taglib.servlet.taglib.ScreenNavigationEntry;
    import com.liferay.frontend.taglib.servlet.taglib.util.JSPRenderer;
    import com.liferay.portal.kernel.exception.PortalException;
    import com.liferay.portal.kernel.language.LanguageUtil;
    import com.liferay.portal.kernel.model.User;
    import com.liferay.portal.kernel.module.configuration.ConfigurationProvider;
    import com.liferay.portal.kernel.settings.GroupServiceSettingsLocator;
    import com.liferay.portal.kernel.settings.ParameterMapSettingsLocator;
    import com.liferay.portal.kernel.util.Portal;

    import java.io.IOException;

    import java.util.Locale;

    import javax.servlet.ServletContext;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    import org.osgi.service.component.annotations.Component;
    import org.osgi.service.component.annotations.Reference;

    @Component(
        property = "screen.navigation.entry.order:Integer=20",
        service = ScreenNavigationEntry.class
    )
    public class
        SampleCommercePaymentEngineMethodConfigurationScreenNavigationEntry
            implements ScreenNavigationEntry<CommercePaymentMethodGroupRel> {

Specify a string to serve as your entry's primary key:

        public static final String
            ENTRY_KEY_SAMPLE_COMMERCE_PAYMENT_METHOD_CONFIGURATION =
                "money-order-configuration";

Then include the methods required by the interface. For more information on
these methods, see 
[Using the Framework for your Application](develop/tutorials/-/knowledge_base/7-1/using-the-framework-for-your-application).

        @Override
        public String getCategoryKey() {
            return CommercePaymentScreenNavigationConstants.
                CATEGORY_KEY_COMMERCE_PAYMENT_METHOD_CONFIGURATION;
        }

        @Override
        public String getEntryKey() {
            return ENTRY_KEY_SAMPLE_COMMERCE_PAYMENT_METHOD_CONFIGURATION;
        }

        @Override
        public String getLabel(Locale locale) {
            return LanguageUtil.get(
                locale,
                CommercePaymentScreenNavigationConstants.
                    CATEGORY_KEY_COMMERCE_PAYMENT_METHOD_CONFIGURATION);
        }

        @Override
        public String getScreenNavigationKey() {
            return CommercePaymentScreenNavigationConstants.
                SCREEN_NAVIGATION_KEY_COMMERCE_PAYMENT_METHOD;
        }

        @Override
        public boolean isVisible(
            User user, CommercePaymentMethodGroupRel commercePaymentMethod) {

            if (SampleCommercePaymentMethod.KEY.equals(
                    commercePaymentMethod.getEngineKey())) {

                return true;
            }

            return false;
        }

        @Override
        public void render(
                HttpServletRequest httpServletRequest,
                HttpServletResponse httpServletResponse)
            throws IOException {

            try {
                SampleGroupServiceConfiguration
                    sampleGroupServiceConfiguration =
                        _configurationProvider.getConfiguration(
                            SampleGroupServiceConfiguration.class,
                            new ParameterMapSettingsLocator(
                                httpServletRequest.getParameterMap(),
                                new GroupServiceSettingsLocator(
                                    _portal.getScopeGroupId(httpServletRequest),
                                    SampleCommercePaymentEngineMethodConstants.
                                        SERVICE_NAME)));

                httpServletRequest.setAttribute(
                    SampleGroupServiceConfiguration.class.getName(),
                    sampleGroupServiceConfiguration);
            }
            catch (PortalException pe) {
                throw new IOException(pe);
            }

            _jspRenderer.renderJSP(
                _servletContext, httpServletRequest, httpServletResponse,
                "/configuration.jsp");
        }

Finally, include the necessary `@Reference` tags.

        @Reference
        private ConfigurationProvider _configurationProvider;

        @Reference
        private JSPRenderer _jspRenderer;

        @Reference
        private Portal _portal;

        @Reference(
            target = "(osgi.web.symbolicname=com.liferay.commerce.payment.method.sample)"
        )
        private ServletContext _servletContext;

    }

Note that `com.liferay.commerce.payment.method.sample` in the final `@Reference`
is the `Bundle-SymbolicName` from the module's `bnd.bnd` file.

### Providing JSP

Now all you need is a JSP to render. Note that the `render` method calls for
`configuration.jsp` to be located in your modules' `META-INF/resources`
directory.

This example provides an admin screen where administrators can enter a message
to be displayed to buyers after placing an order:


    <%
    String messageXml = null;

    SampleGroupServiceConfiguration sampleGroupServiceConfiguration = (SampleGroupServiceConfiguration)request.getAdttribute(SampleGroupServiceConfiguration.class.getName());

    LocalizedValuesMap messageLocalizedValuesMap = sampleGroupServiceConfiguration.message();

    if (messageLocalizedValuesMap != null) {
        messageXml = LocalizationUtil.getXml(messageLocalizedValuesMap, "message");
    }
    %>

    <portlet:actionURL name="editSampleCommercePaymentMethodConfiguration" var="editCommercePaymentMethodActionURL" />

    <aui:form action="<%= editCommercePaymentMethodActionURL %>" cssClass="container-fluid-1280" method="post" name="fm">
        <aui:input name="<%= Constants.CMD %>" type="hidden" value="<%= Constants.UPDATE %>" />
        <aui:input name="redirect" type="hidden" value="<%= currentURL %>" />

        <aui:fieldset-group markupView="lexicon">
            <aui:fieldset>
                <aui:field-wrapper label="message">
                    <liferay-ui:input-localized
                        editorName="alloyeditor"
                        fieldPrefix="settings"
                        fieldPrefixSeparator="--"
                        name="message"
                        type="editor"
                        xml="<%= messageXml %>"
                    />
                </aui:field-wrapper>
            </aui:fieldset>
        </aui:fieldset-group>

        <aui:button-row>
            <aui:button cssClass="btn-lg" type="submit" />

            <aui:button cssClass="btn-lg" href="<%= redirect %>" type="cancel" />
        </aui:button-row>
    </aui:form>

If you've followed these instructions correctly, you should be able to deploy
your module and see your JSP when you select the appropriate option from *Site
Menu* &rarr; *Commerce* &rarr; *Settings* &rarr; *Payment Methods*.
