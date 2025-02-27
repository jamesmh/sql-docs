---
title: "sys.dm_server_external_policy_role_actions (Transact-SQL)"
description: Reference documentation to explain sys.dm_server_external_policy_role_actions (Transact-SQL) dynamic management view.
author: srdan-bozovic-msft
ms.author: srbozovi
ms.date: "11/07/2022"
ms.prod: sql
ms.technology: system-objects
ms.topic: "reference"
f1_keywords:
  - "dm_server_external_policy_role_actions_TSQL"
  - "sys.dm_server_external_policy_role_actions"
  - "dm_server_external_policy_role_actions"
  - "sys.dm_server_external_policy_role_actions_TSQL"
helpviewer_keywords:
  - "sys.dm_server_external_policy_role_actions dynamic management view"
dev_langs:
  - "TSQL"
---

# sys.dm_server_external_policy_role_actions (Transact-SQL)

[!INCLUDE [sqlserver2022](../../includes/applies-to-version/sqlserver2022-asdb.md)]

Lists the links between the roles and actions, could be used to join the two. 
  
|Column name|Data type|Description|  
|-----------------|---------------|-----------------|  
|**role_guid**|**nvarchar(128)**|The role name as defined in external policy source. Has to be unique.|  
|**sql_action_id**|**int**|Internal ID for joining with other dynamic management views. Not used by external policy providers.|  
  
## Permissions  

Principals must have the **VIEW SERVER SECURITY STATE** permission.  

## See also

- [Provision access by data owner for Azure SQL Database](/azure/purview/how-to-policies-data-owner-azure-sql-db)

- [Provision access by data owner for SQL Server on Azure Arc-enabled servers](/azure/purview/how-to-policies-data-owner-arc-sql-server)