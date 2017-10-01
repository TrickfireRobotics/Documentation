The Most Important Thing
==========================
* The Robot **shall** be blue

Primary Use-Case
==========================
1. The Robot **shall** initialize itself upon boot
2. After initialization is complete, the Robot **shall** monitor network traffic for the “Go” command
3. Upon receiving a “Go” command, the Robot **shall** establish localization and orient itself
4. After orientation is complete, the Robot **shall** select an appropriate direction
5. After selecting a movement direction, the Robot **shall** drive to the mining area
6. After arriving to the mining area, the Robot **shall** commence with the mining procedure
7. After mining is complete, the Robot **shall** return to the starting area
8. Upon arriving at the starting area, the Robot **shall** dump its payload
9. After dumping the payload, the Robot **shall** shut itself off

Top-Level System Requirements
==========================
* The Robot shall operate in one of two modes: Autonomous Mode and Manual Mode
* While in Autonomous Mode, the Robot **shall**:
  * Maintain top-level state information
  * Complete its task in under ten minutes
  * Upon receiving a “Mode-Switch” command, switch to Manual Mode
* While in Manual Mode, the Robot **shall**:
  * Perform navigation tasks based on controller information
  * Transmit additional sensor information
  * Upon receiving a “Mode-Switch” command, switch to Autonomous Mode
* While in either mode, the Robot **shall**:
  * Perform self-diagnosis on all subsystems
  * Monitor network traffic for controller information
  * Maintain localization
  * Transmit all diagnostic information to the external controller
  * Transmit state information to the external controller
  * Maintain timing information

Mining Requirements
==========================
* The Robot **shall** not keep track of the weight of the bin
* The Robot **shall** keep track of arm posing information
* The Robot **shall** keep track of where it has been digging
* The Robot **shall** dig only one hole
* The Robot **shall** not store the first scoop
* The Robot **shall** make estimations of where the actual gravel is
* The Robot **shall** follow a predefined series of mining movements during the mining procedure

Navigation Requirements
==========================
* While in Autonomous Mode, the Robot **shall**:
  * Establish and maintain a localization with which to orient itself
  * Drive to and from the mining zone
  * Avoid all obstacles while performing navigation procedures
  * Maintain navigation history data
  * Position itself in front of the dumping bin prior to the dumping procedure


Dumping Requirements
==========================
* While in Autonomous Mode, the Robot **shall**:
  * Only begin dumping procedure while in the dumping zone
  * Avoid interfering with the bin
  * Upon finishing the dumping procedure, shut itself off
* While in Manual Mode, the Robot **shall**:
  * Monitor network traffic for dumping commands
