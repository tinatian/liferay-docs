# Extending the Simulation Menu [](id=extending-the-simulation-menu)

When testing how pages and apps appear for users, it's important to simulate
their views in as many ways as possible. The Simulation Menu on the right-side
of the main page allows this, and you can extend the menu if you need to
simulate something that it does not provide.

First, you must get accustomed to using panel categories/apps. This is
covered in detail in the
[Customizing The Product Menu](/develop/tutorials/-/knowledge_base/7-1/customizing-the-product-menu)
tutorial. Once you know how to create panel categories and panel apps, continue
with this tutorial.

There are few differences between the Simulation Menu and Product Menu, mostly
because they extend the same base classes. The Simulation Menu, by default, is
made up of only one panel category and one panel app. Liferay provides the
[`SimulationPanelCategory`](@app-ref@/web-experience/latest/javadocs/com/liferay/product/navigation/simulation/application/list/SimulationPanelCategory.html)
class, a hidden category needed to hold the `DevicePreviewPanelApp`. This is the
app and functionality you see in the Simulation Menu by default.

![Figure 1: The Simulation Menu offers a device preview application.](../../images/simulation-menu-preview.png)

To provide your own functionality in the Simulation Menu, you must create
a panel app in `SimulationPanelCategory`. If you want to add extensive
functionality, you can even create additional panel categories in the menu to
divide up your panel apps. This tutorial covers the simpler case of creating
a panel app for the already present hidden category.

1.  Follow the steps documented in 
    [Adding Custom Panel Apps](/develop/tutorials/-/knowledge_base/7-0/customizing-the-product-menu#adding-custom-panel-apps)
    for creating custom panel apps. Once you've created the foundation 
    of your panel app, move on to learn how to tweak it so it customizes the
    Simulation Menu.

    You can generate a Simulation Panel App by using Blade CLI's
    [Simulation Panel Entry template](/develop/reference/-/knowledge_base/7-1/simulation-panel-entry-template).
    You can also refer to the [Simulation Panel App sample](/develop/reference/-/knowledge_base/7-1/simulation-panel-app)
    for a working example.

2.  Since this tutorial assumes you're providing more functionality to the
    existing simulation category, set the simulation category in the
    `panel.category.key` of the `@Component` annotation:

        "panel.category.key=" + SimulationPanelCategory.SIMULATION

    In order to use this constant, you must add a dependency on 
    [`com.liferay.product.navigation.simulation`](https://repository.liferay.com/nexus/content/repositories/liferay-public-releases/com/liferay/com.liferay.product.navigation.simulation/).

    Be sure to also specify the order to display your new panel app,
    which was explained in [Adding Custom Panel Apps](/develop/tutorials/-/knowledge_base/7-0/customizing-the-product-menu#adding-custom-panel-apps).

3.  This tutorial assumes you're using JSPs. 
    Therefore, you should extend the [`BaseJSPPanelApp`](@app-ref@/web-experience/latest/javadocs/com/liferay/application/list/BaseJSPPanelApp.html)
    abstract class, which implements the [`PanelApp`](@app-ref@/web-experience/latest/javadocs/com/liferay/application/list/PanelApp.html)
    interface and also provides additional methods necessary for specifying JSPs
    to render your panel app's UI. Remember that you can also implement your own
    `include()` method to use any front-end technology you want, if you want to
    use a technology other than JSP (e.g., FreeMarker).

4.  Define your simulation view. For instance, in `DevicePreviewPanelApp`, the
    `getJspPath` method points to the `simulation-device.jsp` file in the
    `resources/META-INF/resources` folder, where the device simulation interface
    is defined. Optionally, you can also add your own language keys, CSS, or
    JavaScript resources in your simulation module.

    The right servlet context is also provided by implementing this method:

        @Override
        @Reference(
            target = "(osgi.web.symbolicname=com.liferay.product.navigation.simulation.device)",
            unbind = "-"
        )
        public void setServletContext(ServletContext servletContext) {
            super.setServletContext(servletContext);
        }

    As explained in [Customizing The Product Menu](/develop/tutorials/-/knowledge_base/7-0/customizing-the-product-menu),
    a panel app should be associated with a portlet. This makes the panel app 
    visible only when the user has permission to view the portlet.
    This panel app is associated to the Simulation Device portlet using these
    methods:

        @Override
        public String getPortletId() {
            return ProductNavigationSimulationPortletKeys.
                PRODUCT_NAVIGATION_SIMULATION;
        }

        @Override
        @Reference(
            target = "(javax.portlet.name=" + ProductNavigationSimulationPortletKeys.PRODUCT_NAVIGATION_SIMULATION + ")",
            unbind = "-"
        )
        public void setPortlet(Portlet portlet) {
            super.setPortlet(portlet);
        }

    Audience Targeting also provides a good example of how to extend the
    Simulation Menu. When the Audience Targeting app is deployed, the
    Simulation Menu is extended to offer Audience Targeting User Segments and
    Campaigns. You can simulate particular scenarios for campaigns and users
    directly from the Simulation Menu. Its panel app class is similar to
    `DevicePreviewPanelApp`, except it points to a different portlet and JSP.

    ![Figure 2: The Audience Targeting app extends the Simulation Menu to help simulate different users and campaign views.](../../images/simulation-menu-at.png)

5.  You can combine your simulation options with the device simulation options 
    by interacting with the device preview iFrame. To retrieve the device 
    preview frame in an `aui:script` block of your custom simulation view's 
    JavaScript, you can use this code:

        var iframe = A.one('#simulationDeviceIframe');

    Then you can modify the device preview frame URL like this:

        iframe.setAttribute('src', newUrlWithCustomParameters);

Now that you know how to extend the necessary panel categories and panel apps to
modify the Simulation Menu, [create a module](/develop/tutorials/-/knowledge_base/7-1/starting-module-development#creating-a-module) 
of your own and customize the Simulation Menu so it's most helpful for your 
needs.
