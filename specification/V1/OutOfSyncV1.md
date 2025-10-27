# OutOfSyncV1

This message is sent by the Autonomous Haulage System (AHS) to Fleet Management System (FMS) to notify that the Autonomous Vehicle (AV) cannot guarantee that it has an up-to-date list of active policy zones (e.g., the AV was previously offline), and requires the FMS to send the current set of active zones through a `SyncActiveZonesRequestV1`. 

> [!IMPORTANT]
> AV that are out of sync with the active zones must not operate until they have received and internally activated these zones.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | AV re-connecting to AHS with outdated policy zones | FMS to send `SyncActiveZonesRequestV1` |

## Message Attributes

The `OutOfSyncV1` message consists of the following property:

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"EventId"` | EventId | UUID | True | Correlation identifier for this out-of-sync notification. The FMS SHALL send exactly one `SyncActiveZonesRequestV1` per distinct `EventId` and SHALL reuse this value as `SyncActiveZonesRequestV1.RequestId`. If multiple `OutOfSyncV1` messages with the same `EventId` are received (duplicates/retries), the FMS MUST NOT issue additional sync requests and MAY ignore them after confirming the initial request was sent. |


>[!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AV of the `OutOfSyncV1` message 

## Examples
### Typical Message
```JSON
{
    "Protocol": "Open-Autonomy",
    "Version": 1,
    "Timestamp": "2024-08-23T08:19:55.621Z",
    "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
    "OutOfSyncV1": {
        "EventId": "00000000-0000-0000-0000-000000000001"
    }
}
```