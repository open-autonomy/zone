# Policy Zones
Policy Zones is the collection of all defined zones of the mine that in some how are restricted for one or more vehicles. 

## Types of zones: 
- Exclusion
- ?
- ?
- ?

## Sequence diagram

## General connection rules
- All requests are done from FMS to AHS through HTTP and is done individal per autonomous vehicle. 
- HTTP responses are only covering that message as such is accepted, i.e the AHS service is up
- Responses regarding the content of the request is send through Websocket 

## Activate hazard zone
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
            "version": 5,
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
        "Version": 5,
        "Status": "Accepted"
    }
}
```
---
## Update existing hazard zone
* `PUT`            `/v1/equipment/e6d895b0-e377-4567-8b1a-8d2a4f3104ff/zones/00000000-0000-0000-0000-000000000001`
```json
content-type: application/json
{
    "Protocol": "Open-Autonomy",
    "Timestamp": "2024-08-23T07:25:32.344Z",
    "Zone": {
        "type": "Feature",
        "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                    59.154612700275192,
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
                    59.154612700275192,
                    17.62123606784992
                ]
              ]
            ]
        },
        "properties": {
            "id": "00000000-0000-0000-0000-000000000001",
            "version": 6,
            "policyType": Exclusion,
            "name": "grading 1",
            "created": "2024-08-23T07:25:33.6652497Z",
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
        "Version": 5,
        "Status": "Accepted"
    }
}
```

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
        "version": 1,
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
        "version": 1,
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
        "version": 1,
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
                "ZoneId": "00000000-0000-0000-0000-000000000001",
                "Version": 1
            },
            {
                "ZoneId": "00000000-0000-0000-0000-000000000002",
                "Version": 1
            },
            {
                "ZoneId": "00000000-0000-0000-0000-000000000003",
                "Version": 1
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
        "Version": 5,
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

