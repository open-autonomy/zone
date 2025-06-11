# DeactivateZoneResponseV1

This message is sent by the Autonomous Haulage System (AHS) in response to the `DeactivateZoneRequestV1` message from the Fleet Management System (FMS), indicating whether the Autonomous Haulage Truck (AHT) was successful in removing the zone from its internal set of active policy zones zones.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `DeactivateZoneRequestV1` |  |

## Message Attributes

The `DeactivateZoneResponseV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the truck is responding to |
| `"Status"` | [`Accepted`] | String | True |  |

>[!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AHT of the `DeactivateZoneResponseV1` message 

## Examples
### Typical Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "DeactivateZoneResponseV1": {
    "ZoneId": "123e4567-e89b-12d3-a456-426614174000",
    "Status": "Accepted"
  }
}
```
