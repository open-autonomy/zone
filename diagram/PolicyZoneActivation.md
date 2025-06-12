# Policy Zone Activation
When a policy zone is created, the Fleet Management System (FMS) initiates the activation process by sending `ActivateZoneRequestV1` messages to the Autonomous Haulage System (AHS) for each of the Autonomous Vehicle (AV)s defined in the Fleet Definition. The AHS then communicates with each of the AVs to activate the policy zone internally. The following sequence diagram illustrates this process.

> [!IMPORTANT]
> All systems shall implement idempotency when managing Policy Zone Activations.

## Typical Policy Zone Activation

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AV 1
    participant AV N

    Note over FMS,AV N: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    par AV 1
        FMS->>AHS: Send ActivateZoneRequestV1 to AV 1
        AHS->>AV 1: Activate Policy Zone
        AV 1->>AHS: Accept
        AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"
        AV 1->>AV 1: Adheres to Policy
        AV 1->>AHS: Activated
        AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    and AV N
        FMS->>AHS: Send ActivateZoneRequestV1 to AV N
        AHS->>AV N: Activate Policy Zone
        AV N->>AHS: Accept
        AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"
        Note Over AV N: Unable to immediately adhere to policy
        AV N->>AV N: Adheres to Policy
        AV N->>AHS: Activated
        AHS->>FMS: AV N <br/> ActivateZoneResponseV1: Status "Activated"
    end

    User-->-FMS: Pending
    Note Over FMS: All AVs activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

## Policy Zone Activation Deadline Exceed
The policy zone can be created with the `activationDealine` property. This field is an indicative field that lets the AV know it should start to adhere to the policy if possible. However, it is not a strict demand and the AV is allowed to defer compliance up until the specified time.

> [!NOTE]
> The inclusion of the activation deadline does not change the activation process from the perspective of the FMS. For FMS to consider the zone active in the FMS, the FMS still requires confirmation from all AVs that the zone has been activated.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AV 1

    Note over FMS,AV 1: On Connect Sequence
    User->>FMS: Create Policy Zone <br/> with Activation Deadline
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    FMS->>AHS: Send ActivateZoneRequestV1 to AV 1
    AHS->>AV 1: Activate Policy Zone

    Note Over AV 1: Unable to immediately adhere to policy
    AV 1->>AHS: Accept
    AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"

    Note Over AV 1: Activation Dealine Exceeded
    Note Over AV 1: Proceed to Adheres to Policy ...
    Note Over AV 1: Adhering to Policy
    AV 1->>AV 1: Adhers to Policy
    AV 1->>AHS: Activated

    AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
    User-->-FMS: Pending
    Note Over FMS: All AVs activated Policy Zone
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

## Policy Zone Activate Rejection
When an AV cannot adhere to the policy defined in the policy zone definition, the AHS should send a `"Rejected"` status in the `ActiveZoneResponse` message to FMS. The FMS will then notify the user accordingly.

> [!NOTE]
> If an AV rejects the `ActivateZoneRequestV1` message for a given zone, the zone will not be activated within the FMS and will remain as `"pending"` until all AVs have successfully activated the zone.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AV 1

    Note over FMS,AV 1: On Connect Sequence
    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending
    FMS->>AHS: Send ActivateZoneRequestV1 to AV 1
    AHS->>AV 1: Activate Policy Zone
    Note Over AV 1: Cannot Adhere to policy
    AV 1->>AHS: Rejected
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Rejected"
    User-->-FMS: Pending
    FMS-->>FMS: Policy Zone Pending
    FMS-->>User: Error Message
```