# ActivateAllZonesResponseV1

This message is sent by the AHS in response to the `ActivateAllZonesRequestV1` messages

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `ActivateAllZonesRequestV1` |  |


## Message Attributes

The `ActivateAllZonesResponseV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"RequestId"` | RequestId | UUID | True |The request ID of the corresponding request message |
| `"Status"` | [`Accepted`, `Activated`, `Rejected`] | String | True | Determine whether the equipment have accepted the policy zones request|
| `"ActivatedZones"` | Array[`ZoneIdObject`] | Array[] | True | A list of `ZoneIdObject` that indicates the zones that have been activated by the equipment |


**NOTE**: the top-level message headers should contain the `EquipmentId` which indicate the origin equipment of the `ActivateAllZonesResponseV1` message 

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
  "ActivateAllZonesResponseV1": {
    "RequestId": "331f14b1-ef84-4e11-9271-4aabe44414e1",
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
