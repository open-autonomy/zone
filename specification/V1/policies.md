# Policies

This document describes the policies that can be applied to policy zones created by the FMS and adhered to by the AHS equipment. Each policy defines specific behaviors that vehicles must follow while operating within the defined zones.

> [!IMPORTANT]
> A policy zone may contain may contain multiple policies. Equipment that has activated a policy zone must adhere to all policies defined within that zone.

## Exclusion
An exclusion policy indicates that vehicles MUST not enter the zone.

### Example 
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
    "name": "Exclusion Area",
    "policies": {
      "exclusion": { }
    }
  },
  "type": "Feature"
}
```

## Controlled Access

[![experimental](http://badges.github.io/stability-badges/dist/experimental.svg)](http://github.com/badges/stability-badges)

A controlled access policy indicates that vehicles MAY enter the zone, but only under specific conditions or with special permissions. This policy is typically used for areas that require authorization or have restricted access.

### Example 
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
    "name": "Controlled Access Area",
    "policies": {
      "controlledAccess": {}
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

```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "properties": {
    "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
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

```json
{
  "geometry": {
	"coordinates": ...,
	"type": "Polygon"
  },
  "properties": {
	"id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
	"name": "Rough Road Area",
	"policies": {
	  "roughRoad": {}
	}
  },
  "type": "Feature"
}
```
