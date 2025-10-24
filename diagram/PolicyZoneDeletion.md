# Policy Zone Deletion
When a policy zone is deleted, the Fleet Management System (FMS) will send a request to the Autonomous Haulage System (AHS) to deactivate the policy zone on all Autonomous Vehicles (AV) that are currently adhering to it. The AHS will then communicate with each AV to deactivate the policy zone.

> [!IMPORTANT]
> - All systems shall implement idempotency when managing Policy Zone Deletions.
> - To avoid unmanagable synchronization failures, AVs should accept Policy Zone Deletions for zones that do not exist in the AVs memory.

Assuming the policy zone already exists in the FMS

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AV 1
    participant AV N

    Note over FMS,AV 1: On Connect Sequence
    User->>FMS: Delete Policy Zone
    FMS-->>FMS: Policy Zone Pending Delete
    FMS-->+User: Pending Delete

    par AV 1
        FMS->>AHS: Sends DeactivateZoneRequestV1 to AV 1
        AHS->>AV 1: Delete Policy Zone
        AV 1->>AHS: Deactivated
        AHS->>FMS: DeactivateZoneResponseV1
    and AV N
        FMS->>AHS: Sends DeactivateZoneRequestV1 to AV N
        AHS->>AV N: Delete Policy Zone
        AV N->>AHS: Deactivated
        AHS->>FMS: DeactivateZoneResponseV1
    end

    User-->-FMS: Pending Delete

    Note Over FMS: All AVs deactivated Policy Zone
    FMS-->>FMS: Policy Zone Deleted
    FMS-->>User: Policy Zone Deleted
```