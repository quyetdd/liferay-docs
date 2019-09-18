---
header-id: adding-a-liferay-dxp-data-source
---

# Adding a Liferay DXP Data Source

Your Liferay DXP instances are rich with contact data from Users and web
analytics data on user interaction with Liferay DXP pages and assets. When you
Create Liferay DXP data sources, you can select web analytics data from the
Liferay DXP Sites you want. In Users and Organizations, you can choose to use
contact data from all Users or a specified subset of them. Before adding an
instance as a data source, though, you must connect it to your Analytics Cloud
project. 

## Liferay DXP Data Source Prerequisites

Follow these steps to connect your project to your Liferay DXP instance.

### Step 1: Install Required Liferay DXP Fix Packs

Liferay DXP 7.1: 
[Download](https://customer.liferay.com/downloads) 
and
[install](/docs/7-1/deploy/-/knowledge_base/d/installing-patches) 
fix pack 10+. 

Liferay DXP 7.0: 
[Download](https://customer.liferay.com/downloads)
and 
[install](/docs/7-0/deploy/-/knowledge_base/d/patching-tool#installing-patches)
fix pack 79+. 

### Step 2: Register Analytics Cloud with your Liferay DXP instance

Liferay DXP 7.1: 

1.  Download the 
    [Liferay Plugin for OAuth 2.0](https://web.liferay.com/marketplace/-/mp/application/109571986)
    version 1.1.0 (or newer) and
    [install](/docs/7-1/user/-/knowledge_base/u/installing-apps-manually)
    it.

2.  The plugin comes with Analytics Cloud pre-registered. Copy the *Client ID*
    and *Client Secret* for connecting DXP with Analytics Cloud, as described
    in the next section.

Liferay DXP 7.0:

1.  Download the
    [Liferay Connector to OAuth 1.0a](https://web.liferay.com/marketplace/-/mp/application/45261909)
    and
    [install](/docs/7-0/user/-/knowledge_base/u/installing-apps-manually)
    it. 

2.  [Register](/docs/7-0/deploy/-/knowledge_base/d/oauth)
    Analytics Cloud as an OAuth application with the *Write* access level. 

3.  Copy the *Consumer ID* and *Consumer Secret* for connecting DXP with 
    Analytics Cloud, as described in the next section. 

Congratulations on authorizing Analytics Cloud to connect to your Liferay DXP
instance! It's time to add your DXP instance as a data source. 

| **Tip:** If you have problems connecting to your DXP data source, refer to
| [Troubleshooting Liferay DXP Data Sources](https://github.com/liferay/liferay-docs/blob/7.1.x/discover/analytics-cloud/articles/06-troubleshooting/00-troubleshooting-data-sources-intro.markdown).

## Adding the DXP Data Source

Adding a Liferay DXP data source connects your Analytics Cloud project with
a Liferay DXP instance. 

1.  Select *Settings* &rarr; *Data Sources*. A listing of your data sources 
    appears.

2.  Click *Add Data Source*. The *Add Data Source* page appears. 

3.  Select the *Liferay DXP* icon. The *Configure Liferay DXP* page appears.

The Authorization tab is selected by default. It's time to authorize your DXP
instance as a data source. 

### DXP Data Source Authorization

Here's how to authorize your DXP instance as a data source: 

1.  Fill in the data source and client credentials fields. 

    *Description* 

    **Name:** A name for your data source.

    **URL:** The Liferay DXP instance URL. 

    *Client Credentials*

    **Consumer Key/Client ID:** Key/ID for Analytics Cloud to access your 
    Liferay DXP instance. 

    **Consumer Secret/Client Secret:** Secret for Analytics Cloud to access your
    Liferay DXP instance. 

    In Liferay DXP 7.1, the Client ID and Secret are found at *Control Panel*
    &rarr; *Configuration* &rarr; *OAuth 2 Admin*.

    In Liferay DXP 7.0, the Consumer Key and Secret are found at *Control Panel*
    &rarr; *OAuth Admin*. 

2.  Click *Authorize*. A window appears and prompts you to sign in to the DXP 
    instance. 

3.  Sign in by entering your DXP admin (user that has the Admin role) 
    credentials and clicking *Authorize*. 

4.  Click *Save* to save the authorization options. Analytics Cloud advances you
    to the Configure Data Source tab's Data Configuration page. The data
    source's Current Status is *AUTHENTICATED*. 

Here are the Configure Data Source options:
 
**Configure Contacts:** Configures the contact data only.

**Configure Analytics:** Configures the assets and touchpoints only.

Start with configuring contacts. 

### Configuring Contacts

Configuring contacts imports DXP user data. 

1.  Select the *Configure* button for *Configure Contacts*. The Contacts 
    configuration options appear. 

2.  Configuring contacts involves selecting contacts to sync from the Liferay
    DXP instance and its User Groups and Organizations. Contacts belonging to
    multiple User Groups and Organizations are only counted once. 

    **Sync All Contacts:** Selects all Liferay DXP instance contacts and disables
    options for selecting specific User Groups and Organizations.

    **Sync By User Groups:** Selects contacts by User Group.

    **Sync By Organizations:** Selects contacts by Organization. 

    ![Figure 1: Analytics Cloud lets you select and import contacts from a Liferay DXP instance and its Organizations and User Groups.](../../images/select-dxp-contacts-by-org.png)

3.  Click *Save and Continue* to import the selected contacts. Analytics Cloud
    imports the contact data and maps it to your Analytics Cloud contact data
    model. The initial contact data import can take 5 1/2 minutes per 1,000
    contacts. 

4.  Follow instructions for 
    [Mapping Contact Data](https://github.com/liferay/liferay-docs/blob/7.1.x/discover/analytics-cloud/articles/02-getting-started/04-mapping-contact-data.markdown)
    to  map contact data from your Liferay DXP instance to your Analytics Cloud
    contact data model. Once you've mapped the data, click *Save*. The Data
    Configuration page appears again; the button for Configure Contacts is
    labeled *Edit*. 

You've configured Analytics Cloud to use your Liferay DXP contacts. 

### Configuring Analytics

Configuring analytics imports asset and touchpoint data as it relates to DXP
contacts you've imported. 

1.  Click *Configure* for *Configure Analytics*. The Liferay DXP site analytics
    registration page appears. 

2.  Select the Liferay DXP Sites to register for analytics and click 
    *Configure*.

3.  Click the *Done* button. 

The Contacts and Analytics data start syncing into Analytics Cloud. **Initially 
the sync takes a while. After the initial sync, changes are synced 
periodically.**

| **Tip:** If you have problems connecting to your DXP data source, refer to
| [Troubleshooting Liferay DXP Data Sources](https://github.com/liferay/liferay-docs/blob/7.1.x/discover/analytics-cloud/articles/06-troubleshooting/00-troubleshooting-data-sources-intro.markdown).

If you have contact profile data from other sources such as a database, you
might be able to export the data to a CSV file. Then you can add the CSV file as
a data source and import the contact profiles from it. Adding a CSV data source
is next. 
