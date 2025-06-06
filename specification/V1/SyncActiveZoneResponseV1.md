# SyncActiveZonesResponseV1

This message is sent by the AHS in response to the `SyncActiveZonesRequestV1` messages.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `SyncActiveZonesRequestV1` |  |


## Message Attributes

The `SyncActiveZonesResponseV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"RequestId"` | RequestId | UUID | True |The request ID of the corresponding request message |
| `"Status"` | [`Accepted`, `Rejected`] | String | True | Determine whether the equipment have accepted the policy zones request. <br/> - `Accepted`: The equipment can and adhere to the policy zonees. <br/> - `Rejected`: Cannot adhere to the policy. The equipment cannot move until it can adhere to the policies |
| `"ActivatedZones"` | Array[`ZoneIdObject`] | Array[] | True | A list of `ZoneIdObject` that indicates the zones that have been activated by the equipment |

**NOTE**: the top-level message headers should contain the `EquipmentId` which indicate the origin equipment of the `SyncActiveZonesResponseV1` message

### ZoneIdObject
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id |


## Examples
### Typical Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "SyncActiveZonesResponseV1": {
    "Status": "Accepted",
    "ActivatedZones": [
      {
        "ZoneId": "00000000-0000-0000-0000-000000000001"
      },
      {
        "ZoneId": "00000000-0000-0000-0000-000000000002"
      },
      {
        "ZoneId": "00000000-0000-0000-0000-000000000003"
      }
    ]
  } 
}
```
