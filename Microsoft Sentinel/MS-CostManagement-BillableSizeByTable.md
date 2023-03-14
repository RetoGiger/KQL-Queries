# Print Amount of billable Data

## List Costs for each table 

```Kusto
union withsource=TableName1 *
| where TimeGenerated {TimeRange:query}
| project _BilledSize, _IsBillable, TimeGenerated, TableName1
| summarize Entries = count(), Size = sum(_BilledSize), last_log = datetime_diff("second",now(), max(TimeGenerated)), estimate  = sumif(_BilledSize, _IsBillable==true)  by TableName1, _IsBillable
| project ['Table Name'] = TableName1, ['Table Size'] = Size, ['Table Entries'] = Entries,
          ['Size per Entry'] = 1.0 * Size / Entries, ['IsBillable'] = _IsBillable, ['Latest Record Created'] =  last_log, ['Estimated Table Price'] =  (estimate/(1024*1024*1024)) * 2.94 //, ['Latest Record Recieved'] =last_ingestion
 | order by ['Table Size']  desc
```


## Single Table

```Kusto
let Tablename = "CommonSecurityLog";
Usage
| where TimeGenerated > ago(30d)
| where IsBillable == true
| where DataType == Tablename
| summarize TotalVolumeGB = sum(Quantity) / 1024
```

## TO list all Tables

```Kusto
let allTables = Usage
| where TimeGenerated >ago(1d)
| distinct DataType;
allTables!
```
