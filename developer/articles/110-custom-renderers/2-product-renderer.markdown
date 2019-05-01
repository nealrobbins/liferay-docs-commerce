# Creating a Custom Product Renderer

To customize your *Product Details* widget with a renderer, you need:

-   A JSP or other file (SOY, .ftl, etc.) to render.

-   A Java class implementing the `CPContentRenderer` interface. This class
    identifies both the JSP and the product type to which it corresponds.

Once you have your JSP, follow these steps:

1.  Create a module and include a dependency on
    `com.liferay.commerce.product.api` in your `build.gradle` file:

        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.product.api", version: "1.0.0"

2. Implement the interface:

        @Component(
          immediate = true,
          property = {
             "commerce.product.content.renderer.key=" + VirtualCPContentRenderer.KEY,
             "commerce.product.content.renderer.order=" + Integer.MIN_VALUE,
             "commerce.product.content.renderer.type=" + VirtualCPTypeConstants.NAME 
          },
          service = CPContentRenderer.class
        )
        public class VirtualCPContentRenderer implements CPContentRenderer {
          public static final String KEY = "virtual";

The third property, `"commerce.product.content.renderer.type="` is required to
bind the implementation to a product type.

    @Override
    public void render(
        CPCatalogEntry cpCatalogEntry,
        HttpServletRequest httpServletRequest,
        HttpServletResponse httpServletResponse)
     throws Exception {
     ...
     _jspRenderer.renderJSP(
        _servletContext, httpServletRequest, httpServletResponse,
        "/render/view.jsp");
    }

This method is required by the interface and renders `view.jsp` in your
module's `META-INF/resources/render` folder.

    @Reference
      private JSPRenderer _jspRenderer;
    @Reference(
         target = "(osgi.web.symbolicname=com.liferay.commerce.product.type.virtual.web)"
      )
      private ServletContext _servletContext;
    }

Include these `@Reference` tags. Note that instead of
`com.liferay.commerce.product.type.virtual.web`, your reference should match
the `Bundle-SymbolicName` from your module's `bnd.bnd` file. 

3. Specify `Web-ContextPath` in the `bnd.bnd` file.

Once you deploy your module, open a Product Details widget's configuration
screen, select *Use Custom Renderer*, select the product type you assigned your
renderer to, and choose your renderer from the selector.
