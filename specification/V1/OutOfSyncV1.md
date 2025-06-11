# OutOfSyncV1

This message is sent by the Autonomous Haulage System (AHS) to Fleet Management System (FMS) to notify that the Autonomous Haulage Truck (AHT) cannot guarantee that it has an up-to-date list of active policy zones (e.g., the AHT was previously offline), and requires the FMS to send the current set of active zones through a `SyncActiveZonesRequestV1`. 

> [!IMPORTANT]
> AHT that are out of sync with the active zones must not operate until they have received and internally activated these zones.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | AHT re-connecting to AHS with outdated policy zones | FMS to send `SyncActiveZonesRequestV1` |

## Message Attributes

The `OutOfSyncV1` message does not contain any additional attributes beyond the standard message headers.

>[!NOTE]
> The top-level message headers should contain the `EquipmentId` which indicate the origin AHT of the `OutOfSyncV1` message 

## Examples
### Typical Message
```JSON
{
    "Protocol": "Open-Autonomy",
    "Version": 1,
    "Timestamp": "2024-08-23T08:19:55.621Z",
    "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
    "OutOfSyncV1": {
    }
}
```