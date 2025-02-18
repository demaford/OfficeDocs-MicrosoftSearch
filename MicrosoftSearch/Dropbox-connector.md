--- 

title: "Dropbox Graph connector for Microsoft Search and Copilot" 
ms.author: anggao
author: ms-anggao
manager: jecui
audience: Admin
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: Medium 
search.appverid: 
- BFB160 
- MET150 
- MOE150 
description: "Set up the Dropbox Graph connector for Microsoft Search and Copilot" 
ms.date: 11/26/2024
---

# Dropbox Graph connector (Preview)

With the Microsoft Graph connector, your organization in M365 can index files that are accessible to anyone in Dropbox, using Microsoft Copilot and Search. 

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors Dropbox connector. 

>[!NOTE]
>The Dropbox connector is in preview. If you wish to get early access to try it, sign up using [this form](https://forms.office.com/r/JniPmK5bzm).

## Capabilities
- Access Dropbox files using the power of Semantic search
- Retain ACLs defined by your organization
- Customize your crawl frequency
- Create workflows using this connection and plugins from Microsoft Copilot Studio


## Limitations
- Folder, replies & comments aren't indexable 

## Prerequisites
Before you create a Dropbox connector, you must:
### 1. Set up a team admin user
Created a Dropbox account  for business and set up a team admin user.

### 2. Configure a Dropbox app
Configured a Dropbox app with a unique App name, activated Scoped Access and Full Dropbox permissions. See [Dropbox documentation on creating an app](https://www.dropbox.com/developers/reference/getting-started#app%20console).

### 3. Add direct URLs 
Add the following links into the filed “Redirect URLs” in the section OAuth 2 of the setting tab in the Dropbox app console: 

For M365 Enterprise, copy and paste: `https://gcs.office.com/v1.0/admin/oauth/callback`

For M365 Government, copy and paste: `https://gcsgcc.office.com/v1.0/admin/oauth/callback` 

### 4. Add API Scopes 
Navigate to Permissions tab on the Dropbox app console and added the following permissions:

#### Individual Scopes: 
`files.metadata.read`

`files.content.read`

`sharing.read`

`file_requests.read`

#### Team Scopes: 
`team_info.read`

`team_data.member`

`team_data.governance.write`

`team_data.governance.read`

`team_data.content.read`

`files.team_metadata.read`

`members.read`

`groups.read`

`events.read`

### 5. Get App key and App secret
Navigate to the Settings  tab from the navigation pane on the left to get the App key and App secret from this page.


## Set up

### 1. Display name   
Choose a display name that helps users easily recognize associated files or items in a Copilot response.

### 2. Authentication type
Enter the app key and app secret you obtained from your Dropbox app  console. 

### 3. Rollout to limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other search surfaces before expanding the rollout to a broader audience.

For other settings, like Access Permissions, Data inclusion rules, Schema, Crawl frequency etc., we set defaults based on what works best with data in Dropbox. The default values settings are as follows.

**Page** | **Settings** | **Default Values**
--- | ---- | ---
Users | Access Permissions | All files that are accessible to anyone in Dropbox are visible to all Microsoft 365 users in your tenant, from Microsoft Search or Copilot.
Content | Index Content | All published posts and pages are selected by default.
Content | Manage Properties | To check default properties and their schema, [click here](#Content).
Sync | Incremental Crawl | Frequency: Every 4 hours
Sync | Full crawl | Frequency: Every day

If you want to edit any of these values, you need to choose the **Custom Setup** option. 

## Custom Setup 

Custom setup is for those admins who want to edit the default values for settings. Once you click on the ‘Custom Setup’ option, you should see three other tabs – Users, Content, and Sync. 

### Users 

**Access permissions**

The Dropbox Graph connector supports data visible to Only people with access to this data source (recommended) or Everyone. If you choose Everyone, indexed data appears in the search results for all users. 

If you choose Only people with access to this data source, you need to further choose whether your  has Microsoft Entra ID provisioned users or non-AAD users. 

To identify which option is suitable for your organization: 

1. Choose the **Microsoft Entra ID** option if the email ID of Dropbox users is same as the UserPrincipalName (UPN) of users in Microsoft Entra ID. 

2. Choose the **non-AAD** option if the email ID of Dropbox users is **different** from the UserPrincipalName (UPN) of users in Microsoft Entra ID.

>[!Important]
>- If you choose Microsoft Entra ID as the type of identity source, the connector maps the email IDs of users obtained from Dropbox directly to UPN property from Microsoft Entra ID.
>- If you chose "non-AAD" for the identity type see Map your non-Azure AD Identities for instructions on mapping the identities. You can use this option to provide the mapping regular expression from email ID to UPN.
>- Updates to users or groups governing access permissions are synced in full crawls only. Incremental crawls do not currently support the processing of updates to permissions. 

### **Content** 

**Manage Properties**

Here, you can add or remove available properties from your Dropbox data source. Assign a schema to the property (define whether a property is **searchable, queryable, retrievable or refinable**), change the semantic label and add an alias to the property. Properties that are selected by default are:

**Source Property** | **Label** |**Description**| **Schema**
--- | ---- | --- | ---
CreatedBy | Created by | The email address of the creator  | Query, Retrieve, Search
FileExtension | File extension | The file name extension.  | Query, Refine, Retrieve
IconUrl | IconUrl | The URL of the icon. | Retrieve
LastModifiedBy | Last modified date time  | The email address of the user who last modified this file.  | Query, Retrieve, Search
LastModifiedTime | lastModifiedDateTime | The last user to modify the file. This field is only populated when the last modification was performed by a signed-in user.  | Query, Refine, Retrieve
Name	| File name | The file name  | Query, Retrieve, Search
PreviewUrl | url | URL for displaying a web preview of the shared file.   | Retrieve
Size |  |The file size in bytes. | 


### Sync 

You can configure full and incremental crawls based on the scheduling options present here. By default, incremental crawl is set for every 4 hours, and full crawl is set for every day. If needed, you can adjust these schedules to fit your data refresh needs.

## Troubleshooting
### 1. Required permission scopes are missing. Please ensure the necessary scopes are selected in the Dropbox App.
Lack of the required permission scopes, please make sure you have selected all the scopes in Dropbox app console:

#### Individual Scopes: 
`files.metadata.read`

`files.content.read`

`sharing.read`

`file_requests.read`

#### Team Scopes: 
`team_info.read`

`team_data.member`

`team_data.governance.write`

`team_data.governance.read`

`team_data.content.read`

`files.team_metadata.read`

`members.read`

`groups.read`

`events.read`

### 2. OAuth 2.0 flow failed. Please verify the credential information and ensure the Dropbox App is configured with the correct settings.
Common authentication error. Please go back to the Dropbox app console and check if the OAuth2 in the setting tab is correctly configured.

### 3. OAuth 2.0 flow failed. Please confirm that the Dropbox user associated with this team access token holds the team admin role and is an active user.
Common authentication error.  Please go back to the Dropbox app console and check if the creator has an admin role and the account status is active.

### 4. Your security credentials have expired for this session. Please go back and sign in again with your App key and App secret.
Credential info has expired. Please refresh the Dropbox app console and copy the latest App key and App secret from the setting tab to authenticate.

### 5. Invalid Credentials detected. Please check the credential info and check the permission scopes of the Dropbox App.
Common credential error. Please go back to the Dropbox App console and check if the scopes in the permission tab are correctly configured.



















