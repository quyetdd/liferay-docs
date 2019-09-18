---
header-id: blogs-entry-display-screenlet-for-ios
---

# Blogs Entry Display Screenlet for iOS

[TOC levels=1-4]

## Requirements

- Xcode 9.3 or above
- iOS 11 SDK
- Liferay Portal 6.2 CE/EE, Liferay CE Portal 7.0/7.1, Liferay DXP
- Liferay Screens Compatibility app
  ([CE](http://www.liferay.com/marketplace/-/mp/application/54365664) or 
  [EE/DXP](http://www.liferay.com/marketplace/-/mp/application/54369726)). 
  This app is preinstalled in Liferay CE Portal 7.0/7.1 and Liferay DXP. 

## Compatibility

- iOS 9 and above

## Xamarin Requirements

- Visual Studio 7.2
- Mono .NET framework 5.4.1.6

## Features

Blogs Entry Display Screenlet displays a single blog entry. Image Display 
Screenlet renders any header image the blogs entry may have. 

## JSON Services Used

Screenlets in Liferay Screens call JSON web services in the portal. This 
Screenlet calls the following services and methods.

| Service | Method | Notes |
| ------- | ------ | ----- |
| `ScreensassetentryService` (Screens compatibility plugin) | `getAssetEntry` | With `entryId` |
| `ScreensassetentryService` (Screens compatibility plugin) | `getAssetEntry` | With `classPK` and `className` |
| `ScreensassetentryService` (Screens compatibility plugin) | `getAssetEntries` | With `entryQuery` |
| `ScreensassetentryService` (Screens compatibility plugin) | `getAssetEntries` | With `companyId`, `groupId`, and `portletItemName` |

## Module

- None

## Themes

- Default

The Default Theme can use different components to show a blogs entry 
(`BlogsEntry`). For example, it uses `UILabel` to show a blog's text, and 
[User Portrait Screenlet](/docs/7-1/reference/-/knowledge_base/r/userportraitscreenlet-for-ios) 
to show the profile picture of the Liferay user who posted it. Note that other 
Themes may use different components. 

![Figure 1: Blogs Entry Display Screenlet using the Default Theme.](../../../images/screens-ios-blogsentrydisplay.png)

## Offline

This Screenlet supports offline mode so it can function without a network 
connection. For more information on how offline mode works, see the 
[tutorial on its architecture](/docs/7-1/tutorials/-/knowledge_base/t/architecture-of-offline-mode-in-liferay-screens). 
Here are the offline mode policies that you can use with this Screenlet: 

| Policy | What happens | When to use |
|--------|--------------|-------------|
| `remote-only` | The Screenlet loads the data from the Liferay instance. If a connection issue occurs, the Screenlet uses the listener to notify the developer about the error. If the Screenlet successfully loads the data, it stores it in the local cache for later use. | Use this policy when you always need to show updated data, and show nothing when there's no connection. |
| `cache-only` | The Screenlet loads the data from the local cache. If the data isn't there, the Screenlet uses the listener to notify the developer about the error. | Use this policy when you always need to show local data, without retrieving remote data under any circumstance. |
| `remote-first` | The Screenlet loads the data from the Liferay instance. If this succeeds, the Screenlet shows the data to the user and stores it in the local cache for later use. If a connection issue occurs, the Screenlet retrieves the data from the local cache. If the data doesn't exist there, the Screenlet uses the listener to notify the developer about the error. | Use this policy to show the most recent version of the data when connected, but show an outdated version when there's no connection. |
| `cache-first` | The Screenlet loads the data from the local cache. If the data isn't there, the Screenlet requests it from the Liferay instance and notifies the developer about any errors that occur (including connectivity errors). | Use this policy to save bandwidth and loading time in case you have local (but probably outdated) data. |

## Required Attributes

- `assetEntryId` or `classPK`

## Attributes

| Attribute | Data type | Explanation |
|-----------|-----------|-------------|
| `assetEntryId` | `number` | The primary key of the blog entry (`BlogsEntry`). |
| `classPK` | `number` | The `BlogsEntry` object's unique identifier. |
| `autoLoad` | `boolean` | Whether the blog entry automatically loads when the Screenlet appears in the app's UI. The default value is `true`. |
| `offlinePolicy` | `string` | The offline mode setting. The default value is `remote-first`. See [the Offline section](/docs/7-1/reference/-/knowledge_base/r/blogs-entry-display-screenlet-for-ios#offline) for details. |

## Delegate

Blogs Entry Display Screenlet delegates some events to an object that conforms 
to the `BlogsEntryDisplayScreenletDelegate` protocol. This protocol lets you 
implement the following methods: 

- `- screenlet:onBlogEntryResponse:`: Called when the Screenlet receives the 
  `BlogsEntry` object. 

- `- screenlet:onBlogEntryError:`: Called when an error occurs in the process. 
  The `NSError` object describes the error. 
