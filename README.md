# Minimum Viable Autonomous
This project contains the minimal code needed to have a functional autonomous in 2018 _FIRST_ Power Up, with the objective of not letting any robots hinder the completion of the Ranking Point-worthy AUTO QUEST.

## Instructions (READ)

Modify the `speed` and `timeout` to your preference. Add your motor controller variables (separated by a comma) in the `/* left motors */` and `/* right motors */` sections. (e.g. `/* left motors */` becomes `left1, left2, left3`)

Note: if your motor controllers already call `setInverted`, remove the `-` from the right motors.

***Test this to make sure the motors are spinning in the correct direction. If they are not, set `speed` to a negative value***

## Java:
Copy and paste the following into your main Robot Java file. No imports are required.

### PWM Motor Controllers:
```java
// Put this in your Robot main class, replacing the autonomousInit() and autonomousPeriodic() methods.
    long autoStart = 0;
    @Override
    public void autonomousInit() {
        autoStart = System.currentTimeMillis();
    }

    @Override
    public void autonomousPeriodic() {
        // Speed = Motor Speed (-1 to 1). Timeout = amount of time to move (in seconds)
        double speed = 1.0, timeout = 10;
        if (System.currentTimeMillis() - autoStart < (timeout * 1000)) {
            java.util.Arrays.stream((new Object[]{ /* left motors */})).forEach((Object s) -> ((edu.wpi.first.wpilibj.SpeedController)s).set(speed));
            java.util.Arrays.stream((new Object[]{ /* right motors */})).forEach((Object s) -> ((edu.wpi.first.wpilibj.SpeedController)s).set(-speed));
        }
    }
```

### CAN Motor Controllers (Talon SRX / Victor SPX):
```java
// Put this in your Robot main class, replacing the autonomousInit() and autonomousPeriodic() methods.
    long autoStart = 0;
    @Override
    public void autonomousInit() {
        autoStart = System.currentTimeMillis();
    }

    @Override
    public void autonomousPeriodic() {
        // Speed = Motor Speed (-1 to 1). Timeout = amount of time to move (in seconds)
        double speed = 1.0, timeout = 10;
        if ((System.currentTimeMillis() - autoStart) < (timeout * 1000)) {
            java.util.Arrays.stream((new Object[]{ /* left motors */})).forEach((Object s) -> ((com.ctre.phoenix.motorcontrol.IMotorController)s).set(com.ctre.phoenix.motorcontrol.ControlMode.PercentOutput, speed));
            java.util.Arrays.stream((new Object[]{ /* right motors */})).forEach((Object s) -> ((com.ctre.phoenix.motorcontrol.IMotorController)s).set(com.ctre.phoenix.motorcontrol.ControlMode.PercentOutput, -speed));
        }
    }
```

## C++:
***Note: All motor controllers must be in pointer form***

### PWM Motor Controllers:
```cpp
// Put this at the top of your file
#include <stdint.h>
#include <RobotController.h>

// Put this in your Robot main class, replacing the AutonomousInit() and AutonomousPeriodic() methods.
    uint64_t autostart;
    void AutonomousInit() override {
      autostart = ::frc::RobotController::GetFPGATime();
    }

    void AutonomousPeriodic() override {
      double speed = 1.0, timeout = 10;
      if ((::frc::RobotController::GetFPGATime() - autostart) < (uint64_t)(timeout * 1000000)) {
        for (auto l : { /* left motors (pointers) */ }) l->Set(speed);
        for (auto r : { /* right motors (pointers) */ }) r->Set(-speed);
      }
    }
```

### CAN Motor Controllers:
```cpp
// Put this at the top of your file
#include <stdint.h>
#include <RobotController.h>
#include <ctre/phoenix.h>

// Put this in your Robot main class, replacing the AutonomousInit() and AutonomousPeriodic() methods.
    uint64_t autostart;
    void AutonomousInit() override {
      autostart = ::frc::RobotController::GetFPGATime();
    }

    void AutonomousPeriodic() override {
      double speed = 1.0, timeout = 10;
      if ((::frc::RobotController::GetFPGATime() - autostart) < (uint64_t)(timeout * 1000000)) {
        for (auto l : { /* left motors (pointers) */ }) l->Set(::ctre::phoenix::motorcontrol::ControlMode::PercentOutput, speed);
        for (auto r : { /* right motors (pointers) */ }) r->Set(::ctre::phoenix::motorcontrol::ControlMode::PercentOutput, -speed);
      }
    }
```