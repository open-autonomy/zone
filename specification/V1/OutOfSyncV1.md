# OutOfSyncV1

This message is sent by the AHS to FMS to notify that the equipment does not have the up-to-date policy zones information, and requires FMS to send the policy zones.

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | AHS equipment re-connecting with mis-matched policy zones | FMS to send `ActivateAllZoneRequestV1` |

## Message Attributes

The `OutOfSyncV1` message consist the following properties

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"RequestId"` | RequestId | UUID | True |The request ID of the message |

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
        "RequestId": "331f14b1-ef84-4e11-9271-4aabe44414e1"
    }
}
```