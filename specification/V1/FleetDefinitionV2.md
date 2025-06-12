# FleetDefinitionV2

The `FleetDefinitionV2` message is used in to inform the FMS of the set of AVs managed by the AHS to allow the FMS to correctly manage policy zone synchronization with the AVs in the fleet. This message is sent by the AHS to the FMS when the AHS connects to the FMS and whenever an AV is commissioned or decommissioned from the fleet.

> [!IMPORTANT]
> The `FleetDefinitionV2` message is part of the ISO 23725 standard for the exchange of fleet definition information between AHS and FMS. For more information on the ISO 23725 standard, see the [ISO 23725 documentation](https://www.iso.org/standard/73766.html).

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | Connection | The AHS connects to the FMS and sends the `FleetDefinitionV2` message. |
| `AHS`  | AV Commissoned and added to the fleet | The AHS sends an updated `FleetDefinitionV2` message to the FMS. |
| `AHS`  | AV Decommissioned and removed from the fleet | The AHS sends an updated `FleetDefinitionV2` message to the FMS. |

> [!IMPORTANT]
> A vehicle being powered off should not trigger a new `FleetDefinitionV2` message. The AHS shall only send an updated `FleetDefinitionV2` message when an AV is commissioned or decommissioned.

## Message Attributes

As the `FleetDefinitionV2` message is part of the ISO 23725 standard, it follows the structure and requirements outlined in that standard. This includes message headers that differ from the remaining messages in this specification. The message headers include the protocol, version, timestamp, while excluding the `EquipmentId` as it is not applicable for this message.

### Message Headers
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"Protocol"` | Protocol | String | True | The protocol of the messages. This value can be either `ISO23725` or `OpenAutonomy`. |
| `"Version"` | Version | Integer | True | The version of the protocol, which is `1` for this message. |
| `"Timestamp"` | Timestamp | ISO 8601 UTC | True | The date-time of when the message is sent in ISO 8601 format. |
| `"FleetDefinitionV2"` | Fleet Definition | Object | True | The fleet definition object containing the fleet information. |

### Fleet Definition Object
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"AHSId"` | Fleet ID | UUID | True | The unique identifier for the fleet managed by the AHS. |
| `"Equipment"` | Equipment | Array[Equipment] | True | The complete set of AVs comprising the fleet managed by the AHS. |


### Equipment Object
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"EquipmentId"` | Equipment ID | UUID | True | The unique identifier for the AV. |
| `"HID"` | Hardware ID | String | True | The hardware identifier for the AV, which may be used for diagnostics or maintenance purposes. |
| `"Type"` | Equipment Type | String | True | The type of the AV, e.g., `Hauler`, `Dozer`, etc. |
| `"OEM"` | OEM | String | True | The original equipment manufacturer of the AV. |
| `"Model"` | Model | String | True | The model of the AV. |
| `"Autonomous"` | Autonomous | Boolean | True | Indicates whether the AV is autonomous or not. |
| `"Length"` | Length | Number | True | The length of the AV in meters. |
| `"Width"` | Width | Number | True | The width of the AV in meters. |

## Examples
### Typical Message
```JSON
{
	"Protocol":"ISO23725",
	"Version": 1,
	"Timestamp": "2024-08-23T08:19:55.621Z",
	"FleetDefinitionV2": {
		"AHSId": "f1234567-e89b-12d3-a456-426614174000",
		"Equipment": [
			{
				"EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
				"HID": "HID12345",
				"Type": "Hauler",
				"OEM": "OEM Inc.",
				"Model": "Model X",
				"Autonomous": true,
				"Length": 12.5,
				"Width": 3.5
			},
			{
				"EquipmentId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
				"HID": "HID67890",
				"Type": "Dozer",
				"OEM": "Another OEM Inc.",
				"Model": "Model Y",
				"Autonomous": true,
				"Length": 10.0,
				"Width": 3.0
			}
		]
	}
}
```