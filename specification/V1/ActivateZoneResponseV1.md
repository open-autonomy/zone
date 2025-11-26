# ActivateZoneResponseV1

This message is sent by the Autonomous Haulage System (AHS) in response to the `ActivateZoneRequestV1` messages from the Fleet Management System (FMS), indicating whether the Autonomous Vehicle (AV) has accepted, activated or rejected the policy zone request.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `ActivateZoneRequestV1` |  |


## Message Attributes

The `ActivateZoneResponseV1` message consists of the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the truck is responding to |
| `"Status"` | [`Pending`, `Activated`, `Rejected`] | String | True | Determine whether the AV have pending, activated or rejected the policy zone request.<br/>- `Pending`: The AV has received the policy zone and has scheduled it for activation.<br/>- `Activated`: The AV has activated the policy zone as it is now adhering to the associated policies.<br/>- `Rejected`: The AV is unable to activate the policy zone due to an error explained in `"Reason"` |
| `"Reason"` | String Enum | String | False | { DuplicateZoneId, MissingZoneId, MissingPolicies, NonClosedPolygon, TooFewCoordinates, TooManyCoordinates, RobotFailure, Timeout, UnknownZoneRejection, UnexpectedOffline } |

> [!IMPORTANT]

- AV shall only `Reject` a policy zone request if there is an error preventing it from processing the policy zone request.
- An AV may choose to send an `Activate` response without sending a `Pending` response first. In this case, the AV is indicating that it has activated the policy zone and is adhering to the specified policies. An AV shall send a `Pending` response if it cannot immediately adhere to the policy zone.

> AV shall only `Reject` a policy zone request if there is an error preventing it from processing the policy zone request.

> [!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AV of the `ActivateZoneResponseV1` message


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