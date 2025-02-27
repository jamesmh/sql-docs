---
description: "Modify the Target Servers for a Job"
title: "Modify the Target Servers for a Job"
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords: 
  - "modifying target servers"
  - "SQL Server Agent jobs, target servers"
  - "target servers [SQL Server], modifying"
ms.assetid: 9dbe24f2-acec-4aa2-920c-e8e96efa18e4
author: markingmyname
ms.author: maghan
ms.reviewer: ""
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: "= azuresqldb-mi-current || >= sql-server-2016"
---

# Modify the Target Servers for a Job

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> On [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), most, but not all SQL Server Agent features are currently supported. See [Azure SQL Managed Instance T-SQL differences from SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) for details.

This topic describes how to change the target servers for [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent jobs in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] by using SQL Server Management Studio or [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="BeforeYouBegin"></a>Before You Begin  
  
### <a name="Security"></a>Security  
  
#### <a name="Permissions"></a>Permissions  
By default, members of the sysadmin fixed server role can execute this stored procedure. Other users must be granted one of the following SQL Server Agent fixed database roles in the msdb database:  
  
1.  **SQLAgentUserRole**  
  
2.  **SQLAgentReaderRole**  
  
3.  SQLAgentOperatorRole  
  
## <a name="SSMSProcedure"></a>Using SQL Server Management Studio  
  
#### To modify the target servers for a job  
  
1.  In **Object Explorer,** connect to an instance of the [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], and then expand that instance.  
  
2.  Expand **SQL Server Agent**, expand **Jobs**, right-click a job, and then click **Properties**.  
  
3.  In the **Job Properties** dialog, select the **Targets**page, and click **Target local server**, or **Target multiple servers**.  
  
    If you choose **Target multiple servers**, designate the servers that will be targets for the job by checking the box to the left of the server name. Verify that the check boxes for servers that will not be targets of the job are unchecked.  
  
## <a name="TsqlProcedure"></a>Using Transact-SQL  
  
#### To modify the target servers for a job  
  
1.  Connect to the [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  From the Standard bar, click **New Query**.  
  
3.  Copy and paste the following example into the query window and click **Execute**. This example assigns the multiserver job Weekly Sales Backups to the server SEATTLE2.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_jobserver  
    @job_name = N'Weekly Sales Backups',   
    @server_name = N'SEATTLE2' ;   
GO  
```  
  
For more information, see [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md).  
  
## See Also  
[Automated Administration Across an Enterprise](../../ssms/agent/automated-administration-across-an-enterprise.md)  
