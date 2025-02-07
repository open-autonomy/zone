# ActivateZoneResponseV1


|Sender| Triggered by | Triggers|
|---|---|---|
| `AHS` | Each truck that has acknowledge the Zone Request  | |

<br>

## Message attributes
| Key               |   Format    | Description |
|------------------|----------|-------------|
| `"EquipmentId"`    | UUID              | The unique ID of the equipment responded. |
| `"ZoneId"` | UUID   | The ID of the specific Zone that the response is for. |
| `"Version"`  | Integer    | The version for the zone|
| `"Status"` | String     | Should be `Accepted` or `Rejected`  |


## Example of ActivateZoneResponseV1
```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "71b0ac40-5b73-4444-ac37-dbf7e51e8186",
    "Timestamp": "2024-08-23T07:20:35.344Z",
    "ActivateZoneResponseV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
        "ZoneId": "00000000-0000-0000-0000-000000000001",
        "Version": 5,
        "Status": "Accepted"
    }
}
```
