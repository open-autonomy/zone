# SyncActiveZonesResponseV1

This message is sent by the Autonomous Haulage System (AHS) in response to the `SyncActiveZonesRequestV1` messages from Fleet Management System  (FMS). The Autonomous Vehicle (AV) **MUST** internally activate all the zones provided in the `SyncActiveZonesRequestV1` message in order to operate. If any zone cannot be activated, the AV should reject the request using the `SyncActiveZonesResponseV1` message and **MUST** not operate.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `SyncActiveZonesRequestV1` |  |


## Message Attributes

The `SyncActiveZonesResponseV1` message consists of the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ResponseId"` | ResponseId | UUID | True | Correlation identifier for this response. It SHALL be identical to the `SyncActiveZonesRequestV1.RequestId` it answers. Duplicate responses with the same `ResponseId` are idempotent and MAY be ignored after the first is processed. |
| `"Status"` | [`Activated`, `Rejected`] | String | True | Indicates whether the AV has successfully activated the received policy zones. <br/> - `Activated`: The AV has activated all zones and is adhering to their associated policies. <br/> - `Rejected`: The AV cannot adhere to one or more of the policies. In this case, the AV must not operate as it cannot guarantee safety. |
| `"Reason"` | String Enum | String | False | The reason for rejecting the policy zone set when `Status` = `Rejected`. |

>[!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AV of the `SyncActiveZonesResponseV1` message

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
    "ResponseId": "00000000-0000-0000-0000-000000000001",
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
  "SyncActiveZonesResponseV1": {
    "ResponseId": "00000000-0000-0000-0000-000000000001",
    "Status": "Rejected",
    "Reason": "MaxActiveZonesExceeded"
  }
}
```