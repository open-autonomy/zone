# Policies

This document describes the policies that can be applied to policy zones created by the Fleet Management System (FMS) and adhered to by the Autonomous Haulage System's (AHS) Autonomous Vehicles (AV). Each policy defines specific behaviors that AVs must follow while operating within the defined zones.

> [!IMPORTANT]
> - A policy zone may contain multiple policies. An AV that has activated a policy zone must adhere to <ins>all</ins> policies specified in the zone.


### Policies
- [Exclusion](#exclusion)
- [Speed Limit](#speed-limit)
- [Low Traction](#low-traction)
- [Rough Road](#rough-road)
- [Controlled Access](#controlled-access) [![experimental](https://badges.github.io/stability-badges/dist/experimental.svg)](https://github.com/badges/stability-badges)


## Exclusion
An exclusion policy specifies that an AV MUST not enter the zone and MUST not move if it is already inside the zone. This includes preventing motion from all controlled motions - e.g. Raise Tray or Lower Tray.

### Exclusion Policy Attributes
Empty `"exclusion"` object `{}` is used to indicate that the exclusion policy is in effect.

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Exclusion Area",
    "policies": {
      "exclusion": { }
    }
  },
  "type": "Feature"
}
```

## Speed Limit
A speed limit policy specifies that AVs MUST reduce its speed to the specified limit before entering and while inside the zone.

The speed limit can be defined either as an absolute value in m/s or as a percentage of the operating speed of the vehicle in that location.

> [!IMPORTANT]
> When multiple overlapping speed zones exist, the lowest speed limit applies.

### Speed Limit Policy Attributes
The following attributes are used to define the `"speedLimit"` policy:

| Key | Value | Format | Required | Description |
| --- |:---:|:---:|:---:| --- |
| `"type"` | [`"absolute"`, `"percent"`] | string | True | The `"value"` type of the speed limit. |
| `"value"` | number | float | True | The speed limit value. <br/> - If `"type"` is `"absolute"`, this is the exact speed limit in m/s. <br/> - If `"type"` is `"percent"`, this is the percentage of the operating speed as specified in the map. |

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Speed Limited Area",
    "policies": {
      "speedLimit": {
        "type": "absolute",
        "value": 5.555
      }
    }
  },
  "type": "Feature"
}
```

## Low Traction
A low traction policy indicates low traction driving conditions and specifies that AVs MUST enable low traction control. This policy is typically applied in areas with muddy, icy, or otherwise slippery surfaces.

### Low Traction Policy Attributes
Empty `"lowTraction"` object `{}` is used to indicate that the low traction policy is in effect.

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Muddy Access Road",
    "policies": {
      "lowTraction": {}
    }
  },
  "type": "Feature"
}
```
---

## Rough Road
A rough road policy indicates rough driving conditions and specifies that AVs MUST enable control measures to manage the conditions. This policy is typically applied in areas where the road surface is not well maintained and could include potholes, uneven surfaces, or loose gravel.

### Rough Road Policy Attributes
Empty `"roughRoad"` object `{}` is used to indicate that the rough road policy is in effect.

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
	"id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Rough Road Area",
    "policies": {
      "roughRoad": {}
    }
  },
  "type": "Feature"
}
```