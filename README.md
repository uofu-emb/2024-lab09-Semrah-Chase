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
