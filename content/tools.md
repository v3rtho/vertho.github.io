+++
title = 'Tools'
description = 'Free PowerShell tools I built for Microsoft Defender and Intune admins.'
draft = false
ShowToc = false
ShowReadingTime = false
ShowBreadCrumbs = false
ShowShareButtons = false
+++

Over the years I built a few tools to make my own work with Microsoft security products easier. They are free, open source and available on GitHub. All of them are PowerShell scripts with a graphical interface, so there are no commands to remember.

{{< tool name="MDE Troubleshooter" repo="https://github.com/v3rtho/MDE-troubleshooter" tagline="Analyze and troubleshoot Microsoft Defender for Endpoint on a Windows device, all in one window." >}}
Gives you a central view of the security configuration, logs and diagnostic actions on the local endpoint:

- **Defender AV**: versions, service status, real-time protection and scan information
- **Attack Surface Reduction**: status of every ASR rule, including per-rule exclusions
- **Exclusions**: all AV exclusions, and whether tamper protection covers them
- **Updates**: local engine, platform and signature versions compared with the latest from Microsoft
- **Logs**: SENSE, Defender AV, ASR and Controlled Folder Access events with filtering
- **More**: Performance Analyzer, proxy, firewall, connectivity checks and Troubleshooting Mode

A Linux version (shell script) is included in the repository.

**Requirements:** Windows 10/11 or Windows Server 2016+, PowerShell 5.1, administrator rights, Microsoft Defender Antivirus.

**Related posts:** [MDE Troubleshooter is born](/2023/06/30/tool-mde-troubleshooter-is-born/) · [A new release after 2 years](/2025/01/07/after-2-years-a-new-release-of-mde-troubleshooter/) · [v1.6.1 available](/2025/06/07/mde-troubleshooter-v1-6-1-available/)
{{< /tool >}}

{{< tool name="MDI Configurator" repo="https://github.com/v3rtho/MDI-configurator" tagline="A graphical interface for configuring and validating Microsoft Defender for Identity." >}}
A wrapper around the official DefenderForIdentity PowerShell module that covers the full setup of an MDI deployment:

- **Configuration**: apply the required audit policies and settings, in Domain or LocalMachine mode
- **Test & validate**: check whether each configuration is in place
- **Reports**: generate an HTML report of your MDI configuration
- **Proxy**: set, view, clear and test the sensor proxy settings
- **Directory Service Account**: create a DSA and test its permissions

**Requirements:** Windows 10/11 or Windows Server 2016+, PowerShell 5.1, the DefenderForIdentity module, administrator rights.

**Related post:** [MDI-configurator v1.0 is out!](/2025/12/01/mdi-configurator-v1-0-is-out/)
{{< /tool >}}

{{< tool name="EpmTools GUI" repo="https://github.com/v3rtho/EPMtools" tagline="A point-and-click interface for Microsoft Intune Endpoint Privilege Management (EPM)." >}}
A wrapper around the EPM Agent cmdlets, extended with Intune integration through Microsoft Graph:

- **Inspect the device**: view the elevation rules, client settings and policies the EPM Agent received
- **File attributes**: extract publisher and certificate details from an `.exe` to build a rule
- **Create rules**: build an elevation rule and push it to an Intune policy
- **Client settings**: create, deploy and edit Intune elevation settings policies
- **Reports**: elevation events and reports by application, publisher or user, with filtering
- **Export**: save results as CSV or JSON

**Requirements:** Windows 10/11 with the EPM Agent installed via Intune, Windows PowerShell 5.1 x64 (ARM64 supported).
{{< /tool >}}

Found a bug or have an idea for a feature? Open an issue on GitHub or [contact me](/contact/).
