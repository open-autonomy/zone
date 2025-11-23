# Policies

This document describes the policies that can be applied to policy zones created by the Fleet Management System (FMS) and adhered to by the Autonomous Haulage System's (AHS) Autonomous Vehicles (AV). Each policy defines specific behaviors that AVs must follow while operating within the defined zones.

> [!IMPORTANT]
> - When an AV activates a policy, it shall adhere prior to entering and while inside the specified geometry.
> - A policy zone may contain multiple policies. When an AV activates a policy zone, it shall adhere to <ins>all</ins> policies specified in the zone.


### Policies
- [Exclusion](#exclusion)
- [Speed Limit](#speed-limit)
- [Low Traction](#low-traction)
- [Rough Road](#rough-road)
- [Traffic Permission](#traffic-permission)
- [Free Plan](#free-plan)


## Exclusion
An exclusion policy specifies that the AV shall not enter the zone and shall not move if it is already inside the zone. This includes preventing motion from all controlled motions - e.g. Raise Tray or Lower Tray.

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
A speed limit policy specifies that the AV shall reduce its speed to the specified limit before entering and while inside the zone.

The speed limit can be defined either as an absolute value in m/s or as a percentage of the operating speed of the vehicle in that location.

> [!IMPORTANT]
> When speed limits are specified in the map or multiple overlapping speed zones exist, the lowest speed limit applies.

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
A low traction policy indicates low traction driving conditions and specifies that the AV shall enable low traction control. This policy is typically applied in areas with muddy, icy, or otherwise slippery conditions.

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
A rough road policy indicates rough driving conditions and specifies that the AV shall enable control measures to drive to these conditions. This policy is typically applied in areas where the road surface is not well maintained and could include potholes, uneven surfaces, or loose gravel.

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

## Traffic Permission

A traffic permission policy indicates that the AV must obtain permission from the FMS before entering the zone. This policy is typically applied in areas where traffic control is required, such as construction zones or areas with restricted access (see [Traffic Management](https://github.com/open-autonomy/traffic-management) for more information).

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Traffic Permission Area",
    "policies": {
      "trafficPermission": {}
    }
  },
  "type": "Feature"
}
```

## Free Plan

The free plan policy indicates that the AV is allowed to plan its own path within the zone, rather than following a predefined route. This policy is typically applied to roads where the AV needs to navigate around obstacles or other vehicles. 

> ![IMPORTANT]
> This policy should be used in conjunction with the traffic permission policy to avoid conflicts between vehicles travelling in opposite directions.

### Example
```json
{
  "geometry": {
    "coordinates": ...,
    "type": "Polygon"
  },
  "id": "3d3d1bcf-5562-46eb-87a0-cdef15669f9d",
  "properties": {
    "name": "Free Plan Area",
    "policies": {
      "freePlan": {}
    }
  },
  "type": "Feature"
}
```
