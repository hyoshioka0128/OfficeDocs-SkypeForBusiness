---
title: "Managing resource accounts in Teams"
ms.author: jambirk
author: jambirk
manager: serdars
ms.reviewer: jastark, wasseemh
ms.topic: article
ms.tgt.pltfrm: cloud
ms.service: msteams
search.appverid: MET150
ms.collection: Teams_ITAdmin_Help
ms.audience: Admin
appliesto:
- Microsoft Teams
localization_priority: Normal
f1keywords:
- ms.teamsadmincenter.orgwidesettings.resourceaccounts.overview
description: "Managing resource accounts in Microsoft Teams"
---

# Manage resource accounts in Teams 

A Resource Account is also known as a disabled user object in Azure Active Directory, and can be used to represent resources in general. In Exchange it might be used to represent conference rooms, for example, and allow them to have a phone number. A resource account can be homed in Microsoft 365 or on premises using Skype for Business server, and these accounts are created using Powershell commands.

In Microsoft Teams os Skype for Business Online, each call queue or auto attendant is required to have an associated resource account. Whether a resource account needs an assigned phone number will depend on the intended use of the associated call queue or auto attendant. Refer to the articles on call queues and auto attendants linked at the bottom of this article before assigning a phone number to a resource account.

## Prerequisites to assign a phone number to a resource account

To get started it's important to remember a few things:
  
- Your organization must have (at a minimum) an Enterprise E3 plus **Phone System** license or an Enterprise E5 license. The number of **Phone System** user licenses that are assigned affects the number of service numbers that are available to be used for resource accounts assigned to call queues or auto attendants. The number of resource accounts you can have is dependent on the number of **Phone System** and **Audio Conferencing** licenses that are assigned in your organization. To learn more about licensing, see [Skype for Business and Microsoft Teams add-on licensing](/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing.md).

    > [!NOTE]
    > To redirect calls to people in your organization who are Online, they must have a **Phone System** license and be enabled for Enterprise Voice or have Office 365 Calling Plans. See [Assign Skype for Business and Microsoft Teams licenses](/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/assign-skype-for-business-and-microsoft-teams-licenses.md). To enable them for Enterprise Voice, you can use Windows PowerShell. For example run:  `Set-CsUser -identity "Amos Marble" -EnterpriseVoiceEnabled $true`
  
- To learn more about Office 365 Calling Plans, see [What are Calling Plans in Office 365?](/microsoftteams/what-are-calling-plans-in-office-365) and [Calling Plans for Office 365](/microsoftteams/calling-plans-for-office-365).
- You can only assign toll and toll-free service phone numbers that you got in the **Microsoft Teams admin center** or transferred from another service provider to a resource account. To get and use toll-free service numbers, you need to set up Communications Credits.

> [!NOTE]
> User (subscriber) phone numbers can't be assigned to a resource account - only service toll or toll-free phone numbers can be used.

To assign a phone number to a resource account, you will need to get or transfer your existing toll or toll-free service numbers. After you get the toll or toll-free service phone numbers, they will show up in **Microsoft Teams admin center** > **Voice** > **Phone numbers**, and the **Number type** listed will be listed as **Service - Toll-Free**. To get your service numbers, see [Getting service phone numbers](/skypeforbusiness/what-is-phone-system-in-office-365/getting-service-phone-numbers.md) or if you want to transfer and existing service number, see [Transfer phone numbers to Office 365](/microsoftteams/transfer-phone-numbers-to-office-365).
  
> [!NOTE]
> If you are outside the United States, you can't use the Microsoft Teams admin center to get service numbers. Go to [Manage phone numbers for your organization](/microsoftteams/manage-phone-numbers-for-your-organization) instead to see how to do it from the outside of the United States.

## Create a resource account in Powershell

 You would create a resource account by running the appropriate Powershell cmdlet as needed (for one or more resource accounts), and give each one a name and so on. There is currently no option for creating a resource account in the Microsoft Teams admin center, but you can edit phone numbers and change the call queue or auto attendant assignments for a resource account.

Depending on whether your phone number is located online or on premises, you would need to connect to the appropriate Powershell prompt with Admin privileges.

- Hybrid implementations (numbers numbers homed on Direct Routing, OPCH and CCE) will use [New-CsHybridApplicationEndpoint](https://docs.microsoft.com/powershell/module/skype/new-cshybridapplicationendpoint?view=skype-ps) to create a resource account that is homed on premises.  
- Online only implementations will use [New-CsOnlineApplicationInstance](https://docs.microsoft.com/powershell/module/skype/new-CsOnlineApplicationInstance?view=skype-ps) to have a resource account that is homed online.

The following is an online environment example of creating a resource account:

``` Powershell
New-CsOnlineApplicationInstance -DisplayName Node1 -SipAddress sip:node1@litwareinc.com -OU "ou=Redmond,dc=litwareinc,dc=com"
```

See [New-CsOnlineApplicationInstance](https://docs.microsoft.com/powershell/module/skype/new-csonlineapplicationinstance?view=skype-ps) for more details on this command.

> [!NOTE]
> It's easiest to set the phone number using the Microsoft Teams admin center, as described in the next section. You can also use the `Set-CsOnlineApplicationInstance` command to a assign a phone number to the resource account after its initial creation as shown:

``` Powershell
Set-CsOnlineApplicationInstance -Identity "CN={4f6c99fe-7999-4088-ac4d-e88e0b3d3820},OU=Redmond,DC=litwareinc,DC=com" -DisplayName Node1 -LineURI tel:+14255550100
```

See [Set-CsOnlineApplicationInstance](https://docs.microsoft.com/powershell/module/skype/set-csonlineapplicationinstance?view=skype-ps) for more details on this command.

## Manage Resource account settings in Microsoft Teams admin center

To manage Resource account settings in Microsoft Teams admin center, navigate to **Org-wide settings**  > **Resource accounts**, select the resource account you need to change settings for, and then click on the **Edit** button. in the **Edit resource account** screen, you will be able to change the:

- **Display name** for the account
- Call queue or auto attendant that uses the account
- Phone number assigned to the account

When finished, click on **Save**.

## Related Information

For implementations that are hybrid with Skype for Business Server:

[Plan Cloud auto attendant](/SkypeForBusiness/hybrid/plan-cloud-auto-attendant)

[Configure Cloud auto attendants](/SkypeForBusiness/hybrid/configure-cloud-auto-attendant.md)

For implementations in Teams or Skype for Business Online:

[What are Phone System auto attendants?](what-are-phone-system-auto-attendants.md)

[Set up a Phone System auto attendant](/SkypeForBusiness/what-is-phone-system-in-office-365/set-up-a-phone-system-auto-attendant.md)

[Small business example - Set up an auto attendant](https://docs.microsoft.com/en-us/SkypeForBusiness/what-is-phone-system-in-office-365/tutorial-org-aa)

[Create a Phone System call queue](/SkypeForBusiness/what-is-phone-system-in-office-365/create-a-phone-system-call-queue.md)

[New-CsHybridApplicationEndpoint](https://docs.microsoft.com/powershell/module/skype/new-cshybridapplicationendpoint?view=skype-ps)

[New-CsOnlineApplicationInstance](https://docs.microsoft.com/powershell/module/skype/new-csonlineapplicationinstance?view=skype-ps)
