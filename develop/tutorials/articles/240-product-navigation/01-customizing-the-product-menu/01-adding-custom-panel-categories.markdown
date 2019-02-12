# Adding Custom Panel Categories [](id=adding-custom-panel-categories)

As you navigate the Product Menu, you can see that Panel Apps like *Web
Content* and *Site Settings* are organized into Panel Categories such as
*Content* and *Configuration*. This tutorial explains how to add new Panel
Categories to the menu. Adding new Panel Apps is covered in the next section.

There are three steps to creating a new category:

1.  Create the OSGi structure and metadata.

2.  Implement Liferay's Frameworks.

3.  Define the Control Menu Category.

## Creating the OSGi Module [](id=creating-the-osgi-module)

First you must create the project.

1.  Create an OSGi module using your favorite third party tool, or use
    [Blade CLI](/develop/tutorials/-/knowledge_base/7-1/blade-cli). Blade CLI
    offers a 
    [Panel App](/develop/reference/-/knowledge_base/7-1/panel-app-template) template,
    which is for creating a panel category and panel app.

2.  Create a unique package name in the module's `src` directory and create
    a new Java class in that package. To follow naming conventions, give your
    class a unique name followed by `PanelCategory` (e.g.,
    `ControlPanelCategory`).

## Implementing Liferay's Frameworks [](id=implementing-liferays-frameworks)

Next, you must connect your OSGi module to Liferay's frameworks and use those to
define information about your entry. This takes only two steps: 

1.  Insert the `@Component` annotation declaring the panel category keys. 

2.  Implement the `PanelCategory` interface. 

Both of these steps are described below. 

### Insert the `@Component` Annotation [](id=insert-the-component-annotation)

Directly above the class's declaration, insert the following annotation:

        @Component(
            immediate = true,
            property = {
                "panel.category.key=" + [Panel Category Key],
                "panel.category.order:Integer=[int]"
            },
            service = PanelCategory.class
        )

The `property` element designates two properties that should be assigned for
your category. The `panel.category.key` specifies the parent category for
your custom category. You can find popular parent categories to assign in
the [`PanelCategoryKeys`](@app-ref@/web-experience/latest/javadocs/com/liferay/application/list/constants/PanelCategoryKeys.html)
class. For instance, if you wanted to create a child category in the Control
Panel, you could assign `PanelCategoryKeys.CONTROL_PANEL`. Likewise, if you
wanted to create a root category, like the Control Panel or Site
Administration, you could assign `PanelCategoryKeys.ROOT`.

The `panel.category.order:Integer` property specifies the order in which
your category is displayed. The higher the number (integer), the lower your
category is listed among other sibling categories assigned to a parent.

Finally, your `service` element should specify the `PanelCategory.class`
service. You can view an example of a similar `@Component` annotation for
the `UserPanelCategory` class below.

        @Component(
            immediate = true,
            property = {
                "panel.category.key=" + PanelCategoryKeys.ROOT,
                "panel.category.order:Integer=200"
            },
            service = PanelCategory.class
        )

+$$$

**Note:** To insert a panel category between existing categories in the
default menu, you must know the `panel.category.order:Integer` property
for the existing categories. Default categories with a given
panel.category.key are numbered in increments of 100, starting with 100.

For example, the Product Menu's three main sections---Control Panel, User
Menu, and Site Administration---have `panel.category.order:Integer`
properties of 100, 200, and 300, respectively. A new panel inserted between
Control Panel and User Menu would need a `panel.category.key` of ROOT and
a `panel.category.order:Integer` of 150.

$$$

### Implement the `PanelCategory` Interface [](id=implement-the-panelcategory-interface)

The `PanelCategory` interface requires you to implement the following methods:

- `getNotificationCount`: returns the number of notifications to be shown in
  the panel category.
- `include`: renders the body of the panel category.
- `includeHeader`: renders the panel category header.
- `isActive`: whether the panel is selected.
- `isPersistState`: whether to persist the panel category's state to the
  database. This saves the state of the panel category when navigating away from
  the menu.

You can reduce the number of methods you must implement if you extend a base
class that already implements the `PanelCategory` interface. The recommended way
to do this is by extending the
[`BasePanelCategory`](@app-ref@/web-experience/latest/javadocs/com/liferay/application/list/BasePanelCategory.html)
or
[`BaseJSPPanelCategory`](@app-ref@/web-experience/latest/javadocs/com/liferay/application/list/BaseJSPPanelCategory.html)
abstract classes. Typically, the `BasePanelCategory` is extended for basic
categories (e.g., the Control Panel category) that only display the category
name. To add more complex functionality, you can then provide a custom UI for
your panel using any front-end technology by implementing the `include()` or
`includeHeader()` from the `PanelCategory` interface.

If you plan to use JSPs as the front-end technology, extend a base class called
`BaseJSPPanelCategory` that already implements the methods `include()` and
`includeHeader()` for you. This is covered in more detail below.
 
+$$$

**Note:** In this tutorial, example JSPs describe how to provide functionality
to panel categories and apps. JSPs, however, are not the only way to provide
front-end functionality to your categories/apps. You can create your own class
implementing `PanelCategory` to use other technologies such as FreeMarker.

$$$

## Defining the Control Menu Category [](id=defining-the-control-menu-category)

After establishing the framework you're using to create the category, you  must 
add any other methods that are necessary to create your custom panel category. 
As you learned earlier, you can extend the `BasePanelCategory` and
`BaseJSPPanelCategory` abstract classes to implement `PanelCategory`.

### BasePanelCategory [](id=basepanelcategory)

If you need something simple for your panel category like a name, extending
`BasePanelCategory` is probably sufficient. For example, the
[`ControlPanelCategory`](https://github.com/liferay/liferay-portal/blob/7.0.3-ga4/modules/apps/web-experience/product-navigation/product-navigation-control-panel/src/main/java/com/liferay/product/navigation/control/panel/internal/application/list/ControlPanelCategory.java)
extends `BasePanelCategory` and specifies a `getLabel` method to set and display
the panel category name.

        @Override
        public String getLabel(Locale locale) {
            return LanguageUtil.get(locale, "control-panel");
        }

### BaseJSPPanelCategory [](id=basejsppanelcategory)

If you need more complex functionality, extend `BaseJSPPanelCategory` and use
JSPs to render the panel category. For example, the
[`SiteAdministrationPanelCategory`](https://github.com/liferay/liferay-portal/blob/7.0.3-ga4/modules/apps/web-experience/product-navigation/product-navigation-site-administration/src/main/java/com/liferay/product/navigation/site/administration/internal/application/list/SiteAdministrationPanelCategory.java)
specifies the `getHeaderJspPath` and `getJspPath` methods. You could create
a JSP with the UI you want to render and specify its path in methods like these:

        @Override
        public String getHeaderJspPath() {
            return "/sites/site_administration_header.jsp";
        }

        @Override
        public String getJspPath() {
            return "/sites/site_administration_body.jsp";
        }

One JSP renders the panel category's header (displayed when panel is collapsed)
and the other its body (displayed when panel is expanded).

You must also specify the servlet context from where you are loading the JSP
files. If this is inside an OSGi module, make sure your `bnd.bnd` file has
defined a web context path:

        Bundle-SymbolicName: com.sample.my.module.web
        Web-ContextPath: /my-module-web

Then reference the Servlet context using the symbolic name of your module like
this:

        @Override
        @Reference(
            target = "(osgi.web.symbolicname=com.sample.my.module.web)",
            unbind = "-"
        )
        public void setServletContext(ServletContext servletContext) {
            super.setServletContext(servletContext);
        }

Excellent! You've successfully created a custom panel category to display in the
Product Menu. In many cases, a panel category holds panel apps for users to
access. You'll learn about how to add a panel app to a panel category next.
