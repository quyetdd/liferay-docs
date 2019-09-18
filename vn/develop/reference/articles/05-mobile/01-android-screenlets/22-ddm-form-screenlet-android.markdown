---
header-id: ddm-form-screenlet-for-android
---

# DDM Form Screenlet for Android

[TOC levels=1-4]

## Requirements

-   Android SDK 4.1 (API Level 21) or above
-   Liferay DXP 7.1 SP2+
-   Liferay Hypermedia REST APIs. These APIs are installed but disabled by 
    default. To enable them, follow the instructions in the tutorial 
    [Enabling Hypermedia REST APIs](/docs/7-1/tutorials/-/knowledge_base/t/enabling-hypermedia-rest-apis). 

## Compatibility

-   Android SDK 4.1 (API Level 21) or above

## Xamarin Requirements

-   Visual Studio 7.2
-   Mono .NET framework 5.4.1.6

## Features

DDM Form Screenlet shows a set of fields that can be filled in by the user. The 
fields can contain initial or existing values. The following fields are 
supported: 

**Paragraph:** Add a title and/or text in your form.

**Text Field:** A single or multiline text area.

**Single Selection:** Select one item with a radio button.

**Select From List:** Choose one or more items in a list.

**Multiple Selection:** Select multiple items via a checkbox.

**Date:** Select a date from a date picker.

**Grid:** Select items in a matrix.

**Numeric:** Enter an integer or decimal number.

**Upload:** Upload files via Documents and Media.

DDM Form Screenlet also supports the following features:

**Element Sets:** Reuse pre-existing element sets in your form. 

**Multiple Pages:** Use multi-page forms. 

**Success Page:** Show friendly feedback at the end of your form. 
**Autosave:** Automatically save any change in form values to a draft.

**Restore Previous Draft:** Automatically restore the last draft when 
opening the form, independent of platform.

**Rules:** Create complex rules in your form. For example, you can show or 
hide fields depending on the input of other fields.

**Workflow:** Form submission can trigger a specific workflow.

**Required Values:** Require specific values and/or validate form fields. 

**Internationalization:** Support i18n in record values and labels.

## Module

-   DDM

## Views

-   Default
-   Lexicon
-   Material

![Figure 1: The DDM Form Screenlet with the Lexicon View Set.](../../../images/screens-android-ddm-form-screenlet-lexicon-view.png)

### Custom Layouts

To create custom layouts for a field, create the new layout following the naming 
pattern `<field_editor_id>_<view_name>`. The Screenlet automatically loads such 
layouts. 

For example, this table lists the filename you should use when creating custom 
layouts for each field type, for the Lexicon View. Note that because some DDM 
fields inherit from DDL, they are referenced as DDL. 

<table class="table table-striped table-bordered">
<thead>
<tr>
<th>Editor Type</th>
<th>Field Editor ID</th>
<th>Example Using Lexicon View</th>
</tr>
</thead>
<tbody>
<tr>
<td>Checkbox</td>
<td>ddlfield_checkbox</td>
<td>ddlfield_checkbox_lexicon.xml</td>
</tr>
<tr>
<td>Checkbox Multiple</td>
<td>ddmfield_checkbox</td>
<td>ddmfield_checkbox_multiple.xml</td>
</tr>
<tr>
<td>Date</td>
<td>ddlfield_date</td>
<td>ddlfield_date_lexicon.xml</td>
</tr>
<tr>
<td>Number</td>
<td>ddlfield_number</td>
<td>ddlfield_number_lexicon.xml</td>
</tr>
<tr>
<td>Integer</td>
<td>ddlfield_number</td>
<td>ddlfield_number_lexicon.xml</td>
</tr>
<tr>
<td>Decimal</td>
<td>ddlfield_number</td>
<td>ddlfield_number_lexicon.xml</td>
</tr>
<tr>
<td>Radio</td>
<td>ddlfield_radio</td>
<td>ddlfield_radio_lexicon.xml</td>
</tr>
<tr>
<td>Text</td>
<td>ddlfield_text</td>
<td>ddlfield_text_lexicon.xml</td>
</tr>
<tr>
<td>Select</td>
<td>ddlfield_select</td>
<td>ddlfield_select_lexicon.xml</td>
</tr>
<tr>
<td>Text Area</td>
<td>ddlfield_text_area</td>
<td>ddlfield_text_area_lexicon.xml</td>
</tr>
<tr>
<td>Paragraph</td>
<td>ddmfield_paragraph</td>
<td>ddmfield_paragraph_lexicon.xml</td>
</tr>
<tr>
<td>Document</td>
<td>ddlfield_document</td>
<td>ddlfield_document_lexicon.xml</td>
</tr>
<tr>
<td>Grid</td>
<td>ddmfield_grid</td>
<td>ddmfield_grid_lexicon.xml</td>
</tr>
<tr>
<td>Repeatable</td>
<td>ddmfield_repeatable</td>
<td>ddmfield_repeatable_lexicon.xml</td>
</tr>
</tbody>
</table>

## Application Configuration

DDM Form Screenlet needs the following user permissions:

    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

The Documents and Media fields use both to take a picture/video and store it 
locally before uploading it to the portal. 

## Portal Configuration

Before using DDM Form Screenlet, ensure that the following exist in the portal: 

-   A form for the Screenlet to display. For instructions on this, see the 
    article 
    [Creating and Managing Forms](/docs/7-1/user/-/knowledge_base/u/creating-and-managing-forms). 

-   If your form uses it, workflow must be configured. See the 
    [Workflow](/docs/7-1/user/-/knowledge_base/u/workflow) 
    section of the user guide for instructions on configuring and using 
    workflow. 

## Required Attributes

-   `formInstanceId`

## Attributes

<table class="table table-striped table-bordered">
<thead>
<tr>
<th>Attribute</th>
<th>Data Type</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td>formInstanceId</td>
<td>number</td>
<td>The ID of the form to display in the Screenlet. To find the IDs for your data definitions in the portal, select the site to work in and click Content &rarr; Forms. The table that lists the site's forms also lists each form's ID.</td>
</tr>
<tr>
<td>layoutId</td>
<td>@layout</td>
<td>The layout to use to show the View.</td>
</tr>
</tr>
<tr>
<td>autoloadDraftEnabled</td>
<td>boolean</td>
<td>Sets whether the form loads the last draft for the current user when the Screenlet is shown. The default value is `true`.</td>
</tr>
<tr>
<td>autosaveDraftEnabled</td>
<td>boolean</td>
<td>Sets whether the form should autosave a draft for the current user. The default value is `true`.</td>
</tr>
<tr>
<td>syncFormTimeout</td>
<td>number</td>
<td>Time in milliseconds to start synchronize the form (save and evaluate form rules). The default value is 500.</td>
</tr>
</tbody>
</table>

![Figure 2: The red box in this image highlights a form's ID.](../../../images/screens-portal-ddm-form-id.png)

## Permissions

If your form includes at least one Documents and Media field, you must grant 
permissions in the target repository and folder. For more information, see 
[Granting File Permissions and Roles](/docs/7-1/user/-/knowledge_base/u/adding-files-to-a-document-library#granting-file-permissions-and-roles), 
and 
[Setting Folder Permissions](/docs/7-1/user/-/knowledge_base/u/creating-folders#setting-folder-permissions).
To set permissions for Documents and Media's Home folder, navigate to Documents 
and Media and select 
*Options* 
(![Options](../../../images/icon-options.png)) 
&rarr; *Home Folder Permissions*. 

![Figure 3: Select which roles can add a document to a Documents and Media folder.](../../../images/screens-portal-permission-folder-add.png)

## Methods

<table class="table table-striped table-bordered">
<thead>
<tr>
<th>Method</th>
<th>Return Type</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td>load()</td>
<td>void</td>
<td>Starts the request to load the form. The form fields are shown when the response is received.</td>
</tr>
<tr>
<td>setDDMFormListener()</td>
<td>void</td>
<td>Sets the listener for this form.</td>
</tr>
</tbody>
</table>

## Listener

DDM Form Screenlet delegates some events to an object that implements to the 
`DDMFormListener` interface. This interface lets you implement the following 
methods:

`onFormLoaded(FormInstance formInstance)`: Called when the form instance 
successfully loads. 

`onError(Exception e)`: Called when an error occurs in the process. For 
example, this method is called when an error occurs while loading a form 
instance. 

`onDraftLoaded(FormInstanceRecord formInstanceRecord)`: Called when a draft is retored.

`onDraftSaved(FormInstanceRecord formInstanceRecord)`: Called when a draft is saved.

`onFormSubmitted(FormInstanceRecord formInstanceRecord)`: Called when a form is successfully submitted.
