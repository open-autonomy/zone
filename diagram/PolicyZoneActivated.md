# Sequence Diagram - Policy Zone Activation

## Message Sequence
```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    User->>FMS: Create Policy Zone
    FMS-->+User: Pending
    FMS->>AHS: Create Policy Zone
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
    FMS-->>FMS: Zone 1 Activated
    FMS-->>User: Policy Zone 1 Activated
```

## FMS to AHS REST Implementation
(I don't even think we need this...)
```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment

    User->>FMS: Activate Policy Zone <br/> (Optional) Activation Deadline: ISO 8601 UTC
    FMS-->+User: Pending
    FMS->>AHS: Activate Policy Zone <br/> (Optional) Activation Deadline: ISO 8601 UTC
    AHS-->>FMS: Accept 202
    AHS->>Equipment: Activate Policy Zones <br/> (Optional) Activation Deadline: ISO 8601 UTC
    Equipment->>AHS: Accept

    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Accepted"
    User-->-FMS: Pending


    Equipment->>AHS: Activated (Zone 1)
    AHS->>FMS: ActivateZoneResponseV1 (Zone 1): Status "Activated"
    FMS-->>User: Policy Zone 1 Active
```