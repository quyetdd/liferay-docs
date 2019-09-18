---
header-id: introduction-to-frontend-development
---

# Introduction to Front-End Development

[TOC levels=1-4]

@product@ offers complete developer front-end freedom. Whether you like coding 
JavaScript by hand, have used Liferay's front-end frameworks in the past, or 
prefer jQuery, Lodash, or modules, you can use your front-end framework of 
choice. 

Prior users of @product@ can continue to use Liferay's venerable Alloy UI, but
you can also use the front-end technologies you love the most:

-   EcmaScript ES2015+
-   [Metal.js](https://metaljs.com/) (developed by Liferay)
-   [AlloyUI](https://alloyui.com/) (developed by Liferay)
-   jQuery (included)
-   Lodash (included)

To load modules, you must know when they are needed, where they are at build
time, if you want to bundle them together or load them independently, and you
must assemble them at runtime. Keeping track of these tasks can be a hassle.
@product@'s Loaders (YUI/AUI, AMD, and npm in AMD format) handle loading for
you, so you don't have to worry about all the details. Just provide a small bit
of information about your module, and @product@'s loaders take care of the rest. 

If you want to use npm packages in your applications, you can use
[`liferay-npm-bundler`](/docs/7-1/reference/-/knowledge_base/r/liferay-npm-bundler). 
It is built for just this purpose, and even provides several presets for common
module types (AMD, React, Angular JS,  etc.) to save you time. It creates an
OSGi bundle for you, extracts all npm dependencies, and transpiles your
code for the Liferay AMD Loader. 

While developing JavaScript applications for @product@, you may need to access 
@product@-specific information or web services. The `Liferay` global JavaScript 
Object 
[exposes this information for you](/docs/7-1/tutorials/-/knowledge_base/t/liferay-javascript-apis), 
letting you harness the power of @product@ in your JavaScript applications, 
while still using the front-end frameworks and technologies that you love. 

## Lexicon and Clay

@product@ uses its own design language, called 
[Lexicon](https://lexicondesign.io/docs/lexicon/), to provide a common framework 
for building consistent UIs and user experiences across the Liferay product 
ecosystem. The web implementation of Lexicon (CSS, JS, and HTML) is called 
[Clay](https://claycss.com/docs/clay/). It is automatically available to 
application developers through a set of CSS classes or our 
[tag library](/docs/7-1/tutorials/-/knowledge_base/t/using-the-clay-taglib-in-your-portlets). 

## Templates

For templating, you can use Java EE's JSP, FreeMarker, Google's 
[Soy (aka Closure Templates)](/docs/7-1/tutorials/-/knowledge_base/t/liferay-soy-portlet), or
whatever else you like. 

## Themes

A @product@ Theme provides the look and feel for a site. Themes are
a combination of CSS, JavaScript, HTML, and FreeMarker templates. Although the
default themes are nice, you may wish to create your own look and feel for your 
site. 

From the 
[Theme Builder Gradle Plugin](/docs/7-1/reference/-/knowledge_base/r/theme-builder-gradle-plugin), 
to the 
[Liferay Theme Generator](/docs/7-1/tutorials/-/knowledge_base/t/creating-themes), 
to 
[@ide@](/docs/7-1/tutorials/-/knowledge_base/t/creating-themes-with-liferay-ide), 
to 
[Blade CLI](/docs/7-1/tutorials/-/knowledge_base/t/blade-cli)'s 
[Theme Template](/docs/7-1/reference/-/knowledge_base/r/theme-template), you
can choose the development tools you like best, so you can focus on creating
a well designed theme. 

## Front-End Extensions

@product@'s modularity has many benefits for the front-end developer, in the 
form of development customizations and extension points. These extensions assure 
the stability, conformity, and future evolution of your applications. 

Below are some of the available front-end extensions:

- [Theme Contributors](/docs/7-1/tutorials/-/knowledge_base/t/packaging-independent-ui-resources-for-your-site)
- [Context Contributors](/docs/7-1/tutorials/-/knowledge_base/t/injecting-additional-context-variables-into-your-templates)
- [Creating Configurable Styles for Portlet Wrappers](/docs/7-1/tutorials/-/knowledge_base/t/creating-configurable-styles-for-portlet-wrappers)
- [Dynamic Includes](/docs/7-1/tutorials/-/knowledge_base/t/adding-new-behavior-to-an-editor)

Read on to learn more. 
