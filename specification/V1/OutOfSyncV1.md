# OutOfSyncV1

This message is sent by the AHS to FMS to notify that the equipment cannot guarantee that it has an up-to-date list of active policy zones, and requires FMS to send the current set of active zones through a `SyncActiveZonesRequestV1`. Vehicles that are out of sync with the active zones cannot operate until they have received and internally activated these zones.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | AHS equipment re-connecting with mis-matched policy zones | FMS to send `SyncActiveZonesRequestV1` |

## Message Attributes

The `OutOfSyncV1` message does not contain any additional attributes beyond the standard message headers.

**NOTE**: the top-level message headers should contain the `EquipmentId` which indicate the origin equipment of the `OutOfSyncV1` message 

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