# ActivateZoneRequestV1

This message is sent by the FMS to the AHS to indicate a policy zone has has been created on the FMS which the equipment is expected to adhere to. The equipment should then respond with an `ActivateZoneResponseV1` message indicating whether it has been accepted, activated or rejected the policy zone request (see `ActivateZoneResponseV1` for a description of response types).

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `FMS`  | Policy Zone creation or updates | The equipment to start accepting and adher to the policy zones, and fire off `ActivateZoneResponseV1` messages |

## Message Attributes

The `ActivateZoneResponseV1` message consist of the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"Zones"` | | Array[Zone] | True | A single [GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946) object consist of the following properties. |

**NOTE**: the top-level message headers should contain the `EquipmentId`, indicating which equipment the `ActivateZoneRequestV1` message is for. 

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
| `"activateDeadline"` | DateTime | ISO8601 UTC | False | Indicates when equipment the latest time by which equipment that has accepted a policy zone should transition to activating it **NOTE** This is a soft deadline, equipment should aim to adhere to the policy by this time but is not strictly required to do so if it is not possible or safe to do so. |
| `"policies"` | Policies | Object | True | A set of policies that the equipment should adhere to within the zone. |

**TODO** add different zone types here still ....


## Examples
### Typical Message
```JSON
{
    "Protocol": "Open-Autonomy",
    "Version": 1,
    "Timestamp": "2021-09-01T12:00:00Z",
    "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
    "ActivateZoneRequestV1": {
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
