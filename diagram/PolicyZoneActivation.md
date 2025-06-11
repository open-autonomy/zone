# Policy Zone Activation
When a policy zone is created, the Fleet Management System (FMS) initiates the activation process by sending `ActivateZoneRequestV1` messages to the Autonomous Haulage System (AHS) for each of the Autonomous Haulage Truck (AHT) pieces involved. The AHS then communicates with each of the AHTs to activate the policy zone internally. The following sequence diagram illustrates this process.

## Typical Policy Zone Activation

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AHT 1
    participant AHT N

    Note over FMS,AHT N: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    par AHT 1
        FMS->>AHS: Send ActivateZoneRequestV1 to AHT 1
        AHS->>AHT 1: Activate Policy Zone
        AHT 1->>AHT 1: Adheres to Policy
        AHT 1->>AHS: Activated
        AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    and AHT N
        FMS->>AHS: Send ActivateZoneRequestV1 to AHT N
        AHS->>AHT N: Activate Policy Zone
        Note Over AHT N: Unable to immediately adhere to policy
        AHT N->>AHS: Accept
        AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"
    end 
    Note Over AHT N: Adhering to policy
    AHT N->>AHT N: Adheres to Policy
    AHT N->>AHS: Activated
    
    AHS->>FMS: AHT N <br/> ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All AHTs activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

## Policy Zone Activation Deadline Exceed
The policy zone can be created with a `activationDealine` property. This field is an indicative field that lets the AHT know it should start to adhere to the policy if possible. However, it is not a strict demand that the AHT must comply by the specified time.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AHT 1

    Note over FMS,AHT 1: On Connect Sequence
    User->>FMS: Create Policy Zone <br/> with Activation Deadline
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    FMS->>AHS: Send ActivateZoneRequestV1 to AHT 1
    AHS->>AHT 1: Activate Policy Zone

    Note Over AHT 1: Unable to immediately adhere to policy
    AHT 1->>AHS: Accept
    AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"

    Note Over AHT 1: Activation Dealine Exceeded
    Note Over AHT 1: Proceed to Adheres to Policy ...
    Note Over AHT 1: Adhering to Policy
    AHT 1->>AHT 1: Adhers to Policy
    AHT 1->>AHS: Activated
    
    AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All AHTs activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

## Policy Zone Activate Rejection
When a AHT cannot adhere to the policy defined in the policy zone definition, AHS should send a `"Rejected"` status in the `ActiveZoneResponse` message to FM. The FMS will then notify the user accordingly.

> [!NOTE]
> If any vehicle rejects the `ActivateZoneRequestV1` message on the same zone, the zone will not be activated within the FMS and will be marked as `"pending"` until the user resolves the issue.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AHT 1

    Note over FMS,AHT 1: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    FMS->>AHS: Send ActivateZoneRequestV1 to AHT 1
    AHS->>AHT 1: Activate Policy Zone
    Note Over AHT 1: Cannot Adhere to policy
    AHT 1->>AHS: Rejected
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Rejected"
    User-->-FMS: Pending
    FMS-->>FMS: Policy Zone Pending
    FMS-->>User: Error Message
```

## Implementation with REST from FMS to AHS
The following sequence diagram illustrates the implementation of the policy zone activation using REST calls from the FMS to the AHS system, which then communicates with the AHT.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AHT 1
    participant AHT N

    Note over FMS,AHT N: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    par AHT 1
        FMS->>AHS: Send ActivateZoneRequestV1 to AHT 1 (via REST)
        AHS-->>FMS: Accepted 202 (REST Response)
        AHS->>AHT 1: Activate Policy Zone
        AHT 1->>AHT 1: Adheres to Policy
        AHT 1->>AHS: Activated
        AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    and AHT N
        FMS->>AHS: Send ActivateZoneRequestV1 to AHT N (via REST)
        AHS-->>FMS: Accepted 202 (REST Response)
        AHS->>AHT N: Activate Policy Zone
        Note Over AHT N: Unable to immediately adhere to policy
        AHT N->>AHS: Accept
        AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"
    end 
    Note Over AHT N: Adhering to policy
    AHT N->>AHT N: Adheres to Policy
    AHT N->>AHS: Activated
    
    AHS->>FMS: AHT N <br/> ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All AHTs activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

>[!NOTE]
>The REST implementation above ensures that the FMS can handle asynchronous responses from the AHS, allowing for a more robust and scalable system. The AHS responds with a 202 Accepted status to indicate that the request has been received and is being processed, and **MUST** not be used as the final activation status. The final activation status is communicated through the `ActivateZoneResponseV1` message.