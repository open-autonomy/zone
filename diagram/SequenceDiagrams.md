# Policy Zone Sequence Diagrams
The following document describes when the messages should be sent during the lifecycle of the policy zone messages between the Fleet Management System (FMS), the Autonomous Haulage System (AHS) and the Autonomous Haulage Trucks (AHT).

- **[Policy Zone Activation](PolicyZoneActivation.md)**
    - [Policy Zone Activation with Deadline](PolicyZoneActivation.md#policy-zone-activation-deadline-exceed)
    - [Policy Zone Rejection](PolicyZoneActivation.md#policy-zone-activate-rejection)
    - [Policy Zone Activation (REST Implementation)](PolicyZoneActivation.md#implementation-with-rest-from-fms-to-ahs)
- **[Policy Zone Deletion](PolicyZoneDeletion.md)**
- **[Policy Zone Updated](PolicyZoneUpdated.md)**
- **[Resynchronisation](Resynchronisation.md)**
    - [Typical AHT Reconnects](Resynchronisation.md#typical-aht-reconnects)
    - [AHT Reconnects With New Pending Zone](Resynchronisation.md#aht-reconnects-with-new-pending-zone)
    - [AHT Reconnects With Rejection](Resynchronisation.md#aht-reconnects---reject-active-zones)
