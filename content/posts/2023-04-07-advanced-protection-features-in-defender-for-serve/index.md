+++
date = '2023-04-07T10:18:57+01:00'
lastmod = '2023-08-04T12:53:39+01:00'
draft = false
title = "Unleash the power of Defender for Servers Plan 2: The Intro – Part 0"
slug = 'advanced-protection-features-in-defender-for-servers-plan-2-the-intro-part-0'
description = "I decided to start a blog series about the Advanced protection features which are included in the Defender for Servers Plan 2 provided by Microsoft Defender for Cloud. More and more companies are…"
author = 'Thomas Verheyden'
tags = ["Advanced protection features series", "Defender for Cloud", "Defender for Servers"]
categories = ["Defender for Cloud"]
+++

![screenshot](image-8.png)

## Introduction

I decided to start a blog series about the **Advanced protection features** which are included in the **Defender for Servers Plan 2** provided by **Microsoft Defender for Cloud**. More and more companies are starting to use Defender for Servers but are uncertain which plan to choose for. With this blog series I would like to provide enhanced information about the Advanced protection features and a guidance how to get started with the different features. I run against a lot of caveats deploying those features and I happy to share this with the community!

### Available Blogs ( 01/08/2023 )

Part 1: [Advanced protection features in Defender for Servers Plan 2: File integrity monitoring – Part 1](/2023/04/19/advanced-protection-features-in-defender-for-servers-plan-2-file-integrity-monitoring-part-1/)  
Part 2: [Advanced protection features in Defender for Servers Plan 2: Adaptive Application Controls – Part 2 – Vertho | Verheyden Thomas](/2023/05/17/advanced-protection-features-in-defender-for-servers-plan-2-adaptive-application-controls-part-2/)  
Part 3: [Unleash the power of Defender for Servers Plan 2: Agentless scanning – part 3 – Vertho | Verheyden Thomas](/2023/06/14/unleash-the-power-of-defender-for-servers-plan-2-agentless-scanning-part-3/)

Part 4: [Unleash the power of defender plan 2: Just-in-time VM access – part 4 – Vertho | Verheyden Thomas](/2023/07/20/unleash-the-power-of-defender-plan-2-just-in-time-vm-access-part-4/)

### Advanced protection features

Defender for Server Plan 2 provides more extended detection and response capabilities compared to Plan 1. The features which are currently available are:

- **Threat detection for network-level (agentless):** Defender for Servers detects threats that are directed at the control plane on the network, including network-based detections for Azure virtual machines.

- **Adaptive application controls:** Adaptive application controls define allowlists of known safe applications for machines. To use this feature, Defender for Cloud must be enabled on the subscription.

- **Microsoft Defender Vulnerability Management (MDVM) Add-on:** Enhance your vulnerability management program consolidated asset inventories, security baselines assessments, application block feature, and more.

- **Security Policy and Regulatory Compliance:** Customize a security policy for your subscription and also compare the configuration of your resources with requirements in industry standards, regulations, and benchmarks.

- **Just-in-time virtual machine access:** Just-in-time virtual machine access locks down machine ports to reduce the attack surface.

- **Adaptive network hardening:** Network hardening filters traffic to and from resources by using network security groups (NSGs) to improve your network security posture. Further improve security by hardening the NSG rules based on actual traffic patterns.

- **File integrity monitoring:** File integrity monitoring examines files and registries for changes that might indicate an attack. A comparison method is used to determine whether suspicious modifications have been made to files.

- **Docker host hardening:** Assesses containers hosted on Linux machines running Docker containers, and then compares them with the Center for Internet Security (CIS) Docker Benchmark.

- **Network map:** Provides a geographical view of recommendations for hardening your network resources.

- **Agentless scanning**:  Scans Azure virtual machines by using cloud APIs to collect data.

- **Qualys vulnerability assessment:**As an alternative to Defender Vulnerability Management, Defender for Cloud can deploy a Qualys scanner and display the findings.

- **Free data ingestion (500 MB) in workspaces:**Free data ingestion is available for specific data types. Data ingestion is calculated per node, per reported workspace, and per day. It’s available for every workspace that has a Security or AntiMalware solution installed.

### Scope of the blog series

My focus currently is on Adaptive Application Control, File integrity monitoring, Adaptive network hardening, Agentless scanning and the MDVM add-on. The capabilities in MDVM add-on are expanding rapidly and maybe it deserve a blog series on its own.I don’t know yet in which order I will provide the different parts of this blog series but the first part will be about File integrity monitoring and will be released in the next weeks. Stay tuned !

**If you have any suggestion or feature that you would like to know more of you can always contact me on [my social networks](https://twitter.com/ThomasVrhydn) or via [the contact button](/about/#get-in-touch) on my blog site.**

***Happy reading !***
