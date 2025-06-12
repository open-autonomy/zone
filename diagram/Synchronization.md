# Synchronization

Synchronization is required whenever an Autonomous Vehicle (AV) connects to the Autonomous Haulage System (AHS).

> [!IMPORTANT]
>- AVs shall be immobilized when their onboard policy zones are not synchrnised with the FMS.
>- The AHS shall monitor the connection to each AV, and synchronization must also occur on re-connection after all lost communications events.


When an AV connects to the AHS, it shall send an `OutOfSyncV1` message to the Fleet Management System (FMS). This message indicates that the AV cannot guarantee that it has an up-to-date list of active policy zones, and requires the FMS to send the current set of active zones through a `SyncActiveZonesRequestV1`. While the AV is out of sync, the AV shall be immobilized until it has received and internally activated these zones.

## Typical AV Connects
```mermaid
sequenceDiagram
    participant FMS
    participant AHS
    participant AV N

    Note over FMS, AHS: On Connect Sequence
    Note over AV N: AV N offline & immobilized

    Note Over AV N: AV N connects

    AV N->>AHS: Connects

    AHS->>FMS: AV N OutOfSyncV1

    FMS->>AHS: AV N <br/> SyncActiveZonesRequestV1
    AHS->>AV N: Active Policy Zones
    AV N->>AV N: Adheres to all Policies
    AV N->>AHS: Activated

    AHS->>FMS: AV N <br/> SyncActiveZonesResponseV1: Activated

    Note Over AV N: AV N can now operate
```

## AV Connects With New Pending Zone
When an AV connects while a new policy zone is pending, the AV must internally activates all active policy zones sent via `SyncActiveZonesRequestV1` in order to operate. The AV will also receive an `ActivateZoneRequestV1` message for each of the pending policy zones to be activated internally as well.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AV 1
    participant AV N

    Note over FMS,AHS: On Connect Sequence
    Note over AV N: AV N offline and immobilized

    User->>FMS: Create Policy Zone
    FMS-->>FMS: Policy Zone Pending
    FMS-->+User: Pending

    FMS->>AHS: Send ActivateZoneRequestV1 to AV 1
    AHS->>AV 1: Activate Policy Zone
    Note over AV 1: Unable to immediately adhere to policy
    AV 1->>AHS: Accepted
    AHS->>FMS: ActivateZoneResponseV1: Status "Accepted"

    Note Over AV N: AV N connects

    AV N->>AHS: Connects
    Note Over AV N: Requires Policy Zones to operate

    par Sync Active Zones
        AHS->>FMS: AV N OutOfSyncV1

        FMS->>AHS: AV N <br/> SyncActiveZonesRequestV1
        AHS->>AV N: Active Policy Zones
        AV N->>AV N: Adheres to all Policies
        AV N->>AHS: Activated
        Note over AV N: AV N can operate

        AHS->>FMS: AV N <br/> SyncActiveZonesResponseV1: Activated
    and Activate Pending Zone
        loop
            FMS->>AHS: Send ActivateZoneRequestV1 to AV N
            AHS->>AV N: Activate Policy Zone
            AV N->>AV N: Adheres to Policy
            AV N->>AHS: Activated
            AHS->>FMS: ActivateZoneResponseV1: Status "Activated"
        end
    end

    Note Over AV 1: Adhering to policy
    AV 1->>AV 1: Adheres to Policy
    AV 1->>AHS: Activated
     AHS->>FMS: AV 1 <br/> ActivateZoneResponseV1: Status "Activated"


    User-->-FMS: Pending

    Note Over FMS: All AVs activated Policy Zone
    FMS-->>FMS: Policy Zone Activated

    FMS-->>User: Policy Zone Activated
```

## AV Connects - Reject Active Zones
When an AV connects and the active policy zones are internally rejected, it must not operate. The AHS will send a `ActivateZoneResponseV1` with a status of "Rejected", and the FMS shall then notify the user of the error.

> [!IMPORTANT]
> An AV that rejects an `ActivateZoneRequestV1` shall remain immobilized.

```mermaid
sequenceDiagram
    participant User
    participant FMS
    participant AHS
    participant AV 1

    Note over FMS,AHS: On Connect Sequence

    Note over AV 1: AV 1 offline & immobilized

    Note Over AV 1: AV 1 connects

    AV 1->>AHS: Connects
    Note Over AV 1: Requires Policy Zones to operate

    AHS->>FMS: AV 1 OutOfSyncV1

    FMS->>AHS: AV 1 <br/> SyncActiveZonesRequestV1
    AHS->>AV 1: Active Policy Zones
    Note Over AV 1: AV 1 cannot adhere to all zones

    AV 1->>AHS: Rejected

    Note Over AV 1: AV 1 MUST remain immobilized

    AHS->>FMS: ActivateZoneResponseV1: Status "Rejected"

    FMS-->>User: Error Message

```

## Scenarios

### Expected Offline Scenario

In the event that the truck has been parked up and powered off, the AHS may choose to accept the policy zone change request on behalf of the truck.

> [!IMPORTANT]
> It is the responsibility of the AHS to ensure that it is safe to respond on behalf of an AV.

The following message provides an example of a policy zone change request that is accepted after the truck has been powered off.

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T07:26:33.344Z",
    "ActivateZoneResponseV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
        "ZoneId": "00000000-0000-0000-0000-000000000001",
        "Status": "Accepted"
    }
}
```

* When the truck comes back online the truck shall send OutOfSync to the FMS

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T08:19:55.621Z",
    "OutOfSyncV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff"
    }
}
```

* The FMS should then publish all active zones to the truck using [SyncActiveZonesRequestV1](#specification/V1/SyncActiveZonesRequestV1.md) message which will allow the truck to synchronize its set of active zones with the FMS.

* Resend any policy zones that are still pending (including the zones that were rejected due to being unexpectedly offline).

### Loss of Comms Scenario 1 - Truck cannot be guaranteed to have come to a stop

In the event that a truck has lost connection to the AHS, the AHS may choose to reject the policy zone change request until the truck has re-established connection.

The following message provides an example of a policy zone change request that is rejected due to the truck being unexpectedly offline.

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T07:26:33.344Z",
    "ActivateZoneResponseV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
        "ZoneId": "00000000-0000-0000-0000-000000000001",
        "Status": "Rejected",
        "Reason": "UnexpectedOffline"
    }
}
```

In such an event, the FMS may attempt to continue sending the policy zone change request until the truck has re-established connection.

* When the truck comes back online, the truck shall send OutOfSync to the FMS

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T08:19:55.621Z",
    "OutOfSyncV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff"
    }
}
```
* The FMS should then publish all active zones to the truck using [SyncActiveZonesRequestV1](#specification/V1/SyncActiveZonesRequestV1.md) message which will allow the truck to synchronize its set of active zones with the FMS.

* Resend any policy zones that are still pending (including the zones that were rejected due to being unexpectedly offline).

### Loss of Comms Scenario 2 - Truck comes to stop after comms loss timeout

In the event that a truck has lost connection to the AHS, the AHS may choose to accept the policy zone change request after the truck can be guaranteed to have come to a stop due to the comms loss timeout.

> [!IMPORTANT]
> It is the responsibility of the AHS to ensure that it is safe to respond on behalf of an AV.

The following message provides an example of a policy zone change request that is accepted after the truck has come to a stop.

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T07:26:33.344Z",
    "ActivateZoneResponseV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff",
        "ZoneId": "00000000-0000-0000-0000-000000000001",
        "Status": "Accepted"
    }
}
```

* When the truck comes back online the truck shall send OutOfSync to the FMS

```json
{
    "Protocol": "Open-Autonomy",
    "CorrelationId": "7850cb0e-dee0-4b21-8948-73dec03f3887",
    "Timestamp": "2024-08-23T08:19:55.621Z",
    "OutOfSyncV1": {
        "EquipmentId": "e6d895b0-e377-4567-8b1a-8d2a4f3104ff"
    }
}
```

* The FMS should then publish all active zones to the truck using [SyncActiveZonesRequestV1](#specification/V1/SyncActiveZonesRequestV1.md) message which will allow the truck to synchronize its set of active zones with the FMS.
