---
header-id: deploying-projects-with-liferay-ide
---

# Deploying Projects with Liferay Dev Studio

[TOC levels=1-4]

Deploying projects in Liferay Dev Studio is a cinch. Before deploying your
project, make sure you have a Liferay server configured in Dev Studio. To see
how to do this, see the
[Installing a Server in Liferay Dev Studio](/docs/7-1/tutorials/-/knowledge_base/t/installing-a-server-in-liferay-ide)

There are two ways to deploy a project to your Liferay server. You should start
your Liferay server before attempting to deploy to it.

1.  Select the project from the Package Explorer window and drag it to your
    Liferay server in the Servers window.

    ![Figure 1: You can use the drag-and-drop method to deploy your project to @product@.](../../../images/starting-module-dev-drag-module.png)

2.  Right-click the server from the Servers window and select *Add and
    Remove...*. Add the project(s) you'd like to deploy from the Available
    window to the Configured window. Then click *Finish*.

    ![Figure 2: Using the this deployment method is convenient when deploying multiple projects.](../../../images/add-and-remove-ide.png)

| **Note:** For a legacy Maven application, you were able to deploy it by
| right-clicking it in the Package Explorer and selecting *Liferay* &rarr; *Maven*
| &rarr; *liferay:deploy*. This is no longer possible because Liferay's Maven
| archetypes no longer rely on the legacy `liferay-maven-plugin`. To deploy Maven
| projects in Dev Studio, make sure to follow the methods described above.

If you're deploying a project using
[Liferay Workspace](/docs/7-1/tutorials/-/knowledge_base/t/liferay-workspace),
the `watch` Blade CLI task is used to deploy your project. This watches your
local project and quickly propagates any saved changes to the deployed project.
With this, project updates are viewable almost instantaneously from your Liferay
server. For more info on the `watch` task, see the
[Deploying Projects with Blade CLI](/docs/7-1/tutorials/-/knowledge_base/t/deploying-projects-with-blade-cli)
article.

| **Note:** You can deploy standalone projects not residing in a Liferay Workspace
| with the `watch` task by right-clicking the project and selecting *Liferay*
| &rarr; *watch*.

That's it! Once your project is deployed to the Liferay server, you can verify
its installation in Dev Studio's Console window.
