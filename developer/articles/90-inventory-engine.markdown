# Using a custom inventory engine

@commerce@ includes a fully-functional inventory engine, but you don't have to
use it if you don't want to. If you're already keeping inventory data in
a separate database and want to keep the system you have, you can take advantage
of the `CPDefinitionInventoryEngine` extension point to integrate commerce with
your current system.

Once the integration is complete, you can use your own inventory engine to track
a particular product by going to that product's *Configuration* tab in *Site
Menu* &rarr; *Commerce* &rarr; *Catalog*.

![Figure 1: Out of the box, the *Default* inventory engine is the only one available.](../images/inventory-engine.png)

Follow these steps:

1.  Create a new module, including a dependency on `com.liferay.commerce.api` in
    the build script.

2.  Implement the `CPDefinitionInventoryEngine` interface.

First, create the new module. Its `build.gradle` file should look like this.


    dependencies {
    …
    compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.api", version: "5.0.0"
    …
    }

Next, create a component to implement the interface:

    @Component(
      immediate = true,
      property = {
         "cp.definition.inventory.engine.key=" + CPDefinitionInventoryEngineImpl.KEY,
         "cp.definition.inventory.engine.priority:Integer=1"
      },
      service = CPDefinitionInventoryEngine.class
    )
    public class CPDefinitionInventoryEngineImpl
      implements CPDefinitionInventoryEngine {

      public static final String KEY = "default";

Note that the string `KEY` is used to identify your inventory engine in the UI
(see Figure 1 above). The `priority:Integer=1` property sets its location in the
drop-down.

Next, include the methods required by the interface:

    public String[] getAllowedOrderQuantities(CPInstance cpInstance)
      throws PortalException;

    public String getAvailabilityEstimate(CPInstance cpInstance, Locale locale)
      throws PortalException;

    public String getKey();

    public String getLabel(Locale locale);

    public int getMaxOrderQuantity(CPInstance cpInstance)
      throws PortalException;

    public int getMinOrderQuantity(CPInstance cpInstance)
      throws PortalException;

    public int getMinStockQuantity(CPInstance cpInstance)
      throws PortalException;

    public int getMultipleOrderQuantity(CPInstance cpInstance)
      throws PortalException;

    public int getStockQuantity(CPInstance cpInstance);

    public boolean isBackOrderAllowed(CPInstance cpInstance)
      throws PortalException;

    public boolean isDisplayAvailability(CPInstance cpInstance)
      throws PortalException;

    public boolean isDisplayStockQuantity(CPInstance cpInstance)
      throws PortalException;

    public int updateStockQuantity(
      CommerceWarehouseItem commerceWarehouseItem, int quantity);
    ...
    }

These methods collect data from the *Inventory* section of a product's
*Configuration* screen (see [configuration]()). When you write your custom code
for these methods, include logic to provide whatever of this data is
required by your custom inventory engine.
