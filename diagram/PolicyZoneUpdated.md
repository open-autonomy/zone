# Policy Zone Update
Given some of the properties of the policy zone are immutable, the Fleet Management System (FMS) will require a deletion on the old policy zone, and create a new policy zone with the new updated information. When a policy zone is updated, the FMS will send requests to the Autonomous Haulage System (AHS) to deactivate the old policy zone and activate the new one on all Autonomous Haulage Trucks (AHT). The AHS will then communicate with each AHT to internally deactivate the old policy zone and activate the new one.

Assuming the policy zone already exist and the AHTs are aware of the policy zone

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AHT 1
    participant AHT N

    Note over FMS,AHT 1: On Connect Sequence
    User->>FMS: Updated Policy Zone
    FMS-->>FMS: Old Policy Zone Pending Delete
    FMS-->+User: Old Policy Zone Pending Delete
    FMS-->>FMS: New Policy Zone Pending
    FMS-->+User: Pending

    par Deactivate Zone
        par AHT 1
            FMS->>AHS: Sends DeactivateZoneRequestV1 to AHT 1
            AHS->>AHT 1: Activate Old Policy Zone
            AHT 1->>AHS: Accept
            AHS->>FMS: DeactivateZoneResponseV1
        and AHT N
            FMS->>AHS: Sends DeactivateZoneRequestV1 AHT N
            AHS->>AHT N: Activate Old Policy Zone
            AHT N->>AHS: Accept
            AHS->>FMS: DeactivateZoneResponseV1
        end

    and Activate Zone
        par AHT 1
            FMS->>AHS: Send ActivateZoneRequestV1 to AHT 1
            AHS->>AHT 1: Activate New Policy Zone
            AHT 1->>AHT 1: Adhers to New Policy
            AHT 1->>AHS: Activated
            AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
        and AHT N
            FMS->>AHS: Send ActivateZoneRequestV1 to AHT N
            AHS->>AHT N: Activate New Policy Zone
            AHT N->>AHT N: Adhers to New Policy
            AHT N->>AHS: Activated
            AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
        end
    end
    
    User-->-FMS: New Policy Zone Pending
    Note Over FMS: All AHTs activated New Policy Zone
    FMS-->>FMS: New Policy Zone Activated

    User-->-FMS: Old Policy Zone Pending Delete
    Note Over FMS: All AHTs deactivated Old Policy Zone
    FMS-->>FMS: Old Policy Zone Deleted

    FMS-->>User: Updated Policy Zone Activated
```
