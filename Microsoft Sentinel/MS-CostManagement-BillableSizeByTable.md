# Print Amount of billable Data


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
