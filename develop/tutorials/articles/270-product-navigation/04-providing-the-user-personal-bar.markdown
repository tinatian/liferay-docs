# Providing the User Personal Bar [](id=providing-the-user-personal-bar)

The User Personal Bar displays options unique to the current user. By default,
this menu appears as an avatar button that expands the User Settings sub-menu in
the Product Menu. In a custom theme, the User Personal Bar could appear anywhere
in the interface.

![Figure 1: By default, the User Personal Menu contains the signed-in user's avatar, which navigates to the Product Menu when selected.](../../images/user-personal-bar.png)

Although Liferay's default User Personal Bar is bare-bones, you can
add more functionality to fit your needs. Unlike other product navigation menus
(e.g., Product Menu), the User Personal Bar does not require the
extension/creation of panel categories and panel apps.

<!-- Add below reference once portlet providers tutorial is available.

It uses another common
Liferay framework for providing functionality:
[Portlet Providers](develop/tutorials/-/knowledge_base/7-1/portlet-providers).
-->

The User Personal Bar can be seen as a placeholder in every Liferay theme. By
default, Liferay provides one sample *User Personal Bar* portlet that fills that
placeholder, but the portlet Liferay provides can be replaced by other portlets.

+$$$

**Note:** You can add the User Personal Bar to your theme by adding the
following snippet into your `portal_normal.ftl`:

    <@liferay.user_personal_bar />

$$$

In this tutorial, you'll learn how to customize the User Personal Bar. You'll
create a single Java class where you'll specify a portlet to replace the
existing default portlet.

1.  [Create an OSGi module](/develop/tutorials/-/knowledge_base/7-1/starting-module-development).

2.  Create a unique package name in the module's `src` directory and create a
    new Java class in that package.

3.  Above the class declaration, insert the following annotation:

        @Component(
            immediate = true,
            property = {
                "model.class.name=" + PortalUserPersonalBarApplicationType.UserPersonalBar.CLASS_NAME,
                "service.ranking:Integer=10"
            },
            service = ViewPortletProvider.class
        )

     The `model.class.name` property must be set to the class name of the entity
     type you want the portlet to handle. In this case, you want your portlet to
     be provided based on whether it can be displayed in the User Personal Bar.

     <!-- Add below reference once portlet providers tutorial is available.

     You may recall from the 
     [Portlet Providers](develop/tutorials/-/knowledge_base/7-1/portlet-providers)
     tutorial that you can request portlets in several different ways (e.g.,
     *Edit*, *Browse*, etc.).
     -->

     You should also specify the service rank for your new portlet so it
     overrides the default. Make sure to set the `service.ranking:Integer`
     property to a number that is ranked higher than the portlet being used by
     default.

     Since you only want the User Personal Bar to display your portlet, the
     `service` element should be `ViewPortletProvider.class`.

4.  Update the class's declaration to extend the 
    [BasePortletProvider](@platform-ref@/7.0-latest/javadocs/portal-kernel/com/liferay/portal/kernel/portlet/BasePortletProvider.html)
    abstract class and implement `ViewPortletProvider`:

        public class ExampleViewPortletProvider extends BasePortletProvider implements ViewPortletProvider {

5.  Specify the portlet you want in the User Personal Bar by declaring the
    following method in your class:

        @Override
        public String getPortletName() {
            return PORTLET_NAME;
        }

    Replace the `PORTLET_NAME` text with the portlet you want to provide Liferay
    when it requests one to be viewed in the User Personal Bar. For example,
    Liferay declares
    `com_liferay_product_navigation_user_personal_bar_web_portlet_ProductNavigationPersonalBarPortlet`
    for its default User Personal Bar portlet.

You've successfully provided a portlet to be displayed in the User Personal Bar.
If you want to inspect the entire module used for Liferay's default User
Personal Bar, see
[product-navigation-user-personal-bar-web](https://github.com/liferay/liferay-portal/tree/7.1.x/modules/apps/product-navigation/product-navigation-user-personal-bar-web).
Besides the `*ViewPortletProvider` class, this module contains two classes
defining constants and a portlet class defining the default portlet to provide.
Although these additional classes are not required, your module should have
access to the portlet you want to provide.
