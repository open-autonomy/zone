# Policy Zone Sequence Diagrams
The following document describes when the messages should be sent during the lifecycle of the policy zone messages between the Fleet Management System (FMS), the Autonomous Haulage System (AHS) and the Autonomous Vehicles (AV).

- **[Policy Zone Activation](PolicyZoneActivation.md)**
    - [Policy Zone Activation with Deadline](PolicyZoneActivation.md#policy-zone-activation-deadline-exceed)
    - [Policy Zone Rejection](PolicyZoneActivation.md#policy-zone-activate-rejection)
    - [Policy Zone Activation (REST Implementation)](PolicyZoneActivation.md#implementation-with-rest-from-fms-to-ahs)
- **[Policy Zone Deletion](PolicyZoneDeletion.md)**
- **[Policy Zone Updated](PolicyZoneUpdated.md)**
- **[Resynchronisation](Resynchronisation.md)**
    - [Typical AV Reconnects](Resynchronisation.md#typical-AV-reconnects)
    - [AV Reconnects With New Pending Zone](Resynchronisation.md#AV-reconnects-with-new-pending-zone)
    - [AV Reconnects With Rejection](Resynchronisation.md#AV-reconnects---reject-active-zones)
