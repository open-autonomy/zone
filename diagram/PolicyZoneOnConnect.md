# On Connect
The Autonomous Haulage System (AHS) will send an ISO 23725 `FleetDefinitionV2` message to the Fleet Management System (FMS) of the Autonomous Vehicles(AV) in the fleet when the connection is established.

```mermaid

sequenceDiagram
    participant FMS
    participant AHS
    participant AV 1
    participant AV N

    AV 1->>AHS: Connected
    AV N->>AHS: Connected

    FMS->AHS: Connected

    AHS->>FMS: ISO 23725 - FleetDefinitionV2

```