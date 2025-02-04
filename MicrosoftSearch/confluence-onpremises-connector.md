---
ms.date: 11/28/2024
title: "Confluence On-premises Microsoft Graph connector (preview)"
ms.author: mansipakhale
author: Mansipakhale10
manager: harshkum
audience: Admin
ms.audience: Admin
ms.topic: article
ms.service: mssearch
ms.localizationpriority: high
search.appverid:
- BFB160
- MET150
- MOE150
description: "Set up the Confluence On-premises Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot"
---

# Confluence On-premises Microsoft Graph Connector

The Confluence On-premises Microsoft Graph connector allows your organization to index Confluence server or data center content. After you configure the connector and index data from the Confluence site, end users can search for those contents in Microsoft Search and Microsoft 365 Copilot.

This article is intended for Microsoft 365 administrators and who are responsible for configuring, running, and monitoring the Confluence On-premises Microsoft Graph connector. It supplements the general instructions provided in setup Microsoft Graph connectors in the Microsoft 365 admin center.

## Benefits
- **Enhanced Search Capabilities**: Users can ask natural language questions about Wiki content in Copilot, such as:
   - Summarize the architecture document </br>
   - How to get access to a portal </br>
- **Semantic Search Support**: Users can perform natural language queries for accurate responses. </br>
- **Compatibility**: Supports Confluence versions above 8.0.

## Prerequisites
1. **Install the GCA [Graph connector agent]**: Ensure that the GCA is installed on a Windows machine within the same network as the data source, accessible via the Confluence URL. You can find more information [Microsoft Graph connector agent](./graph-connector-agent.md)
2. **Install plugin**: Download and install the confluence on-prem plugin from Atlassian marketplace on your confluence setup. Get the plugin from [Microsoft Graph Connectors Confluence On-prem Plugin | Atlassian Marketplace](https://marketplace.atlassian.com/apps/1234846?tab=reviews&hosting=datacenter)
3. **Authentication**: Ensure that you have authentication credentials with right access. 

>[!IMPORTANT]
   >
   > **Recommended : The Confluence Global Administrator should create the connection** </br>
   > 
   > **Who is the confluence global administrator?**
   > A Confluence Administrator is a user who has full administrative permissions. </br>
   > * To check permissions: Go to **Administration** > **General Configuration** > **Global Permissions** </br>
   > * Look for the group **Confluence-administrators**, which has all permissions enabled, including - Can Use, Personal Space, Create Space, Confluence Administrator, and System Administrator.
   > * Any user creating a token must be a member of this group

## Get Started

### 1. Display name

A Display name is used to identify each reference in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. Display name is also used as a [content source filter](./custom-filters.md#content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. Confluence on-premises URL

To connect to your Confluence On-premises data, you need your organization's Confluence instance URL. Your organization's Confluence instance URL typically looks like 'https://contoso.atlassian.net'.

### 3. Graph connector agent (GCA)

To index your Confluence server or data center content, you must install and register the connector agent. See [Install the Microsoft Graph connector agent](./graph-connector-agent.md) for details. You must be the administrator for your organization's Microsoft 365 tenant and the administrator for your organization's Confluence site.

>[!NOTE]
> GCA can be installed on a different Windows machine and need not be on the same machine as the On-premises server. The machine can help generate App ID and secret which can be used for the setup. You must ensure that the GCA machine is on during the crawling. 
> You may find answers to common GCA related questions in [FAQ section](./frequently-asked-questions.md) .

### 4. Install the Confluence on-premises plugin

Verify that the Microsoft Graph Connectors Confluence On-prem Plugin is installed. You do not need to install the plugin for each confluence connector; if it's already installed in your Confluence instance, you can skip this step for subsequent Confluence on-prem connections.

   - Download the app from [Microsoft Graph Connectors Confluence On-prem Plugin | Atlassian Marketplace](https://marketplace.atlassian.com/apps/1234846?tab=overview&hosting=datacenter).
   - Log in to your confluence system
   - Click on settings icon -> Click on manage apps
[![Screnshot that shows clicking on settings icon -> clicking on manage apps.](https://github.com/user-attachments/assets/16a6a8f0-844e-49bd-9939-694d8741eaea)](https://github.com/user-attachments/assets/16a6a8f0-844e-49bd-9939-694d8741eaea#lightbox)
   - Click on upload app
[![Screnshot that shows clicking on upload app](https://github.com/user-attachments/assets/e1216fef-7c35-4e87-a1ad-4aef7062fd1a)](https://github.com/user-attachments/assets/e1216fef-7c35-4e87-a1ad-4aef7062fd1a#lightbox)
   - Choose the downloaded file and proceed
[![Screnshot that shows Plugin sussuesfully installed)](https://github.com/user-attachments/assets/58ba9e9e-e2c9-47e9-967d-401cd79a7c5d)](https://github.com/user-attachments/assets/58ba9e9e-e2c9-47e9-967d-401cd79a7c5d#lightbox)

>[!NOTE]
>Plugin is supported for confluence version above 8.0.

### 5. Authentication type

To authenticate and synchronize content from Confluence On-prem, choose **one of three** supported methods:<br>

   **a. Basic authentication** <br>
   To authenticate to your instance, enter the username and password of Confluence account. <br>

   **b. OAuth1.0a** <br>
   Generate a public/private key pair and create an application link in the Confluence On-premises site so that the connector agent can access the instance. To learn more, see [step 1 in Atlassian developer documentation](https://developer.atlassian.com/server/jira/platform/oauth/#step-1--configure-jira) on how to configure OAuth 1.0a. <br>
 
   **c. OAuth 2.0 (recommended)** <br> 
   The following steps provide guidance on how to register the app [Configure an incoming link](https://confluence.atlassian.com/doc/configure-an-incoming-link-1115674733.html). 

  1. Go to Administration  > General configuration > Application links. 
  2. Select Create link
  3. Select External application, and then choose Incoming as the direction. 
  4. Fill in the 
   - Redirect URL: `https://gcs.office.com/v1.0/admin/oauth/callback` 
   - Scope: </br>
     a) If you are a Confluence Global Administrator, either of the following scopes is supported: **Admin** or **Read**. </br>
     b) If you are any other type of Confluence Administrator, provide the **Admin** scope </br>
  5. Complete the connection settings step using the client ID and secret. 

### 6. Rollout to limited audience

Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience. To know more about limited rollout, click [here](./staged-rollout-for-graph-connectors.md).

At this point, you are ready to create the connection for ServiceNow Knowledge. You can click on the "Create" button and the Microsoft Graph connector starts indexing page from your Confluence account.

For other settings, like Access Permissions, Data inclusion rules, Schema, Crawl frequency etc., We set defaults based on what works best with Confluence data. The default values are as follows:

|**Users** |&nbsp;|
|----|---|
|Access permissions|_Only people with access to content in Data source._|
|Map Identities|_Data source identities mapped using Microsoft Entra IDs._|

|**Content**|&nbsp;|
|---|---|
|Include/Exclude space|_All_|
|Manage Properties|_To check default properties and their schema, click here_|

|**Synchronization**|&nbsp;|
|---|---|
|Incremental Crawl|_Frequency: Every 15 mins_|
|Full Crawl|_Frequency: Every Day_|

If you want to edit any of these values, you need to choose the `Custom Setup` option.

## Custom Setup

Custom setup is for those admins who want to edit the default values for settings listed in the default table. Once you click on the `Custom Setup` option, you see three more tabs – Users, Content, and Sync.

### Users

**Access Permissions**

The Confluence On-premises Microsoft Graph connector supports search permissions visible to Everyone or Only people with access to this data source. If you choose Everyone, indexed data appears in the search results for all users. If you choose Only people with access to this data source, indexed data appears in the search results for users who have access to them. 
In Confluence On-premises, security permissions for users and groups are defined using space permissions and page restrictions. The Confluence On-premises Microsoft Graph connector applies effective permissions provided by [Content restrictions API](https://docs.atlassian.com/ConfluenceServer/rest/7.15.0/#api/content/%7Bid%7D/restriction)

If you choose Only people with access to this data source, you need to further choose whether your Confluence site has Microsoft Entra ID provisioned users or non-AAD users.

To identify which option is suitable for your organization:
1. Choose the **Microsoft Entra ID** option if the email ID of Confluence users is **same** as the UserPrincipalName (UPN) of users in Microsoft Entra ID.
2. Choose the **non-AAD** option if the email ID of Confluence users is **different** from the UserPrincipalName (UPN) of users in Microsoft Entra ID. 

>[!IMPORTANT]
   >
   > * If you choose Microsoft Entra ID as the type of identity source, the connector maps the email IDs of users obtained from Confluence directly to UPN property from Microsoft Entra ID.
   > * If you chose "non-AAD" for the identity type, see [Map your non-Azure AD Identities](map-non-aad.md) for instructions on mapping the identities. You can use this option to provide the mapping regular expression from email ID to UPN.
   > * Updates to users or groups governing access permissions are synced in full crawls only. Incremental crawls do not currently support the processing of updates to permissions.

### Content

**Include or exclude data which you want to index**

In this step, you can add or remove available properties from your Confluence data source. Microsoft 365 selects a few properties.
By default, the connector will index all spaces. However, you can choose to include or exclude specific spaces using the space filter option. 

Enter the list of space keys you wish to include or exclude, and you see a preview of the results. Enter Space key: Each Confluence space has a space key, which is a short, unique identifier that forms part of the URL for that space. To get the space key, contact confluence admin. For more information visit: [Space Keys](https://confluence.atlassian.com/doc/space-keys-829076188.html)

**Page filter**

In this step, you can specify the date range for indexing your documents. The parameters are as follows:
- Last Created Date: The date when a document was created. Only documents modified after this date are indexed.
- Last Modified Date: The date when a document was modified. Only documents modified after this date are indexed.

>[!IMPORTANT]
   >
   > * Ensure that the dates are in chronological order. The "Last Created Date" should not be later than the "Last Modified Date."
   > * If no dates are specified, all documents are considered for indexing.

**Manage Properties**

Here, you can add or remove available properties from your Confluence On-prem data source. Assign a schema to the property (define whether a property is searchable, queryable, retrievable or refinable). Additionally you can change the semantic label and add an alias to the property. Default properties are as follows:

|Source Property|Label|Schema|
|----|----|----|
|Author|authors|Query, Retrieve|
|Content| |Search|
|CreatedByName|Created by|Search, Query, Retrieve|
|CreatedOn|Created date time|Query, Retrieve|
|Id| |Query, Retrieve|
|PageTree| |Retrieve|
|SpaceName| |Search, Query, Retrieve|
|Title|title|Search, Retrieve|
|UpdatedByName|lastModifiedBy|Retrieve|
|UpdatedOn|lastModifiedDateTime|Query, Retrieve, Refine|
|URL|url|Retrieve|

**Preview Data**

Use the preview results button to verify selected properties and filters.
>[!NOTE]
   > The preview only respects space-level filtering.

### Synchronization

The refresh interval determines how often your data is synchronized between the data source and the Graph connector index. There are two types of refresh intervals – full crawl and incremental crawl. For more details, click [here](configure-connector.md#guidelines-for-sync-settings).
You can change the default values of refresh interval from here if you want to.

### Review and Test your connection

- For testing, you can choose [publish to limited audience](./staged-rollout-for-graph-connectors.md#modify-or-stop-staged-rollout).
- Search and validate your indexed content and permissions using [Index browser](./connectors-index-search.md).
- Find answers to common questions in our [FAQ section](./frequently-asked-questions.md).

For MS Search, if you need to customize the search results page. To learn about customizing search results, see [Customize the search results page](customize-search-page.md).

## Limitations
- Doesn't index blogs, attachment files, or comments.
- Only indexes current pages; archived pages are excluded.
- CQL (Confluence Query Language) isn't supported for Confluence on-premises however we support a space and page level filter

## Troubleshooting
After publishing your connection, you can review the status under the **Data sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).
You can find troubleshooting steps for commonly seen issues [here](troubleshoot-confluence-onpremises-connector.md).

If you have issues or want to provide feedback, contact [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
