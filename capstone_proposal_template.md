# Machine Learning Engineer Nanodegree
## Capstone Proposal
Said Aboulaban  
August 23rd, 2018

## Proposal

### Domain Background

The project proposed has a main focus on Reinforcement Learning, focusing on Autonomus Quadcopter tasks. Autonomus UAVs has been a field of study for quite a long time, and there were major breakthroughs in the field, ranging from Model Free Actor/Critic methods; e.g. https://arxiv.org/abs/1509.02971, Trust Region Policy Optimization; e.g. https://arxiv.org/pdf/1502.05477.pdf, or a combination of both; e.g. https://arxiv.org/pdf/1707.05110.pdf. Studies focused on learning tasks that are mainly focused around flight learning, stabilization, etc. However, given the potential applications for UAVs, there are more tasks a UAV would require o accomplish. One of those is UAV's self protection through motion maneuvers. The idea is is inspired by bird's ability to avoid threats, through motion maneuvers by taking decisions in split seconds. This area is different than obstacle detection and avoidance, the project attempts to trust one predefined body, trust learning will not be part of this project, and react to actions targeted at it by untrusted bodies. This work will be part of a personal project to create UAVs for children safety, that can go into a "panic mode" when feels threatened. 

### Problem Statement

As per: http://www.dronesglobe.com/guide/long-flight-time/, the longest flight time reported by the reference is 27 minutes, UAVs can be loaded with multiple sensors, cameras, etc, and all of them are necessary functions, however on the other hand cause battery drainage, that takes away from the flight duration. Thus if the goal is for example being a child companion to protect the child against risky interactions, or keeping the parents informed about unknown interactions, the UAV is expected to fly only when detects the situation, thus minimizing the actual flight time. Another issue is the risk of catching, and/or damaging the UAV by untrusted bodies. To summarize, the problem can be summarized into: battery drainage during flights, risk of collision and/or damage by unknown body, risk of collision and/or damage by 3D "hopping" into an obstacle.

### Datasets and Inputs

Since this is a Reinforcement Learning project no datasets will be used, however, flight and environment simulators will be used. The initial Quadcopter simulator used will be the physics_sim.py located at: https://github.com/udacity/RL-Quadcopter-2. More advanced simulators can be used in the future if time of the project permits.

As the project is broken in multiple tasks, namely; hopping from steady state to another 3D point, there might be multiple hopping if needed, stabilizing at target location for a period of time, then returning to initial point. For that multiple proposed architectures will be trained initially for hopping against physics_sim simulator, where the agent would ultimately learn how to hop and navigate to a 3D location within a flight zone; flight zone will be pre-determined partial sphere, with untrusted object being the center. The untrusted object will be a volume in space randomly initialized every episode. The environment will define static objects as well to simulate an indoor environment, takeoff/landing position, and moving untrusted objects approaching the drone, that the drone will avoid by hopping. The environment class will be developed as part of this project. 

Environment class will be based on multiple setups, and will be constructed randomly at the begining of every episode. Setups being currently considered are:

1- Indoor setup with 4 walls, floor and a cieling
2- Indoor setup with 3 walls, floor and a cieling
3- Static Objects to be randomly placed in indoor setup:
	1- Desk(s)
	2- Side Table(s)
	3- Sofa
4- Outdoor setup
5- Static Objects to be randomly placed in outdoor setup:
	1- Tree(s)
	2- Fense(s)
	3- Car(s)
6- Takeoff/Landing Object: Cube with height unifromly randomly selected between 0.9m and 1.2m with allowed takeoff/landing area of 0.1x0.1 area
7- Untrusted Target: Cube with height unifromly randomly selected between 1.5m and 2.0m
8- Unstructed objects: Circle with random diameter between 0.01m to 0.3m
9- Trusted objects: Cube with height unifromly randomly selected between 0.9m and 2.0m

Other inputs include:
1- Panic Signal: A toggle to warn the drone of a risky encounter
2- Trust Signal: A toggle received to trust the encounter
3- Battery Level Signal: Random signal indicating battery state

The above signals will be randomly fed into the agent.

### Solution Statement

The solution to the problem in hand is to focus on the primary task of the drone, if the task is to protect and monitor risky encounters, minimizing the flight time becomes crucial in preserving the battery, and focus on the task in hand; screening the untrusted target. While maintaining proximity to trusted objects, during this process, the drone should avoid moving untrusted objects. The drone should be remain airborne until risky encounter is cleared, battery low, or trust signal is received from parent to trust the encounter. Imaging, Learning threats, and waiting for interaction to finish learning are not part of this project. In Reinfrocement Learning terminology during episode lifetime, a random panic signal will be issued, random battery low signal will be issued, and a random trust  signal will be issued. The agent will be rewarded for achieving the task of hopping, avoiding moving untrusted objects, avoiding crashing into objects, stabalizing in the flight zone for random duration, and landing back at the target location. And will be heavily penalized and episode is terminated if crashed into a moving or static object, or ran out of battery. Rewards convergence, average reward per episode, and learning time in terms of number of episodes,  will be the measures used to assess the performance of the agent(s) 

### Benchmark Model

Since the agent is planned to learn multiple taks, different benchmark models will be used. For navigation to a target within a flying zone, the following study will be used: https://arxiv.org/abs/1509.02971, and the main benchmark will be time to convergence. For collision detection and avoidance, the following study will be used: https://arxiv.org/pdf/1702.01182.pdf, and the benchmark will be the number of collisions per speed.

The challenge that I foresee is how to merge the methods; navigation and obsticle avoidance, or to maintain 2 agents; obstacle avoidance agent and navigation agent. I don't have a clear answer for this challenge at this point of time, as it requires thorough testing, and will be addressed during the project phase.

### Evaluation Metrics

As highlighted in the previous section, the following benchmarks will be used:

1- Average sum of rewards (costs) and its convergence
2- Number of crashes due to collision at different speeds.

It is important to note that although https://arxiv.org/pdf/1702.01182.pdf focuses collision avoidance for static objects, and the project focuses on dynamic objects while the drone is in a static location in the flight zone, the relative velocity if the same, whether the object is moving or it is the drone that is moving, thus I believe this study is still relevent.

Collisioning benchamrking will be based on different scenarios:
1- Collision with static objects during hopping
2- Collision with dynamic objects while drone airborne in steady state
3- Collision with static and dynamic objects while drone is airborne; hopping or steady state


### Project Design

Project will be broken into multiple phases:

Phase I: Environment Class creation and testing

During this phase, the focus will be on creating an env class to address the different env scenarios

Phase II: Actor/Critic/Agent Design:

During this phase, the plan is to implement the following classes:

1- Risk Averse Network: based on https://arxiv.org/pdf/1702.01182.pdf
	- Input Layers:
		- Current State
		- Shortest Distance to collision object
		- Relative velocity to collision object
		- Actions performed
	- Hidden Layers: 
		- Hidden Layer 1: 40 units, Activation RELU
		- Hidden Layer 2: 40 units, Activation RELU
	- Output Layer:
		- Collision Probability, None x 1, Activation Sigmid
	- Bootstrap: 5
	- Dropout Ratio: 0.05
	- Average of the 5 models is used to estimate along with a hyper-parameters lambda-std, and sample from that distribution a collision probability.
 
2- Critic Class: Will be based on function estimation for value function, implementation will be based on tensorflow
Expected Architecture is: based on https://arxiv.org/abs/1509.02971. The modification proposed is to add collision probability as an input into the critic networks
	- Input Layers:
		- Current State
		- Panic Signal
		- Safe Signal
		- Collision Probability
		- Batery State
		- Current Velocity
		- Current Angular Velocity
		- Last Action
	- Hidden Layers:
		- Action Hidden Layer 1: 32 units, Activation RELU
		- Action Hidden Layer 2: 64 units, Activation RELU
		- State Hidden Layer 1: 32 units, Activation RELU
		- State Hidden Layer 2: 64 units, Activation RELU
	- Output Layer:
		- Action Value Function Estimate: None x 1, Activation None
3- Actor Class: based on https://arxiv.org/abs/1509.02971. The modification proposed is to add collision probability as an input into the critic networks 
	- Input Layers:
		- Current State
		- Panic Signal
		- Safe Signal
		- Collision Probability
		- Batery State
		- Current Velocity
		- Current Angular Velocity
	- Hidden Layers:		
		- Hidden Layer 1: 32 units, Activation RELU
		- Hidden Layer 2: 64 units, Activation RELU
		- Hidden Layer 3: 32 units, Activation RELU
	- Output Layer:
		- Rotor Thrust
4- Agent:
	- The agent will be based on https://arxiv.org/abs/1509.02971 with modification of including collision probability. And if time permits, will test advantage function as a critic output into the actor class.
4- Taks: 3 main subtask will be created
	- Hop to fly zone
	- Stabilize and avoid moving obstacles
	- Land back
	- Reward function is planned as per the following:
		- Episode Ends with high penalty:
			- if Collision with any object
		- Episode Ends with high reward:
			- if returns to landing position
		- Incremental reward relative to distance as approaching fly zone
		- Higher incremental reward for every time step stayed in place while airborne in fly zone while panic signal is triggered
		- Higher incremental penlty for every time step stayed in place while airborne in fly zone while safe signal is triggered
		- Incremental penalty for every time step stayed in non-fly zone
		- Higher incremental reward relative to distance as approaching landing area from fly zone

Phase II: Benchmarking:

1- Benchmarking the agent's ability to navigate to fly zone, and return. No obstacles introduced. KPIs considered are reward function convergence, and episodes to convergence
2- Benchmarking the agent's ability to avoid obstacles given a randomly changing destination per episode. KPI considered is avergae number of crashes per speed 
3- Benchmarking the agent's ability to avoid moving obstacles while the drone is in steady state. KPI considered is avergae number of crashes per speed
4- Benchmarking the agent's ability to peform the full task; hop, steady, avoid, and land. KPIs considered are reward function convergence, and episodes to convergence
