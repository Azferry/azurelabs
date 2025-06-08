# Resource Graph - Network Related

## Network Interfaces

Lists all IP addresses in the environment that have static or dynamic address

```kusto
resources
| where type == "microsoft.network/networkinterfaces"
| mv-expand properties['ipConfigurations']
| project resourceGroup, name, properties_ipConfigurations.properties.privateIPAllocationMethod, properties_ipConfigurations.properties.privateIPAddress
```

Shows the network interfaces that have a static ip address

```kusto
resources
| project  name, type, location, resourceGroup, subscriptionId, properties
| where properties contains "PrivateIPAllocationMethod"
| parse properties with * 'IPAllocationMethod":"' AllocationMethod '"' *
```

## Public IP

Lists all Azure resources that have the word 'publicIPAddresses' in the type.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
```
