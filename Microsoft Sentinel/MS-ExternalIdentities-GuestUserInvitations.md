# Guest User Invitations


```Kusto
AuditLogs
| where TimeGenerated > ago(30d)
    | where ActivityDisplayName == "Invite external user"
    | extend initiatedBy = tostring(parse_json(tostring(InitiatedBy.user)).userPrincipalName)
    | extend InvitedUser = tostring(AdditionalDetails[5].value)
    | extend ObjectId = tostring(TargetResources[0].id)
    | extend RefObjectId = tostring(parse_json(tostring(InitiatedBy.user)).id)
    | project TimeGenerated,initiatedBy, InvitedUser, ObjectId, RefObjectId
    ``` 
