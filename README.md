

SMS For Office (SMS Via SharePoint List) - CSOM Extensions!
============================================================================

This is the main repository for the SMS For Office Add-in extensions for SharePoint.

Table of contents
-------------

*   [SMS For Office (SMS Via SharePoint List) - CSOM Extensions!](#)
    *   [About](#about)
    *   [Getting Started](#getting-started)
        *   [Install from NUGET](#install-from-nuget)
        *   [Include Refrences](#include-refrences)
    *   [Send SMS to SharePoint User](#-send-sms-to-sharepoint-user)
        *   [SMS By Username / Email id](#sms-by-username--email-id)
        *   [Include Refrences](#include-refrences)



<i class="icon-help-circled"></i> About
-------------

SMS For Office is SharePoint Add-in available in Microsoft SharePoint Store, that allows easy SMS integration in SharePoint Site. With zero manual configuration, the add-in provisions a SharePoint List which is used to Send SMS request. This repository intends to achieve a community driven repository of implementations extensions and samples of the Add-in.

•	Official Website: https://timeparity.com/sms-sharepoint 

•	On SharePoint Store: https://store.office.com/en-us/app.aspx?assetid=WA104380716 
   
> **Note:**

> - Please note this github repository is intended for developers to integrate the add-in programmatically in applications.
> - If you are looking for tutorial to send SMS from SharePoint Site, or need help with Installation refer the tutorial site here :    https://smsappsharepoint.wordpress.com/ 

![Sending SMS from SharePoint Online / SharePoint 2016.](https://timeparity.com//img/sp_smsrequestlist_additem_512x384.png)



Getting Started
-------------

### Install from NUGET

The Library is available in on Nuget, please get the latest version depending on your SharePoint Environment:

>**SharePoint Online**

Nuget:  [SharePoint Online SMS Integration](https://www.nuget.org/packages/TimeParity.SharePointOnline.SMS)
```powershell
PM> Install-Package TimeParity.SharePointOnline.SMS
```


>**SharePoint 2016**

Nuget:  [SharePoint 2016 SMS Integration](https://www.nuget.org/packages/TimeParity.SharePoint2016.SMS)
```powershell
PM> Install-Package TimeParity.SharePoint2016.SMS
```

### Include refrences

Include the following refrences in your .Net Application:

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
```

 <i class="icon-user"></i>Send SMS to SharePoint User
-------------

Once you have included the [references](#include-refrences) , you have the following options to chose from for sending SMS to your SharePoint Site User

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
```
#### <i class="icon-user"></i>SMS By Username / Email id

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to user of provided user id (loginame or email id)
/// </summary>
/// <param name="loginNameOrEmail">loginname or email id of the user</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToUser(this ClientContext context, string loginNameOrEmail, 
string message, string title = "Custom")
