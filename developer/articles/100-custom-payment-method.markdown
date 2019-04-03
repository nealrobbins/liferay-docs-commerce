# Creating Custom Payment Methods

@commerce@ includes a number of payment methods out of the box, but these will
not satisfy all users' needs. Fortunately, payments are highly customizable.

This tutorial covers the creation of new payment methods which can be activated
by administrators from *Settings* &rarr; *Payment Methods*. The business logic
of how you process payments or integrate your system with any particular
payment provider is outside the scope of this article.

## Overview

A Payment Method---that is, a class implementing
`CommercePaymentMethod`---handles payment processing. There are three main
types of payment methods:

**Offline:** @commerce@ does not process payment (for example, a buyer might
pay in cash when picking up a product that has been shipped to a local store).

**Online Standard:** Payment is processed entirely by @commerce@.

**Online Redirect:** @commerce@ passes information to a third-party payment processor. Payment is processed online by a third party.

In all cases the process involves passing information from the servlet to the
payment method via one of two payment engines.

![Figure 1: All payment methods---even offline methods---follow this standard for transferring information.](../images/payment-process.png)

First, the Payment Servlet receives a request from the Checkout Widget.
Alternately---for example if @commerce@ is being used headless---the request may
come from the API.

If the request specifies that a standard payment is being made, the servlet
calls the Payment Engine. If the request specifies a subscription payment, the
Subscription Engine is called.

The engine then selects the appropriate Payment Method. The Payment Method
processes payment, either by calling a third-party payment processor or else by
whatever internal logic it contains. It then returns confirmation to the servlet
that payment has been processed.

### Methods

Your Payment Method---an implementation of `CommercePaymentMethod`---may need
to handle calls to the following methods. The interface includes a default
implementation, so it is not necessary to include methods you don't want to
use.

These methods handle actions that buyers may take:

`processPayment`: The payment process begins when this method is called by
`CommercePaymentEngine.java`'s `startPayment` method. `processPayment` is
required for any non-recurring payment to be processed.

`processRecurringPayment`: When a recurring payment is processed, this method
is called instead of `processPayment`. It it is required if you want to support
recurring payments.

`completePayment` and `completeRecurringPayment`: these methods are required to
complete the processing of standard and recurring payments.

`cancelPayment` and `cancelRecurringPayment`: These methods are called when a
user cancels an order or a subscription.

`getSubscriptionValidity`: This method is called when @commerce@ checks to see
if a subscription is still active.

These methods handle actions that administrators may take:

`authorizePayment`: This method is called to get payment authorization from
a credit card company.

`capturePayment`: This method is called to initiate the transfer of funds once
payment is authorized.

`refundPayment` and `partiallyRefundPayment`: These methods are called to issue
a refund in whole or in part.

`voidTransaction`: This method is called to cancel an authorized transaction
that has not yet been captured.

`suspendRecurringPayment`: this method is called to stop payments on a
subscription without canceling it.

`activateRecurringPayment`: this method is called to resume payments on a
suspended subscription.

The interface also calls for a number of more generic methods (`getKey` for
example) that are covered in the example below.

## Creating a New Payment Method

To create a new payment method follow these steps:

1.  Create a module, referencing `commerce-api` and `commerce-payment-api` in
    its build script.

2.  Create a component implementing the
    `com.liferay.commerce.payment.method.CommercePaymentMethod` interface.
    Provide whatever utility classes your component requires.

If you want to provide a configuration screen for administrators to manage your
payment method, create a second component implementing the
`ScreenNavigationEntry` interface, and provide a JSP for the implementation to
render. See 
[Adding Custom Screens to Liferay Applications](develop/tutorials/-/knowledge_base/7-1/adding-custom-screens-to-liferay-applications)
for details.

### Creating a Module

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

This example is a simple offline payment method. Since payment is not processed
online, the transaction is assumed to be successful---a payment status of
`PAID` is actually hard-coded into the `completePayment` method. However, the
implementation still follows the @commerce@ standard of exchanging data between
the payment method and payment engine, and the servlet in order to store the
result of the transaction.

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

Once your implementation is complete---along with whatever utility classes your
method might require---you can deploy it. If you want administrators to be able
to manage the payment method from the UI, however, create a configuration
screen by implementing the `ScreenNavigationEntry` interface in a new
component. See 
[Adding Custom Screens to Liferay Applications](develop/tutorials/-/knowledge_base/7-1/adding-custom-screens-to-liferay-applicationsusing-the-framework-for-your-application)
for more information.

If you choose to include a configuration screen, then you should be able to
deploy your module and see the JSP rendered by your `ScreenNavigationEntry`
implementation when you select the appropriate option from *Site Menu* &rarr;
*Commerce* &rarr; *Settings* &rarr; *Payment Methods*.
