+++
date = '2025-03-25T14:17:56+01:00'
lastmod = '2025-03-25T14:24:31+01:00'
draft = false
title = "Proactive Exposure hunting: OAuth Applications and Azure permissions"
slug = 'proactive-exposure-hunting-oauth-applications-and-azure-permissions'
description = "Microsoft recently announced that OAuth applications are now integrated into the attack path experience within Exposure Management. This also means that all the data available in the Attack…"
author = 'Thomas Verheyden'
tags = ["Defender XDR", "Microsoft Exposure Management"]
categories = ["Defender XDR", "Microsoft Exposure Management"]
+++

## Intro

Microsoft recently announced that OAuth applications are now integrated into the attack path experience within [Exposure Management](https://learn.microsoft.com/en-us/security-exposure-management/enterprise-exposure-map). This also means that all the data available in the Attack surface map is also available in advanced hunting in the Exposure Management tables. This triggered me to have a look at the available data and I could quickly created some use cases around it which I wanted to share with the community.

Don’t limit yourself to my use cases—think outside the box, explore new possibilities, and create your own. Let me know what you come up with. Happy hunting!

## OAuth applications with Microsoft Graph permissions

Let’s start with a basic use case. let’s have a look at the OAuth applications that have app permissions or delegated permissions to the Microsoft Graph:

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
 where 
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
            and NodeGraph.NodeName =~ "Microsoft Graph"
      project NodeOauthApp.NodeName,NodeOauthApp,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal,HasPermissionsTo,NodeGraph.NodeName, NodeGraph.NodeLabel
```

Now lets limited it only to the application that your tenant is owner off:

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
 where 
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
            NodeOauthApp.NodeProperties.rawData.isBelongToExternalApplication contains "False"   and NodeGraph.NodeName =~ "Microsoft Graph"
      project NodeOauthApp.NodeName,NodeOauthApp,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal,HasPermissionsTo,NodeGraph.NodeName, NodeGraph.NodeLabel
```

Lets us add an additional attack path to it. Lets hunt for users that has any kind of role on the application which has app permissions to the MS Graph

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (NodeUser)-[HasRoleOn]->(NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
where 
            HasRoleOn.EdgeLabel =~ "has role on" and
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
            (NodeOauthApp.NodeProperties.rawData.isBelongToExternalApplication contains "False"   and NodeGraph.NodeName =~ "Microsoft Graph")
   project NodeUser.NodeName, HasRoleOn,NodeOauthApp.NodeName,NodeOauthApp,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal,HasPermissionsTo,NodeGraph.NodeName, NodeGraph.NodeLabel
```

How can an attacker obtain credentials of users? Yes, on vulnerable devices with unprotected credentials in memory. Lets add them as a additional attack path connection:

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (DeviceWithRCE)-[HasCredentialsOf]->(NodeUser)-[HasRoleOn]->(NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
where 
            HasCredentialsOf.EdgeLabel =~ "has credentials of" and
            HasRoleOn.EdgeLabel =~ "has role on" and
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
             (NodeOauthApp.NodeProperties.rawData.isBelongToExternalApplication contains "False"   and NodeGraph.NodeName =~ "Microsoft Graph") and 
              (DeviceWithRCE.Categories == "[""compute"",""device"",""virtual_machine""]") 
      project DeviceWithRCE.NodeName,HasCredentialsOf.EdgeLabel,NodeUser.NodeName,HasRoleOn.EdgeLabel,NodeOauthApp.NodeName,NodeOauthApp.NodeLabel,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal.NodeName,HasPermissionsTo.EdgeLabel,NodeGraph.NodeName, NodeGraph.NodeLabel
```

## Service Principals permissions to Azure resources

During searching through all the available data i found another interesting use case. I saw that there are a lot of raw data available around Servie principals in the tables. Let’s have a look at the Service Principals with access to your Azure resources:

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
 where 
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
           NodeGraph.NodeLabel =~ "subscriptions"
      project NodeOauthApp.NodeName,NodeOauthApp,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal,HasPermissionsTo.EdgeProperties.rawData.permissions.roles,NodeGraph.NodeName, NodeGraph.NodeLabel
```

Lets limit to only owner or contributor roles:

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
 where 
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
           NodeGraph.NodeLabel =~ "subscriptions" and 
           (HasPermissionsTo.EdgeProperties.rawData.permissions.roles contains "owner" or HasPermissionsTo.EdgeProperties.rawData.permissions.roles contains "contributor")
      project NodeOauthApp.NodeName,NodeOauthApp,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal,HasPermissionsTo.EdgeProperties.rawData.permissions.roles,NodeGraph.NodeName, NodeGraph.NodeLabel
```

Let add the user who have a role on those service principals:

```
ExposureGraphEdges
| make-graph SourceNodeId --> TargetNodeId with ExposureGraphNodes on NodeId
| graph-match (NodeUser)-[HasRoleOn]->(NodeOauthApp)-[CanAuthenticateAs]->(NodeServicePrincipal)-[HasPermissionsTo]->(NodeGraph) 
 where 
             HasRoleOn.EdgeLabel =~ "has role on" and
            CanAuthenticateAs.EdgeLabel =~ "can authenticate as" and
            HasPermissionsTo.EdgeLabel =~  "has permissions to" and
           NodeGraph.NodeLabel =~ "subscriptions" and 
           (HasPermissionsTo.EdgeProperties.rawData.permissions.roles contains "owner" or HasPermissionsTo.EdgeProperties.rawData.permissions.roles contains "contributor")
      project NodeUser.NodeName, HasRoleOn, NodeOauthApp.NodeName,NodeOauthApp,CanAuthenticateAs.EdgeLabel,NodeServicePrincipal,HasPermissionsTo.EdgeProperties.rawData.permissions.roles,NodeGraph.NodeName, NodeGraph.NodeLabel
```

## Finishing

My examples are limited to my own use cases, but I hope they provide an idea of the available capabilities enabled by having access to raw data on OAuth applications and Service Principals. If you’re developing your own hunting queries or have other use cases in mind, feel free to reach out—I’d be happy to create queries tailored to your needs!”
