# Log Analytics Cost Management

```kusto
// Get all computers
find where TimeGenerated between(startofday(ago(1d))..startofday(now())) project _BilledSize, _IsBillable, Computer, Type
| where _IsBillable == true and Type != "Usage"
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by computerName
| sort by BillableDataBytes desc nulls last
```

```kusto
// #Grabs individual computer stats
find where TimeGenerated between(startofday(ago(1d))..startofday(now())) project _BilledSize, _IsBillable, Computer, Type
| where _IsBillable == true and Type != "Usage"
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName == 'CHANGETOVMNAME'
| summarize BillableDataBytes = sum(_BilledSize) by  Type
| sort by BillableDataBytes desc nulls last
```