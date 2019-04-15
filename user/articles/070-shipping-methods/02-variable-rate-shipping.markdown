# Variable Rate Shipping [](id=variable-rate-shipping)

Variable rate shipping calculates shipping costs using three factors: the
order's weight, its subtotal (cost before shipping and taxes), and any fixed
price you impose.

You can create multiple different shipping options with variable rates. For
example, you might create a "Standard Ground" option with a relatively low cost
per unit of weight, as well as a more expensive "Two-Day Air" option. When a
buyer places an order, he is prompted to choose from whatever options have been
applied.

Additionally, you can create multiple *Settings* for each shipping option,
allowing a single option to store more than one formula for calculating shipping
costs. This allows costs to be calculated differently depending---for
example---on whether an order must be shipped overseas. Multiple settings within
a single option are invisible to the buyer, who sees only the single shipping
option in the checkout process. 

## Creating a Variable Rate Shipping Option [](id=creating-a-variable-rate-shipping-option)

Variable-rate shipping costs are determined by the following formula: `shipping
costs = [fixed price] + ([order total weight] x [rate unit weight price])
+ ([order subtotal] x [rate percentage])`.

Follow these steps:

1.  Go to *Site Menu* &rarr; *Commerce* &rarr; *Settings* and click the
    *Shipping Method* tab. Choose the *Variable Rate* method and then the
    *Shipping Options* tab.

2.  Click ![Add](../../images/icon-add.png) and fill in the following fields:

    **Name:** Buyers see this name when selecting a shipping option.

    **Description:** Information for buyers---delivery time, guarantees,
    insurance and the like---should go in this field.

    **Priority:** Sets the option's display order. Lower numbers come first.

3.  Click *Save*. Then click the *Shipping Option Settings* tab.

4.  Click ![Add](../../images/icon-add.png) and fill in the following fields:

    ![Figure 1: The first set of fields determines the orders to which this shipping option applies. The second set determines shipping costs.](../../images/variable-shipping.png)

    First, fill in the fields that select the orders to which this shipping
    option applies:

    **Shipping Option:** Select the shipping option for this setting. The first
    time through, select the option you named in step 2.

    **Warehouse:** The option setting will be applied to shipments from the
    selected warehouse. Leave blank to apply regardless of warehouse.

    **Country, Region, Zip:** The option setting will be applied to orders with
    shipping addresses matching the area you define. Leave blank to apply
    regardless of shipping address.

    **Weight From, Weight To:** The option setting will be applied to orders
    with a total weight within the range you define. Leave blank to apply
    regardless of order weight.

    Then fill in the remaining fields to provide the numbers for calculating
    the shipping cost.

    **Fixed Price:** An entry in this field sets an effective minimum price and
    contributes the fixed component of the shipping cost formula. It can be left
    blank.

    **Rate Unit Weight Price:** An entry in this field imposes a cost per unit
    of weight. It can be left blank.

    **Rate Percentage:** An entry in this field imposes a shipping cost based on
    a percentage of the order subtotal. It can be left blank.

5.  Click *Save*. To create additional settings within this option, click
    ![Add](../../images/icon-add.png) and repeat step 4. Alternatively, to
    create additional shipping options, return to the *Shipping Options* tab and
    repeat steps 2 through 4.

6.  To apply your new shipping options, click the *Details* tab and check the
    *Active* box. Click *Save*.

The *Details* tab also contains fields for changing the name and description of
the variable rate shipping method type. They may be useful for reference, but
the text is not displayed to buyers. You can also set a priority, which orders
variable rate shipping methods relative to other types in the checkout widget.
