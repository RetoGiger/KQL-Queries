# Overivew of the Devices with disabled Defender

```Kusto
// Devices where Antivirus is disabled
DeviceInfo
| summarize arg_max(Timestamp, *) by DeviceId 
| project DeviceId,Timestamp,DeviceName,ClientVersion,OnboardingStatus,DeviceType,MachineGroup
| project-rename LatestDeviceData = Timestamp
| join kind = inner ( 
    DeviceTvmSecureConfigurationAssessment 
    | where ConfigurationId == "scid-2010"
    | project DeviceId,Timestamp,ConfigurationId,ConfigurationSubcategory, IsApplicable,IsCompliant,Context 
    | project-rename TimeStampTVMEval = Timestamp
    | join kind = inner (
        DeviceTvmSecureConfigurationAssessmentKB
        | project ConfigurationId,ConfigurationName, ConfigurationDescription
        ) on ConfigurationId 
) on DeviceId
| where IsApplicable == 1 and   IsCompliant == 0
```
