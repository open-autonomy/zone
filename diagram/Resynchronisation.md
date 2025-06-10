## Resynchronisation

Resynchronisation is required whenever an equipment is switched on, or come back online after having lost communications (it is up to the AHS to determine what consititutes a loss of communication event).

When an equipment comes back online, it should send an `OutOfSyncV1` message to the FMS. This message indicates that the equipment cannot guarantee that it has an up-to-date list of active policy zones, and requires the FMS to send the current set of active zones through a `SyncActiveZonesRequestV1`. While the equipment is out of sync, it must not operate until it has received and internally activated these zones.

There are two scenarios outlined below that 

### Typical Equipment Reconnects
```mermaid
sequenceDiagram
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    Note over FMS,Equipment N: On Connect Sequence
    Note over Equipment N: Equipment N went offline
    AHS->>FMS: Update Fleet Definition

    Note Over Equipment N: Equipment N reconnects

    Equipment N->>AHS: Connects
    AHS->>FMS: Update Fleet Definition
    
    AHS->>FMS: Equipment N OutOfSyncV1

    FMS->>AHS: Equipment N <br/> SyncActiveZonesRequestV1
    AHS->>Equipment N: Active Policy Zones

    Equipment N->>AHS: Activated

    AHS->>FMS: Equipment N <br/> SyncActiveZonesResponseV1: Activated

    Note Over Equipment N: Equipment N can now operate
```

### Equipment Reconnects With New Pending Zone
When an equipment reconnects while a new policy zone is pending, the equipment must internally activates all active policy zones sent via `SyncActiveZonesRequestV1` in order to operate. The equipment will also receive an `ActivateZoneRequestV1` message for each of the pending policy zones to be activated internally as well.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1
    participant Equipment N

    Note over FMS,Equipment N: On Connect Sequence
    Note over Equipment N: Equipment N went offline
    AHS->>FMS: Update Fleet Definition

    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending

    FMS->>AHS: Send ActivateZoneRequestV1 to Equipment 1
    AHS->>Equipment 1: Activate Policy Zone
    Note over Equipment 1: Unable to immediately adhere to policy
    Equipment 1->>AHS: Accepted
    AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"

    Note Over Equipment N: Equipment N reconnects

    Equipment N->>AHS: Connects
    Note Over Equipment N: Requires Policy Zones to operate
    AHS->>FMS: Update Fleet Definition

    par Sync Active Zones
        AHS->>FMS: Equipment N OutOfSyncV1

        FMS->>AHS: Equipment N <br/> SyncActiveZonesRequestV1
        AHS->>Equipment N: Active Policy Zones

        Equipment N->>AHS: Activated

        AHS->>FMS: Equipment N <br/> SyncActiveZonesResponseV1: Activated
    and Activate Pending Zone
        loop
            FMS->>AHS: Send ActivateZoneRequestV1 to Equipment N
            AHS->>Equipment N: Activate Policy Zone
            Equipment N->>Equipment N: Adheres to Policy
            Equipment N->>AHS: Activated
            AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
        end
    end

    Note Over Equipment 1: Adhering to policy
    Equipment 1->>Equipment 1: Adheres to Policy
    Equipment 1->>AHS: Activated
     AHS->>FMS: Equipment 1 <br/> ActivateZoneResponseV1: Status "Activated"


    User-->-FMS: Pending

    Note Over FMS: All equipments activated Policy Zone
    FMS-->>FMS: Policy Zone Activated

    FMS-->>User: Policy Zone Activated
```

### Equipment Reconnects - Reject Active Zones
When an equipment reconnects and the active policy zones are internally rejected, it must not operate. The AHS will send a `ActivateZoneResponseV1` with a status of "Rejected", and the FMS shall then notify the user of the error.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant Equipment 1

    Note over FMS,Equipment 1: On Connect Sequence
    Note over Equipment 1: Equipment 1 went offline
    AHS->>FMS: Update Fleet Definition

    Note Over Equipment 1: Equipment 1 reconnects

    Equipment 1->>AHS: Connects
    Note Over Equipment 1: Requires Policy Zones to operate
    AHS->>FMS: Update Fleet Definition

    AHS->>FMS: Equipment 1 OutOfSyncV1

    FMS->>AHS: Equipment 1 <br/> SyncActiveZonesRequestV1
    AHS->>Equipment 1: Active Policy Zones

    Equipment 1->>AHS: Rejected

    Note Over Equipment 1: Equipment 1 MUST not operate

    AHS->>FMS: ActivateZoneResponseV1: Status "Rejected"

    FMS-->>User: Error Message

```