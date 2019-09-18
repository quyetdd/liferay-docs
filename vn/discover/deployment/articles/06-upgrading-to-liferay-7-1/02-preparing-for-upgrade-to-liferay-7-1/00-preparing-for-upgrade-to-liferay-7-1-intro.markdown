---
header-id: preparing-an-upgrade-to-liferay-7
---

# Preparing for Upgrade to @product-ver@

[TOC levels=1-4]

Now that you've completed the
[pre-upgrade process of cleaning and normalizing your data](/docs/7-1/deploy/-/knowledge_base/d/pre-upgrade-speed-up-the-process),
you're ready to prepare your environment for upgrading to @product-ver@. Here's
a summary of the preparation steps: 

**Step 1**: Upgrade your Marketplace apps 

**Step 2**: Publish all changes from the staged site to the live site

**Step 3**: Remove duplicate web content structure field names

**Step 4**: Synchronize a complete @product@ backup

**Step 5**: Update your portal properties

**Step 6**: Configure your Documents and Media file store

**Step 7**: Install @product-ver@ and the latest fix pack

**Step 8**: Disable indexing during the upgrade process

This tutorial describes these steps in detail. 

## Step 1: Upgrade Your Marketplace Apps

Upgrade each Marketplace app (Kaleo, Calendar, Notifications, etc.) that you're
using to its latest version for your installation. Before proceeding with the
upgrade, troubleshoot any issues regarding these apps. 

## Step 2: Publish All Changes from the Staged Site to the Live Site

If you have
[local/remote staging enabled](/docs/7-1/user/-/knowledge_base/u/enabling-staging)
and have content or data saved on the staged site, you must
[publish](/docs/7-1/user/-/knowledge_base/u/publishing-staged-content-efficiently)
it to the live site. If you skip this step, you must run a full publish (or
manually publish changes) after the upgrade, since the system won't know what
content changed since the last publishing date.

## Step 3: Remove Duplicate Web Content Structure Field Names

If you've used Web Content Management extensively, you might have structures
whose field names aren't unique. You must 
[find and remove duplicate field names](/docs/6-2/deploy/-/knowledge_base/d/upgrading-liferay#find-and-remove-duplicate-field-names)
before upgrading. If you upgraded to Liferay Portal 6.2 previously and skipped doing this, you'll encounter this error: 

    19:29:35,298 ERROR [main][VerifyProcessTrackerOSGiCommands:221] com.liferay.portal.verify.VerifyException: com.liferay.dynamic.data.mapping.validator.DDMFormValidationException$MustNotDuplicateFieldName: The field name page cannot be defined more than once
    com.liferay.portal.verify.VerifyException: com.liferay.dynamic.data.mapping.validator.DDMFormValidationException$MustNotDuplicateFieldName: The field name page cannot be defined more than once
 
If this is the case, roll back to your previous backup of Liferay 6.2 and 
[find and remove duplicate field names](/docs/6-2/deploy/-/knowledge_base/d/upgrading-liferay#find-and-remove-duplicate-field-names). 

## Step 4: Synchronize a Complete Backup @product@

[Back up your @product@ database, installation, and Document Library store](/docs/7-1/deploy/-/knowledge_base/d/backing-up-a-liferay-installation). 

## Step 5: Update Your Portal Properties

It is likely that you have overridden portal properties to customize your
installation to your requirements. If so, you must update the properties files
(e.g., `portal-setup-wizard.properties` and `portal-ext.properties` files) to be
compatible with @product-ver@. As you do this, you should account for property
changes in all versions of @product@ since your current version up to and
including @product-ver@.

If you're coming from a version prior to Liferay Portal 6.2, start with
these property-related updates:

-   If you're on Liferay Portal 6.1,
    [adapt your properties to the new defaults that Liferay Portal 6.2 introduced](/docs/6-2/deploy/-/knowledge_base/d/upgrading-liferay#review-the-liferay-6). 

-   If you're on Liferay 6.0.12, 
    [migrate the Image Gallery](/docs/6-2/deploy/-/knowledge_base/d/upgrading-liferay#migrate-your-image-gallery-images).

-   If you have a sharded environment,
    [configure your upgrade for sharding](/docs/7-0/deploy/-/knowledge_base/d/upgrading-sharded-environment).

When a new version of @product@ is released, there are often changes to default
settings, and this release is no different. If you rely on the defaults from
your old version, you'll want to review the changes and decide whether you want
to keep the defaults from your old version or accept the defaults of the new. 

Here's a list of the 6.2 properties that have changed in 7.1: 
    
    users.image.check.token=false
    organizations.types=regular-organization,location
    organizations.rootable[regular-organization]=true
    organizations.children.types[regular-organization]=regular-organization,location
    organizations.country.enabled[regular-organization]=false
    organizations.country.required[regular-organization]=false
    organizations.rootable[location]=false
    #organizations.children.types[location]=
    organizations.country.enabled[location]=true
    organizations.country.required[location]=true
    layout.set.prototype.propagate.logo=true
    editor.wysiwyg.portal-web.docroot.html.taglib.ui.discussion.jsp=simple
    web.server.servlet.check.image.gallery=true
    blogs.trackback.enabled=true
    discussion.comments.format=bbcode
    discussion.max.comments=0
    dl.file.entry.thumbnail.max.height=128
    dl.file.entry.thumbnail.max.width=128

Properties in features that have been modularized have changed and must now be
deployed separately in
[OSGi configuration files](/docs/7-1/user/-/knowledge_base/u/system-settings#exporting-and-importing-configurations). 
The
[7.1 portal properties reference docs](@platform-ref@/7.1-latest/propertiesdoc/portal.properties.html)
provide property details and examples. 

## Step 6: Configuring Your Documents and Media File Store

It's time to migrate and update your document store configuration to
@product-ver@. Here's what's changed for document stores:

-   Store implementation class package names changed from 
    `com.liferay.portlet.documentlibrary.store.*` in Liferay Portal 6.2 to
    `com.liferay.portal.store.*` in @product@ 7.0+. Make sure your
    `portal-ext.properties` sets the `dl.store.impl` property one of these ways:

        dl.store.impl=com.liferay.portal.store.file.system.FileSystemStore
        dl.store.impl=com.liferay.portal.store.db.DBStore
        dl.store.impl=com.liferay.portal.store.file.system.AdvancedFileSystemStore
        dl.store.impl=com.liferay.portal.store.s3.S3Store

-   JCR Store was deprecated as of Liferay DXP Digital Enterprise 7.0 Fix Pack 
    14 and Liferay Portal CE 7.0 GA4. The
    [Document Repository Configuration](/docs/7-1/deploy/-/knowledge_base/d/document-repository-configuration)
    documentation describes other store options.
    [Migrate your document data](/docs/7-1/user/-/knowledge_base/u/server-administration)
    to one of the other store options before upgrading from your current Liferay
    version. 

-   Since @product@ 7.0, document store specific configuration (e.g., 
    configurations specific to Simple File Store, Advanced File Store, S3, etc.)
    is done in the Control Panel at *Configuration &rarr; System Settings &rarr;
    File Storage* or done using OSGi configuration files (`.config` files).

    Note, general document store configuration (e.g., `dl.store.impl=[File Store
    Impl Class]`) continues to be done using `portal-ext.properties`.  

    Here are steps, for example, that create a `.config` file that specifies an
    Advanced File Store's root file location:

    1.  Create a `.config` file named after the store implementation class 
        (i.e., the class assigned to your `dl.store.impl` property):

            com.liferay.portal.store.file.system.configuration.AdvancedFileSystemStoreConfiguration.config

    2.  Set the following property in the `.config` file and replace 
        `{document_library_path}` with  your file store's path. 

            rootDir="{document_library_path}"

    3.  Copy the `.config` file to your `[Liferay Home]/osgi/configs` folder

The
[Document Repository Configuration](/docs/7-1/deploy/-/knowledge_base/d/document-repository-configuration)
provides document store configuration details.

## Step 7: Install @product-ver@

Next,
[install @product@ on your application server](/docs/7-1/deploy/-/knowledge_base/d/deploying-product)
or
[use @product@ bundled with your application server of choice](/docs/7-1/deploy/-/knowledge_base/d/installing-liferay).

Then
[install the latest fix pack](https://customer.liferay.com/documentation/7.1/deploy/-/official_documentation/deployment/patching-liferay). 

**Important**: Once you have installed @product-ver@, **DON'T START IT!** In
Liferay Portal 6.2 and earlier, once you prepared your system for an upgrade,
the upgrade process ran when you started the new version for the first time. Now
to streamline server startup, @product@ ships with an upgrade tool (described in
the next article) that you must use to upgrade your database.

| **Note**: @product@ throws the following exception if you attempt to start
| @product-ver@ before running the upgrade tool:
| 
|     [MainServlet:237] java.lang.RuntimeException: You must first upgrade to @product@ 7000

Copy your custom portal properties files (e.g., `portal-ext.properties`) that
you updated in previous steps and your Documents and Media store into your new
installation. 

## Step 8: Disable Indexing During the Upgrade Process

Before starting the upgrade process in your new installation, you must disable
indexing to prevent upgrade process performance issues that arise when the
indexer attempts to reindex content. 

To disable indexing, create a file called
`com.liferay.portal.search.configuration.IndexStatusManagerConfiguration.config`
in your `[Liferay Home]/osgi/configs` folder and add the following content: 

    indexReadOnly="true"

After you complete the upgrade (described in the next article), re-enable
indexing by removing the `.config` file or setting `indexReadOnly="false"`. 

Ready to upgrade? The next article shows you how. 
