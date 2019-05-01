# Customizing Product List Entries

To customize product list entries with a renderer, you need:

-   A JSP or other file to render

-   A Java class implementing `CPContentListEntryRenderer`

Follow these steps:

1.  Create a module and include a dependency on
    `com.liferay.commerce.product.content.api` in your `build.gradle` file.

The dependency should look like this:

    compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.product.content.api", version: "1.0.0"

2. Implement the interface:

    @Component(
      immediate = true,
      property = {
         "commerce.product.content.list.entry.renderer.key=" + DefaultSimpleCPContentListEntryRenderer.KEY,
         "commerce.product.content.list.entry.renderer.order=" + Integer.MIN_VALUE,
         "commerce.product.content.list.entry.renderer.portlet.name=" + CPPortletKeys.CP_PUBLISHER_WEB,
         "commerce.product.content.list.entry.renderer.portlet.name=" + CPPortletKeys.CP_SEARCH_RESULTS
      },
      service = CPContentListEntryRenderer.class
    )
    public class DefaultSimpleCPContentListEntryRenderer
      implements CPContentListEntryRenderer {

      public static final String KEY = "default-simple";

The `"commerce.product.content.list.entry.renderer.portlet.name="` property
specifies the widget whose entries you want to customize. As in this example,
you can specify more than one. The property is not required; if you leave it
out the customization will be available to all widgets.

      @Override
      public void render(
            HttpServletRequest httpServletRequest,
            HttpServletResponse httpServletResponse)
         throws Exception {

         _jspRenderer.renderJSP(
            _servletContext, httpServletRequest, httpServletResponse,
            "/render/list_entry/default.jsp");
      }

Include the `render` method. In this case, the JSP in
`META-INF/resources/render/list_entry` is rendered.

      @Reference
      private JSPRenderer _jspRenderer;
      @Reference(
         target = "(osgi.web.symbolicname=com.liferay.commerce.product.type.simple)"
      )
      private ServletContext _servletContext;
    }

Include your `@Reference` tags. Note that the `target` in the second tag is the
`Bundle-SymbolicName` not from the renderer's module, but from the bnd.bnd file
in the module defining the product type you want the renderer to apply to.

3. Specify a `Web-ContextPath` in your module's `bnd.bnd` file.

Once you deploy your module, you can select your renderer from the widget's
configuration screen. Click the *Render Selection* tab, select *Use Custom
Renderer*, and pick a product type from the *Product Type Renderer* section.
Then select your renderer from the dropdown.
