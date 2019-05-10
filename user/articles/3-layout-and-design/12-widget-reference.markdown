# Commerce Widgets

Liferay Commerce ships with a wide range of widgets you'll use to construct your
site. This section provides a convenient overview in one place.

* **Account Management**: The Account Management widget allows buyers, sales agents and administrators to
  manage their accounts without having to access to the Liferay control panel.

  Administrators can view a list of all accounts, create new accounts, existing
  accounts, relate accounts to organizations, add members to accounts and manage
  the roles and profile data of account members. Different roles, e.g. sales
  agents or buyers, can be given fewer permissions.

* **Cart**: The Cart widget displays the cart’s current contents. It includes controls
  to change the quantity of the products it holds, or to delete them entirely.

* **Cart Summary**: The Cart Summary widget displays the total price of all items in the cart,
  including a checkout button. It doesn’t list the cart’s contents.

* **Mini Cart**: The Mini Cart displays the cart’s contents and includes a checkout button,
  but has no controls to edit its content. Instead, it displays an Edit Cart
  button that links to the Cart widget. Because of this link, the Cart Mini widget
  can’t function unless a page containing a Cart Content widget is present.

* **Category Content**: The Category Content widget allows you to display the products contained in a category and all the related products and content.

* **Checkout**: The checkout widget handles purchase process. Without any configuration, the
  Checkout widget features a four-step process: shipping address, billing address,
  order summary and order confirmation. When you configure 
  [Shipping Methods](/web/liferay-emporio/documentation/-/knowledge_base/1-0/shipping-methods)
  and 
  [Payment Methods](/web/liferay-emporio/documentation/-/knowledge_base/1-0/payment-methods),
  additional steps are automatically added to the process.

  Custom checkout steps can be added using the 
  [custom checkout step extension point](/web/liferay-emporio/documentation/-/knowledge_base/1-0/custom-checkout-step).

* **Commerce Categories Navigation**: The Commerce Categories Navigation widget works with [Category
  Content](#category-content) facilitate navigation to product category pages.

  Select a vocabulary on the widget's configuration screen. All of the categories
  in that vocabulary will be displayed by the widget as links to category pages.
  When a user selects one of those links, that selection is provided as context to
  the Category Content widget, which then displays the desired category.

* **Orders**: The Order Content widget displays a list of orders that have been placed. Click
  on an order to display additional information about the transaction, with an
  option to reorder.

* **Open Carts**: The Pending Orders widget displays orders that have not been submitted to the
  seller. The cart's current contents are included as a pending order, as well as
  any orders that are in the process of a buyer-side workflow.

* **Coupon Code Entry**: you've created a discount that requires the entry of a coupon code, then
  buyers who know the code will need something to do with it. Drop a coupon code
  widget onto a page--sharing a page with Cart or Pending Orders is logical, but
  you can put it anywhere a buyer will have access prior to completing an order.

  See
  [Discounts](/web/liferay-emporio/documentation/-/knowledge_base/1-0/discounts)
  for more on coupon codes.

* **Product Comparison Table**: Displays the information for different products side-by-side. The Product
  Comparsion Bar is used to add products to the table.

* **Product Comparison Bar**: When placed on a product page, this widget modifies the Product Detail widget to
  include a *Compare* button. Clicking it adds the current product to the compare
  products widget. Clicking the *Compare* button on the Compare Products Mini
  widget navigates to the Compare Products widget and displays selected products
  side-by-side.

* **Product Details**: The Product Details widget displays information about a product.When a user
  clicks on a link to a product page, the widget retrieves the appropriate content
  from the catalog and displays it on the page. 

  Product Details works much like Category Content (see above). Rather than
  forcing you to configure a unique page for every product, the widget allows you
  to use a single page where the widget simply reads the current context to
  determine what product's information should be displayed.

* **Product Downloads**: For Virtual Products

* **Product Publisher**: The Product Publisher simply displays a list of products. You can select
  products manually, or provide criteria so Commerce can make the selection for
  you.
  * Dynamic: Publish products associated with certain categories and or tags.
    From the Configuration screen, open the *Filter* section and specify what
    categories or tags you want to publish or exclude from publishing.

  * dataSource: Select a criteria that will be applied depending on the current context.

* **Product Subscriptions**: For the buyer to track his subscriptions

* **Search Results**: To be merged with Search's search results? For now, it only finds products

* **Option Facet**: Group with search results
* **Specification Facet**:
* **Shipments**:

* **Wish List Contents**: Used for editing and viewing the content of a single wishlist. It looks like the typcial use case is to put both of these widgets side-by-side on a page.

* **Wish Lists**: A wishlist isn't so much a wishlist as a list of products that a buyer might
  want to track.

  The My Commerce Wish List widget manages wish lists--whatever wish lists you
  have access to. If you're an administrator, that's all of them.