

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
        *   [SMS By CSOM User Object](#sms-by-csom-user-object)
    *   [Send SMS to Multiple SharePoint Users](#-send-sms-to-multiple-sharepoint-users)
        *   [SMS By Username / Email id](#sms-by-username--email-id-1)
        *   [SMS By CSOM UserCollection Object](#sms-by-csom-usercollection-object)


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
using Microsoft.SharePoint.Client;
```

 <i class="icon-user"></i>Send SMS to SharePoint User
-------------

Once you have included the [references](#include-refrences) , you have the following options to chose from for sending SMS to your SharePoint Site User

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
using Microsoft.SharePoint.Client;
```
#### <i class="icon-user"></i>SMS By Username / Email id

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message  
/// to user of provided user id (loginame or email id)
/// </summary>
/// <param name="loginNameOrEmail">loginname or email id of the user</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToUser(this ClientContext context, string loginNameOrEmail, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

// Replace with a valid user email id from your SharePoint site
string emailid = "bob@foobar.onmicrosoft.com";

// Replace with a valid username from your SharePoint site
string username = "i:0#.f|membership|steve@foobar.onmicrosoft.com";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
    SMSRequestResult result = clientcontext.SendSMSToUser(emailid , smscontent);
    Console.WriteLine("Requested by email id, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
    
    SMSRequestResult result2 = clientcontext.SendSMSToUser(username , smscontent);
    Console.WriteLine("Requested by username, Result : {0} , Message: {1} "
    , result2.status.ToString(), result2.status_message);
}
```
#### <i class="icon-user"></i>SMS By CSOM User Object

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to user   
/// </summary>
/// <param name="user">SharePoint User Object</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToUser(this ClientContext context, User user, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
   // Get current logged in user
    clientContext.Load(clientContext.Web);
    clientContext.ExecuteQueryRetry();    
    User user = clientcontext.Web.CurrentUser;
    
    //Send SMS to User
    SMSRequestResult result = clientcontext.SendSMSToUser(user , smscontent);
    Console.WriteLine("Requested user object, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
     
}
```
 <i class="icon-user"></i>Send SMS to Multiple SharePoint Users
-------------

Once you have included the [references](#include-refrences) , you have the following options to chose from for sending SMS to your SharePoint Site Users

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
using Microsoft.SharePoint.Client;
```
#### <i class="icon-user"></i>SMS By Username / Email id

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message  
/// to users with provided user id(s) - loginame(s) or email id(s)
/// </summary>
/// <param name="loginNameOrEmail">A list/Array of loginname or email id of the users</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
///  recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToMultipleUsers(this ClientContext context, IEnumerable<string> loginNameOrEmail, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

// Replace with a valid user email id from your SharePoint site
string emailid = "bob@foobar.onmicrosoft.com";

// Replace with a valid username from your SharePoint site
string username = "i:0#.f|membership|steve@foobar.onmicrosoft.com";

// An Array of users
string[] userArray = { emailid, username };

// List of users
List<string> userList = userArray.ToList();

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
    // Send SMS to An array of users, 
    // create a new request for each user (batchsize = 1)   
    SMSRequestResult result = clientcontext.SendSMSToMultipleUsers(userArray , smscontent, 1);
    Console.WriteLine("Requested by array");
    Console.WriteLine("Result : {0}",result.status.ToString());
    Console.WriteLine("Result Message: {0} ", result.status_message);
    
    // Send SMS to An List of users, 
    // create a single request for every 25 users  (batchsize default value is 25)
    SMSRequestResult result2 = clientcontext.SendSMSToMultipleUsers(userList , smscontent);
    Console.WriteLine("Requested by list");
    Console.WriteLine("Result : {0}",result2.status.ToString());
    Console.WriteLine("Result Message: {0} ", result2.status_message);
```
#### <i class="icon-user"></i>SMS By CSOM UserCollection Object

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to user   
/// </summary>
/// <param name="users">SharePoint UserCollection Object</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
/// recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToMultipleUsers(this ClientContext context, UserCollection users, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
   // Get users from default memebers group
    clientContext.Load(clientContext.Web);
    clientContext.Load(clientContext.Web.AssociatedMemberGroup, i => i.Users);
    clientContext.ExecuteQueryRetry(); 
    
    //CSOM UserCollection object
    UserCollection users = clientContext.Web.AssociatedMemberGroup.Users;
    
    //Send SMS to Users
    SMSRequestResult result = clientcontext.SendSMSToMultipleUsers(users , smscontent);
    Console.WriteLine("Requested by usercollection object");
    Console.WriteLine("Result : {0}",result.status.ToString());
    Console.WriteLine("Result Message: {0} ", result.status_message);
     
}
```
