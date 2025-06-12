# Policy Zone Sequence Diagrams
The following document describes when the messages should be sent during the lifecycle of the policy zone messages between the Fleet Management System (FMS), the Autonomous Haulage System (AHS) and the Autonomous Vehicles (AV).

- **[Fleet Synchronization](FleetSynchronization.md)**
    - [On Connect](FleetSynchronization.md#on-connect)
    - [Vehicle Commissioning](FleetSynchronization.md#vehicle-commissioning)
    - [Vehicle Decommissioning](FleetSynchronization.md#vehicle-decommissioning)
- **[Policy Zone Activation](PolicyZoneActivation.md)**
    - [Policy Zone Activation with Deadline](PolicyZoneActivation.md#policy-zone-activation-deadline-exceed)
    - [Policy Zone Rejection](PolicyZoneActivation.md#policy-zone-activate-rejection)
    - [Policy Zone Activation (REST Implementation)](PolicyZoneActivation.md#implementation-with-rest-from-fms-to-ahs)
- **[Policy Zone Deletion](PolicyZoneDeletion.md)**
- **[Policy Zone Updated](PolicyZoneUpdated.md)**
- **[Resynchronization](Resynchronization.md)**
    - [Typical AV Reconnects](Resynchronization.md#typical-AV-reconnects)
    - [AV Reconnects With New Pending Zone](Resynchronization.md#AV-reconnects-with-new-pending-zone)
    - [AV Reconnects With Rejection](Resynchronization.md#AV-reconnects---reject-active-zones)
