# Railroad Crossing FSM

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
