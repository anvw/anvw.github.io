---
layout: post
title:  "Publishing to Azure from VS"
date:   2020-12-17 10:58:45 +1000
categories: azure
---
Publishing to Azure from Visual Studio:
-- Create Resource Group and VM in Azure Portal<br>
-- Make sure VM has at least 2gb<br>
-- InBound ports open: 3389 (at least initially), 443, 80, 8172<br>
-- RDP into VM<br>
-- In Server Manager, install IIS, IIS Management Service (under Management Tools)<br>
-- Once installed - go into Management Service and ensure "Enable Remote Connections" is ticked.<br>
[https://port135.com/could-not-reach-the-web-deploy-endpoint-specified-virtual-machine/]