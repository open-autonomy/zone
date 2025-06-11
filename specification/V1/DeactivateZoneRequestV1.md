# DeactivateZoneRequestV1

This message is sent by the Fleet Management System (FMS) to Autonomous Haulage System (AHS) to indicate a policy zone has been flagged for deletion, and Autonomous Haulage Truck (AHT) should remove it from its own internal set of active policy zones.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `FMS`  | Deletion of policy zone | AHT to internally deactivate the zone, and response with `DeactivateZoneResponseV1` message |

## Message Attributes

The `DeactivateZoneRequestV1` message consist the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the AHT is responding to |

>[!NOTE]
> The top-level message headers should contain the `EquipmentId`, indicating which AHT the `deactivateZonesRequestV1` message is for. 


## Examples
### Typical Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "DeactivateZoneRequestV1": {
    "ZoneId": "123e4567-e89b-12d3-a456-426614174000",
  }
}
```
