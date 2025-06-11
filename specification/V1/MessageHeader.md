# Message Header

This document describes the top-level data structure that serves as a message header for all other messages defined within this specification.

## Message Attributes
| Key | Value | Format | Required | Description |
| --- |:---:|:---:|:---:| --- |
| `"Protocol"` | `"Open-Autonomy"` | string | True | The protocol of the messages. |
| `"Version"` | 1 | integer | True | The version of the protocol. |
| `"Timestamp"` | DateTime | ISO 8601 | True | The date-time of when the message is sent in ISO 8601 format. |
| `"EquipmentId"` | EquipmentId | UUID | False | The UUID identifying the equipment defined in the fleet definition, indicating the equipment that the message is intended for or from. This shall be in all messages except for ServiceIdentification |

### Message Header Examples
An example `ActivateZoneRequestV1` enclosed in the message header data structure

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2018-10-31T09:30:10.435Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "ActivateZoneRequestV1": {...}
}
```