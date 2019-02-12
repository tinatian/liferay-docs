# Simple Theme [](id=theme)

The Simple Theme sample provides the base files for a theme, using the
[Theme Builder Gradle plugin](/develop/reference/-/knowledge_base/7-1/theme-builder-gradle-plugin).
When deploying this sample with no customizations, a theme based off of the 
`_styled` base theme is created.

![Figure 1: A theme based off of the Styled base theme is created when the Theme Blade sample is deployed to Liferay Portal.](../../../images/theme.png)

For more information on themes, visit the 
[Introduction to Themes](/develop/tutorials/-/knowledge_base/7-1/introduction-to-themes)
tutorial.

## What API(s) and/or code components does this sample highlight? [](id=what-apis-and-or-code-components-does-this-sample-highlight)

This sample demonstrates a way to create a simple theme in @product@.

## How does this sample leverage the API(s) and/or code component? [](id=how-does-this-sample-leverage-the-apis-and-or-code-component)

To modify this sample, add the `images`, `js`, or `templates` folder, along with
your modified files, to the `src/main/webapp` folder. The sample already
provides the `src/main/resources/resources-importer`, `src/main/webapp/WEB-INF`,
and `src/main/webapp/css` folders for you. Add your style modifications to the
provided `css/_custom.scss` file. For a complete explanation of a theme's files,
see the
[Theme Reference Guide](/develop/reference/-/knowledge_base/7-1/theme-reference-guide).

## Where Is This Sample? [](id=where-is-this-sample)

There are three different versions of this sample, each built with a different
build tool:

- [Gradle](https://github.com/liferay/liferay-blade-samples/tree/7.1/gradle/themes/simple-theme)
- [Liferay Workspace](https://github.com/liferay/liferay-blade-samples/tree/7.1/liferay-workspace/wars/simple-theme)
- [Maven](https://github.com/liferay/liferay-blade-samples/tree/7.1/maven/themes/simple-theme)
