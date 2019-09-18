---
header-id: web-screenlet-for-android
---

# Web Screenlet for Android

[TOC levels=1-4]

## Requirements

-   Android SDK 4.1 (API Level 16) or above 
-   Liferay Portal 6.2 CE/EE, Liferay CE Portal 7.0/7.1, Liferay DXP
-   Liferay Screens Compatibility app
    ([CE](http://www.liferay.com/marketplace/-/mp/application/54365664) or 
    [EE/DXP](http://www.liferay.com/marketplace/-/mp/application/54369726)). 
    This app is preinstalled in Liferay CE Portal 7.0/7.1 and Liferay DXP. 

## Compatibility

- Android SDK 4.1 (API Level 16) or above

## Xamarin Requirements

- Visual Studio 7.2
- Mono .NET framework 5.4.1.6

## Features

Web Screenlet lets you display any web page. It also lets you customize the web 
page through injection of local and remote JavaScript and CSS files. If you're 
using @product@ as backend, you can use 
[Application Display Templates](/docs/7-1/user/-/knowledge_base/u/styling-apps-and-assets) 
in your page to customize its content from the server side. 

## Module

- None

## Views

- Default

![Figure 1: The Web Screenlet with the Default View Set.](../../../images/screens-android-webscreenlet.png)

## Configuration

To learn how to use Web Screenlet, follow the steps in the tutorial 
[Rendering Web Pages in Your Android App](/docs/7-1/tutorials/-/knowledge_base/t/rendering-web-pages-in-your-android-app). 
That tutorial gives detailed instructions for using the configuration items 
described here. 

Web Screenlet has `WebScreenletConfiguration` and 
`WebScreenletConfiguration.Builder` classes that you can use together to supply 
the parameters that the Screenlet needs to work. 
`WebScreenletConfiguration.Builder` has the following methods, which let you 
supply the described configuration parameters: 

| Method | Return | Explanation |
|-----------|-----------|-------------| 
| `addLocalJs(fileName)` | `WebScreenletConfiguration.Builder` | Adds a local JavaScript file with the supplied filename. The JavaScript files must be in the first level of your app's `assets` folder. Create this folder at the same level of the `res` folder. |
| `addLocalCss(fileName)` | `WebScreenletConfiguration.Builder` | Adds a local CSS file with the supplied filename. The CSS files must be in the first level of your app's `assets` folder. Create this folder at the same level of the `res` folder. |
| `addRawJs(rawJs, name)` | `WebScreenletConfiguration.Builder` | Adds a JavaScript file from your app's `res/raw` folder. Create this folder if it doesn't exist. Reference the file using `R.raw.yourfilename`. This method also takes a second parameter called `name`, which is only for debugging purposes. If there's an error, the console displays it with this `name` value. |
| `addRawCss(rawCss, name)` | `WebScreenletConfiguration.Builder` | Adds a CSS file from your app's `res/raw` folder. Create this folder if it doesn't exist. Reference the file using `R.raw.yourfilename`. This method also takes a second parameter called `name`, which is only for debugging purposes. If there's an error, the console displays it with this `name` value. |
| `addRemoteJs(url)` | `WebScreenletConfiguration.Builder` | Adds a JavaScript file from the supplied URL. |
| `addRemoteCss(url)` | `WebScreenletConfiguration.Builder` | Adds a CSS file from the supplied URL. |
| `setWebType(webType)` | `WebScreenletConfiguration.Builder` | Sets the [`WebType`](/docs/7-1/reference/-/knowledge_base/r/web-screenlet-for-android#webtype). |
| `enableCordova(observer)` | `WebScreenletConfiguration.Builder` | Enables Cordova inside the Web Screenlet. |
| `load()` | `WebScreenletConfiguration` | Returns the `WebScreenletConfiguration` object that you can set to the Screenlet instance. |

| **Note:** If you want to add comments in the scripts, use the `/**/` notation.

### WebType

-   **WebType.LIFERAY_AUTHENTICATED** (default): Displays a @product@ page that 
    requires authentication. The user must therefore be logged in with Screens 
    via Login Screenlet or a `SessionContext` method. For this `WebType`, the 
    URL you must pass to the `WebScreenletConfiguration.Builder` constructor is 
    a relative URL. For example, if the full URL is 
    `http://screens.liferay.org.es/web/guest/blog`, then the URL you must supply 
    to the constructor is `/web/guest/blog`. 

-   **WebType.OTHER**: Displays any other page. For this `WebType`, the URL you 
    must pass to the `WebScreenletConfiguration.Builder` constructor is a full 
    URL. For example, if the full URL is 
    `http://screens.liferay.org.es/web/guest/blog`, then you must supply that 
    URL to the constructor. 

## Attributes

| Attribute | Data type | Explanation |
|-----------|-----------|-------------|
| `autoLoad` | `boolean` | Whether to load the page automatically when the Screenlet appears in the app's UI. The default value is `true`. |
| `layoutId` | `@layout` | The layout to use to show the View. |
| `isLoggingEnabled` | `boolean` | Whether logging is enabled. |
| `isScrollEnabled` | `boolean` | Whether to enable scrolling on the page inside the Screenlet. |

## Methods

| Method | Return | Explanation |
|-----------|-----------|-------------| 
| `load()` | `void` | Checks if the page's URL is valid, and then loads it. The operation fails if the URL is invalid. |
| `clearCache()` | `void` | Clears the Web Screenlet's cache. |
| `injectScript(script)` | `void` | Injects a script when the page is already loaded. |

## Listener

Web Screenlet delegates some events to an object or class that 
implements its 
[`WebListener` interface](https://github.com/liferay/liferay-screens/blob/master/android/library/src/main/java/com/liferay/mobile/screens/web/WebListener.java). 
This interface extends the 
[`BaseCacheListener` interface](https://github.com/liferay/liferay-screens/blob/master/android/library/src/main/java/com/liferay/mobile/screens/base/interactor/listener/BaseCacheListener.java). 
Therefore, Web Screenlet's listener methods are as follows: 

-   `onPageLoaded(String url)`: Called when the Screenlet loads the page 
    correctly.

-   `onScriptMessageHandler(String namespace, String body)`: Called when the 
    `WebView` in the Screenlet sends a message. The `namespace` parameter is the 
    source namespace key, and `body` is the source namespace body. 

-   `error(Exception e, String userAction)`: Called when an error occurs in the 
    process. The `userAction` argument distinguishes the specific action in 
    which the error occurred. 
