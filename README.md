[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/L0glmnMn)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23552673&assignment_repo_type=AssignmentRepo)
# RMPC-lab1

First laboratory work for course Robot Motion Planning and Control.

Assignment

1. For the selected robot option, load the manipulator model from the toolbox **different then Puma560** and output the Denavit-Hartenberg parameters.
2. Specify the masses of the links, the positions of their centers of mass, the tensors of inertia, the moments of inertia of the drives, the coefficients of viscous friction of the drives, the coefficients of Coulomb friction of the drives, the gear ratios of the reducers and the constraints on the generalized coordinates. It is recommended to refer to the data sheets of the manipulators used.
3. Specify and display arbitrary initial and final configurations of the robot.
4. Plan a trajectory between the specified configurations.
5. Solve the inverse dynamics problem using the Newton-Euler method for the following scenarios: <br>
+ non-zero velocities and accelerations $\dot{q} \neq 0, \ddot{q} \neq 0$;
+ non-zero velocities and negligible accelerations $\dot{q} \neq 0, \ddot{q} \approx 0$ - quasi-statics;
+ zero velocities and accelerations $\dot{q} = 0, \ddot{q} = 0$ - maintaining a given position;
6. Determine the numerical values ​​of the elements of the matrices $M(q), C(q, \dot{q}), G(q)$ at each calculated moment of time.
7. Plot graphs of the moments of the manipulator links along the trajectory for each scenario (see point #5).
8. Create a report in .ipynb format, with detailed comments.
9. All steps and conclusions are formulated based on the results of the work in README.md

---

## Solution: KUKA IRB140 Industrial Manipulator

### 1. Full name and Group number

**Full Name:** Bui Dinh Khai Nguyen

**Group:** R4134c

**Robot Model:** KUKA IRB140 - A 6-DOF industrial collaborative manipulator

### 2. Robot Dynamic Parameters

Dynamic parameters were configured based on KUKA IRB140 technical specifications:

**Link Masses (kg):**
- Link 0 (Base): 28.0 kg
- Link 1 (Shoulder): 24.0 kg
- Link 2 (Upper arm): 9.2 kg
- Link 3 (Lower arm): 1.68 kg
- Link 4 (Wrist): 0.56 kg
- Link 5 (End-effector): 0.18 kg

**Inertia Tensors (kg⋅m²):** Configured for each link with principal axes aligned to DH frame

**Motor Parameters:**
- Motor inertia: 0.45×10⁻³ to 4×10⁻⁵ kg⋅m² (decreasing with joint position)
- Viscous friction coefficients (B): 0.0018 to 0.000044 N⋅s⋅m⁻¹
- Coulomb friction: ±0.48 to ±0.0048 N⋅m per joint
- Gear ratios: 73.5 to 90.0 (different sign convention per joint)
- Joint limits: ±2.96 to ±4.71 radians depending on joint

### 3. Trajectory Configuration

**Start Configuration:** $q_{\text{start}} = [0, 0, 0, 0, 0, 0]$ radians

**End Configuration:** $q_{\text{end}} = [\frac{\pi}{4}, -\frac{\pi}{3}, -\frac{\pi}{4}, \frac{\pi}{3}, -\frac{\pi}{3}, \frac{\pi}{4}]$ radians

### 4. Inverse Dynamics — Results

Section 7 plots show torque profiles for all six joints across three scenarios:

- **Full dynamics** (red): Joints 1–3 have the largest demands (~40 N⋅m, −40 N⋅m, ~20 N⋅m respectively), while distals (4–6) are modest (<2 N⋅m). Curves are smooth with gradual variation over time.
- **Quasi-static** (green dashed): Nearly identical to full dynamics, indicating that inertial and Coriolis effects contribute minimally to torque in this trajectory.
- **Static** (blue): Gravity-only baseline; noticeably different from dynamic curves, especially at proximal joints (e.g., Joint 2 shifts to ~−50 N⋅m holding).

---

## Conclusion

In this lab, I computed inverse dynamics for the KUKA IRB140 using Newton-Euler and discovered that torque demands are primarily driven by gravity and trajectory profile, not large acceleration spikes. The Section 7 plots show that moving along this trajectory requires moderate torques (peaking ~40 N⋅m on the shoulder joint), while the quasi-static approximation closely matches full dynamics, meaning this trajectory is relatively gentle. For accurate control, use gravity compensation as the baseline and add damping; aggressive accelerations would demand full dynamic modeling.
