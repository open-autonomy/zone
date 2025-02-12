# Policy Zones
Policy Zones is the collection of all defined zones of the mine that in some how are restricted for one or more vehicles. 

## Types of zones: 
- Exclusion (Truck MUST not enter)
- SpeedLimit (Truck MUST reduce speed)
- Road Conditions (Low Traction or Rough Road)



## Sequence diagram
### Activate Policy Zone
![bild](draw.io/ActivatePolicyZone.png)

## General connection rules
- All requests are done from FMS to AHS through HTTP and is done individal per autonomous vehicle. 
- HTTP responses are only covering that message as such is accepted, i.e the AHS service is up
- Responses regarding the content of the request is send through Websocket 

## Common Structure
Each Policy Zone follows this basic structure
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "uuid",
    "name": "Zone Name",
    "createdAt": "UTC timestamp",
    "version": "integer",
    "policies": {
      // Policy objects go here
    }
  },
  "type": "Feature"
}
```

## Exclusion Zone
An exclusion policy indicates that vehicles MUST not enter the zone while it exists.

The behaviour of vehicles already inside the zone is controlled by the vacateBy property where:

  *  Vehicles already inside the zone will operate as per normal until the vacateBy time has been reached

    * Once the timeout has elapsed, all operations must cease immediately

    * A vacateBy property matching the time the zone was created indicates that vehicles should cease all operations as soon as the zone is created

    * Vehicles that leave the zone prior to the vacateBy time may continue normal operations outside the zone

### Example 
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
    "name": "Speed Limited Area",
    "createdAt": "2024-04-04T05:05:47Z",
    "version": 1,
    "policies": {
      "speedLimit": {
        "type": "absolute",
        "value": 20
      }
    }
  },
  "type": "Feature"
}
```

## Speed Limit Zone
A speed limit policy indicates that vehicles MUST reduce speed to the indicated limit while inside the zone while it exists.
The speed limit can be defined either as an absolute value in km/h or as a percentage of the typically operating speed of the vehicle in that location.
NOTE: When multiple overlapping speed zones exist, the lowest speed limit applies.

### Example 
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
    "name": "Speed Limited Area",
    "createdAt": "2024-04-04T05:05:47Z",
    "version": 1,
    "policies": {
      "speedLimit": {
        "type": "absolute",
        "value": 20
      }
    }
  },
  "type": "Feature"
}
```

## Road Conditions
Road condition policies indicate that vehicles MUST adjust their driving behavior while inside the zone based on specified conditions. Two types of conditions are supported:
Low Traction: Indicates reduced road grip
Rough Road: Indicates poor road surface conditions
Each condition is represented as a separate policy. The presence of a condition policy indicates that the condition applies to the zone.

```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
    "name": "Muddy Access Road",
    "createdAt": "2024-04-04T05:05:47Z",
    "policies": {
      "lowTraction": {},
      "roughRoad": {}
    }
  },
  "type": "Feature"
}
```
---

## Activate Policy zone
* `POST`            `/v1/equipment/e6d895b0-e377-4567-8b1a-8d2a4f3104ff/zones`
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
            "policyType": Exclusion,
            "name": "grading 1",
            "created": "2024-08-23T07:20:33.6652497Z",
            "vacateBy": "2024-08-23T08:20:33.6652639Z"
        }
    }
}
```

* AHS response: `HTTP/1.1 202 Accepted`

* The truck acceps the zone and AHS responds over Websocket
```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "71b0ac40-5b73-4444-ac37-dbf7e51e8186",
    "Timestamp": "2024-08-23T07:20:35.344Z",
    "ActivateZoneResponseV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
        "ZoneId": "00000000-0000-0000-0000-000000000001",
        "Status": "Accepted"
    }
}
```
---
## Update existing hazard zone
Policy zones contain a number of fields that are immutable. These include:
- the geometry of the zone.
- the set of policies (and their respective attribtues) associated with zone.

Edits made by end-users to these attributes must be managed through the following process:
- Create a new policy zone reflecting the desired new state of the policy zone,
- Delete the previous version of the policy zone.

This is intended to remove any ambiguity about the state of the policy zone.

---
# Publish all zones 
* `POST`            `/v1/equipment/e6d895b0-e377-4567-8b1a-8d2a4f3104ff/zones/all`

```json
{
  "Protocol": "Open-Autonomy",
  "Timestamp": "2024-08-23T08:19:56.631Z",
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
      "properties": {
        "id": "00000000-0000-0000-0000-000000000001",
        "policyType": "Exclusion",
        "name": "grading 1",
        "created": "2024-08-23T07:20:33.6652497Z",
        "vacateBy": "2024-08-23T08:20:33.6652639Z"
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
      "properties": {
        "id": "00000000-0000-0000-0000-000000000002",
        "policyType": "Exclusion",
        "name": "grading 2",
        "created": "2024-08-23T07:20:33.6652497Z",
        "vacateBy": "2024-08-23T08:20:33.6652639Z"
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
      "properties": {
        "id": "00000000-0000-0000-0000-000000000003",
        "policyType": "Exclusion",
        "name": "grading on-road",
        "created": "2024-08-23T07:20:33.6652497Z",
        "vacateBy": "2024-08-23T08:20:33.6652639Z"
      }
    }
  ]
}
```

* AHS response: `HTTP/1.1 202 Accepted`

* The truck acceps the zone and AHS responds over Websocket
```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "34b53af5-cc4a-4e4f-a5e2-19fa0357ec5f",
    "Timestamp": "2024-08-23T09:13:43.815Z",
    "ActivateAllZonesResponseV1": {
        "EquipmentId": "e3239fd8-a625-4ef5-89b1-ef26bb13f2db",
        "Status": "Accepted",
        "ActivatedZones": [
            {
                "ZoneId": "00000000-0000-0000-0000-000000000001"
            },
            {
                "ZoneId": "00000000-0000-0000-0000-000000000002"
            },
            {
                "ZoneId": "00000000-0000-0000-0000-000000000003"
            }
        ]
    }
}
```

---

# Truck is offline
If the truck is offline when a request is sent to it, the response will be 

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T07:26:33.344Z",
    "ActivateZoneResponseV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
        "ZoneId": "00000000-0000-0000-0000-000000000001",
        "Status": "Rejected",
        "Reason": "UnexpectedOffline"
    }
}
```
* When the truck comes back online the truck shall send OutOfSync to FMS

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T08:19:55.621Z",
    "OutOfSyncV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff"
    }
}
```
* The FMS should then publish all zones [Publish All Zones](#publish-all-zones)
