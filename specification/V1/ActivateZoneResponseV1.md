# ActivateZoneReposneV1

This message is sent by the AHS in response to the `ActivateZoneRequestV1` or `ActivateAllZonesRequestV1` messages

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `ActivateAllZonesRequestV1` or `ActivateZoneRequestV1` |  |


## Message Attributes

The `ActivateZoneReposneV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the truck is responding to |
| `"Status"` | [`Accepted`, `Activated`, `Rejected`] | String | True | Determine whether the equipment have accepted, activated or rejected the policy zone request.<br/>- `Accepted`: Unable to adher to policy zone as of now, but will comply when able  to.<br/>- `Activated`: Adher to policy zone.<br/>- `Rejected`: Cannot adher to policy zone |
| `"Reason"` | String Enum | String | False | The reason for rejecting policy zone request |

**NOTE**: the top-level message headers should contain the `EquipmentId` which indicate the origin equipment of the `ActivateZoneResponseV1` message 


## Examples
### Typical Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "ActivateZoneResponseV1": {
    "ZoneId": "123e4567-e89b-12d3-a456-426614174000",
    "Status": "Activated"
  }
}
```

### Rejection Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "ActivateZoneResponseV1": {
    "ZoneId": "123e4567-e89b-12d3-a456-426614174000",
    "Status": "Rejected",
    "Reason": "UnexpectedOffline"
  }
}
```