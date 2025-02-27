---
title: "SQL Server 2022 Release Notes"
description: Find information about SQL Server 2022 (16.x) limitations, known issues, help resources, and other release notes.
ms.date: 11/01/2022
ms.prod: sql
ms.reviewer: ""
ms.technology: release-landing
ms.topic: "article"
ms.custom:
- event-tier1-build-2022
author: MikeRayMSFT
ms.author: mikeray
monikerRange: "= sql-server-ver16"
---

# [!INCLUDE[SQL Server 2022](../includes/sssql22-md.md)] release notes

[!INCLUDE[SQL Server 2022](../includes/applies-to-version/sqlserver2022.md)]

This article describes requirements, limitations and known issues for [!INCLUDE[SQL Server 2022](../includes/sssql22-md.md)].

## Hardware and software requirements

For hardware and software requirements, see [SQL Server 2022: Hardware and software requirements](install/hardware-and-software-requirements-for-installing-sql-server-2022.md).

## Feature notes

This section identifies known issues you may experience with this product.

### SQL Setup

#### Help

When you execute `setup /HELP` the information returned does not include the new `/AZUREEXTENSION` feature. Complete information for `setup` is at [Install SQL Server on Windows from the command prompt](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).

#### Localized language interface

In certain localized languages, the Azure Extension configuration screen controls may be partially overwritten or missing. To resolve issue, expand or maximize the Setup window from the default window sizing.

#### Software Assurance installation parameter

There is a new Setup command line installation parameter - `/PRODUCTCOVERDBYSA`. The parameter indicates if the provided product key (`/PID=`) license is covered under a Software Assurance or SQL Server Subscription contract, or just a SQL Server license.

[Install SQL Server on Windows from the command prompt](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) describes this parameter.

#### Deprecated feature parameters

The following features are not available in Setup in SQL Server 2022. If specified in command line installations or scripts, these previously supported parameters may fail.

:::row:::
    :::column:::
     - `\PolyBaseJava`
     - `\SQL_INST_MR`
     - `\SQL_INST_JAVA`
     - `\SQL_INST_MPY`
    :::column-end:::
    :::column:::
     - `\SQLJAVADIR`
     - `\SQL_SHARED_MPY`
     - `\SNAC_SDK`
     - `\SQL_SHARED_MR`
    :::column-end:::
    :::column:::
     - `\SDK`
     - `\DREPLAY_CTLR`
     - `\TOOLS`
     - `\DREPLAY_CLT`
    :::column-end:::
:::row-end:::

#### Reboot requirement

When you install an initial SQL Server 2022 instance on a Windows Server 2022 machine, if the server does not have `VCRuntime140` version 14.29.30139 or later installed, Setup will require reboot.

Windows Server 2022 was released with VCRuntime version 14.28.29914. 

### Query Store for secondary replicas

[Query Store for secondary replicas](../relational-databases/performance/query-store-for-secondary-replicas.md) is available for preview. It isn't available for use in production environments.

### RPC calls fail with Encrypt=Strict
- **Issue and customer impact**: Due to defect in TDS 8.0 protocol implementation RPC calls would fail if Encrypt option is set to Strict in connection string. Example would be running "sp_who" stored procedure.
- **Applies to**: [!INCLUDE[SQL Server 2022](../includes/sssql22-md.md)] RTM

## Build number

| Preview build | Version number | Date |
|:--|:--|:--|
| General release to market (RTM) | 16.0.1000.6  | November 16, 2022 |
| RC 1    | 16.0.950.9 | September 22, 2022|
| RC 0    | 16.0.900.6 | August 23, 2022|
| CTP 2.1 | 16.0.700.4 | July 27, 2022 |
| CTP 2.0 | 16.0.600.9 | May 20, 2022 |

## Next steps

For related information about new features and capabilities in this version of SQL Server, see:

[What's new in [!INCLUDE[sql-server-2022](../includes/sssql22-md.md)]](what-s-new-in-sql-server-2022.md).
