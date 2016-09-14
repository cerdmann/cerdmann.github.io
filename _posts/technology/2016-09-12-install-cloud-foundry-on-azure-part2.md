---
layout: post
title: Install Pivotal Cloud Foundry on Microsoft Azure (Install)
excerpt: "Microsoft Azure makes it easy to install Pivotal Cloud Foundry. Follow these steps to be up and running with Pivotal Cloud Foundry in a few hours."
author: erds
categories: technology
tags:
  - Pivotal Cloud Foundry
  - Azure
image:
  feature: cross-cut-image-4.jpg
  thumb: thumbnail-cross-cut-image-4.jpg
comments: false
share: true
---

<a title="By Joel Baldwin (LOOK April 29, 1969. p.74) [Public domain, Public domain or Public domain], via Wikimedia Commons"><img width="256" alt="JohnnyCash1969" class="pull-right" src="{{ site.url }}/images/pcf-on-azure/JohnnyCash1969.jpg"/></a>


Throughout his career as a musician, Johnny Cash filled our lives with wisdom. We learned important life lessons, such as:

* <a href="https://play.google.com/music/preview/Trppte3wdqzvsehzhlgmzwvz54q?lyrics=1" target="\_blank">Don't take your guns to town</a>
* <a href="https://play.google.com/music/preview/Txqr3w7pza3jtwynlgxqqo5taya?lyrics=1" target="\_blank">Always be a good boy, don't ever play with guns</a>
* <a href="https://play.google.com/music/preview/Tqshbtiku2jdpmsfetstzt2rkzm?lyrics=1" target="\_blank">Lay off the whiskey and let that cocaine be</a>
* <a href="https://play.google.com/music/preview/Tmf2am7pkabtjiass65j65zzw54?lyrics=1" target="\_blank">Depending on circumstances, naming your son Sue is acceptable</a>

In fact, many enterprises today are following Johnny's advice when outfitting their development teams. They are building their Platform as a Service (PaaS) <a href="https://play.google.com/music/preview/T7d4o3gr6arqoojow32jvc3qnji?lyrics=1" target="\_blank">one piece at a time</a>. This, however, isn't the only option. You can help your development teams work faster and more safely **today** using <a href="https://pivotal.io" target="\_blank">Pivotal Cloud Foundry (PCF)</a> on top of <a href="https://azure.microsoft.com" target="\_blank">Microsoft Azure</a>.

# Overview
1. [Prerequisites]({% post_url 2016-09-08-install-cloud-foundry-on-azure %})
2. [Install]({% post_url 2016-09-12-install-cloud-foundry-on-azure-part2 %})
3. Take it for a test run - *Coming Soon*

# Install

> This article assumes you have already completed the steps in the [Prerequisites]({% post_url 2016-09-08-install-cloud-foundry-on-azure %}).

Once completing the [prerequisites]({% post_url 2016-09-08-install-cloud-foundry-on-azure %}), you should have the following:

* Microsoft Azure, Pay-As-You-Go Subscription with quotas increased
* Pivotal Network API token
* azure-credentials.json file with your Service Principal information
* Public and private key to access your PCF install

### Login to your Azure account

Use your existing account on <a href="https://azure.microsoft.com/en-us/free/" target="\_blank">Microsoft Azure</a>.

<img alt="Azure Home" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Azure-Home-1200.jpg"/>

### Search the marketplace for Pivotal Cloud Foundry and install

Click on New and enter **Pivotal Cloud Foundry** in the *Search the marketplace* textbox.

<img alt="Search the marketplace for Pivotal Cloud Foundry" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Search-For-PCF-1200.jpg"/>

Hit Enter, or click on *Pivotal Cloud Foundry on Microsoft Azure* in the dropdown box. In the next panel (search results), click on *Pivotal Cloud Foundry on Azure*

<img alt="Select Pivotal Cloud Foundry" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Found-PCF-1200.jpg"/>

Review the description and press **Create**

In the next panel, enter the information that we took care of in the [prerequisites]({% post_url 2016-09-08-install-cloud-foundry-on-azure %}).

* Storage Account Name Prefix - Enter anything that will help you differentiate your PCF storage accounts from your other accounts. I entered **pcfonazure**
* SSH public key - copy the text from your public key file (the one with the extension .pub) into this textbox
* Service Principal - Click in the box which will open up a file finder. Select the *azure-credentials.json* file you created in prerequisites.
* Pivotal Network Token - Paste your API Token from your account at <a href="https://network.pivotal.io" target="\_blank">network.pivotal.io</a>.
* Subscription - Ensure your subscription is selected
* Resource group - Either choose an existing, or Create New. I like to create a new resource group so I can tear the whole thing down by deleting my resource group. I chose **PCF-on-Azure**
* Location - Choose your location where you increased your quota

<img alt="Configure Basic Settings" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Configure-Basic-Settings-1200.jpg"/>

On the next panel, wait for the validation to pass and then click on OK.

<img alt="Validation Passed" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Validation-Passed-1200.jpg"/>

On the next panel, read through the content, and if you agree, click on the *Purchase* buttons

<img alt="Accept Terms" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Terms-Accept-1200.jpg"/>

Once you accept, you should be taken back to the Dashboard and you will see a *Deploying Pivotal Cloud Foundry on Microsoft...* tile. Feel free to click on the tile to watch the progress.

<img alt="Deploying Pivotal Cloud Foundry" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Deploying-PCF-1200.jpg"/>

After about 5 - 10 minutes you will receive notification that the deploy is complete. However, only the <a href="https://bosh.io" target="\_blank">bosh</a> director has been deployed. The bosh director will work in the background to deploy the components of PCF.

<img alt="Bosh Director Installed" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Deployed-1200.jpg"/>

At the end of this step, your PCF install will look something like this:

<img alt="Bosh Director Installed" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/First-Install-1200.jpg"/>

### Check on progress

Once you receive notification that the deploy is complete, you will need to check on progress of the underlying PCF install. To do this, we need to grab the url to the *Progress Monitor URL*.

Go back to your Azure Dashboard and click on **Resource Group**.

<img alt="Select Resource Groups" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Select-Resource-Groups-1200.jpg"/>

Click on your Resource Group. If you followed the naming conventions I user, it will be **PCF-on-Azure**.

<img alt="Select Resource Group" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Select-Resource-Group-1200.jpg"/>

Select **Deployments** in the next panel.

<img alt="Select Deployments" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Select-Deployments-1200.jpg"/>

Select the **pivotal.pivotal-cloud-foundryazure-....** deployment in the next panel.

<img alt="Select Deployment" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Select-Deployment-1200.jpg"/>

In the next panel, under the *Outputs* section, you will see a text box labeled **PROGRESSMONITORURL**. Either copy the contents of the text box, or click the button to the right of the text box to copy the contents to the clipboard.

<img alt="Get Progress Monitor Url" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Get-Progress-Monitor-Url-1200.jpg"/>

Paste that url into a new browser window and you will see a browser tmux session that shows you the contents of the pcf_install.log located on the jumpbox. It will continue to update as the install progresses. You should see something like this:

<img alt="Progress on Tmux" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Progress-Tmux-1200.jpg"/>

Keep checking back. When you see the Apps Manager Url and Admin password (after a couple of hours), the install is complete. Your screen should look like this, and you are finished:

<img alt="Install Complete" style="border:2px solid #000" src="{{ site.url }}/images/install-pcf-on-azure/Install-Complete-1200.jpg"/>

Go ahead and put the url into a browser. Use admin as the user name and the above password to log in.
