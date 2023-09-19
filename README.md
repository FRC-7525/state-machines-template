# Floor Intake State Machine

* Create a new WPILib project.
* Copy this README, and put it in a file named `README.md` in the root directory of the new project.

Check in with a mentor.

You'll be using pneumatics to control the floor intake in this project. See [this code snippet](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/pneumatics/pneumatics.html#single-solenoids-in-wpilib) for how to use pneumatics in WPILib.

* In `teleopPeriodic`, create the two floor intake pistons, and set them to be extended (floor intake out).

Check in with a mentor.

Now, imagine if everything was controlled in the `teleopPeriodic` function. Things would get pretty messy, right? That's why we separate subsystem functionality into their own files and classes.

Additionally, because of the way periodic functions work, we need some way for the robot to remember what state it's in between calls to the periodic function. This is done using a state machine. Here's the basic layout (from our [Style Guide](https://github.com/FRC-7525/StyleGuide/blob/main/src/main/java/frc/robot/subsystems/Subsystem.java)):

```java
// name is <Class>States
enum SubsystemStates {
    // States should have clearly understandable names
    OFF,
    // SCREAMING_SNAKE_CASE
    TURNING_OFF
}

public class Subsystem {
    private SubsystemStates state = SubsystemStates.OFF;
    private Robot robot = null;

    public Subsystem(Robot robot) {
        // variables should not be constructed here
        // this constructor can take an instance of robot for sharing variables
        // like the controller
        this.robot = robot;
    }

    public void periodic() {
        // State machine should be written with if statements
        if (state == SubsystemStates.OFF) {
            // State operations
            
            // State transition
        } else if (state == SubsystemStates.TURNING_OFF) {
            // include an else if even for the last state!
            // State operations

            // State transition
        }
    }
}
```

We use an enum to define the different states that a subsystem can be in. In each state, we have the things that the state is doing (e.g. setting a motor to a speed, setting a PID to a setpoint, etc.), and checks for whether the state should be changed (e.g. if a button is pressed, change the state to something else). These changes are called *state transitions*.

* Create a folder called `subsystems` in the folder `Robot.java` is in, and make a file there called `FloorIntake.java`. Use the sample code from above to fill it in, changing the names as needed (class should be called `FloorIntake`).
* Move the piston variable declarations into this class.
* Create an OFF and ON state. In the OFF state, pistons should be pulled in, and in the ON state, pistons should be pushed out. (state operation)

Check in with a mentor.

* Now for the state transition. Set it up so whenever A is pressed in OFF, it transitions to ON, and whenever it's pressed in ON, it transitions to OFF.
* Construct FloorIntake in `Robot.java`:
```java
private FloorIntake floorintake = new FloorIntake(this);
```
* Call `floorintake.periodic()` in `teleopPeriodic` so the subsystem's code actually runs.
* Test it out!

Check in with a mentor once you have this working (or if you're having issues!).