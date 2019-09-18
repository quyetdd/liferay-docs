---
header-id: theme-components-and-workflow
---

# Theme Components and Workflow

[TOC levels=1-4]

If you want to develop a website, you must have three key components: CSS,
JavaScript, and HTML. @product@ supports SASS as well as multiple JavaScript
frameworks. The HTML, however, is rendered via
[FreeMarker](https://freemarker.apache.org/) theme templates. This reference
guide provides an overview of @product@'s theme development components and
workflow, covering the following topics:

- Theme templates
- Theme customizations and extensions
- Portlet customizations and extensions
- Theme workflow
- CSS Frameworks and extensions

## Theme Templates

@product@ provides several default FreeMarker templates that each handle a key 
piece of functionality for the page:

- `portal_normal.ftl`: Similar to a static site's `index.html`, this file acts
  as a hub for all theme templates and provides the overall markup for the page.
- `init.ftl`: Contains common FreeMarker variables that can be used in your
  theme templates. Useful for reference if you need access to theme objects.
  **We recommended that you DO NOT override this file**.
- `init_custom.ftl`: Used to override FreeMarker variables in `init.ftl` and to
  define new variables, such as 
  [theme settings](/docs/7-1/tutorials/-/knowledge_base/t/making-configurable-theme-settings).
- `portlet.ftl`: This template controls the theme's portlets. If your theme uses 
  Portlet Decorators, you can modify this file to create application 
  decorator-specific theme settings. See the 
  [Portlet Decorators](/docs/7-1/tutorials/-/knowledge_base/t/creating-configurable-styles-for-portlet-wrappers) 
  tutorial for more info.
- `navigation.ftl`: Contains the navigation markup. To customize pages in the
  navigation, you must use the `liferay.navigation_menu` macro. Then you can
  leverage
  [ADTs](https://github.com/liferay/liferay-portal/tree/7.1.x/modules/apps/site-navigation/site-navigation-menu-web/src/main/resources/com/liferay/site/navigation/menu/web/portlet/template/dependencies)
  for the navigation menu. Note that `navigation.ftl` also defines the hamburger
  icon and `navbar-collapse` class that provides the simplified navigation
  toggle for mobile viewports, as shown in the snippet below for the Classic
  theme:

      <#if has_navigation && is_setup_complete>
        <button aria-controls="navigationCollapse" aria-expanded="false" 
        aria-label="Toggle navigation" class="navbar-toggler navbar-toggler-right" 
        data-target="#navigationCollapse" data-toggle="collapse" type="button">
          <span class="navbar-toggler-icon"></span>
        </button>

        <div aria-expanded="false" class="collapse navbar-collapse" id="navigationCollapse">
          <@liferay.navigation_menu default_preferences="${preferences}" />
        </div>
      </#if>

![Figure 1: The collapsed navbar provides simplified user-friendly navigation for mobile devices.](../../images/portal-layout-mobile-nav.png)

- `portal_pop_up.ftl`: The theme template controlling pop up dialogs for the
  theme's portlets. Similar to `portal_normal.ftl`, `portal_pop_up.ftl` provides
  the markup template for all pop-up dialogs, such as a portlet's Configuration 
  menu. It also has access to the FreeMarker variables defined in `init.ftl` and 
  `init_custom.ftl`.

![Figure 2: Each theme template provides a portion of the page's markup and functionality.](../../images/portal-layout-theme-templates.png)

### Theme Template Utilities

@product@ provides several FreeMarker variables and macros that you can use in 
your theme templates to include portlets, use taglibs, access theme objects, and 
more. You can see examples of these in `portal_normal.ftl`. These utilities are 
included in the files listed below:

- [`Init.ftl`](https://github.com/liferay/liferay-portal/blob/7.1.x/modules/apps/frontend-theme/frontend-theme-unstyled/src/main/resources/META-INF/resources/_unstyled/templates/init.ftl): 
  Provides access to common theme variables
- [`FTL_Liferay.ftl`](https://github.com/liferay/liferay-portal/blob/7.1.x/modules/apps/portal-template/portal-template-freemarker/src/main/resources/FTL_liferay.ftl): 
  Provides macros for commonly used portlets and theme resources. See the 
  [Macros tutorial](/docs/7-1/tutorials/-/knowledge_base/t/using-liferays-macros-in-your-theme) 
  for more information.
- `taglib-mappings.properties`: 
  Maps the portal taglibs to FreeMarker macros. Taglibs let you quickly create 
  common UI components. This properties file is also provided separately for
  each app taglib. For convenience, these FreeMarker macros appear in the
  [FreeMarker Taglib Mappings reference guide](/docs/7-1/reference/-/knowledge_base/r/freemarker-taglib-macros). 
  See the 
  [Taglib tutorials](/docs/7-1/tutorials/-/knowledge_base/t/front-end-taglibs) 
  for more information on using each taglib in your theme templates.

## CSS Frameworks and Extensions

As noted above, @product@ supports the Sass CSS extension, so you can take
full advantage of Sass mixins, nesting, partials, and variables in your CSS.

Also important to note is 
[Clay CSS](https://clayui.com/), 
the web implementation of Liferay's 
[Lexicon design language](https://lexicondesign.io/). 
An extension of Bootstrap, Clay CSS fills the gaps between Bootstrap and the 
needs of @product@, providing additional components and CSS patterns that you 
can use in your themes. Clay base, Liferay's Bootstrap API extension, along with 
Atlas, a custom Bootstrap theme, creates @product@'s Classic theme. See the 
[importing Clay CSS tutorial](/docs/7-1/tutorials/-/knowledge_base/t/importing-clay-css-into-a-theme) 
for more information.

## Theme Customizations and Extensions

The theme templates, along with the CSS, provide much of the overall look and 
feel for the page, but additional extension points/customizations are available. 
The following extensions and mechanisms are available for themes:

- **Color Schemes:** specifies configurable color scheme settings for 
  Administrator's to configure via the Look and Feel menu. See the 
  [color scheme tutorial](/docs/7-1/tutorials/-/knowledge_base/t/creating-color-schemes-for-your-theme) 
  for more information.
- **Configurable Theme Settings:** settings that let Administrators configure 
  aspects of a theme that may need changed frequently, such as controlling the 
  visibility of certain elements, changing a daily quote, etc. See the 
  [Configurable Theme Settings tutorial](/docs/7-1/tutorials/-/knowledge_base/t/making-configurable-theme-settings) 
  for more information. 
- **Context Contributor:** Exposes Java variables and functionality for  you to 
  use in your FreeMarker templates. This lets you use non-JSP templating languages 
  for themes, ADTs, and any other templates used in @product@. See the 
  [Context Contributors tutorial](/docs/7-1/tutorials/-/knowledge_base/t/injecting-additional-context-variables-into-your-templates) 
  or more information.
- **Theme Contributor:** a package containing UI resources, not attached to a 
  theme, that you want to include on every page. See the 
  [Theme Contributors tutorial](/docs/7-1/tutorials/-/knowledge_base/t/packaging-independent-ui-resources-for-your-site) 
  for more information. 
- **Themelet:** small, extendable, and reusable pieces of code that contain CSS
  and JavaScript. It can be shared with other developers to provide common
  components for themes, and it only requires the files you want to extend. See
  the 
  [Themelets tutorial](/docs/7-1/tutorials/-/knowledge_base/t/creating-reusable-pieces-of-code-for-your-themes)
  for more information.

## Portlet Customizations and Extensions

You can customize portlets with these mechanisms and extensions:

- **Portlet FTL Customizations:** customize the base template markup for all 
  portlets. See the 
  [Theming Portlets tutorial](/docs/7-1/tutorials/-/knowledge_base/t/theming-portlets#portlet-ftl) 
  for more information.
- **Application Display Templates (ADTs):** provides an alternate display style 
  for a portlet. Note that not all portlets support ADTs. See the 
  [Application Display Templates (ADTs) User Guide](/docs/7-1/user/-/knowledge_base/u/styling-widgets-with-application-display-templates) 
  for more information.
- **Portlet Decorator:** lets you customize the exterior decoration for a portlet. 
  See the 
  [Portlet Decorators tutorial](/docs/7-1/tutorials/-/knowledge_base/t/creating-configurable-styles-for-portlet-wrappers) 
  for more information.
- **Web Content Template:** defines how structures are displayed for web content. 
  See the 
  [Web Content Templates User Guide articles](/docs/7-1/user/-/knowledge_base/u/designing-web-content-with-templates) 
  for more information.

![Figure 3: There are several extension points for customizing portlets](../../images/portal-layout-portlet-customizations.png)

## Theme Workflow

Themes are built on top of one of the following base themes: 

- **Unstyled:** provides basic markup, functions, and images for Portal
- **Styled:** inherits from the Unstyled base theme and adds some styling on top

You can use the development tools you're most comfortable with so you can focus
on creating a well designed theme. The following Liferay tools help you build
themes:

- [Theme Builder Gradle Plugin](/docs/7-1/reference/-/knowledge_base/r/theme-builder-gradle-plugin)
- [Liferay Theme Generator](/docs/7-1/tutorials/-/knowledge_base/t/creating-themes)
- [Dev Studio](/docs/7-1/tutorials/-/knowledge_base/t/creating-themes-with-liferay-ide)
- [Blade CLI](/docs/7-1/tutorials/-/knowledge_base/t/blade-cli)'s 
  [Theme Template](/docs/7-1/reference/-/knowledge_base/r/theme-template). 

Depending on the tool you choose 
(
  [Theme Generator](/docs/7-1/reference/-/knowledge_base/r/theme-reference-guide), 
  [Gradle](/docs/7-1/reference/-/knowledge_base/r/theme-builder-gradle-plugin), 
  [Blade CLI](/docs/7-1/reference/-/knowledge_base/r/theme-template), 
  [Maven](/docs/7-1/reference/-/knowledge_base/r/theme-template), 
  or 
  [Dev Studio](/docs/7-1/reference/-/knowledge_base/r/theme-template)
), 
the theme anatomy is a bit different. The overall development process is the 
same though: 

1.  Mirror the structure of the files you want to modify. The main modifications 
    are placed in the following files:

    - `portal_normal.ftl`: main theme markup
    - `_custom.scss`: custom CSS styling
    - `main.js`: the theme's JavaScript

2.  Build and deploy the theme to your @product@ server.

3.  Apply the theme 
    [through the Look and Feel menu](/docs/7-1/user/-/knowledge_base/u/page-set-look-and-feel) 
    by selecting your 
    [theme's thumbnail](/docs/7-1/tutorials/-/knowledge_base/t/creating-a-thumbnail-preview-for-your-theme). 

The finished theme is bundled as a WAR (Web application ARchive) file. 

| **Note:** While developing your theme, we recommend that you enable
| [Developer Mode](/docs/7-1/tutorials/-/knowledge_base/t/using-developer-mode-with-themes).
| This un-minifies JS files and disable caching for CSS and FreeMarker template
| files, making the debugging process much easier.

During theme development, if you've built your theme with the Liferay Theme
Generator, you can use some helpful Gulp tasks to make the process easier:

- **build:** builds your theme's files based on the specified base theme. 
  See the 
  [gulp build tutorial](/docs/7-1/tutorials/-/knowledge_base/t/building-your-themes-files) 
  for more information.
- **extend:** sets the base theme or themelet to extend. See the 
  [gulp extend tutorial](/docs/7-1/tutorials/-/knowledge_base/t/changing-your-base-theme) 
  for more information.
- **init:** specifies the app server to deploy your theme to (automatically run
  during the initial creation of the theme). See the 
  [gulp init tutorial](/docs/7-1/tutorials/-/knowledge_base/t/configuring-your-themes-app-server)
  for more information. 
- **kickstart:** copies files from an existing theme into your theme to help 
  kickstart it. See the 
  [gulp kickstart tutorial](/docs/7-1/tutorials/-/knowledge_base/t/copying-an-existing-themes-files) 
  for more information.
- **status:** lists the base theme/themelets that your theme extends. See the 
  [gulp status tutorial](/docs/7-1/tutorials/-/knowledge_base/t/listing-your-themes-extensions) 
  for more information.
- **watch:** watches for changes to your theme's files and automatically deploys 
  them to the server when a change is made. See the 
  [gulp watch tutorial](/docs/7-1/tutorials/-/knowledge_base/t/automatically-deploying-theme-changes) 
  for more information.
