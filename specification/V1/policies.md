# Policies

This document describes the policies that can be applied to policy zones created by the Fleet Management System (FMS) and adhered to by the Autonomous Haulage System's (AHS) Autonomous Haulage Trucks (AHT). Each policy defines specific behaviors that AHTs must follow while operating within the defined zones.

> [!IMPORTANT]
> A policy zone may contain may contain multiple policies. AHT that has activated a policy zone must adhere to all policies defined within that zone.

### Policies
- [Exclusion](#exclusion)
- [Speed Limit](#speed-limit)
- [Low Traction](#low-traction)
- [Rough Road](#rough-road)
- [Controlled Access](#controlled-access) [![experimental](https://badges.github.io/stability-badges/dist/experimental.svg)](https://github.com/badges/stability-badges)


## Exclusion
An exclusion policy indicates that vehicles MUST not enter the zone.

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
A speed limit policy indicates that vehicles MUST reduce speed to the indicated limit while inside the zone.

The speed limit can be defined either as an absolute value in m/s or as a percentage of the typical operating speed of the vehicle in that location.

> [!IMPORTANT]
> When multiple overlapping speed zones exist, the lowest speed limit applies.

### Speed Limit Policy Attributes
The following attributes are used to define the `"speedLimit"` policy:

| Key | Value | Format | Required | Description |
| --- |:---:|:---:|:---:| --- |
| `"type"` | [`"absolute"`, `"percent"`] | string | True | The `"value"` type of the speed limit. |
| `"value"` | number | float | True | The speed limit value. <br/> - If `"type"` is `"absolute"`, this is the exact speed limit in m/s. <br/> - If `"type"` is `"percent"`, this is the percentage of the typical operating speed. |

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
A low traction policy indicates that the vehicle must exercise caution due to reduced road grip conditions. This policy is typically applied in areas with muddy, icy, or otherwise slippery surfaces.

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
A rough road policy indicates that the vehicle must exercise caution due to poor road conditions, such as potholes, uneven surfaces, or loose gravel. This policy is typically applied in areas where the road surface is not well maintained.

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

## Controlled Access

[![experimental](https://badges.github.io/stability-badges/dist/experimental.svg)](https://github.com/badges/stability-badges)

A controlled access policy indicates that vehicles MAY enter the zone, but only under specific conditions or with special permissions. This policy is typically used for areas that require authorization or have restricted access.

### Controlled Access Policy Attributes
Empty `"controlledAccess"` object `{}` is used to indicate that the controlled access policy is in effect.

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Controlled Access Area",
    "policies": {
      "controlledAccess": {}
    }
  },
  "type": "Feature"
}
```