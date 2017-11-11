# Questions

## 1. Student describes their model in detail. This includes the state, actuators and update equations.

##### The model used consits of 2-d cordinates, velocity, angle, and errors in cross track and orientation of the vehicle. Actuators are used to obtain the steering angle and the throttle of the vehicle around the track. As for the update equations, the equations on lesson 18 section 4 were used. Previous data were used to calculate current data.

	x_{t+1} & = x_t + v_t * cos(\psi_t) * dt \nonumber \\
	y_{t+1} & = y_t + v_t * sin(\psi_t) * dt \nonumber \\
	\psi_{t+1} & = \psi_t + \frac{v_t}{L_f} * \delta_t * dt \nonumber \\
	v_{t+1} & = v_t + a_t * dt \nonumber \\
	cte_{t+1} & = f(x_t) - y_t + v_t * sin(e\psi_t) * dt \nonumber \\
	e\psi_{t+1} &= \psi_t - {\psi}des_t + \frac{v_t}{L_f} * \delta_t * dt \nonumber
	
## 2. Student discusses the reasoning behind the chosen N (timestep length) and dt (elapsed duration between timesteps) values. Additionally the student details the previous values tried.

##### The values were initially set to N = 25 and dt = 0.05 to start off from the example given in the lessons. However to drive around the track smoothly and efficiently, I started lowering N and increasing dt. The Slack channel for this particular project was also looked into for some additional info. After trying many values (N = 22,20,18,15,10,5 and dt = 0.05, 0.07, 0.09, 0.1, 0.15), N and dt are now set to 10 and 0.1.

## 3. A polynomial is fitted to waypoints. If the student preprocesses waypoints, the vehicle state, and/or actuators prior to the MPC procedure it is described.

##### The vehicle's data was transformed so that the vehicle's x-y position would be at the origin and its psi(angle) at zero. This allows the fitting of the polynomial to waypoints.

## 4. The student implements Model Predictive Control that handles a 100 millisecond latency. Student provides details on how they deal with latency.

##### To handle the latency, the cost function of MPC was tweaked. In addition to suppression of variables as shown in the lesson, delta_start and v_start were suppressed to lessen the "swirving" when driving around the track. Also the actuations are activated one step later compared to the original example to take the latency in consideration. 

-------------------
## why smaller dt is better (finer resolution)
## why larger N isn't always better (computational time)
## how does time horizon (N * dt) affect the predicted path

##### Time horizon (T) is determined by two variables, N (Number of timesteps) and dt (timestep duration). N, the number of timesteps, seems not to be too large of a number, but not too small either. This is because when N is too large, although larger number allows the model to plan further ahead, the computation becomes much more complex, and when N is too small, the models prediction is too short, resulting in unstable drive path. Timesep duration (dt) needs to be small enough for fast and accurate computations, but when it is too small, it prevents the model from predicting furthur ahead without needing larger computation. When the time horizon (d * dt) is short, it is more responsive but less accurate. On the other hand, when the time horizon is long, the controls are less responsive but smoother.