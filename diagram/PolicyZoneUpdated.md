# Policy Zone Update
Given some of the properties of the policy zone are immutable, the FMS will require a deletion on the old policy zone, and create a new policy zone with the new updated information.

Assuming the policy zone already exist and the equipments are aware of the policy zone

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    Note over FMS,Equipment 1: On Connect Sequence
    User->>FMS: Updated Policy Zone
    FMS-->>FMS: Old Policy Zone Pending Delete
    FMS-->+User: Old Policy Zone Pending Delete
    FMS-->>FMS: New Policy Zone Pending
    FMS-->+User: Pending

    par Deactivate Zone
        par Equipment 1
            FMS->>AHS: Sends DeactivateZoneRequestV1 to Equipment 1
            AHS->>Equipment 1: Activate Old Policy Zone
            Equipment 1->>AHS: Accept
            AHS->>FMS: DeactivateZoneResponseV1
        and Equipment N
            FMS->>AHS: Sends DeactivateZoneRequestV1 Equipment N
            AHS->>Equipment N: Activate Old Policy Zone
            Equipment N->>AHS: Accept
            AHS->>FMS: DeactivateZoneResponseV1
        end

    and Activate Zone
        par Equipment 1
            FMS->>AHS: Send ActivateZoneRequestV1 to Equipment 1
            AHS->>Equipment 1: Activate New Policy Zone
            Equipment 1->>Equipment 1: Adhers to New Policy
            Equipment 1->>AHS: Activated
            AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
        and Equipment N
            FMS->>AHS: Send ActivateZoneRequestV1 to Equipment N
            AHS->>Equipment N: Activate New Policy Zone
            Equipment N->>Equipment N: Adhers to New Policy
            Equipment N->>AHS: Activated
            AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
        end
    end
    
    User-->-FMS: New Policy Zone Pending
    Note Over FMS: All equipments activated New Policy Zone
    FMS-->>FMS: New Policy Zone Activated

    User-->-FMS: Old Policy Zone Pending Delete
    Note Over FMS: All equipments deactivated Old Policy Zone
    FMS-->>FMS: Old Policy Zone Deleted

    FMS-->>User: Updated Policy Zone Activated
```
