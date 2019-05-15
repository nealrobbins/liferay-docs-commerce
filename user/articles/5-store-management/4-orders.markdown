# Orders

An Order stores data on a planned or past transaction.

A new pending order is created when a user places products into a cart. The data
stored includes the identity and quantity of the products, as well as the
account which created it. The order's status is also stored as *Open*.

When the user checks out and the transaction is completed, the order's status is
changed to *Pending*. Additional information---such as billing and shipping
addresses and payment information---is collected by the checkout process and
added to the order.

When the order is processed (by an administrator) and sent to a warehouse for
fulfillment, the status is again updated to *transmitted*. Additional
information can be added to the order, such as shipping information and an
estimated arrival time.

## Order Management

Administrators can manage orders with the Orders Administrative Portlet in *Site
Menu* &rarr; *Commerce* &rarr; *Orders*.

![Figure 1: The Orders portlet displays a list of orders sorted by status into
three tabs.](../../images/orders.png)

Administrators can edit any of the information stored by an order. This
information can be presented to other users--particularly buyers--using the Open
Carts widget (for open orders) and the Orders widget (for pending and
transmitted orders). See 
[Displaying Orders](/web/liferay-emporio/documentation/-/knowledge_base/1-0/displaying-orders)
for details.
