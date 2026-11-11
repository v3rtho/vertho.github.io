+++
date = '2025-12-01T15:29:41+01:00'
lastmod = '2025-12-01T15:29:41+01:00'
draft = false
title = "MDI-configurator v1.0 is out !"
slug = 'mdi-configurator-v1-0-is-out'
description = "MDI-configurator v1.0: a free PowerShell GUI to configure, test and report on Microsoft Defender for Identity without remembering the commands."
author = 'Thomas Verheyden'
tags = ["Defender for Identity", "Defender XDR", "Microsoft Security"]
categories = ["Defender for Identity", "Defender XDR"]
+++

A user-friendly graphical interface for managing Microsoft Defender for Identity (MDI) configurations using PowerShell.

This PowerShell script provides a comprehensive WPF-based GUI wrapper around the Microsoft Defender for Identity PowerShell module. It simplifies the configuration, testing, and management of MDI deployments through an intuitive interface, eliminating the need to remember complex PowerShell commands.

Download: [v3rtho/MDI-configurator: Guidance scripts related to Defender For Identity: Prereqs, configuration etc](https://github.com/v3rtho/MDI-configurator)

## Features

### 🔧 Configuration Management

- Set MDI configurations for Domain or LocalMachine mode
- All (all configurations)
- AdfsAuditing
- AdRecycleBin
- AdvancedAuditPolicyCAs
- AdvancedAuditPolicyDCs
- CAAuditing
- ConfigurationContainerAuditing
- EntraConnectAuditing
- RemoteSAM
- DomainObjectAuditing
- NTLMAuditing
- ProcessorPerformance

### 🧪 Testing & Validation

- Test MDI configurations
- Validate specific audit policies
- Support for both Domain and LocalMachine testing modes

### 📊 Reporting

- Generate comprehensive HTML configuration reports
- Customizable output path
- Automatic report opening option
- Domain and LocalMachine report modes

### 🌐 Proxy Configuration

- Configure MDI sensor proxy settings
- Support for authenticated proxies
- Get current proxy configuration
- Clear proxy settings
- Test sensor API connectivity

### 👤 Directory Service Account (DSA) Management

- Create DSA accounts
- Test DSA permissions and delegations
- View current MDI configuration

## 🚀 Prerequisites

- **Operating System:** Windows 10/11 or Windows Server 2016+
- **PowerShell:** Version 5.1 or later
- **Module:** DefenderForIdentity PowerShell module
- **Permissions:** Administrator rights required for most operations

## 📦 Installation

### 1. Install the DefenderForIdentity Module

Open PowerShell as Administrator and run:

```powershell
Install-Module -Name DefenderForIdentity -Force
```

### 2. Download the Script

Clone this repository or download the `MDI-configurator.ps1`

### 3. Execution Policy

Ensure your PowerShell execution policy allows script execution:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## 🎯 Usage

### Running the Script

#### Option 1: From PowerShell

```
.\MDI-configurator.ps1
```

### First-Time Setup

1. The script will automatically check if the DefenderForIdentity module is installed
2. If not found, it will prompt you with installation instructions
3. After installing the module, restart the application
