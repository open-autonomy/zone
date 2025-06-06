# ActivateZoneResponseV1

This message is sent by the AHS in response to the `ActivateZoneRequestV1` messages

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | `ActivateZoneRequestV1` |  |


## Message Attributes

The `ActivateZoneResponseV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | The policy zone id in which the truck is responding to |
| `"Status"` | [`Accepted`, `Activated`, `Rejected`] | String | True | Determine whether the equipment have accepted, activated or rejected the policy zone request.<br/>- `Accepted`: The equipment has processed the policy zone and has scheduled it for activation.<br/>- `Activated`: The equipment has activated the policy zone as it is now adhering to the associated policies.<br/>- `Rejected`: The equipment is unable to activate the policy zone due to an error explained in `"Reason"` |
| `"Reason"` | String Enum | String | False | The reason for rejecting policy zone request |

> [!IMPORTANT]
> The equipment may respond with an `Activated` status if it is in a position to immediately adhere to the policy zone policies, or it may respond with an `Accepted` status if it needs to (or is permitted to via the `activationDeadline`) schedule the activation for a later time. The equipment should not send a response with `Rejected` status unless it cannot adhere to the policy zone at all.

> [!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin equipment of the `ActivateZoneResponseV1` message 


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