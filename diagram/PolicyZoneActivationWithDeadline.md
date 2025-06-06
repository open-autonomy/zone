
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
