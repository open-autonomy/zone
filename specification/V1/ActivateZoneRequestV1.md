# ActivateZoneRequestV1

This message is sent by the FMS to the AHS to indicate a policy zone have been created or updated

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `AHS`  | Policy Zone creation or updates | The equipment to start accepting and adher to the policy zones, and fire off `ActivateZoneResponseV1` messages |

## Message Attributes

The `ActivateZoneReposneV1` message consist of the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"RequestId"` | RequestId | UUID | True | The request ID of the message |
| `"Zones"` | | Array[Zone] | True | A single [GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946) object consist of the following properties. |

### Zone Object
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"type"` | `Feature` | String | True | The feature type |
| `"geometry"` | Geometry | Object | True | A GeoJSON compatible geometry object |
| `"properties"` | Properties | Object | True | A GeoJSON compatible properties object |

### Geometry Object
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"type"` | `Polygon` | String | True | The geometry type that conforms with GeoJSON geometry `Polygon` |
| `"coordinates"` |  | Array[Array[Array[Number, Number, Number]]] | True | A GeoJSON compatible polygon geometry coordinates. **NOTE** each coordinate must consist of 3 number, [longitude, latitude, elevation]. See [GeoJSON Geometry Object](https://datatracker.ietf.org/doc/html/rfc7946#section-3.1) |


### Properties Object
The the attributes within the `Properties` object depends on the `policyType`

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"id"` | ZoneId | String | True | The policy zone id |
| `"name"` |  | String | True | The name of the policy zone |
| `"activateDeadline"` | DateTime | ISO8601 UTC | False | The time when the equipment should start acting on the policy zone by. 
**NOTE** it is a soft indication to suggest the equipment to start adhering to the policy, and not a hard command. |
| `"policies"` | Policies | Object | True | A set of policies that the equipment should adhere to within the zone. |

**TODO** add different zone types here still ....


## Examples
### Typical Message
```JSON
{
    "Protocol": "Open-Autonomy",
    "Version": 1,
    "Timestamp": "2021-09-01T12:00:00Z",
    "ActivateZoneRequestV1": {
        "RequestId": "331f14b1-ef84-4e11-9271-4aabe44414e1",
        "Zone": {
            "type": "Feature",
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                [
                    [
                        59.154612700275194,
                        17.62123606784992,
                        0
                    ],
                    [
                        59.15444657134832,
                        17.621361182777765,
                        0
                    ],
                    [
                        59.154458381940245,
                        17.62176503107635,
                        0
                    ],
                    [
                        59.154774479447724,
                        17.621645401146836,
                        0
                    ],
                    [
                        59.154612700275194,
                        17.62123606784992,
                        0
                    ]
                ]
                ]
            },
            "properties": {
                "id": "00000000-0000-0000-0000-000000000001",
                "name": "grading 1",
                "policies": {
                    "Exclusion": { }
                },
                "activationDeadline": "2024-04-04T06:05:47Z"
            }
        }
    }
}
```
