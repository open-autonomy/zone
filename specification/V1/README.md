This document describes Version 1 of the Policy Zone Specification.

# Policy Zone Specification V1
This specification defines the messages and protocols used for managing policy zones in autonomous haulage vehicles (AHS). It includes the activation, deactivation, synchronization, and handling of policy zones to ensure safe operation of the equipment.

# Message definition
Find below the Specification for the Version 2 protocol for the spot service of Open-Autonomy
- [Policies](#policies)
- [Policy Zone Messages](#policy-zone-messages)

# Policies

The following policies are defined in this specification.
- [Exclusion](policies.md#exclusion)	
- [Speed Limit](policies.md#speed-limit)
- [Low Traction](policies.md#low-traction)
- [Rough Road](policies.md#rough-road)

> [!WARNING]
> This list is not exhaustive and more policies can be added in the future.

# Policy Zone Messages

> [!IMPORTANT]
All messages described below must be embedded within the [top-level message header](MessageHeader.md) data structure. 

The following messages are defined in this specification for managing policy zones:
- [ActivateZoneRequestV1](ActivateZoneRequestV1.md)
- [ActivateZoneResponseV1](ActivateZoneResponseV1.md)
- [DeactivateZoneRequestV1](DeactivateZoneRequestV1.md)
- [DeactivateZoneResponseV1](DeactivateZoneResponseV1.md)
- [OutOfSyncV1](OutOfSyncV1.md)
- [SyncActiveZonesRequestV1](SyncActiveZonesRequestV1.md)
- [SyncActiveZonesResponseV1](SyncActiveZonesResponseV1.md)
