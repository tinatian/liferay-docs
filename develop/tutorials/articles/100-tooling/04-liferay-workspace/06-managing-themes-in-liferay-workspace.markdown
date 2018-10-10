# Managing Themes in Liferay Workspace [](id=managing-themes-in-liferay-workspace)

Creating a @product@ theme can be accomplished using two different tools:

- [Liferay Themes SDK](/develop/tutorials/-/knowledge_base/7-1/creating-themes)
  (Node.js-based)
- [Project template/archetype](/develop/reference/-/knowledge_base/7-1/theme-template)
  (Gradle/Maven-based)

Liferay Workspace offers an environment where developers can use the Liferay
Themes SDK to create Node.js-based themes and their work can be seamlessly
integrated into their overall DevOps strategy. You can leverage the Liferay
Themes SDK to create Node.js-based themes inside workspace or you can leverage
it externally and copy themes into Workspace.

Workspace also offers a traditional Java-based theme approach (leveraging
Gradle/Maven) for those that can't use Node.js tools in their CI environment.

Below you'll learn how to manage both Node.js and traditional themes in
Workspace. 

## Node.js Themes in Workspace [](id=node-js-themes-in-workspace)

Liferay Workspace reserves the `themes` folder only for Node.js-based themes.
There are no Blade CLI-provided commands or Maven archetypes to create a Liferay
Node.js theme. You must leverage Liferay's 
[Themes SDK](/develop/tutorials/-/knowledge_base/7-1/creating-themes) 
from within the `themes` folder to create them, a generated theme can be copied
into the folder.

You'll demo workspace's Node.js theme management capability next. Be sure the
Theme SDK's required tooling is installed.

1.  Navigate to your workspace's `themes` folder and run the following command:

        yo liferay-theme

    Follow the prompts to create your theme.

2.  Navigate into your new theme and run `../gradlew build`. Liferay Workspace
    builds the front-end theme using Gradle. Under the hood, Liferay's 
    [Node Gradle Plugin](/develop/reference/-/knowledge_base/7-1/node-gradle-plugin)
    is applied and used to build your theme.

3.  Workspace is smart enough to differentiate between theme types. For
    instance, you can't copy a Node.js-based theme into the `wars` folder and
    expect it to build. You can test if your project is recognized by Workspace
    by running this command from Workspace's root folder:

        ../gradlew projects

    Your CLI should display your new theme under the `themes` project.

        Root project 'liferay-workspace'
        +--- Project ':themes'
        |    \--- Project ':themes:my-nodejs-theme'

    If you moved a non-Node.js theme (e.g., WAR-style theme) into the `themes`
    folder, it is not recognized by the Gradle `projects` command.

    **Note:** Workspace identifies a Node.js theme by checking whether it has a
    `package.json` file. Any theme without this file is not compatible in the
    `themes` folder.

Excellent! You learned how Node.js themes are recognized in workspace and where
they should reside. Next you'll learn how workspace manages WAR-style themes.

## Gradle/Maven Themes in Workspace [](id=gradle-maven-themes-in-workspace)

Liferay Workspace provides the `wars` folder for any WAR-style project. Themes
created with [Blade CLI](/develop/tutorials/-/knowledge_base/7-1/blade-cli) or
Maven using the [`theme`](/develop/reference/-/knowledge_base/7-1/theme-template)
project template or archetype are automatically generated here when creating the
project within Workspace.

Themes built using Liferay's `theme` project template are always WARs and should
always reside in Workspace's `wars` folder. They should never be moved to the
`themes` folder; that folder is reserved for Node.js-based themes only.

To build an existing WAR-style theme in Workspace, run the `../gradlew build`
command. Liferay Workspace builds the theme using Gradle. Under the hood,
Liferay's
[Theme Builder Gradle
Plugin](/develop/reference/-/knowledge_base/7-1/theme-builder-gradle-plugin) is
applied and used to build your theme. It works similarly in a Maven workspace.
See the 
[Building Themes in a Maven Project](/develop/tutorials/-/knowledge_base/7-1/building-themes-in-a-maven-project)
tutorial for more information.

Awesome! You know how WAR-style themes are built in workspace and where they
should reside.
