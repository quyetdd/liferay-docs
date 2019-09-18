---
header-id: automatically-deploying-theme-changes
---

# Automatically Deploying Theme Changes

[TOC levels=1-4]

You may have noticed that you have to deploy your theme manually each time you 
make a change. This can become tedious during the development process. The 
`gulp watch` task lets you preview changes to your theme without requiring a 
full redeploy. 

| **Note:** Gulp is included as a local dependency in generated themes, so you
| are not required to install it. It can be accessed by running
| `node_modules\.bin\gulp` followed by the Gulp task from a generated theme's
| root folder.

Follow these steps to preview changes to your theme automatically: 

1.  Enable
    [Developer Mode](/docs/7-1/tutorials/-/knowledge_base/t/using-developer-mode-with-themes)
    in your server. Without this enabled, the gulp watch task **will not work**.

2.  Navigate to your theme's root folder and run `gulp watch`. This sets up a 
    proxy for your app server (`http://localhost:9080`) and opens it in a new 
    window in the browser. It also provides an IP address for you to view your 
    app server across all devices connected to the local network. The browser is 
    synced across all devices that use the given IP address.
    
    | **Note:** Live changes are only viewable on port `9080`
    | (`http://localhost:9080`). Live changes **are not viewable** on your app
    | server (e.g. `http://localhost:8080`). 

    ![Figure 1: Run the `gulp watch` task to automatically deploy any changes to your theme.](../../../../images/theme-dev-watching-themes-gulp-watch-startup.png)

    You can verify that the watch task is running by checking that the 
    `webBundleDir` property is present in your theme's `liferay-theme.json` 
    file. It should have the value `watching`:

        {
          "LiferayTheme": {
            "appServerPath": "C:\\Users\\liferay\\opt\\Liferay\\bundles\\7.1-master-bundle\\bundles\\tomcat-8.0.32",
            "deployPath": "C:\\Users\\liferay\\opt\\Liferay\\bundles\\7.1-master-bundle\\bundles\\deploy",
            "url": "http://localhost:8080",
            "appServerPathPlugin": "C:\\Users\\liferay\\Desktop\\projects\\themes\\7-1-themes\\my-liferay-theme\\.web_bundle_build",
            "deployed": true,
            "pluginName": "my-liferay-theme",
            "webBundleDir": "watching"
          }
        }

3.  Make a change to your theme and save the file. The updated files are built, 
    compiled, and copied directly to port `9080`. CSS changes are deployed 
    live, so no page reload is needed. For JS and template changes, **you must** 
    reload the browser to see the changes.

    ![Figure 2: The watch task notifies you that the changes are deployed.](../../../../images/theme-dev-watching-themes-gulp-watch-auto-deploy.png)

4.  Once you're happy with the previewed changes, deploy your theme to your app 
    server to apply the changes.

## Related Topics

[Configuring Your Theme's App Server](/docs/7-1/tutorials/-/knowledge_base/t/configuring-your-themes-app-server)

[Copying an Existing Theme's Files](/docs/7-1/tutorials/-/knowledge_base/t/copying-an-existing-themes-files)

[Deploying Themes](/docs/7-1/tutorials/-/knowledge_base/t/deploying-your-theme)
