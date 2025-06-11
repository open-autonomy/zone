# Policy Zone Deletion
When a policy zone is deleted, the Fleet Management System (FMS) will send a request to the Autonomous Haulage System (AHS) to deactivate the policy zone on all Autonomous Haulage Trucks (AHT) that are currently adhering to it. The AHS will then communicate with each AHT to deactivate the policy zone.

Assuming the policy zone already exist and the AHTs are aware of the policy zone

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AHT 1
    participant AHT N

    Note over FMS,AHT 1: On Connect Sequence
    User->>FMS: Delete Policy Zone
    FMS-->>FMS: Policy Zone Pending Delete
    FMS-->+User: Pending Delete

    par AHT 1
        FMS->>AHS: Sends DeactivateZoneRequestV1 to AHT 1
        AHS->>AHT 1: Activate Policy Zone
        AHT 1->>AHS: Accept
        AHS->>FMS: DeactivateZoneResponseV1
    and AHT N
        FMS->>AHS: Sends DeactivateZoneRequestV1 to AHT N
        AHS->>AHT N: Activate Policy Zone
        AHT N->>AHS: Accept
        AHS->>FMS: DeactivateZoneResponseV1
    end

    User-->-FMS: Pending Delete

    Note Over FMS: All AHTs deactivated Policy Zone
    FMS-->>FMS: Policy Zone Deleted
    FMS-->>User: Policy Zone Deleted
```