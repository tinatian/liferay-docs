# Services in JSF [](id=services-in-jsf)

Creating services works the same in a JSF portlet as it would in any other
standard WAR-style MVC portlet; generate custom services as separate API and
Impl JARs and deploy them as individual modules to @product@. You can generate
custom services for your JSF portlet using Service Builder. To learn more about
how Service Builder works in @product@, visit the
[Service Builder](/develop/tutorials/-/knowledge_base/7-1/service-builder)
tutorials.

The JSF WAR can then rely on the API module as a *provided* dependency. The main
benefit for packaging your services this way is to allow multiple WARs to
utilize the same custom service API without packaging it inside every WAR's
`WEB-INF/lib` folder. This practice also enforces a separation of concerns, or
*modularity*, between the UI layer and service layer of a system.

To call OSGi-based Service Builder services from your JSF portlet, you need a
mechanism that gives you access to the OSGi service registry, because you can't
look up services published to the OSGi runtime using Declarative Services.
Instead, you should open a
[ServiceTracker](https://osgi.org/javadoc/r6/core/org/osgi/util/tracker/ServiceTracker.html)
when you want to call a service that's in the OSGi service registry.

To implement a service tracker in your JSF portlet, you can add a type-safe
wrapper class that extends `org.osgi.util.tracker.ServiceTracker`. For example,
this is done in a demo JSF portlet as follows

    public class UserLocalServiceTracker extends ServiceTracker<UserLocalService, UserLocalService> {

        public UserLocalServiceTracker(BundleContext bundleContext) {
            super(bundleContext, UserLocalService.class, null);
        }
    }

After extending the `ServiceTracker`, just call the constructor and the service
tracker is ready to use in your managed bean.

In a managed bean, whenever you need to call a service, open the service
tracker. For example, this is done in the same demo JSF portlet to open the
service tracker, using the
[`@PostContruct`](http://docs.oracle.com/javaee/7/api/javax/annotation/PostConstruct.html)
annotation:

    @PostConstruct
    public void postConstruct() {
        Bundle bundle = FrameworkUtil.getBundle(this.getClass());
        BundleContext bundleContext = bundle.getBundleContext();
        userLocalServiceTracker = new UserLocalServiceTracker(bundleContext);
        userLocalServiceTracker.open();
    }

Then the service can be called:

    UserLocalService userLocalService = userLocalServiceTracker.getService();
    ...

    userLocalService.updateUser(user);

When it's time for the managed bean to go out of scope, you must close the
service tracker using the
[`@PreDestroy`](http://docs.oracle.com/javaee/7/api/javax/annotation/PreDestroy.html)
annotation:

    @PreDestroy
    public void preDestroy() {
        userLocalServiceTracker.close();
    }

For more information on service trackers and how to use them in WAR-style
portlets, see the
[Service Trackers](/develop/tutorials/-/knowledge_base/7-1/service-trackers)
tutorial.

## Related Topics [](id=related-topics)

[Fundamentals](/develop/tutorials/-/knowledge_base/7-1/fundamentals)

[Internationalization](/develop/tutorials/-/knowledge_base/7-1/internationalization)

[Configurable Applications](/develop/tutorials/-/knowledge_base/7-1/configurable-applications)
