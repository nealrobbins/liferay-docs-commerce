# Displaying Products with Custom Renderers

Many widgets in @product@ can be customized using an 
[Application Display Template](/discover/portal/-/knowledge_base/7-1/styling-widgets-with-application-display-templates),
but there are a number of cases where this isn't ideal for @commerce@.

With custom renderers, you can render certain widgets any way you want---using
JSP, SOY, ftl or any other template---simply by implementing an interface. This
gives you more power and flexibility than an ADT. An additional benefit is that
it allows widgets to display products differently depending on their 
[product type](/web/commerce/documentation/-/knowledge_base/1-0/products).

Once you've deployed a renderer, you can configure widgets to use it. The front
end for this is already in place:

![Figure 1: Once a custom renderer is in place, you can select it from a widget's configuration screen.](../../images/cpcontentrenderer.png)

[Caption:] Once a custom renderer is in place, you can select it from a widget's configuration screen.

You need two things to create a new product renderer:

-   A file to render. The examples in the following pages use JSP, but you can
    use what you want (SOY, .ftl, etc.).

-   A Java class implementing the appropriate interface.

There are three renderer interfaces:

-   `CPContentRenderer`

-   `CPContentListRenderer`

-   `CPContentListEntryRenderer`

These can be used to customize the *Product Details*, *Search Results*,
*Product Publisher*, and *Product Comparison Table* widgets.

## Content Renderer

Use an implementation of `CPContentRenderer` to customize the appearance of a
*Product Details* widget, particularly to alter the display depending on the
type of product being viewed. The implementation identifies both the template
to render and the product type you to which it corresponds. See details on the
code
[here](/web/commerce/documentation/-/knowledge_base/1-0/creating-a-custom-product-renderer).

## Content List Renderer

Use `CPContentListRenderer` to customize a portlet that displays a list of
products: *Search Results*, *Product Publisher*, or *Product Comparison Table*.
You can only customize the list itself---for instance, you might create a grid,
a list, an animated display, or practically anthing else you can think of---but
you can't customize the appearance of entries in the list (use
`CPContentListEntryRenderer` for that). Your implmentation
must specify which widget it applies to.  Details
[here](/web/commerce/documentation/-/knowledge_base/1-0/customizing-product-lists).

## Content List Entries Renderer

Use `CPContentListEntryRenderer` to customize the appearance of the entries in
a list of products. Like `CPContentListRenderer`, this can be applied to
*Search Results*, *Product Publisher*, or *Compare Products*. Unlike the List
Renderer, it can display products differently depending on their product type.
Your implementation must specify both the widgets and the product type you want
to render. Details
[here](/web/commerce/documentation/-/knowledge_base/1-0/customizing-product-list-entries).
