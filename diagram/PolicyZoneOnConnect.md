# On Connect
The Autonomous Haulage System (AHS) will send an ISO 23725 `FleetDefinitionV2` message to the Fleet Management System (FMS) of the Autonomous Haulage Trucks(AHT) in the fleet when the connection is established.

```mermaid

sequenceDiagram
    participant FMS
    participant AHS
    participant AHT 1
    participant AHT N

    AHT 1->>AHS: Connected
    AHT N->>AHS: Connected

    FMS->AHS: Connected

    AHS->>FMS: ISO 23725 - FleetDefinitionV2

```