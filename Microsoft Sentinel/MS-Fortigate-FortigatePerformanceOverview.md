# Fortigate Performance Overview

```Kusto
CommonSecurityLog
| where DeviceEventClassID == "40704"
| extend CPUusageSTR = extract('FTNTFGTcpu=[0-9]{1,2}',0,AdditionalExtensions)
| extend MEMUsageStr = extract('FTNTFGTmem=[0-9]{1,2}',0,AdditionalExtensions)
| extend TotalSessionSTR = extract('FTNTFGTtotalsession=[0-9]{1,4}',0,AdditionalExtensions)
| extend FreeDiskStorageSTR = extract('FTNTFGTfreediskstorage=[0-9]{1,4}',0,AdditionalExtensions)
| extend CPUUsage = extract('[0-9]{1,2}',0,CPUusageSTR)
| extend MEMUsage = extract('[0-9]{1,2}',0,MEMUsageStr)
| extend TotalSessions = extract('[0-9]{1,4}',0,TotalSessionSTR)
| extend FreeDiskStorage = extract('[0-9]{1,4}',0,FreeDiskStorageSTR)
| project TimeGenerated, CPUUsage, MEMUsage, TotalSessions, FreeDiskStorage
```

### Can be visualized with a Workbook
