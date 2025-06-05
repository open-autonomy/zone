# Policy Zone Sequence Diagrams
The following sections describes when the messages should be sent during the lifecycle of the policy zone messages between the Fleet Management System (FMS) and the Autonomous Haulage System (AHS)

### Language
| Acronyms | Extended Name |
| --- | --- |
| AHS | Autonomouse Haulage System |
| FMS | Fleet Management System |


## On Connect
```mermaid
sequenceDiagram
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    AHS->FMS: Connection Established
    FMS->>+AHS: Get Fleet Definition
    AHS-->>-FMS: Fleet Definition

    FMS-->>FMS: Policy Zones Pending

    FMS->>AHS: Sends All Existing Policy Zones

    par Zone 1 Equipment 1
        AHS->>Equipment 1: Activate Zone 1
        Equipment 1->>Equipment 1: Adhers to Policy
        Equipment 1->>AHS: Activated
        AHS->>FMS: Equipment 1 - Zone 1 <br /> ActivateZoneResponseV1: "Activated"
    and Zone 1 Equipment N
        AHS->>Equipment N: Activate Zone 1
        Note Over Equipment N: Unable to immediately adhere to policy
        Equipment N->>AHS: Accepted
        AHS->>FMS: Equipment N - Zone 1 <br /> ActivateZoneResponseV1: "Accepted"
    and Zone N Equipment 1
        AHS->>Equipment 1: Activate Zone N
        Equipment 1->>Equipment 1: Adhers to Policy
        Equipment 1->>AHS: Activated
        AHS->>FMS: Equipment 1 - Zone N <br /> ActivateZoneResponseV1: "Activated"
    and Zone N Equipment N
        AHS->>Equipment 1: Activate Zone N
        Equipment 1->>Equipment 1: Adhers to Policy
        Equipment 1->>AHS: Activated
        AHS->>FMS: Equipment N - Zone N<br /> ActivateZoneResponseV1: "Activated"
    end

    Note Over FMS: All equipments activated Zone N

    FMS-->>FMS: Zone N Activated
    
    Note Over Equipment N: Becomes able to adhere to policy
    Equipment N->>Equipment N: Adhers to Policy
    Equipment N->>AHS: Activated Zone 1
    AHS->>FMS: Equipment N - Zone 1 <br /> ActivateZoneResponseV1: "Activated"

    Note Over FMS: All equipments activated Zone 1

    FMS-->>FMS: Zone N Activated

```

NOTE: The above diagram is a typical flow of the on connect sequence for policy zone between the FMS and AHS systems.


## Policy Zone Creation and Activation
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
    FMS->>AHS: Send Policy Zone
    AHS->>Equipment 1: Activate Policy Zone
    AHS->>Equipment N: Activate Policy Zone
    Equipment 1->>Equipment 1: Adhers to Policy
    Equipment 1->>AHS: Activated
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Activated"

    Note Over Equipment N: Unable to immediately adhere to policy
    Equipment N->>AHS: Accept
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Accepted"

    Note Over Equipment N: Becomes able to adhere to policy
    Equipment N->>Equipment N: Adhers to Policy
    Equipment N->>AHS: Activated (Zone 1)
    
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Activated"
    User-->-FMS: Pending
    FMS-->>FMS: Policy Zone Activated
    FMS-->>User: Policy Zone Activated
```

## Policy Zone Rejection
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
    FMS->>AHS: Send Policy Zone
    AHS->>Equipment 1: Activate Policy Zone
    Note Over Equipment 1: Cannot Adher to policy
    Equipment 1->>AHS: Rejected
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Rejected"
    User-->-FMS: Pending
```