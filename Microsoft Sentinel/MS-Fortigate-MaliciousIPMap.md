# Malicious IP Map

```Kusto
CommonSecurityLog
| extend TrafficDirection = iff(CommunicationDirection != "Outbound","InboundOrUnknown", "Outbound"), Country=MaliciousIPCountry, Latitude=MaliciousIPLatitude, Longitude=MaliciousIPLongitude, Confidence=ThreatDescription, Description=ThreatDescription
| where isnotempty(MaliciousIP) and isnotempty(Country) and isnotempty(Latitude) and isnotempty(Longitude)
| summarize total = count(), arg_max(TimeGenerated,TimeGenerated) by Country, Latitude , Longitude
```
