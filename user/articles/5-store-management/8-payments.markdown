# Payments

Payments are processed through a *Payment Method*. There are three types of
payment method:

**Offline:** Liferay Commerce does not process payment (for example, a buyer
might pay in cash when picking up a product that has been shipped to a local
store).

**Online Standard:** Payment is processed entirely by Liferay Commerce. This
option is not available out-of-the-box but can be customized using the 
[Payment Method SPI](/docs/7-2/frameworks/-/knowledge_base/f/creating-custom-payment-methods).

**Online Redirect:** Liferay Commerce passes information to a third-party
payment processor, redirecting the buyer to the processor's website to complete
payment.

Commerce ships with four payment methods. One---Money Order---is an offline
method while the others---Authorize.net, Mercanet, and Paypal---redirect buyers
to a third-party website. Additional methods can be added using the 
[Payment Method SPI](/docs/7-2/frameworks/-/knowledge_base/f/creating-custom-payment-methods).

To use a payment method, you must configure it and set it to *Active*.
Configuring a *Redirect* method requires information you'll obtain from the
relevant third-party processor.

## Managing Payment Methods

To manage payment methods, go to *Site Menu* &rarr; *Commerce* &rarr; *Settings*
and select the *Payment Methods* tab. Select a method to edit it.

The *Edit Payment Method* UI has three tabs: *Details*, *Configuration* and
*Restrictions*. Details and Restrictions are the same in all cases, but each
payment method has a unique configuration screen.

**Details:** Each details screen contains fields that determine how the payment
method is rendered in the Checkout widget. You can change the name, enter
descriptive text, and upload an image.

You can also enter a number in the *priority* field to order payment methods in
the Checkout widget. Lower numbers are listed first.

Enable the *Active* toggle to make the payment method available to buyers.

**Restrictions:** A restriction deactivates a payment method for buyers in
specified countries. To create a restriction, click the
![Add](../../images/icon-add.png) button and select countries from the list.

### Configuration

Each payment method has a unique configuration screen. Keep reading to learn how
to configure your preferred payment method.

| **Note:** The payment providers listed here offer many more configuration
| options than are supported on these configuration screens. Further customization
| can be achieved using the 
| [Payment Method SPI](/docs/7-2/frameworks/-/knowledge_base/f/creating-custom-payment-methods).

#### Authorize.net

The Authorize.net configuration screen contains these fields:

**API Login ID, Transaction Key:** Entries for these fields must be obtained
from the third party.

**Environment:** Select *Sandbox* for testing and *Production* to conduct real
transactions.

**Display and Security:** the remaining fields customize the Authorize.net page
to which customers are redirected.

**Show Bank Account:** select to enable payment via direct deposit from a bank
account.

**Show Credit Card:** Select to enable credit card payments.

**Show Store Name:** Select to display your site's name on Authorize.net's page.

**Require CAPTCHA:** Require buyers to pass a CAPTCHA test before completing
a transaction.

**Require Card Code Verification:** Require buyers to enter their credit card's
3-digit Security Code.

#### Mercanet

The Mercanet configuration screen contains these fields:

**Merchant ID, Secret Key, Key Version:** Entries for these fields must be
obtained from the third party.

**Environment:** Select *Test* for testing and *Production* to conduct real
transactions. *Simulation* is also available as a testing environment, but at a
late stage in development Mercanet recommended that *Test* be used instead.

#### Money Order

The Money Order configuration screen contains only one field:

**Message:** Enter text to be displayed by the checkout widget after a buyer
selects this payment method.

| **Note:** The embedded text editor allows you a wide range of formatting
| options. You can upload files, embed video, or type HTML directly into the
| editor.

#### PayPal

The PayPal configuration screen contains these fields:

**Client ID, Client Secret:** Entries for these fields must be obtained from the
third party.

**Mode:** Select *Sandbox* for testing and *Live* to conduct real transactions.

**Payment Attempts Max Count:** Enter the number of attempts to make payment on
a 
[subscription](docs/7-2/user/-/knowledge_base/u/subscriptions) 
before cancelling the subscription. This only applies to subscriptions; in the
case of a regular purchase the buyer can be immediately informed if he has
insufficient funds for a transaction.
