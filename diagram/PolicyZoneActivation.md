# Policy Zone Activation
When a policy zone is created, the Fleet Management System (FMS) initiates the activation process by sending `ActivateZoneRequestV1` messages to the Autonomous Haulage System (AHS) for each of the equipment pieces involved. The AHS then communicates with each of the equipments to activate the policy zone internally. The following sequence diagram illustrates this process.

## Typical Policy Zone Activation

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

## Policy Zone Activation Deadline Exceed
The policy zone can be created with a `activationDealine` property. This field is an indicative field that lets the equipment know it should start to adhere to the policy if possible. However, it is not a strict demand that the equipment must comply by the specified time.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1

    Note over FMS,Equipment 1: On Connect Sequence
    User->>FMS: Create Policy Zone <br/> with Activation Deadline
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    FMS->>AHS: Send ActivateZoneRequestV1 to Equipment 1
    AHS->>Equipment 1: Activate Policy Zone

    Note Over Equipment 1: Unable to immediately adhere to policy
    Equipment 1->>AHS: Accept
    AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"

    Note Over Equipment 1: Activation Dealine Exceeded
    Note Over Equipment 1: Proceed to Adheres to Policy ...
    Note Over Equipment 1: Adhering to Policy
    Equipment 1->>Equipment 1: Adhers to Policy
    Equipment 1->>AHS: Activated
    
    AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All equipments activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

## Policy Zone Activate Rejection
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

## Implementation with REST from FMS to AHS
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