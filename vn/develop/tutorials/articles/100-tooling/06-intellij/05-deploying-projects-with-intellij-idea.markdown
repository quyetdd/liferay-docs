---
header-id: deploying-projects-with-intellij-idea
---

# Deploying Projects with IntelliJ IDEA

[TOC levels=1-4]

Once you've
[created a project](/docs/7-1/tutorials/-/knowledge_base/t/creating-projects-with-intellij-idea)
and
[installed your Liferay server](/docs/7-1/tutorials/-/knowledge_base/t/installing-a-server-in-intellij-idea)
in IntelliJ, you'll want to deploy your project. Follow the steps below to do
this:

1.  Right-click your project from within the Liferay Workspace folder structure
    and select *Liferay* &rarr; *Deploy*.

    This automatically loads a build progress window viewable at the bottom of
    your IntelliJ instance.

    ![Figure 1: Verify that your project build successfully.](../../../images/intellij-project-build.png)

2.  Verify that your project builds successfully from the build progress window.
    Then navigate back to your server's window and confirm it starts in your
    configured @product@ instance. You should receive a message like this:

        INFO  [fileinstall-C:/liferay-workspace/bundles/osgi/modules][BundleStartStopLogger:35] STARTED com.liferay.docs_1.0.0 [652]

The `watch` Blade CLI task deploys your project. This watches your local project
and propagates saved changes to the deployed project. With this, project updates
are viewable almost instantaneously from your Liferay server. For more info on
the `watch` task, see the 
[Deploying Projects with Blade CLI](/docs/7-1/tutorials/-/knowledge_base/t/deploying-projects-with-blade-cli)
article.

That's it! You've successfully deployed your project to @product@!
