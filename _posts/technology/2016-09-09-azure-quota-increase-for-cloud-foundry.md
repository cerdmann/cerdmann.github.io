---
layout: post
title: How to request an Azure quota increase for Pivotal Cloud Foundry
excerpt: "Pivotal Cloud Foundry requires a bump in quota on Azure. These steps will help you request the correct quota."
author: erds
categories: technology
tags:
  - Pivotal Cloud Foundry
  - Azure
image:
  feature: cross-cut-image-1.jpg
  thumb: thumbnail-cross-cut-image-1.jpg
comments: false
share: true
---

Pivotal Cloud Foundry (PCF) needs increased resources on your Azure subscription. When installing PCF for the first time, these steps should help you get the quote increase correct.

> Please note that in order to request a quota increase, you will need to have a Pay-As-You-Go account. If you are starting off with a free account, you need to switch over. As of the time of writing this article, your free account credit still transfers to the paid account and billing does not kick in until that credit is consumed.

# Start a Support Request

Once you have logged into your <a href="https://portal.azure.com" target="\_blank">Microsoft Azure</a> account, click the **Help + Support** tile on the Dashboard.

<img alt="Azure Dashboard" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Azure-Dashboard-1200.jpg"/>

# Fill out the Core Increase Ticket

Start a new Support Request, by selecting **New Support Request**

<img alt="Azure Start Support Ticket" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Azure-Dashboard-Start-Ticket-1200.jpg"/>

Since we are requesting an increase in quota for cores:
* Change the Issue type to **Quota**
* Select which subscription this applies to
  *Note: If your subscription says Free, you will need to upgrade it first to a Pay-As-You-Go subscription*
* Change Quota Type to **Cores**
* Support plan should default to **Quota Support - Included**

<img alt="Basic Info on Support Request" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-First-Step-1200.jpg"/>

Under the Problem Step, fill out:
* Severity - B Moderate Impact (I don't know if this has an impact on response time for quota increase)
* Deployment model - Resource Manager
* Location - Choose your location where you will be deploying PCF
* SKU family - Select **DS Series**, **Dv2 Series**, **D Series**
* New quota (Cores) - 252

<img alt="Problem Info on Support Request (with SKU family)" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-Step-2a-1200.jpg"/>

<img alt="Problem Info on Support Request" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-Step-2b-1200.jpg"/>

And finally, enter your contact information so support can keep you updated on the progress:

<img alt="Contact Info on Support Request" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-Step-3-1200.jpg"/>

# Fill out the Storage Increase Ticket

Start a new Support Request, by selecting **New Support Request**

<img alt="Azure Start Support Ticket" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Azure-Dashboard-Start-Ticket-1200.jpg"/>

Since we are requesting an increase in quota for storage:
* Change the Issue type to **Quota**
* Select which subscription this applies to
  *Note: If your subscription says Free, you will need to upgrade it first to a Pay-As-You-Go subscription*
* Change Quota Type to **Storage: ARM**
* Support plan should default to **Quota Support - Included**

<img alt="Contact Info on Support Request" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-Step-4-1200.jpg"/>

Under the Problem Step, fill out:
* Severity - B Moderate Impact (I don't know if this has an impact on response time for quota increase)
* Details - Enter *Please allocate 5 TBs of premium storage. No standard storage will be used.*
* File upload - *Leave empty*

<img alt="Problem Info on Storage Support Request" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-Step-5-1200.jpg"/>

And finally, enter your contact information so support can keep you updated on the progress:

<img alt="Contact Info on Support Request" style="border:2px solid #000" src="{{ site.url }}/images/azure-quota-increase/Quota-Step-3-1200.jpg"/>

# Microsoft Response

I submitted these requests on a Saturday at about 2:30 in the afternoon. Here is the timeline I experienced:

* Saturday 2:30pm: Submitted request
* Saturday 2:30pm: I received an email from Microsoft confirming the request
* Saturday 4:00pm: I received an email from a Microsoft support engineer stating that he is assigned to my tickets
* Saturday 4:15pm: I received a call from my Microsoft support engineer talking through and confirming my requests.
* Saturday 7:25pm: I received an email from my Microsoft support engineer stating that my quota has been increased for the DV1 and DV2 series. I am still waiting to hear on the DS series.
* Monday 7:59pm: I received an email that my quota was increased on DS series as well as my 5 TB of Premium Storage allocated.

Total time 53.5 hours. 
