# Policy Zone Activate Rejection
When a equipment cannot adhere to the policy defined in the policy zone definition, AHS should send a `"Rejected"` status in the `ActiveZoneResponse` message to FM. The FMS will then notify the user accordingly.

> [!NOTE]
> If any vehicle rejects the `ActivateZoneRequestV1` message on the same zone, the zone will not be activated within the FMS and will be marked as `"pending"` until the user resolves the issue.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1

    Note over FMS,Equipment 1: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    FMS->>AHS: Send ActivateZoneRequestV1 to Equipment 1
    AHS->>Equipment 1: Activate Policy Zone
    Note Over Equipment 1: Cannot Adhere to policy
    Equipment 1->>AHS: Rejected
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Rejected"
    User-->-FMS: Pending
    FMS-->>FMS: Policy Zone Pending
    FMS-->>User: Error Message
```