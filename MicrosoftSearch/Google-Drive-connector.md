--- 

title: "Google Drive Microsoft Graph connector" 
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
description: "Set up the Google Drive Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot" 
ms.date: 09/19/2024
---

# Google Drive Microsoft Graph connector 

With the Microsoft Graph connector, your organization in Microsoft 365 can index files that are accessible to anyone in Google Drive, using Microsoft 365 Copilot and Microsoft Search. 

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors Microsoft Graph Google Drive connectors. 


## Capabilities
- Access Google Drive files using the power of semantic search
- Retain ACLs defined by your organization
- Customize your crawl frequency
- Create workflows using this connection and plugins from Microsoft Copilot Studio

## Limitations
- Folder replies & comments aren't indexable 

## Prerequisites
Before you create a Microsoft Graph Google Drive connector, you must:

### 1. Be a Google Workspace super admin role or be granted access
Either be granted access by a super admin role or are a user with administrative privileges. You do not need a super admin role for yourself if you have been granted access by a super admin role.

### 2. Configure Google Drive Service Account connection credentials
Configured Google Drive Service Account connection credentials containing your admin account email, client email (service account email), and private key. For more information, see [Create and delete service account keys](https://cloud.google.com/iam/docs/keys-create-delete).

### 3. Create a Google Cloud Service Account
Create a Google Cloud Service Account (an account with delegated authority to assume a user identity) with Enable G Suite Domain-wide Delegation activated for server-to-server authentication, and then generate a JSON private key using the account. For more information, see [Create service accounts](https://cloud.google.com/iam/docs/service-accounts-create).

### 4. Add required APIs to your user account
Add Admin SDK API and Google Drive API to your user account.

### 5. Add the OAuth scopes to your service account
Add (or ask a user with a super admin role to add) the following OAuth scopes to your service account using a super admin role. These API scopes are needed to crawl all documents, and access control (ACL) information for all users in a Google Workspace domain:

`https://www.googleapis.com/auth/admin.directory.user.readonly`

`https://www.googleapis.com/auth/admin.directory.group.readonly`

`https://www.googleapis.com/auth/drive.readonly`   


## Setup

### 1. Display name   
A display name is used to identify each reference in Copilot, helping users easily recognize the associated file or item. The display name also represents trusted content.

### 2. Google Apps domain
To sign up for Google Workspace, you need an internet domain name, like your-company.com. This domain can host a website (`www.your-company.com`) and email (`info@your-company.com`). For more information, see [What is a domain?](https://support.google.com/a/answer/177483?hl=en&ref_topic=3540977&sjid=7603839478714180429-AP).

### 3. Google Apps administrator account email
Enter the email of a Google Apps administrator account in the `user@company.com` format.

### 4. Rollout to a limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other search surfaces before expanding the rollout to a broader audience.

For other settings, like Access permissions, Data inclusion rules, Schema, Crawl frequency, etc., we set defaults based on what works best with data in Google Drive. The default values settings are as follows.

**Page** | **Settings** | **Default values**
--- | ---- | ---
Users | Access Permissions | All files that are accessible to anyone in Google Drive are visible to all Microsoft 365 users in your tenant, from Microsoft Search or Microsoft 365 Copilot.
Content | Index Content | All published posts and pages are selected by default.
Content | Manage Properties | To check default properties and their schema, [click here](#content).
Sync | Incremental Crawl | Frequency: Every 15 mins
Sync | Full crawl | Frequency: Every day

If you want to edit any of these values, you need to choose **Custom setup**.

## Custom setup 

Custom setup is for those admins who want to edit the default values for settings. Once you click **Custom setup** , you should see three other tabs – Users, Content, and Sync. 

### Users 

**Access permissions**

The Google Drive Graph connector supports data visible to Only people with access to this data source (recommended) or Everyone. If you choose Everyone, indexed data appears in the search results for all users. 

If you choose Only people with access to this data source, you need to further choose whether your  has Microsoft Entra ID provisioned users or non-AAD users. 

To identify which option is suitable for your organization: 

1. Choose the **Microsoft Entra ID** option if the email ID of Google Drive users is same as the UserPrincipalName (UPN) of users in Microsoft Entra ID. 

2. Choose the **non-AAD** option if the email ID of Google Drive users is **different** from the UserPrincipalName (UPN) of users in Microsoft Entra ID.

>[!Important]
>- If you choose Microsoft Entra ID as the type of identity source, the connector maps the email IDs of users obtained from Google Drive directly to UPN property from Microsoft Entra ID.
>- If you chose "non-AAD" for the identity type see Map your non-Azure AD Identities for instructions on mapping the identities. You can use this option to provide the mapping regular expression from email ID to UPN.
>- Updates to users or groups governing access permissions are synced in full crawls only. Incremental crawls do not currently support the processing of updates to permissions. 

### Content 

**Manage properties**

Here, you can add or remove available properties from your Google Drive data source. Assign a schema to the property (define whether a property is **searchable, queryable, retrievable or refinable**), change the semantic label, and add an alias to the property. Properties that are selected by default are:

|Source property|Label|Description|Schema|
|--- | ---- | --- | ---|
|createdTime | Created date time | The time at which the file was created.  | Search, Query, Retrieve.|
|description |  | A short description of the file.| 
|fileExtension | File extension | The final component of fullFileExtension. This parameter is only available for files with binary content in Google Drive.  | Query, Refine, Retrieve.|
|fileType |  | The type of the file.  | 
|iconLink | IconUrl | A static, unauthenticated link to the file's icon.  | Retrieve.|
|lastModifingUser | lastModifiedDateTime | The last user to modify the file. This field is only populated when the last modification was performed by a signed-in user.  | Query, Retrieve, Search.|
|modifiedTime	| Last modified by | The last time the file was modified by anyone (RFC 3339 date-time).  | Query, Retrieve, Search.|
|link | url | A link for opening the file in a relevant Google editor or viewer in a browser.  | Retrieve.|
|name | File Name | The name of the file.  | Query, Retrieve, Search.|
|owner | Created by | The owner of this file. Only certain legacy files may have more than one owner. This field isn't populated for items in shared drives.  | Search, Query, Retrieve.|
|parentFolderLink |  |  A link for the parent folder containing the file.  | Retrieve.|
|parentFolderName |  | The name of the parent folder containing the file.  | Search, Query, Retrieve.|
|size |  | Size in bytes of blobs and first-party editor files.  | Search, Query, Retrieve.|


### Sync 

You can configure full and incremental crawls based on the scheduling options present here. By default, incremental crawl is set for every 15 minutes, and full crawl is set for every day. If needed, you can adjust these schedules to fit your data refresh needs.

## Troubleshooting

### Invalid credentials detected. Check the credential info and check the permissions of the service account.
This error occurs when the service account lacks the necessary permissions for Google Drive access. Check the credentials info of the account and ensure that they are correctly filled in on the setup page. 

### The required permissions for users/files are missing.
Authentication error, one or more required OAuth scopes to your service account are missing. Your service account must include both API scopes:

`https://www.googleapis.com/auth/admin.directory.user.readonly` 

`https://www.googleapis.com/auth/drive.readonly`

`https://www.googleapis.com/auth/admin.directory.group.readonly`

### Failed to capture file information. Ensure the workspace is not empty and has files accessible to the admin.
 During the connector setup, at least one file must be present in your organization's workspace to test the connection successfully.

## What's next

After publishing your connection, you can review the status under the **Data sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).

If you have any other issues or want to provide feedback, reach out to us at [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
