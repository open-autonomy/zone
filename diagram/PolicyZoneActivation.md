## Policy Zone Activation
When a policy zone is created, the Fleet Management System (FMS) initiates the activation process by sending `ActivateZoneRequestV1` messages to the Autonomous Haulage System (AHS) for each of the equipment pieces involved. The AHS then communicates with each of the equipments to activate the policy zone internally. The following sequence diagram illustrates this process.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    Note over FMS,Equipment N: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    par Equipment 1
        FMS->>AHS: Send ActivateZoneRequestV1 to Equipment 1
        AHS->>Equipment 1: Activate Policy Zone
        Equipment 1->>Equipment 1: Adheres to Policy
        Equipment 1->>AHS: Activated
        AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    and Equipment N
        FMS->>AHS: Send ActivateZoneRequestV1 to Equipment N
        AHS->>Equipment N: Activate Policy Zone
        Note Over Equipment N: Unable to immediately adhere to policy
        Equipment N->>AHS: Accept
        AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"
    end 
    Note Over Equipment N: Adhering to policy
    Equipment N->>Equipment N: Adheres to Policy
    Equipment N->>AHS: Activated
    
    AHS->>FMS: Equipment N <br/> ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All equipments activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

### Implementation with REST from FMS to AHS
The following sequence diagram illustrates the implementation of the policy zone activation using REST calls from the FMS to the AHS system, which then communicates with the equipment.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    Note over FMS,Equipment N: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    par Equipment 1
        FMS->>AHS: Send ActivateZoneRequestV1 to Equipment 1 (via REST)
        AHS-->>FMS: Accepted 202 (REST Response)
        AHS->>Equipment 1: Activate Policy Zone
        Equipment 1->>Equipment 1: Adheres to Policy
        Equipment 1->>AHS: Activated
        AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    and Equipment N
        FMS->>AHS: Send ActivateZoneRequestV1 to Equipment N (via REST)
        AHS-->>FMS: Accepted 202 (REST Response)
        AHS->>Equipment N: Activate Policy Zone
        Note Over Equipment N: Unable to immediately adhere to policy
        Equipment N->>AHS: Accept
        AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"
    end 
    Note Over Equipment N: Adhering to policy
    Equipment N->>Equipment N: Adheres to Policy
    Equipment N->>AHS: Activated
    
    AHS->>FMS: Equipment N <br/> ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All equipments activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

>[!NOTE]
>The REST implementation above ensures that the FMS can handle asynchronous responses from the AHS, allowing for a more robust and scalable system. The AHS responds with a 202 Accepted status to indicate that the request has been received and is being processed, and **MUST** not be used as the final activation status. The final activation status is communicated through the `ActivateZoneResponseV1` message.