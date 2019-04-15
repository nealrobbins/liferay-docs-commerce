# Pricing [](id=pricing)

Pricing in @commerce@ is handled by a hierarchy of prices which are applicable
in different circumstances. If more than one price applies to a given
transaction, then the price nearest the top of the hierarchy supersedes others.

The hierarchy consists of these levels:

A **Base Price** is set in a product's 
[SKUs](web/commerce/documentation/-/knowledge_base/1-0/skus#pricing) tab and
applies to all purchases of that product, unless it is superseded by
another price.

A **Price List** applies prices to specified products for a specified user
segment. If a price list applies to a transaction, it supersedes the base price.

A **Tiered Price** is a price that is applied to orders that meet specified
quantity requirements. It is only available in the context of a price list, and
supersedes the list's price.

A **Promo Price** can be applied to a base price, a price list price, and to
a tiered price, superseding each. When a price is superseded by a price list or
tiered price, its promo is superseded as well.

A **Discount** operates outside the price hierarchy, and modifies a price (10%
off, for instance, or $10 off) rather than superseding it. A discount can be
applied to a product (individually or by category), an order subtotal, shipping
costs, or to an order total.

![Figure 1: An order subtitle is the sum cost of all the products in an order, after category and product discounts are applied.](../../images/price-hierarchy2.png)

+$$$

Should you use a price list or a discount to adjust your products' prices?

It depends---and you may want a combination of both. If you want to offer
a special price exclusively to a particular group of customers, and you want to
set the adjusted price for each product individually, you should use a price
list.

If you want to set prices using a currency other than your store's default, or
if you want to increase prices rather than lower them, you should use a price
list.

If you want to offer a reduced rate to bulk buyers, you should use a price list
with tiered pricing.

If you want to apply a blanket modifier to a group of products, however---for
instance, 10% off everything in the catalog, or $10 off everything in
a particular category---you should use a discount. A discount can be applied to
a specific user segment or made available to all customers.

You should use a discount if you want to:

- limit the number of products that can be bought for the adjusted price 
 
- Make special pricing available to customers who enter a coupon code 

- Make special pricing available to customers who spend or have spent
  a specified amount 

$$$
