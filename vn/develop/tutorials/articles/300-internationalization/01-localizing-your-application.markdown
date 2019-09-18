---
header-id: localizing-your-application
---

# Localizing Your Application

[TOC levels=1-4]

If you're writing a Liferay Application, you're probably a genius who is also
really cool, which means your application will be used throughout the entire
world. At least, if its messages can be translated into their language, it will.
Thankfully, Liferay makes it easy to support translation of your application's
language keys.

| **Note:** Even if you don't plan to translate your application into multiple
| languages, use the localization pattern presented here for any messages
| displayed in your user interface. It's much easier to change the messages by
| updating a language properties file than by finding every instance of a message
| and replacing it in your JSPs and Java classes.

You just need to create a default language properties file
(`Language.properties`) and one for each translation you'd like to support (for
example, `Language_fr.properties` for your French translation), and put them in
the correct location in your application. Use the two letter locale that
corresponds to the language you want to translate in your file names (for example,
`Language_es.properties` provides a Spanish translation for each key).

<!-- Discuss adding a new locale? -->

Application localization topics:

-   [What are Language Keys?](#what-are-language-keys)
-   [What Locales are Available By Default?](#what-locales-are-available-by-default)
-   [Where do I Put Language Files?](#where-do-i-put-language-files)
-   [Creating a Language Module](#creating-a-language-module)
-   [Using a Language Module](#using-a-language-module)
-   [Using @product@'s Language Properties](#using-liferays-language-properties)

## What are Language Keys?

Each language property file holds key/value pairs. The key is the same in all
the language property files, while the value is translated in each file. You
specify the key in your user interface code, and the appropriately translated
message is returned automatically for your users, depending on the locale being
used in Liferay. If you have Liferay running locally, append the URL with
a supported locale to see the translations (for example, enter
`localhost:8080/es`).

![Figure 1: Append the locale to your running Liferay's URL and see Liferay's translation power in action.](../../images/locale-message-spain.png)

Language keys are just keys to use in place of a hard coded, fully translated
String value in your user interface code. You use a language key in your JSP
with a `<liferay-ui:message />` tag. 

If you wanted to hard code a message, you'd use the tag like this:

    <liferay-ui:message key="Howdy, Partner!" />

In that case you'll get a properly capitalized and punctuated message in your
application.

Instead, specify a simple key instead of the final value:

    <liferay-ui:message key="howdy-partner" />

That way you can provide a translation of the key in a default language
properties file (`Language.properties`):

    howdy-partner=Howdy, Partner!

Either way, you get the same output. The properties file lets you put all your
messages in one place, and you can add additional language properties files with
translations later. You just need to make sure there's a locale that corresponds
to your translation.

The values from your default `Language.properties` file appear if no locale is
specified. If a locale is specified, a key from a file corresponding to that
local is retrieved. For example, if a Spanish translation is sought,
a `Language_es.properties` file must be present to provide the proper values. If
it isn't, the default language properties (from the `Language.properties` file)
are used.

## What Locales are Available By Default?

There are a bunch of locales available by default in Liferay. Look in the
`portal.properties`
[file](@platform-ref@/7.1-latest/propertiesdoc/portal.properties.html#Languages%20and%20Time%20Zones)
to find them.

    locales=ar_SA,eu_ES,bg_BG,ca_AD,ca_ES,zh_CN,zh_TW,hr_HR,cs_CZ,da_DK,nl_NL,
        nl_BE,en_US,en_GB,en_AU,et_EE,fi_FI,fr_FR,fr_CA,gl_ES,de_DE,el_GR,
        iw_IL,hi_IN,hu_HU,in_ID,it_IT,ja_JP,ko_KR,lo_LA,lt_LT,nb_NO,fa_IR,
        pl_PL,pt_BR,pt_PT,ro_RO,ru_RU,sr_RS,sr_RS_latin,sl_SI,sk_SK,es_ES,
        sv_SE,tr_TR,uk_UA,vi_VN

To provide a translation for one of these locales, specify the locale in the
file name containing the translated keys (for example, `Langauge_es.properties`
holds the Spanish translation).

## Where do I Put Language Files?

In an application with only one module that holds all your application's views
(for example, all its JSPs) and portlet components, create
a `src/main/resources/content` folder in that module, and place your
`Language.properties` and `Language_xx.properties` files there.

After that, make sure any portlet components (the `@Component` annotation
in your `-Portlet` classes) in the module include this property:

    "javax.portlet.resource-bundle=content.Language"

Providing translated language properties files and specifying the
`javax.portlet.resource-bundle` property in your portlet component is all you
must do to point @product@ at your translations. Users see the translations for
the locales they select. 

In a more complicated, well-modularized application, you might have language
keys spread over multiple modules providing portlet components and JSP files.
Moreover, there might be a fair number of duplicated language keys between the
modules. Thankfully you don't need to maintain language properties files in each
module.

## Creating a Language Module

If you're crazy about modularity (and you should be), you might have an
application with multiple modules that provide the view layer. These modules are
often called web modules.

    my-application/
    my-application-web/
    my-admin-application-web/
    my-application-content-web/
    my-application-api/
    my-application-service/

Each of these modules can have language keys and translations to maintain, and
there will probably be duplicate keys. You don't want to end up with different
values for the same key, and you don't want to maintain language keys in
multiple places. In this case, you need to go even crazier with modularity and
create a new module, which we'll call a language module.

In the root project folder (the one that holds your service, API, and web
modules),
[create a new module](/docs/7-1/tutorials/-/knowledge_base/t/starting-module-development#creating-a-module)
to hold your app's language keys. For example, here's the folder structure of a
language module called `my-application-lang`.

    my-application-lang/
        bnd.bnd
        src/
            main/
                resources/
                    content/
                        Language.properties
                        Language_ar.properties
                        Language_bg.properties
                        ...

In the language module, create a `src/main/resources/content` folder. Put your
language properties files here. A `Language.properties` file might look like
this: 

    application=My Application
    add-entity=Add Entity

Create any translations you want, adding the translation locale ID to the
language file name. File `Language_es.properties` might look like this: 

    my-app-title=Mi Aplicación
    add-entity=Añadir Entity

On building the language module, @product@'s
`ResourceBundleLoaderAnalyzerPlugin` detects the `content/Language.properties`
file and adds a resource bundle
[*capability*](http://blog.osgi.org/2015/12/using-requirements-and-capabilities.html)
to the module. A capability is a contract a module declares to @product@'s OSGi
framework. Capabilities let you associate services with modules that provide
them. In this case, @product@ registers a
[ResourceBundleLoader](@platform-ref@/7.1-latest/javadocs/portal-kernel/com/liferay/portal/kernel/util/ResourceBundleLoader.html)
service for the resource bundle capability. 

Next, you'll configure a web module to use the language module resource
bundle. 

## Using a Language Module

A module or traditional Liferay plugin can use a resource bundle from another
module and optionally include its own resource bundle. OSGi manifest headers
`Require-Capability` and `Provide-Capability` make this possible, and it's
especially easy in modules generated from Liferay project templates.
Instructions for using a language module are divided into these environments:

-  [Using a Language Module from a Module](#using-a-language-module-from-a-module)
-  [Using a Language Module from a Traditional Plugin](#using-a-language-module-from-a-traditional-plugin)

If you're using bnd with Maven or Gradle, you need only specify Liferay's
`-liferay-aggregate-resource-bundle:` bnd instruction--at build time, Liferay's
bnd plugin converts the instruction to `Require-Capability` and
`Provide-Capability` parameters automatically. Both approaches are demonstrated. 

### Using a Language Module from a Module

Modules generated from Liferay project templates have a Liferay bnd build time
instruction called `-liferay-aggregate-resource-bundles`. It lets you use other
resource bundles (e.g., including their language keys) along with your own.
Here's how to do it:

1.  Open your module's `bnd.bnd` file.

2.  Add the `-liferay-aggregate-resource-bundles:` bnd instruction and assign it
    the bundle symbolic names of modules whose resource bundles to aggregate
    with the current module's resource bundle. 

        -liferay-aggregate-resource-bundles: \
            [bundle.symbolic.name1],\
            [bundle.symbolic.name2]

For example, a module that uses resource bundles from modules
`com.liferay.docs.l10n.myapp1.lang` and `com.liferay.docs.l10n.myapp2.lang`
would set this in its `bnd.bnd` file:

    -liferay-aggregate-resource-bundles: \
        com.liferay.docs.l10n.myapp1.lang,\
        com.liferay.docs.l10n.myapp2.lang

The current module's resource bundle is prioritized over those of the listed
modules. 

| The
| [Shared Language Key sample project](/docs/7-1/reference/-/knowledge_base/r/shared-language-keys)
| is a working example that demonstrates aggregating resource bundles. You can
| deploy it in Gradle, Maven, and Liferay Workspace build environments.

At build time, Liferay's bnd plugin converts the bnd instruction to
`Require-Capability` and `Provide-Capability` parameters automatically. In
traditional Liferay plugins, you must specify the parameters manually. 

| **Note:** You can always specify the `Require-Capability` and `Provide-
| Capability` OSGi manifest headers manually, as the next section demonstrates.

### Using a Language Module from a Traditional Plugin

To use a language module, from a traditional Liferay plugin you must specify the
language module using `Require-Capability` and `Provide-Capability` OSGi
manifest headers in the plugin's `liferay-plugin-package.properties` file. 

Follow these steps to configure your traditional plugin to use a language
module:

1.  Open the plugin's `liferay-plugin-package.properties` file and add a 
    `Require-Capability` header that filters on the language module's resource
    bundle capability. For example, if the language module's symbolic name is
    `myapp.lang`, you'd specify the requirement like this:

        Require-Capability: liferay.resource.bundle;filter:="(bundle.symbolic.name=myapp.lang)"

2.  In the same `liferay-plugin-package.properties` file, add a 
    `Provide-Capability` header that adds the language module's resource bundle
    *as* this plugin's (the `myapp.web` plugin) own resource bundle:

        Provide-Capability:\
        liferay.resource.bundle;resource.bundle.base.name="content.Language",\
        liferay.resource.bundle;resource.bundle.aggregate:String="(bundle.symbolic.name=myapp.lang)";bundle.symbolic.name=myapp.web;resource.bundle.base.name="content.Language";service.ranking:Long="4";\
        servlet.context.name=myapp-web

In this case, the `myapp.web` plugin solely uses the language module's resource
bundle---the resource bundle aggregate only includes language module
`myapp.lang`. 

Aggregating resource bundles comes into play when you want to use a language
module's resource bundle *in addition to* your plugin's resource bundle. These
instructions show you how to do this, while prioritizing your current plugin's
resource bundle over the language module resource bundle. In this way, the
language module's language keys compliment your plugin's language keys. 

For example, a portlet whose bundle symbolic name is `myapp.web` uses keys from
language module `myapp.lang`, in addition to its own. The portlet's
`Provide-Capability` and `Web-ContextPath` OSGi headers accomplish this.

    Provide-Capability:\
    liferay.resource.bundle;resource.bundle.base.name="content.Language",\
    liferay.resource.bundle;resource.bundle.aggregate:String="(bundle.symbolic.name=myapp.web),(bundle.symbolic.name=myapp.lang)";bundle.symbolic.name=myapp.web;resource.bundle.base.name="content.Language";service.ranking:Long="4";\
    servlet.context.name=myapp-web

The example `Provide-Capability` header has two parts: 

1.  `liferay.resource.bundle;resource.bundle.base.name="content.Language"` 
    declares that the module provides a resource bundle whose base name is
    `content.language`. 

2.  The `liferay.resource.bundle;resource.bundle.aggregate:String=...` directive
    specifies the list of bundles whose resource bundles are aggregated, the
    target bundle, the target bundle's resource bundle name, and this service's
    ranking:

    -   `"(bundle.symbolic.name=myapp.web),(bundle.symbolic.name=myapp.lang)"`:
        The service aggregates resource bundles from bundles
        `bundle.symbolic.name=myapp.web` (the current
        module) and `bundle.symbolic.name=myapp.lang`.
        Aggregate as many bundles as desired. Listed bundles are prioritized in
        descending order. 
    -   `bundle.symbolic.name=myapp.web;resource.bundle.base.name="content.Language"`:
        Override the `myapp.web` bundle's resource bundle named
        `content.Language`.
    -   `service.ranking:Long="4"`: The resource bundle's service ranking is 
        `4`. The OSGi framework applies this service if it outranks all other
        resource bundle services that target `myapp.web`'s
        `content.Language` resource bundle. 
    -   `servlet.context.name=myapp-web`: The target resource bundle is in 
        servlet context `myapp-web`. 

Now the language keys from the aggregated resource bundles compliment your
plugin's language keys.

Did you know that @product@'s core language keys are also available to your
module? They're up next. 

## Using Liferay's Language Properties

If you have @product@'s source code, you can check out @product@'s core language
properties by looking in the `portal-impl/src/main/content` folder. Otherwise,
you can look in the `portal-impl.jar` that's in your Liferay bundle.

    liferay-portal/portal-impl/src/content/Language_xx.properties

    [Liferay Home]/tomcat-[version]/webapps/ROOT/WEB-INF/lib/portal-impl.jar

These keys are available at runtime, so when you use any of @product@'s default
keys in your user interface code, they're automagically swapped out for the
appropriately translated value. Using @product@'s keys where possible saves you
time and ensures that your application follows Liferay's UI conventions.

If you want to generate language files for each supported locale automatically,
or to configure your application to generate translations automatically using
the Microsoft Translator API, check out the tutorial
[Automatically Generating Language Files](/docs/7-1/tutorials/-/knowledge_base/t/automatically-generating-language-files).
