# Customizing Product Lists

To customize a product list with a renderer, you need:

-   A JSP or other file to render

-   A Java class implementing `CPContentListRenderer`

Follow these steps:

1.  Create a module and include a dependency on
    `com.liferay.commerce.product.content.api` in your `build.gradle` file.

The dependency should look like this:

    compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.product.content.api", version: "1.0.0"

2.  Implement the interface:

    @Component(
      immediate = true,
      property = {
         "commerce.product.content.list.renderer.key=" + CPPortletKeys.CP_COMPARE_CONTENT_WEB,
         "commerce.product.content.list.renderer.order=" + Integer.MIN_VALUE,
         "commerce.product.content.list.renderer.portlet.name=" + CPPortletKeys.CP_COMPARE_CONTENT_WEB
      },
      service = CPContentListRenderer.class
    )
    public class CompareProductsCPContentListRenderer
      implements CPContentListRenderer {

The `"commerce.product.content.list.renderer.portlet.name="` property is
required to specify the widget that you're customizing.
      
      @Override
      public void render(
            HttpServletRequest httpServletRequest,
            HttpServletResponse httpServletResponse)
         throws Exception {

         _jspRenderer.renderJSP(
            httpServletRequest, httpServletResponse,
            "/compare_products/render/view.jsp");
      }

      @Reference
      private JSPRenderer _jspRenderer;
    }

Include the `render` method. In this example, the JSP rendered is in
`META-INF/resources/compare_products/render`.

3. Specify a `Web-ContextPath` in the `bnd.bnd` file.

Once you deploy your module, you can select your renderer from the widget's
configuration screen. Click the *Render Selection* tab, select *Use Custom
Renderer*, and select your renderer from the *Product List Renderer* dropdown.
