# Subscriptions

Subscriptions allow buyers to make purchases that automatically recur.
A subscription can be applied to virtual products (service agreements,
periodical literature, etc.) as well as to inventoried products.

There are two ways to make products available by subscription:

- When applied to a product, a subscription applies to all sales of that
product and all of its variants. 

- When applied to a single SKU, a subscription applies only to the product
variant represented by that SKU. This overrides any setting applied at the
product level.

If you apply subscriptions to SKUs, you can offer range of different
subscription options for a single product. Buyers can choose the subscription
they want by by selecting the option that corresponds to the appropriate SKU.

## Creating a Subscription

To make a subscription available to buyers, first decide whether to apply the
subscription to all of a product's variants or only to some of them.

To apply to all of a product's variants, go to the catalog, select a product,
and click *Configuration* &rarr; *Subscription*.

To apply a subscription to a single SKU, go to the catalog, select a product,
and click on the *SKUs* tab. Select an SKU and click on the *Subscription
Override* tab.

![Figure 1: Either way takes you to the subscription settings page. The *Subscription Override* tab includes an extra toggle to override settings in *Configuration*.](../../../images/subscription-settings.png)

Then follow these steps:

1.  Toggle *Override Subscription Settings* (if applicable) and *Enable
    Subscriptions* both to *Yes*.

2.  Set a *Subscription Type*. This sets the unit of time involved.

3.  Select a *Mode* to choose the day on which the order should be filled. This
    field is not available if you selected *Day* as the subscription type.

4.  Set the *Subscription Length* to determine the interval---in the
    unit you selected in step 2---between shipments.

6.  Set the subscription to either *Never Ends* or choose a number of cycles.

Click *Save*. Your product---or product variant---is now a subscription product.

## Administration

Active subscriptions can be managed from the subscriptions administrative
portlet. Go to *Site Menu* &rarr; *Commerce* &rarr; *Subscriptions* to access
a list of subscriptions.

![Figure 2: Sort or filter this list to find the subscription you want.](../../../images/subscription-admin.png)

To manage a subscription, click
![Options](../../../icon-kebab-gray-on-white.png) &rarr; *Edit*. From this
screen an administrator can suspend or cancel a subscription or change any of
its settings.

![Figure 3: An admin can make changes to a subscription after it has been purchased.](../../../images/subscription-edit.png)

## Control Panel

The behavior of subscriptions can be fine-tuned from *Control Panel* &rarr;
*Configuration* &rarr; *System Settings* &rarr; *Catalog* &rarr; *Subscriptions*.

![Figure 4: These settings apply to all of your subscriptions.](../../../images/subscriptions-control-panel.png)

**Check Renew Interval**: Set how often---in minutes---*commerce* checks to see what
subscriptions are up for renewal.

**Check Paid Order Interval:** Set how often @commerce@ checks that payments
have been successfully processed.

**Payment Attempts Max Count:** Set the number of times @commerce@ will check
for a successful payment. Check **Cancel Subscription** or **Suspend
Subscription** to set the action to be taken if no payment is made.
