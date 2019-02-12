# Writing Your First @product@ Application [](id=writing-your-first-liferay-application)

<div class="learn-path-step">
    <p>Developing Your First Portlet<br>Step 1 of 8</p>
</div>

Now you'll learn step-by-step how to create your project and deploy your
application to @product@. Before you know it, you'll have your application
deployed alongside those that come with @product@. 

Your first application is simple: you'll build a guestbook application that 
looks like this: 

![Figure 1: You'll create this simple application.](../../../images/first-guestbook-portlet.png)

By default, it shows guestbook messages that users leave on your website. To
add a message, you click the *Add Entry* button to show a form for entering and
saving a message. 

Ready to write your first @product@ application?

## Creating Your First @product@ Application [](id=creating-your-first-liferay-application)

Your first step is to create a *Liferay Module Project*. Modules are the core 
building blocks of @product@ applications. Every application is made from one or
more modules. Each module encapsulates a functional piece of an application.
Multiple modules form a complete application. 

These modules are 
[OSGi](https://www.osgi.org/) modules. The OSGi container in @product@ can run 
any OSGi module. Each module is packaged as a JAR file that contains a manifest 
file. The manifest is needed for the container to recognize the module. 
Technically, a module that contains only a manifest is still valid. Of course, 
such a module wouldn't be very interesting. 

Now you'll create your first module. For the purpose of this Learning Path, 
you'll create your modules inside your Liferay Workspace. Follow these 
instructions to create your first Liferay Module Project: 

1.  In the Project Explorer in Liferay @ide@, right click on your Liferay 
    Workspace and select *New* &rarr; *Liferay Module Project*. 

2.  Complete the first screen of the wizard with the following information: 

    - Enter `guestbook-web` for the Project name. 
    - Use the *Gradle* Build type.
    - Select `mvc-portlet` for the Project Template. 

    Click *Next*. 

5.  On the second screen of the wizard, enter `Guestbook` for the component 
    class name, and `com.liferay.docs.guestbook.portlet` for the package name. 
    Click *Finish*.

Note that it may take a while for @ide@ to create your project, because Gradle 
downloads your project's dependencies for you during project creation. Once this 
is done, you have a module project named `guestbook-web`. The `mvc-portlet` 
template configured the project with the proper dependencies and generated all 
the files you need to get started: 

- The portlet class (in the package you specified)
- JSP files (in `/src/main/resources`)
- Language properties (also in `/src/main/resources`)

![Figure 2: Your new module project appears in your Liferay Workspace's `modules` folder.](../../../images/guestbook-web-project.png)

Your new module project is a *portlet* application. Next, you'll learn exactly 
what a portlet is. 

## What is a Portlet? [](id=what-is-a-portlet)

Web applications can be simple: they might show you one piece of information,
such as an article. A complex application might track your taxes as you enter
lots of data into an application that calculates whether you owe or are due
a refund. These applications run on a *platform* that provides application
developers the building blocks they need to make applications.

![Figure 3: Many Liferay applications can run at the same time on the same page.](../../../images/portlet-applications.png)

@product@ provides a platform that contains common features needed by today's
applications, including user management, security, user interfaces, services, 
and more. Portlets are one of those basic building blocks. Often a web 
application takes up the entire page. If you want, you can do this with
applications in @product@ as well. Portlets, however, let you serve many
applications on the same page at the same time. @product@'s framework takes this
into account at every step. For example, features like platform-generated URLs
exist to support Liferay's ability to serve multiple applications on the same
page. 

## What is a Component? [](id=what-is-a-component)

Portlets created in Liferay Module Projects are generated as *Components*. If
a module (sometimes also called a *bundle*) encapsulates pieces of your
application, a component is the object that contains the core functionality.
A Component is managed by a component framework or container. Components are
deployed inside modules, and they're created, started, stopped, and destroyed as
needed by the container. What a perfect model for a web application! It can be
made available only when needed, and when it's not, the container can make sure
it doesn't use resources needed by other components. 

In this case, you created a Declarative Services (DS) component. With
Declarative Services, you declare that an object is a component, and you define 
data about the component so the container knows how to manage it. A default 
configuration was created for you; you'll examine it later. 

## Deploying the Application [](id=deploying-the-application)

Even though all you've done is generate it, the `guestbook-web` project is ready
to be built and deployed.

1.  Make sure that your server is running, and if it isn't, select it in 
    @ide@'s Servers pane and click the start button (![Start Server](../../../images/icon-start-server.png)).

2.  After it starts, drag and drop the `guestbook-web` project from the Project
    Explorer to the server.
 
    ![Figure 4: Drag and drop the module.](../../../images/deploy-module.gif)

3.  Open a browser and navigate to @product@
    ([http://localhost:8080](http://localhost:8080) by default).

    If this is your first time starting @product@, you'll go through a short 
    wizard to set up your server. In this wizard, make sure you use the default 
    database (Hypersonic). Although this database isn't intended for production 
    use, it works fine for development and testing. 

4.  To add an application to a page, click *Add* 
    (![Add Widget](../../../images/icon-add-app.png)) in the upper right hand 
    corner.

5.  Select *Applications*. In the Applications list, your application should
    appear in the Sample category. Its name is `Guestbook`. 

![Figure 5: This is the default Liferay home page. It contains the Hello World widget and the initial version of the Guestbook application that you created.](../../../images/default-portlet-application.png)

Now you're ready to jump in and start developing your Guestbook portlet. 
