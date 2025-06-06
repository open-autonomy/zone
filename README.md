# Policy Zones
Policy Zones are geographically bounded regions within a map in which autonomous haulage equipment (AHT) are required to modify their behaviour to comply with one or more policies associated with that zone. Examples of policies include: Exclusions, where AHTs are forbidden to enter or driving within the zone; speed limits, in which AHTs are required to regulate their speeds to comply with specified limits; and controlled access, where AHTs may only enter if explicitly instructed to move into the zone.

### Language
| Acronyms | Extended Name |
| --- | --- |
| AHS | Autonomouse Haulage System |
| FMS | Fleet Management System |
| AHT | Autonomous Haulage Truck |


## Policy Zones Specification
This repository defines the messages and protocols used for managing policy zones in autonomous haulage vehicles (AHS), where policy zones are described using the GeoJSON [RFC7946](https://datatracker.ietf.org/doc/html/rfc7946) geospatial data interchange format. Policies can be associated with these zones via the `properties` field of the GeoJSON object, allowing for flexible and extensible definitions of how AHS should behave within these zones.

The following policies are defined in this specification:
- [Exclusion](policies.md#exclusion)
- [Controlled Access](policies.md#controlled-access)
- [Speed Limit](policies.md#speed-limit)
- [Low Traction](policies.md#low-traction)
- [Rough Road](policies.md#rough-road)

> [!WARNING]
> This list is not exhaustive and more policies can be added in the future.

For more information on the messages used in the V1 protocol for managing policy zones, see the [V1 Specification](specification/V1/README.md).

## Policy Zone State Machine

Policy zones have a lifecycle that is managed by the FMS. This lifecycle includes the following states:
- `Pending`: The policy zone has been created but has not yet been activated.
- `Active`: The policy zone is currently active and is being enforced.
- `Pending Delete`: The policy zone has been marked for deletion but has not yet been removed.
- `Deleted`: The policy zone has been removed and is no longer active.

The following state machine describes the lifecycle of a policy zone as it transitions between these states:

![Policy Zone State Machine](draw.io/PolicyZoneStateMachine.svg)

> [!NOTE]
> Policy zones that are rejected are still considered pending and can be re-sent to the truck for activation at a later time.

> [!IMPORTANT] 
> Policy zones contain a number of fields that are immutable. These include:
> - the geometry of the zone.
> - the set of policies (and their respective attributes) associated with zone.
> 
> Edits made by end-users to these attributes must be managed through the following process:
> - Create a new policy zone reflecting the desired new state of the policy zone,
> - Delete the previous version of the policy zone.
> 
> This is intended to remove any ambiguity about the state of the policy zone.


## Sequence diagrams

See [Sequence Diagrams](diagram/SequenceDiagrams.md) for a detailed set of scenarios that describe the interactions between the FMS, AHS, and equipment when managing policy zones.
