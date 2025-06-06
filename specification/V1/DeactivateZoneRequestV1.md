# DeactivateZoneRequestV1

This message is sent by the FMS to AHS to indicate a policy have been removed, and equipment will no longer need to adher to.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `FMS`  | Deletion of policy zone | AHS equipment to accept the deactive zone request, and response with `DeactivateZoneResponseV1` message |

## Message Attributes

The `DeactivateZoneRequestV1` message consist the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"RequestId"` | RequestId | UUID | True | The request ID of the message |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the truck is responding to |

**NOTE**: the top-level message headers should contain the `EquipmentId`, indicating which equipment the `deactivateZonesRequestV1` message is for. 


## Examples
### Typical Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "DeactivateZoneRequestV1": {
    "RequestId": "331f14b1-ef84-4e11-9271-4aabe44414e1",
    "ZoneId": "123e4567-e89b-12d3-a456-426614174000",
  }
}
```
