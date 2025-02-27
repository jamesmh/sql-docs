---
title: "Generating commands with CommandBuilders"
description: Explains how to use command builders to automatically generate INSERT, UPDATE, and DELETE commands for a `DataAdapter` that has a single-table SELECT command.
author: David-Engel
ms.author: v-davidengel
ms.reviewer: v-chmalh
ms.date: "11/25/2020"
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
dev_langs:
  - "csharp"
---
# Generating commands with CommandBuilders

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

When the `SelectCommand` property of the <xref:System.Data.Common.DbDataAdapter> object is dynamically specified at run time, such as through a query tool that takes a textual command from the user, you may not be able to specify the appropriate `InsertCommand`, `UpdateCommand`, or `DeleteCommand` at design time. If your <xref:System.Data.DataTable> maps to or is generated from a single database table, you can take advantage of the <xref:System.Data.Common.DbCommandBuilder> object to automatically generate the `DeleteCommand`, `InsertCommand`, and `UpdateCommand` of the <xref:System.Data.Common.DbDataAdapter>.

> [!NOTE]
> In Microsoft SqlClient Data Provider for SQL Server, the <xref:Microsoft.Data.SqlClient.SqlDataAdapter> class is derived from the <xref:System.Data.Common.DbDataAdapter> class, and the <xref:Microsoft.Data.SqlClient.SqlCommandBuilder> class is derived from the <xref:System.Data.Common.DbCommandBuilder> class.

As a minimum requirement, you must set the `SelectCommand` property in order for automatic command generation to work. The table schema retrieved by the `SelectCommand` property determines the syntax of the automatically generated INSERT, UPDATE, and DELETE statements.

The <xref:System.Data.Common.DbCommandBuilder> must execute the `SelectCommand` in order to return the metadata necessary to construct the INSERT, UPDATE, and DELETE SQL commands. As a result, an extra trip to the data source is necessary, and this can hinder performance. To achieve optimal performance, specify your commands explicitly rather than using the <xref:System.Data.Common.DbCommandBuilder>.

> [!NOTE]
> The `SelectCommand` must also return at least one primary key or unique column. If none is present, an `InvalidOperation` exception is generated, and the commands are not generated.

When associated with a `DataAdapter`, the <xref:System.Data.Common.DbCommandBuilder> automatically generates the `InsertCommand`, `UpdateCommand`, and `DeleteCommand` properties of the `DataAdapter` if they are null references. If a `Command` already exists for a property, the existing `Command` is used.

Database views that are created by joining two or more tables together are not considered a single database table. In this instance, you cannot use the <xref:System.Data.Common.DbCommandBuilder> to automatically generate commands; you must specify your commands explicitly. For information about explicitly setting commands to resolve updates to a `DataSet` back to the data source, see [Update data sources with DataAdapters](update-data-sources-with-dataadapters.md).

You might want to map output parameters back to the updated row of a `DataSet`. One common task would be retrieving the value of an automatically generated identity field or time stamp from the data source. The <xref:System.Data.Common.DbCommandBuilder> will not map output parameters to columns in an updated row by default. In this instance, you must specify your command explicitly. For an example of mapping an automatically generated identity field back to a column of an inserted row, see [Retrieve identity or autonumber values](retrieve-identity-or-autonumber-values.md).

## Rules for automatically generated commands

The following table shows the rules for how automatically generated commands are generated.

|Command|Rule|  
|-------------|----------|  
|`InsertCommand`|Inserts a row at the data source for all rows in the table with a <xref:System.Data.DataRow.RowState%2A> of <xref:System.Data.DataRowState.Added>. Inserts values for all columns that are updateable (but not columns such as identities, expressions, or timestamps).|  
|`UpdateCommand`|Updates rows at the data source for all rows in the table with a `RowState` of <xref:System.Data.DataRowState.Modified>. Updates the values of all columns except for columns that are not updateable, such as identities or expressions. Updates all rows where the column values at the data source match the primary key column values of the row, and where the remaining columns at the data source match the original values of the row. For more information, see "Optimistic Concurrency Model for Updates and Deletes," later in this topic.|  
|`DeleteCommand`|Deletes rows at the data source for all rows in the table with a `RowState` of <xref:System.Data.DataRowState.Deleted>. Deletes all rows where the column values match the primary key column values of the row, and where the remaining columns at the data source match the original values of the row. For more information, see [Optimistic Concurrency Model for Updates and Deletes](#optimistic-concurrency-model-for-updates-and-deletes), later in this topic.|

## Optimistic concurrency model for updates and deletes

The logic for generating commands automatically for UPDATE and DELETE statements is based on *optimistic concurrency*, that is, records are not locked for editing and can be modified by other users or processes at any time. Because a record could have been modified after it was returned from the SELECT statement, but before the UPDATE or DELETE statement is issued, the automatically generated UPDATE or DELETE statement contains a WHERE clause, specifying that a row is only updated if it contains all original values and has not been deleted from the data source. This is done to avoid overwriting new data.
 
> [!NOTE]
> Where an automatically generated update attempts to update a row that has been deleted or that does not contain the original values found in the <xref:System.Data.DataSet>, the command does not affect any records, and a <xref:System.Data.DBConcurrencyException> is thrown.

If you want the UPDATE or DELETE to complete regardless of original values, you must explicitly set the `UpdateCommand` for the `DataAdapter` and not rely on automatic command generation.

## Limitations of automatic command generation logic

The following limitations apply to automatic command generation.

### Unrelated tables only

The automatic command generation logic generates INSERT, UPDATE, or DELETE statements for stand-alone tables without taking into account any relationships to other tables at the data source. As a result, you may encounter a failure when calling `Update` to submit changes for a column that participates in a foreign key constraint in the database. To avoid this exception, do not use the <xref:System.Data.Common.DbCommandBuilder> for updating columns involved in a foreign key constraint; instead, explicitly specify the statements used to perform the operation.

### Table and column names

Automatic command generation logic may fail if column names or table names contain any special characters, such as spaces, periods, quotation marks, or other nonalphanumeric characters, even if delimited by brackets. Depending on the provider, setting the QuotePrefix and QuoteSuffix parameters may allow the generation logic to process spaces, but it cannot escape special characters. Fully qualified table names in the form of *catalog.schema.table* are supported.

## Use the CommandBuilder to automatically generate an SQL statement

To automatically generate SQL statements for a `DataAdapter`, first set the `SelectCommand` property of the `DataAdapter`, then create a `CommandBuilder` object, and specify as an argument the `DataAdapter` for which the `CommandBuilder` will automatically generate SQL statements.

[!code-csharp[SqlCommandBuilder_Create#1](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#1)]

## Modify the SelectCommand

If you modify the `CommandText` of the `SelectCommand` after the INSERT, UPDATE, or DELETE commands have been automatically generated, an exception may occur. If the modified `SelectCommand.CommandText` contains schema information that is inconsistent with the `SelectCommand.CommandText` used when the insert, update, or delete commands were automatically generated, future calls to the `DataAdapter.Update` method may attempt to access columns that no longer exist in the current table referenced by the `SelectCommand`, and an exception will be thrown.

You can refresh the schema information used by the `CommandBuilder` to automatically generate commands by calling the `RefreshSchema` method of the `CommandBuilder`.

If you want to know what command was automatically generated, you can obtain a reference to the automatically generated commands by using the `GetInsertCommand`, `GetUpdateCommand`, and `GetDeleteCommand` methods of the `CommandBuilder` object and checking the `CommandText` property of the associated command.

The following code example writes to the console the update command that was automatically generated.

[!code-csharp[SqlCommandBuilder_Create#2](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#2)]

The following example recreates the table in the dataset. The **RefreshSchema** method is called to refresh the automatically generated commands with this new column information.

[!code-csharp[SqlCommandBuilder_Create#3](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#3)]

## See also

- [Commands and parameters](commands-parameters.md)
- [Executing a command](execute-command.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
