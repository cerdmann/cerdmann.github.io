---
layout: post
title: Install Pivotal Cloud Foundry on Microsoft Azure (Prerequisites)
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

# Prerequisites

Many of the preliminary steps in this article are taken from the <a href="https://docs.pivotal.io/pivotalcf/1-7/customizing/pcf_azure.html" target="\_blank">documentation</a>. I will go into a bit more depth with screen shots and explanations. Furthermore, it is my intent to follow up this series of postings with more articles about operationalizing the platform and development workflow. Let's get started.

### Setting up a Pivotal Network account

<figure class="half">
	<a href="{{ site.url }}/images/pcf-on-azure/JoinPivotalNetwork-1200.jpg"><img src="{{ site.url }}/images/pcf-on-azure/JoinPivotalNetwork-600.jpg" alt="image"></a>
	<a href="{{ site.url }}/images/pcf-on-azure/JoinPivotalNetworkForm-1200.jpg"><img src="{{ site.url }}/images/pcf-on-azure/JoinPivotalNetworkForm-600.jpg" alt="image"></a>
</figure>
Your automated PCF install will need access to the install packages and the ability to accept the EULA on your behalf. You can create an account by navigating to <a href="https://network.pivotal.io" target="\_blank">Pivotal Network</a>. Simply:

* Click on one of the **Join** buttons to begin
* Fill out the form to create a login

### Capture your API token

<figure class="half">
	<a href="{{ site.url }}/images/pcf-on-azure/ApiToken-EditProfile-1200.jpg"><img src="{{ site.url }}/images/pcf-on-azure/ApiToken-EditProfile-600.jpg" alt="image"></a>
	<a href="{{ site.url }}/images/pcf-on-azure/ApiToken-Capture-1200.jpg"><img src="{{ site.url }}/images/pcf-on-azure/ApiToken-Capture-600.jpg" alt="image"></a>
</figure>
Once your account is created, click on the dropdown arrow next to your name. In the dropdown list, select **Edit Profile** as seen in the above image. On your profile page scroll all the way to the bottom where you will see your **API TOKEN**.

>This would be a good time to start a text file to keep all the information in that you will need to complete the automated setup. The **API TOKEN** should be your first addition to this file.

### Setup an Azure account

Use your existing account, or setup a new account on <a href="https://azure.microsoft.com/en-us/free/" target="\_blank">Microsoft Azure</a>.

PCF will need increased resources on your subscription. In particular, you will need:

* 53 VMs (I know what you are thinking)
  <iframe src="https://www.youtube.com/embed/mjCRUvX2D0E?rel=0;modestbranding=1;fs=0;cc_load_policy=1;controls=0"></iframe>

  Of those 53, 15 Vms are used during the install phase to create builds and run errands such as tests. Once the deploy is finished, those 15 VMs are destroyed. This leaves your install with 38 VMs under its control. We'll dive a bit into the architecture later and you'll see the reason for those VMs.

* 1 storage account - This setup is for a 90 day trial version of PCF. In an actual production install, you will utilize multiple storage accounts
* 3 public IP addresses
* 1 jumpbox VM that runs the deployment

You will also need to increase your core and storage account quotas on Azure. This [link]({% post_url 2016-09-09-azure-quota-increase-for-cloud-foundry %}) will show you how to do request the increase. Please get the requests in the queue before continuing.

### Install the Azure Command Line Interface (CLI)

* (Linux/Unix/Mac OS X) Follow these <a href="https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/" target="\_blank">instructions</a>.
* (Windows) Follow these <a href="https://azure.microsoft.com/en-us/documentation/articles/powershell-install-configure/" target="\_blank">instructions</a>.

For this install I chose to install the Azure CLI in a Docker container. It's as simple as

```
docker run -it microsoft/azure-cli
```

### Login to your Azure account through the CLI and create a Service Principal

We need to create a Service Principal (sp) for the PCF install to use. We will use our Docker azure-cli container to login to our subscription and then create the sp using this <a href="https://github.com/danhigham/azure-sp-tool" target="\_blank">tool</a>. We will then transfer the output file to our local machine and upload it to Azure during the PCF install process. Let's get started:

##### Run Azure-Cli Docker image

Ensure Docker is running properly in your terminal. Try:
```
docker run hello-world
```
If this works, proceed to the next step. If not, get Docker setup using this <a href="https://docs.docker.com/v1.8/installation/" target="\_blank">link</a>.

Now we are ready to run a container.

```
docker run -it microsoft/azure-cli
```

<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Install-1200.jpg"/>

##### Install Azure-Sp-Tool

In your docker container, execute:
```
npm install azure-sp-tool -g
```

<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Install-Tool-1200.jpg"/>

##### Login to Azure through the CLI

In your docker container, execute:
```
azure login
```

<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Install-Login-Terminal-1200.jpg"/>

Follow the directions in the terminal. In a browser on your local machine open the url indicated. When prompted enter the code to authenticate.

<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Install-Login-Browser-1200.jpg"/>

Click Continue and you will be prompted to login to your azure account, or simply click on the user with access to your subscription if you have already logged in as that user in another tab.

<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Install-Confirm-Browser-1200.jpg"/>

Hop back over to your terminal with the Docker container running in it and when you see **login** command **OK**, you are ready to run the sp create tool.

<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Install-Login-Success-1200.jpg"/>

##### Create the Service Principal

Ensure that your desired subscription is set to default by running
```
azure account list
```
<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Account-List-1200.jpg"/>

If you have multiple subscriptions, please use this <a href="https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-connect/#multiple-subscriptions" target="\_blank">link</a> to learn how to set the desired subscription as default.

Next, run the following command in the docker container to create your Service Principal
```
azure-sp-tool create-sp
```
<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Create-SP-1200.jpg"/>

##### Copy the credential file to the local machine

We are going to open up another terminal and use the docker cp command to copy the file created in your container to your local machine. Without exiting your Docker container, open a new terminal and type:

```
eval $(docker-machine env default)

docker ps
```
<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Get-Container-1200.jpg"/>

Capture the container id. In my case it is 08639683cc69. I will use that in the following command to copy the credential file from the Docker container to my home directory *~/*.

```
docker cp 08639683cc69:/azure-credentials.json ~/
```
<img alt="Install azure-sp-tool" style="border:2px solid #000" src="{{ site.url }}/images/pcf-on-azure/Azure-CLI-Docker-Get-File-1200.jpg"/>

Protect this file. Please store it in a secure location because it contains sensitive credentials. Once the copy is complete, go ahead and exit out of your docker container.

If you need any clarification on the above steps, here is a <a href="https://youtu.be/Sc3gdIMi_Ec" target="\_blank">video</a> demonstrating the full workflow:

<iframe src="https://www.youtube.com/embed/Sc3gdIMi_Ec?rel=0;modestbranding=1" allowfullscreen></iframe>


### Create a public key

This key will be used to identify you when ssh-ing into the jumpbox. You must generate 2048-bit RSA public and private key files.

* For Linux/Unix/Mac OS X - Use ```ssh-keygen -t rsa -b 2048```
* For Windows - Download, install, and use <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="\_blank">PuTTYgen</a>.

Which ever method you use, protect the key files and store them in a secure location.
