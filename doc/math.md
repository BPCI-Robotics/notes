# Math notes

## Spreadsheet
You don't need to do all this. Just use the [magic spreadsheet](https://docs.google.com/spreadsheets/d/1cvkjG-kS0T1l_ln-m6KFD1xE3opr2bzPi1rK3nq2KZU/edit?usp=sharing)!  
Just make a copy and put your values in.

### Torque curve
The motors are torque-limited, such that their torque goes down as the RPM goes up.  
![Graph showing motor current and torque with respect to RPM](https://kb.vex.com/hc/article_attachments/10706717749396)  
This is complex, but the way you can estimate this, and the way the spreadsheet does it is like this:  
`IF(B11<=58%, 2.1, 2.1-(1.25/0.4)*(B11-58%))`  
Where `B11` is the percentage rotation speed of the motor.  
```python
slope = 1.25 / 0.4

if rotation_speed <= 0.58:
    torque = 2.1

else:
    torque = 2.1 - slope * (rotation_speed - 0.58)
```
This is a reasonably good estimation. For the power curve, we can simply use the formula:  
`P = 2πωT` where `ω` is angular velocity in rotations per second, and T is the torque we just calculated.  


## Units

Symbol | Quantity | Unit | Unit Name | Breakdown
------ | -------- | ---- | --------- | ---------
m      | mass     | kg   | kilogram  | kg
t      | time     | s    | second    | s
s      | distance | m    | meter     | m
v      | velocity | m/s  |           | m/s
a      | acceleration | m/s^2 |      | m/s^2
F      | force    | N    | newton    | kg m/s^2
T      | torque   | Nm   | newton-meter | kg m^2/s^2
T      | torque   | J/rad | joule/radian | kg m^2/s^2
E      | energy   | J    | joule    | kg m^2/s^2
P      | power    | W    | watt     | kg m^2/s^3
ω      | angular velocity | rpm or rps | | rad/s
θ      | angle | rad | radians | rad

## Formulas

### Force, mass, acceleration, velocity, distance, time

`a = Δv / Δt`  
If the car has a `0-100` of `2.5 seconds`, what is its acceleration?  
`a = (100 km/h)/(2.5 s) = 40 km/h/s` of acceleration.  

`v = Δs / Δt`  
If the car travelled `10 meters` in `2 seconds`, how fast is it?  
`v = (10 m)/(2 s) = 5 m/s` of speed.  

`F = ma`  
If someone has pushed me (`100 kg`) on the ice to `2 m/s` in one second, I have experienced:  
`a = (2 m/s)/(1 s) = 2 m/s/s`  
`F = (100 kg)(2 m/s/s) = 200 N` of force.  

`a = F/m`  
If someone exerts `200 N` of force on me (`100 kg`), I will accelerate by:  
`a = (200 N)/(100 kg) = 2 m/s/s` of acceleration.  

`m = F/a`  
If I accelerate by `2 m/s each second` due to a force of `200 N`, I weigh:  
`m = (200 N)/(2 m/s/s) = 100 kg` of mass.  

`s = s0 + vΔt`  
If I am `2 meters` away from home, and I travel `2 m/s` for `2 seconds`, I am:  
`s = (2 m) + (2 m/s)(2 s) = 6` m away from home.  

`s = s0 + v0Δt + (1/2)aΔt^2`  
If I am `200 m` off the ground, I am falling due to gravity (`-9.8 m/s/s`), and I am not falling initially (`0 m/s`),  
How far would I be off the ground in `2 seconds`?  
`s = (200 m) + (0 m/s)(2 s) + (1/2)(-9.8 m/s/s)(2 s)^2`  
`= 180.4 m` off the ground.  

`v = v0 + aΔt`  
I am at a stop (`0 m/s`) and my car can go to `20 m/s` in `4 seconds`. How fast will I go in `one second`?  
`a = (20 m/s)/(4 s) = 5 m/s/s`  
`v = (0 m/s) + (5 m/s/s)(1 s) = 5 m/s` of velocity.  

### Rotation

`C = πd = 2πr`  
This circle has a radius of `2 m`, so it has a diameter of `4 m` and a circumference of about `12.6 m`.  

`P = 2πωT`  
My motor has a torque of `1 Nm` at `200 rpm`, what is its power consumption?  
`ω = (200 rpm)/60 = 3.3 rps`  
`P = 2π(1 Nm)(3.3 rps) = 20.73 W` of power.  

`T = T0 + T1 + ...`  
I have 4 motors with `1 Nm` of torque each. They have `4 Nm` of torque in total.  
** RPM does not add with more motors. **  

`F = T/r`  
My wheels have a radius of `0.05 m` and a torque of `4 Nm` in total. What is the force exerted?  
`F = (4 Nm)/(0.05 m) = 80 N`  

`ω = Δθ/Δt`  
My motor makes a `full rotation` each `second`. What is its rpm?  
`ω = (1 revolution)/(1 s) = 1 rps = 60 rpm`.  

`v = ωC`  
My wheels rotate at `200 rpm` and have a circumference of `31 cm`. How fast is the robot?  
`ω = (200 rpm)/60 = 3.3 rps`  
`v = (3.3 rps)(0.31 m) = 1.02 m/s` of speed.  

### Energy, power, time

`E = Pt`  
The motor used `12 watts` for `20 seconds`, how much energy did it use?  
`E = (12 W)(20 s) = 240 J` of energy.  

`t = E/P`  
The battery has a capacity of `50400 joules`. My motors use `48 W`. How long will it last?  
`t = (50400 J)/(48 W) = 1050 s` of time.  

`P = E/t`  
What is the power consumption of my motors if they finished the battery (`50400 J`) in `1050 seconds`?  
`P = (50400 J)/(1050 s) = 48 W` of power.  

### Notes
`Δ` should be read as "change in".  
A `Δt` is a change in time. So if two seconds have elapsed, `Δt` is `2 s` which is the result of `t1 - t0`  

When doing calculations, always check your units. If you are solving for velocity, and you try this:  
`acceleration * distance`  
This is not right, if you check the units:  
`(m/s^2) * (m) = m^2 / s^2`  
This is because velocity is measured in `m / s` and not `m^2 / s^2`.  

The difference between velocity and speed is that velocity has a direction, while speed does not.  
If you are travelling down, you could have a velocity of `-2 m/s` or `2 m/s down`, but your speed is just `2 m/s`.  
In this case, they are used interchangeably.  

The difference betweeen distance and displacement is that distance is the full distance between two points,  
following a specific path, while displacement is the distance in a straight line, the shortest distance.  
They are used interchangeably here.  

Careful with units. You can use feet, inches, metres, whatever. But when you calculate, make sure it's all the same unit.  
I suggest converting everything to SI units (metres, kilograms, seconds, ...).  

You may notice that torque is identical to energy. This is because radians are unitless.  
Rather torque is energy / rotation.  

Watch out for the difference between radians and rotations. A rotation is `2π` times more than a radian.  