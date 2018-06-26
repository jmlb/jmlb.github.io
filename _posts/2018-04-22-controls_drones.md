---
layout: post
title: Controls of a Drone - Robotics Nanodegree by Udacity (Term1)
meta: "robotics, lidar"
category: robotics
author: "Jean-Marc Beaujour"
img_post: 20180420-lidar_teensy.png
summary: 
github-link: ""
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



## 1. Introduction

A pair of motors are spinning counterclockwise $$-b_z$$, whereas the other pair are spinning clockwise ($$+b_z$$). 
<img src="/images/20180422/quadrotor-top-view.png" width="500rem">

Motors on the $$b_x$$ axis rotate clockwise and on the axis $$b_y$$ axis anticlockwise: this is to negate the induced torque from the rotating blade.

We define the thrust $$F_i$$, and $$M_i$$ the torque moment from the propeller $$i$$ on $$\{ B\}$$.


Let's define the roll $$\phi$$, pitch $$\theta$$ and yawn $$\psi$$ angles as an intrinsic $$(zxy)$$ sequence of rotations:


$$
\begin{align}
R_x(\phi) &= \left[ \begin{matrix} 1 & 0 & 0 \\ 0 \text{cos}\phi & -\text{sin}\phi \\ 0 & \text{sin} \phi & \text{cos} \phi\end{matrix} \right] \\
R_y(\theta) &= \left[ \begin{matrix} \text{cos}\theta & 0 & \text{sin}\theta \\ 0 & 1 & 0 \\ -\text{sin} \theta & 0 & \text{cos} \theta\end{matrix} \right] \\
R_z(\psi) &= \left[ \begin{matrix} \text{cos}\psi & -\text{sin}\psi & 0 \\ \text{sin}\psi & \text{cos} \psi & 0 \\ 0 & 0 &  1 \end{matrix} \right] \\
\end{align}
$$


$$
\begin{align}

_B ^W R = R_z(\psi) R_x(\phi) R_y(\theta)
\end{align}
$$

Hence:

$$
\begin{align}
\left[ \begin{matrix} p_x \\ p_y \\ p_z \end{matrix} \right] = _B ^W R \left[\begin{matrix} p_x \\ p_y \\ p_z \end{matrix} \right]
\end{align}
$$

#### Movement along vertical direction (along $b_z$) : each motor produces a force:

$$
F_i = k_F \omega_i ^2
$$
where $$k_F$$ is a constant and $$\omega_i$$ is the angular speed.
If $$F_i > -mg$$, the quadropter will accelerate upwards.

#### Yaw motion (rotation about $$b_z$$)

Magnitude of rotor torque:


$$
T_i = k_M \omega_i ^2
$$

The torque produces on the quadropter is opposite the direction of spin of the propeller.
In order to perform a pure **yawning** motion, 1 pair of motors would have to increase their angular speeds by the same amount as the opposite pair would have t odecreased theirs.

By Newton's law, the **torque** produce on the quadrotor is opposite to the direction of spin of the propeller: in order to have a CW rotation, torque from 2-4 must be larger than torque from 1-3

## 2. Kinematics and Dynamics model

<img src="/images/20180422/quadrotor-annotated-2-pinta.png" width="500rem">



Quadrator Kinematics and Dynamics Model:

$$
\begin{align}
\vec{u} = \left[ \begin{matrix} x \\ y \\ z \\ \phi \\ \theta \\ \psi \\ \dot{x} \\ \dot{y} \\ \dot{z} \\ \dot{\phi} \\ \dot{\theta} \\ \dot{\psi} \end{matrix}\right]
\end{align}
$$

$$\vec{u}$$ is the state vector and includes 12 parameters. We define an equilibrium (**hevering**) state, i.e simply maintain a constant position when the quad is in the air).

  * x, y, and z = the position of the quadrotor’s center of mass (CoM)
  * roll ($$\phi$$), pitch ($$\theta$$), and yaw ($$\psi$$) Euler angles = the orientation of the body frame {B} relative to the fixed world frame {W}
  * $$\dot{x}$$, $$\dot{y}$$, $$\dot{z}$$ are the linear velocities of the quadrotor’s CoM
  * $$\dot{\phi}$$, $$\dot{\theta}$$, $$\dot{\psi}$$ are the angular velocities of {B} relative to {W}

For the quad to hover in place, we need:

$$
\begin{align}
\frac{dx}{dt} = \frac{dy}{dt} = \frac{dz}{dt}  = \frac{d\psi}{dt}  = \frac{d\theta}{dt} = \frac{d\phi}{dt} = \phi = \theta = 0  
\end{align}
$$


* nominal motor thrust to hover in an equilibrium:

$$
\begin{align}
&\sum F_z = 0 \\
& -mg + F_1 + F_2 + F_3 + F_4 = 0 \\
& -mg \vec{w}_z + (F_1 + F_2 _+ F_3 + F_4) \vec{b}_z = 0 \\
& -mg \vec{w}_z.\vec{z} + (F_1 + F_2 _+ F_3 + F_4) \vec{b}_z.\vec{w}_z = 0 \\
& -mg + 4 F \vec{b_z}.\vec{w}_z = -mg + 4F = 0 \\
\end{align}
$$

Using $$F=k_F \omega_i ^2$$, we arrive at:

$$
\begin{align}
\omega_i = \sqrt{ \frac{mg}{4 k_F}}
\end{align}
$$


## 3. Cascade Controller

Although motor speed is what is directly manipulated by the onboard computer, it is actually easier to think about controlling the quadropters in terms of net thrust and movement about the roll/pitch/yaw axes.

* controller 1; mainatains quad in position
* controller 2: 

In reality, most practical systems are MIMO (Multi-Input, Multi-Output). One way to handle MIMO systems is with a cascaded structure.
A general form of a cascaded controller looks like this:

<img src="/images/20180422/07-cascade-pid-control.png" width="500rem">

 Notice that there are two closed loop feedback controllers, with the primary controller wrapping around the secondary loop. This arrangement typically requires that the inner (secondary) loop execute much faster (>10x faster) than the outer (primary) loop. In other words, if the outer loop operates at 10 Hz the inner loop needs to operate at least 100 Hz. With this difference in update frequency, the primary setpoint of the outer loop will essentially look static to the inner loop.

In the context of the quadrotor, the primary controller is responsible for maintaining its 3D position and yaw angle while the inner loop controls attitude (i.e., the roll, pitch, and yaw angles). This control scheme would allow an operator to “let go” of the control inputs and have the quadrotor maintain a steady position and orientation in 3D space (assuming external disturbances are not too great!).

Without getting too deep into the mathematics involved, let’s let r represent the vector between the quadrotor’s current 3D location and its target position. The desired linear accelerations of the quadrotor’s center of mass can be found from PID feedback of this position error. By assuming that the roll and pitch angles stay near the equilibrium hover state, it is possible to linearize the equations of motion and express the desired roll and pitch angles as a function of the desired linear accelerations. The position controller passes the desired roll, pitch, yaw angles to the attitude controller. The attitude controller returns the changes in the nominal motor speeds that would move the quadrotor towards these desired roll and pitch angles.