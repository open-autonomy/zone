# ActivateZoneRequestV1

This message is sent by the Fleet Management System (FMS) to the Autonomous Haulage System (AHS) to indicate a policy zone has been created in the FMS which the Autonomous Vehicles (AV) are expected to adhere to. Each AV should then respond with an `ActivateZoneResponseV1` message indicating whether it has accepted, activated or rejected the policy zone request (see `ActivateZoneResponseV1` for a description of response types).

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `FMS`  | Policy Zone creation or updates | The AV to start accepting and adher to the policy zones, and fire off `ActivateZoneResponseV1` messages |

## Message Attributes

The `ActivateZoneRequestV1` message consists of the following properties.

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"Zone"` | Zone | Object | True | A single [GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946) object consist of the following properties. |

>[!NOTE]
> The top-level message headers should contain the `EquipmentId`, indicating which AV the `ActivateZoneRequestV1` message is for.

### Zone Object
The Zone object is a GeoJSON [RFC7946](https://datatracker.ietf.org/doc/html/rfc7946) compatible feature object that describes the policy zone. It contains the following properties:
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"id"` | ZoneId | String | True | A unique identifier for the policy zone |
| `"type"` | `Feature` | String | True | The GeoJSON compatible feature type |
| `"geometry"` | Geometry | Object | True | A GeoJSON compatible geometry object |
| `"properties"` | Properties | Object | True | A GeoJSON compatible properties object |

### Geometry Object
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"type"` | `Polygon` | String | True | The geometry type that conforms with GeoJSON geometry `Polygon` |
| `"coordinates"` |  | Array[Array[Array[Number, Number, Number]]] | True | A GeoJSON compatible polygon geometry coordinates. <br/> **NOTE** each coordinate must consist of 3 number, [longitude, latitude, elevation]. See [GeoJSON Geometry Object](https://datatracker.ietf.org/doc/html/rfc7946#section-3.1) |


### Properties Object
| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"name"` |  | String | True | The name of the policy zone |
| `"activationDeadline"` | DateTime | ISO8601 UTC | False | Indicates the latest time by which an AV that is pending a policy zone should transition to activating it. <br/> **NOTE** This is a soft deadline, AV should aim to adhere to the policy by this time but it is not strictly required to do so if it is not possible or safe to do so. |
| `"policies"` | Policies | Object | True | A set of policies that the AV shall adhere to within the zone. <br/><br/> See [policies](policies.md) for the possible policies and their properties. |


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
            "id": "00000000-0000-0000-0000-000000000001",
            "properties": {
                "name": "grading 1",
                "policies": {
                    "exclusion": { }
                },
                "activationDeadline": "2024-04-04T06:05:47Z"
            }
        }
    }
}
```
