# SyncActiveZonesRequestV1

This message is sent by the Fleet Management System (FMS) to the Autonomous Haulage System (AHS) to provide an out of sync Autonomous Vehicle (AV) with the current complete set of **active** zones. The AV must activate all zones within the request before it is permitted to operate.

> [!IMPORTANT]
> The `SyncActiveZonesRequestV1` message shall not contain any zones that are pending. Pending zones shall be communicated separately using the `ActivateZoneRequestV1` message.
---

| Sender | Triggered by | Triggers |
| --- | --- | --- |
| `FMS`  | `OutOfSyncV1` | The AV to adhere to the existing activated policy zones, and fire off `SyncAllActiveZonesResponseV1` messages |

## Message Attributes

The `SyncAllActiveZonesRequestV1` is

| Key | Value | Format | Required | Description |
| --- | :---: | :---: | :---: | --- |
| `"RequestId"` | RequestId | UUID | True | A unique identifier for the request. This is used to match the request with the response. |
| `"Zones"` | | Array[Zone] | True |An array of [GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946) object consist in the Zone Object. |

>[!NOTE]
> The top-level message headers should contain the `EquipmentId`, indicating which AV the `SyncAllActiveZonesRequestV1` message is for.

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
| `"id"` | ZoneId | String | True | The policy zone id |
| `"name"` |  | String | True | The name of the policy zone |
| `"policies"` | Policies | Object | True | A set of policies that the AV should adhere to within the zone. <br/><br/> See [policies](policies.md) for the possible policies and their properties. |


## Examples
### Typical Message
```JSON
{
  "Protocol": "Open-Autonomy",
  "Version": 1,
  "Timestamp": "2024-08-23T08:19:56.631Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "SyncActiveZonesRequestV1": {
    "RequestId": "00000000-0000-0000-0000-000000000001",
    "Zones": [
        {
        "type": "Feature",
        "geometry": {
            "type": "Polygon",
            "coordinates": [
            [
            [
                59.154612700275194,
                17.62123606784992
            ],
            [
                59.15444657134832,
                17.621361182777765
            ],
            [
                59.154458381940245,
                17.62176503107635
            ],
            [
                59.154774479447724,
                17.621645401146836
            ],
            [
                59.154612700275194,
                17.62123606784992
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
        }
        },
        {
        "type": "Feature",
        "geometry": {
            "type": "Polygon",
            "coordinates": [
            [
            [
                59.154244958079,
                17.620554410746
            ],
            [
                59.154391048629,
                17.620733999199
            ],
            [
                59.154275508018,
                17.621266018812
            ],
            [
                59.154121113519,
                17.621085793262
            ],
            [
                59.154244958079,
                17.620554410746
            ]
            ]
            ]
        },
        "id": "00000000-0000-0000-0000-000000000002",
        "properties": {
            "name": "grading 2",
            "policies": {
                "exclusion": { }
            },
        }
        },
        {
        "type": "Feature",
        "geometry": {
            "type": "Polygon",
            "coordinates": [
            [
            [
                59.154998915988,
                17.621767651459
            ],
            [
                59.154835229909,
                17.621800464999
            ],
            [
                59.154847165895,
                17.622115535915
            ],
            [
                59.155000821112,
                17.622085194746
            ],
            [
                59.15499725505,
                17.621767524328
            ],
            [
                59.154998915988,
                17.621767651459
            ]
            ]
            ]
        },
        "id": "00000000-0000-0000-0000-000000000003",
        "properties": {
            "name": "grading on-road",
            "policies": {
                "exclusion": { }
            },
        }
        }
    ]
  }
}
```
