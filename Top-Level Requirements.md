# Software Requirements
## List of Terms
* East **shall** be defined as the direction perpendicular and away to the face of the designated mining receptacle.
## Requirements
### 1.  Executive System
* 1.1 The executive system **shall** process mission control commands.
* 1.3 The executive system **shall** monitor and delegate system responsibilities and state in between mission subsystems.
* 1.4 The executive system **shall** support two modes of operation:
    * 1.4.1 *Autonomous* Commences the main mission phases and tracks autonomous mission state.
    * 1.4.2 *Teleop* lightweight user command processing and subsystem control.
* 1.5 The executive system **shall** track mission progress and mission time elapsed.
* 1.6 The executive system **shall** be able to preempt all subsystems.
* 1.7 The executive system **shall** attempt to return the system to a safe state upon failure.
### 2.  Mission Control Interface
* 2.1 The control interface **shall** have commands to:
    * 2.1.1 Start autonomous operation.
    * 2.1.2 Halt autonomous operation.
    * 2.1.3 Send directional driving commands.
    * 2.1.4 Execute a digging procedure.
    * 2.1.5 Perform an emergency stop.
    * 2.1.6 Reset the position of the mining arm.
### 3.  Communication Subsystem
* 3.1 The communication subsystem **shall** monitor all top level diagnostic communications, and display them in the command center.
* 3.2 The communication subsystem **shall** render necessary and sufficient data to the command center to control the mission.
* 3.3 The communication subsystem **could** monitor compressed forward and rear video feeds when requested.
* 3.4 The communication subsystem **shall** monitor bandwidth of communications.
* 3.5 The communication subsystem **shall** adhere to NASA rules and regulations:
    <https://www.nasa.gov/sites/default/files/atoms/files/2018_rulesrubrics_partii.pdf>
* 3.6 The subsystem **could** encrypt all incoming and outgoing transmissions
* 3.7 The communication subsystem **shall** be able to receive incoming transmissions at all times.
### 4.  Sensor Subsystem
* 4.1 Individual sensor subsystems **shall**:
    * 4.1.1 Collect raw sensor data.
    * 4.1.2 Process it as needed.
    * 4.1.3 Publish the processed data at a set interval.
* 4.2 The subsystem **shall** have a sensor fusion subsystem.
* 4.3 Any or all sensor data from subsystems **shall** be fused and published by the sensor fusion subsystem.
### 5.  Control Subsystem
* 5.1 A control subsystem **shall**:
    * 5.1.1 Maintain a group of mechanical components to control.
    * 5.1.2 Maintain state information about current pose of tracked components.
    * 5.1.3 Send controlling signals to those components and respond to tracked states in real time.
* 5.2 The subsystem **shall** have a control subsystem for base movement.
* 5.3 The subsystem **shall** have a control subsystem for arm movement.
* 5.4 The subsystem **shall** have a control subsystem for dumping bin movement.
* 5.5 The subsystem **shall** expose an emergency stop mechanism to manually disable motor movement.
### 6.  Localization Subsystem
* 6.1 Localization **shall** be defined as: detecting a fiducial marker and determining the robot’s current position based on that detection.
* 6.2 Until the subsystem has localized the robot, the localization subsystem **shall**:
    * 6.2.1 Rotate the robot a set angle.
    * 6.2.2 Scan for fiducial markers in order to localize the robot.
* 6.3 Once the subsystem is localized the localization subsystem **shall** signal the executive system.
### 7.  Navigation Subsystem
#### 7.1  Fundamental
* 7.1.1 The navigation subsystem **shall** define a goal as a position and quaternion orientation.
* 7.1.2 The navigation subsystem **shall** provide and read necessary and sufficient data to calculate path information.
* 7.1.3 The navigation subsystem **shall** move the rover towards the goal, based on path information.
* 7.1.4 The navigation subsystem **shall** cease operation upon arriving at a goal within a set tolerance.
* 7.1.5 The navigation subsystem **shall**  provide appropriate error handling procedures.
#### 7.2  Pre-Mining
* 7.2.1 The software **shall** define static point at a configurable distance directly East of the bin, and within the mining zone as the initial goal.
#### 7.3  Post-Mining
* 7.3.1 The software **shall** define a static point at a configurable distance directly East of the bin as the goal and navigate there.
### 8.  Mining Subsystem
* 8.1 The mining subsystem **shall** be provided a time limit and not operate longer than that time limit.
* 8.2 The mining subsystem **shall** hold a predefined mining procedure.
* 8.3 The mining subsystem **shall** follow a predefined series of mining movements during the mining procedure.
* 8.4 The mining procedure **shall** define an ordered set of mining locations (and dumping locations) relative to the robot position
* 8.5 The mining procedure **shall** further be divided into atomically-operating subprocedures (scoops)
* 8.6 The mining procedure **shall** always check timing flags before beginning scoops
* 8.7 The mining procedure **shall** be terminated upon detecting timing flags
* 8.8 Upon completing the mining procedure, the mining subsystem **shall**:
    * 8.8.1 Define an “obstacle” representing the digging hole and send it to the navigation subsystem.
    * 8.8.2 Return the mining arm to the starting position.
* 8.9 The mining subsystem **shall** expose a mechanism to reset arm postition.
### 9.  Dumping Subsystem
* 9.1 As a precondition the rover **shall** be facing west, relatively close to the dumping receptacle.
* 9.2 The dumping subsystem **shall** define a goal called the dumping position, defined as touching the mining receptable facing west.
* 9.3 To reach the dumping postion the dumping subsystem **shall**:
    * 9.3.1 Orient itself towards the center of the bin.
    * 9.3.2 Back up towards the center of the receptacle using sensor data.
    * 9.3.3 In the event of data loss back up at it's last know heading.
* 10.4 Upon reaching the dumping position the dumping subsystem **shall** raise the dumping bin.
