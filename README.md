# Railroad Crossing FSM

# First Session - Simple FSM

## Proposed Invariants:
1. **Multiple Trains**: Two or more trains cannot be present at the same time.
2. **False Sensor**: False sensor readings cannot occur in the system.
3. **Alarm On**:
   - **Arrival**: Alarm is always on after an arrival event.
   - **Barrier**: Alarm is always on when the barrier is down.
   - **Train Present**: Alarm is always on when a train is present.
4. **Alarm Off**: Alarm is always off after a departure followed by an elapsed event.
5. **Arrival/Depature**: An arrival event always precedes a departure event.
6. **Power**: The Ashton example, power will never be interrupted.
7. **One-Way Tracks**: Trains can only go one direction on their respective tracks.
8. **~IDLE**: Idle state is unreachable if train is present.
9. **Armed Alarmed No Train**: The arms cannot be down if there are no trains.

## Inputs (Events/Triggers):
1. **northbound_approach**: Triggered when the first car of a northbound train crosses the approach sensor.
2. **southbound_approach**: Triggered when the first car of a southbound train crosses the approach sensor.
3. **northbound_depart**: Triggered when the last car of a northbound train crosses the depart sensor.
4. **southbound_depart**: Triggered when the last car of a southbound train crosses the depart sensor.
5. **elapsed**: Triggered 10 seconds after the alarm starts.

## States:
1. **Idle**: No train approaching or at the crossing, the alarm is off, and the barrier is raised.
2. **Alarm_Ringing**: A train has been detected (either northbound or southbound), the alarm is ringing, but the barrier is still raised.
3. **Barrier_Lowering_Train_Crossing**: The alarm has been ringing for 10 seconds, and the barrier is being lowered. A train is currently crossing, the barrier is lowered, and the alarm is ringing.
4. **Train_Cleared**: The train has passed the crossing, but the system is waiting for the 10-second timer to stop the alarm and raise the barrier.

## Outputs (Actions):
1. **Alarm**: Can be in the state **On** (audible) or **Off**.
2. **Barrier**: Can be in the state **Lowered** or **Raised**.

## Transitions (Between States):
1. **Idle → Alarm_Ringing**:
   - **Trigger**: `northbound_approach` or `southbound_approach` event.
   - **Action**: Alarm_On.
   
2. **Alarm_Ringing → Barrier_Lowering**:
   - **Trigger**: `elapsed` event (after 10 seconds).
   - **Action**: Lower_Barrier.
   
3. **Barrier_Lowering_Train_Crossing → Train_Cleared**:
   - **Trigger**: `northbound_depart` or `southbound_depart` event (the last car of the train crosses the sensor).
   - **Action**: Raise_Barrier (after the train departs, but the alarm still sounds).

4. **Train_Cleared → Idle**:
   - **Trigger**: `elapsed` event (10 seconds after the train has cleared the crossing).
   - **Action**: Alarm_Off.

# Part 2

| number | arms_down | alarm_on | northbound_present | southbound_present |    north_approach | south_approach | north_depart | south_depart | elapsed | safety_hazard |
|--------|-----------|----------|--------------------|--------------------|   ----------------|----------------|--------------|--------------|---------|---------------|
| 0      | 0         | 0        | 0                  | 0                  |    6              | 5              | 17           | 17           | 17      |               |
| 1      | 0         | 0        | 0                  | 1                  |                   |                |              |              |         | 18            |
| 2      | 0         | 0        | 1                  | 0                  |                   |                |              |              |         | 18            |
| 3      | 0         | 0        | 1                  | 1                  |                   |                |              |              |         | 18            |
| 4      | 0         | 1        | 0                  | 0                  |   6               | 5              | 17           | 17           | 0       |               |
| 5      | 0         | 1        | 0                  | 1                  |   16              | 16             | 17           | 4            | 13      |               |
| 6      | 0         | 1        | 1                  | 0                  |   16              | 16             | 4            | 17           | 14      |               |
| 7      | 0         | 1        | 1                  | 1                  |                   |                |              |              |         | 16            |
| 8      | 1         | 0        | 0                  | 0                  |                   |                |              |              |         | 18            |
| 9      | 1         | 0        | 0                  | 1                  |                   |                |              |              |         | 18            |
| 10     | 1         | 0        | 1                  | 0                  |                   |                |              |              |         | 18            |
| 11     | 1         | 0        | 1                  | 1                  |                   |                |              |              |         | 18            |
| 12     | 1         | 1        | 0                  | 0                  |                   |                |              |              |         | 24            |
| 13     | 1         | 1        | 0                  | 1                  |  16               | 16             | 17           | 4            | stays13 |               |
| 14     | 1         | 1        | 1                  | 0                  |  16               | 16             | 4            | 17           | stays14 |               |
| 15     | 1         | 1        | 1                  | 1                  |                   |                |              |              |         | 16            |

| number | invariant             |
|--------|-----------------------|
| 16     | Multiple Trains       |
| 17     | False Sensor          |
| 18     | Alarm On              |
| 19     | Alarm off             |
| 20     | Arrival/Departure     |
| 21     | Power	         |
| 22     | One-Way Tracks        |
| 23     | IDLE                  |
| 24     | Armed Alarmed No train|

## See Updated_FSM pdf to see the above table in action.

## Pre-Lab (Exchanging Initial FSMs)

We both designed extended FSMs without realizing that wasn't what the question asked. When we got to class we read more carefully and started from scratch.
The original FSM is our combined idea, which has the word Simple in the pdf name.

## Lab Question (Counter Examples)

We found an issue while working through the table, where we added an arrow going from Train_Cleared to Alarm_Ringing states to account for State 4 of the table.
This also applies to the example provided in the lab, which does not account for State 4.




