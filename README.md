# scurve_traj_generation
This package generates jerk limited motion profile using s-curve [1-5]


### Usage
- To generate a jerk limited motion profile for any trajectory segment, call the `fit_traj_segment` function. this function requires the following arguments
```
p_start: initial position 
p_end  : final position
v_start: initial velocity
v_end  : final velocity
p_max  : position limitation 
v_max  : velocity limit
a_max  : acceleration limit
j_max  : jerk limit
```
The output (return value) of the function will be a set of a PiecewiseFunction describes the motion profile of the segment:
```
position     : a PiecewiseFunction describes the position w.r.t time  
velocity     : a PiecewiseFunction describes the velocity w.r.t time
acceleration : a PiecewiseFunction describes the acceleration w.r.t time
jerk         : a PiecewiseFunction describes the jerk w.r.t time
```

- To visualize the trajectory segment, call the function `fit_and_plot_segment` using the same argument of the `fit_traj_segment` function. this function will fit and plot the trajectory segment using matplotlib.

### Example
- `traj_segment_example.py`: this scripts contains predefined trajectory segments with different initial and final values for both position and velocity, this values will result in different motion profiles. By running this script, it will fit the predefined segments into the corresponding motion profile and then it will plot each segment in a separate figure.  

### Exceptions
This high level functions `fit_traj_segment` will first check the feasibility of generating a jerk limited motion profile based on the given arguments. If the given arguments are not feasible, then the function will throw an error message with a description why the given a arguments are not feasible. Examples of non-feasible motion:
- if any of the initial of final values violates the corresponding limits of the position, velocity, acceleration, and jerk 
- If the final velocity can not be reached from the initial position within the given motion in position (the position difference between the initial position and final position). because reaching any velocity from initial value requires a min position difference considering the acceleration and jerk limits.
- if the velocity direction is opposite to the motion direction required to achieve final position. for example, if initial and final position have  

### Test Files
- `test_traj_segment.py`: it has 34 test cases, each of them generates a different profile structure based on the given inputs. For each of them, the initial and final values (position & velocity) of the generated profile are tested against the given values to ensure that the generated profile reflects the given inputs. To run all the test cases inside this file:
```
nosetests test_traj_segment.py
```

- `test_traj_segment_middel_points.py`: it has 38 test cases, each of them generates a different profile structure. For each of them, the values at the transient points* of the position, velocity, acceleration, and jerk of the generated profiles are tested if they are within the corresponding limits or not. To run all the test cases inside this file:
```
nosetests test_traj_segment_middel_points.py
```

*transient points: it used here to represent the points at which the profile move from one phase to the next phase (e.g. from constant_jerk_phase to constant_acceleration_phase)

### Random Test Cases:
beside the provided test files which include around 38 test case, you can generate as many test cases as you wish using the following scripts:
- `generate_random_test_cases.py`: 
	This scripts generate random test cases (random initial and final position & velocity) which results in feasible motion. you can set the number of the test cases using the variable `num_of_cases`. you can change the position, velocity, acceleration, and jerk limits as well be setting the variables `p_max`, `v_max`, `a_max`, `j_max` respectively. You also can make the script generate random limits by resetting the variable `fixed_limit` to `false`. This scripts generates two scripts:
	- `test_autogenerated_random_cases.py`: this files contains all test cases which randomly generated using. To run it type  ```nosetests test_autogenerated_random_cases.py```
	- `plot_autogenerated_random_cases.py`: you can run this script to visualize all the generated test cases.
 
- `generate_exception_test_cases.py`: 
it does the same job as the previuos one `generate_random_test_cases.py`, However, it generates only test cases which results in non-feasible motion, see Exceptions section. 

### Assumption
* The current version considers that initial and final acceleration values are zeros.
* The current version assumes that the minimum limit for position, velocity, acceleration, and jerk are equal to the minus value the corresponding maximum value (e.g. minimum velocity = - maximum velocity)  


### References
1. [CONSTANT JERK EQUATIONS FOR A TRAJECTORY GENERATOR]
2. [Optimize S-Curve Velocity for Motion Control]
3. [On Algorithms for Planning S-curve Motion Profiles]
4. [A New Seven-Segment Profile Algorithm for an Open Source Architecture in a Hybrid Electronic Platform]
5. [Soft-Motion and Visual Control for service robots]
6. [A New Approach to Time-Optimal Path Parameterization Based on Reachability Analysis]
7. [Online Trajectory Generation: Basic Concepts for Instantaneous Reactions to Unforeseen Events]


[CONSTANT JERK EQUATIONS FOR A TRAJECTORY GENERATOR]: http://www.et.byu.edu/~ered/ME537/Notes/Ch5.pdf

[Optimize S-Curve Velocity for Motion Control]: https://www.researchgate.net/publication/257576837_Optimize_S-Curve_Velocity_for_Motion_Control

[On Algorithms for Planning S-curve Motion Profiles]: https://journals-sagepub-com.tudelft.idm.oclc.org/doi/pdf/10.5772/5652

[Soft-Motion and Visual Control for service robots]: https://www.researchgate.net/publication/228570194_Soft-Motion_and_Visual_Control_for_service_robots

[A New Seven-Segment Profile Algorithm for an Open Source Architecture in a Hybrid Electronic Platform]: https://www.researchgate.net/publication/333676286_A_New_Seven-Segment_Profile_Algorithm_for_an_Open_Source_Architecture_in_a_Hybrid_Electronic_Platform

[A New Approach to Time-Optimal Path Parameterization Based on Reachability Analysis]: https://www.researchgate.net/publication/318671280_A_New_Approach_to_Time-Optimal_Path_Parameterization_Based_on_Reachability_Analysis

[Online Trajectory Generation: Basic Concepts for Instantaneous Reactions to Unforeseen Events]: https://www-cs.stanford.edu/groups/manips/publications/pdfs/Kroeger_2010_TRO.pdf
