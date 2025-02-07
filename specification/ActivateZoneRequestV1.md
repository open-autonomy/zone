# ActivateZoneRequestV1


|Sender| Triggered by | Triggers|
|---|---|---|
| `zone` | Zone server sending a new zone request when a zone needs to be activated  | |

<br>

## Message attributes
| Key               |   Format    | Description |
|------------------|----------|-------------|
| `"Timestamp"`    | ISO 8601              | The UTC timestamp when the request was created. |
| `"Zone.type"`    |  String   | Type of geo-spatial object (GeoJSON format). |
| `"Zone.geometry.type"` | String   | The shape type of the zone in the GeoJSON format. |
| `"Zone.geometry.coordinates"`  | Array    | List of latitude, longitude, and altitude points defining the zone. |
| `"Zone.properties.id"` || UUID     | Unique identifier for the zone. |
| `"Zone.properties.version"`      | Integer  | Version number of this zone definition. |
| `"Zone.properties.policyType"`     | String   | Defines the policy type (e.g., Exclusion). |
| `"Zone.properties.name"`  | String   | Name of the zone. |
| `"Zone.properties.created"` | ISO 8601 | Timestamp when the zone was created. |
| `"Zone.properties.vacateBy"`| ISO 8601 | Time by which the area must be vacated. |


## Example of ActivateZoneRequestV1
```json
{
    "Protocol": "Open-Autonomy",
    "Timestamp": "2024-08-23T07:20:33.665Z",
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
            "version": 5,
            "policyType": Exclusion,
            "name": "grading 1",
            "created": "2024-08-23T07:20:33.6652497Z",
            "vacateBy": "2024-08-23T08:20:33.6652639Z"
        }
    }
}
```
