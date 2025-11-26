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
| `"Reason"` | String Enum | String | Conditional | Required if `Status` = `Rejected`. { MultipleZoneRejections, DuplicateZoneId, MissingZoneId, MissingPolicies, NonClosedPolygon, TooFewCoordinates, TooManyZones, TooManyCoordinates, RobotFailure, Timeout, UnknownZoneRejection } |
| `"RejectedZones"` | Array[`RejectedZoneObject`] | Array[] | False | Granular list of zones the AV failed to activate along with per-zone rejection reasons. Present only when some, but not all, zones failed. |

>[!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AV of the `SyncActiveZonesResponseV1` message

### RejectedZoneObject
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"ZoneId"` | ZoneId | UUID | True | Identifier of the zone that failed activation |
| `"Reason"` | String Enum | String | True | { DuplicateZoneId, MissingZoneId, MissingPolicies, NonClosedPolygon, TooFewCoordinates, TooManyCoordinates, RobotFailure, Timeout, UnknownZoneRejection } |

> [!TIP]
> Use `Reason` at the top level for a single global failure (e.g. `MaxActiveZonesExceeded`). Use `RejectedZones[i].Reason` for individual zones when some, but not all, failed. If both are present, the top-level `Reason` should summarize while per-zone reasons provide detail.

## Examples
### Activated (All Zones Succeeded)
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

### Rejected (Global Reason Only)
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "SyncActiveZonesResponseV1": {
    "ResponseId": "00000000-0000-0000-0000-000000000001",
    "Status": "Rejected",
    "Reason": "TooManyZones"
  }
}
```

### Rejected (Per-Zone Granular Reasons)
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2021-09-01T12:00:00Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "SyncActiveZonesResponseV1": {
    "ResponseId": "00000000-0000-0000-0000-000000000001",
    "Status": "Rejected",
    "Reason": "MultipleZoneRejections",
    "RejectedZones": [
      { "ZoneId": "00000000-0000-0000-0000-000000000001", "Reason": "DuplicateZoneId" },
      { "ZoneId": "00000000-0000-0000-0000-000000000002", "Reason": "MissingPolicies" }
    ]
  }
}