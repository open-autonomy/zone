# ActivateZoneResponseV1

This message is sent by the Autonomous Haulage System (AHS) in response to the `ActivateZoneRequestV1` messages from the Fleet Management System (FMS), indicating whether the Autonomous Haulage Truck (AHT) has accepted, activated or rejected the policy zone request.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `ActivateZoneRequestV1` |  |


## Message Attributes

The `ActivateZoneResponseV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the truck is responding to |
| `"Status"` | [`Accepted`, `Activated`, `Rejected`] | String | True | Determine whether the AHT have accepted, activated or rejected the policy zone request.<br/>- `Accepted`: The AHT has processed the policy zone and has scheduled it for activation.<br/>- `Activated`: The AHT has activated the policy zone as it is now adhering to the associated policies.<br/>- `Rejected`: The AHT is unable to activate the policy zone due to an error explained in `"Reason"` |
| `"Reason"` | String Enum | String | False | The reason for rejecting policy zone request |

> [!IMPORTANT]
> AHT may choose to send an `Activate` response without sending an `Accepted` response first. In this case, the AHT is indicating that it has activated the policy zone and is adhering to the associated policies. AHT shall send an `Accepted` response if it needs to schedule the activation of the policy zone for a later time. AHT shall only `Reject` a policy zone request if there is an error preventing it from processing the policy zone request.

> [!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AHT of the `ActivateZoneResponseV1` message 


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