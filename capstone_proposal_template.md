# Machine Learning Engineer Nanodegree
## Capstone Proposal
Said Aboulaban  
August 23rd, 2018

## Proposal
_(approx. 2-3 pages)_

### Domain Background

The project proposed has a main focus on Reinforcement Learning, focusing on Autonomus Quadcopter tasks. Autonomus UAVs has been a field of study for quite a long time, and there were major breakthroughs in the field, ranging from Model Free Actor/Critic methods; e.g. https://arxiv.org/abs/1509.02971, Trust Region Policy Optimization; e.g. https://arxiv.org/pdf/1502.05477.pdf, or a combination of both; e.g. https://arxiv.org/pdf/1707.05110.pdf. Studies focused on learning tasks that are mainly focused around flight learning, stabilization, etc. However, given the potential applications for UAVs, there are more tasks a UAV would require o accomplish. One of those is UAV's self protection through motion maneuvers. The idea is is inspired by bird's ability to avoid threats, through motion maneuvers by taking decisions in split seconds. This area is different than obstacle detection and avoidance, the project attempts to trust one predefined body, trust learning will not be part of this project, and react to actions targeted at it by untrusted bodies. This work will be part of a personal project to create UAVs for children safety, that can go into a "panic mode" when feels threatened.  


### Problem Statement
_(approx. 1 paragraph)_

As per: http://www.dronesglobe.com/guide/long-flight-time/, the longest flight time reported by the reference is 27 minutes, UAVs can be loaded with multiple sensors, cameras, etc, and all of them are necessary functions, however on the other hand cause battery drainage, that takes away from the flight duration. Thus if the goal is for example being a child companion to protect the child against risky interactions, or keeping the parents informed about unknown interactions, the UAV is expected to fly only when detects the situation, thus minimizing the actual flight time. Another issue is the risk of catching, and/or damaging the UAV by untrusted bodies. To summarize, the problem can be summarized into: battery drainage during flights, risk of collision and/or damage by unknown body, risk of collision and/or damage by 3D "hopping" into an obstacle.


### Datasets and Inputs
_(approx. 2-3 paragraphs)_

Since this is a Reinforcement Learning project no datasets will be used, however, flight and environment simulators will be used. The initial Quadcopter simulator used will be the physics_sim.py located at: https://github.com/udacity/RL-Quadcopter-2. 
model will be  

In this section, the dataset(s) and/or input(s) being considered for the project should be thoroughly described, such as how they relate to the problem and why they should be used. Information such as how the dataset or input is (was) obtained, and the characteristics of the dataset or input, should be included with relevant references and citations as necessary It should be clear how the dataset(s) or input(s) will be used in the project and whether their use is appropriate given the context of the problem.

### Solution Statement
_(approx. 1 paragraph)_

The aim of the project, is to train a Quadcopter to 3D "hop" when receives a panic signal; panic signal training is out of scope of this project, stabelize at the new 3D location for a given period of time; that time in reality can be used to take an image of the untrusted person and wait for the interaction to finish, or an acknowledgement signal from parent to trust the interaction, avoid approaching untrusted objects when airborne, and return to initial take-off position when airborne period is finished, or battery at low levels. Imaging, Learning threats, and waiting for interaction to finish learning are not part of this project. 

In this section, clearly describe a solution to the problem. The solution should be applicable to the project domain and appropriate for the dataset(s) or input(s) given. Additionally, describe the solution thoroughly such that it is clear that the solution is quantifiable (the solution can be expressed in mathematical or logical terms) , measurable (the solution can be measured by some metric and clearly observed), and replicable (the solution can be reproduced and occurs more than once).

### Benchmark Model
_(approximately 1-2 paragraphs)_

In this section, provide the details for a benchmark model or result that relates to the domain, problem statement, and intended solution. Ideally, the benchmark model or result contextualizes existing methods or known information in the domain and problem given, which could then be objectively compared to the solution. Describe how the benchmark model or result is measurable (can be measured by some metric and clearly observed) with thorough detail.

### Evaluation Metrics
_(approx. 1-2 paragraphs)_

In this section, propose at least one evaluation metric that can be used to quantify the performance of both the benchmark model and the solution model. The evaluation metric(s) you propose should be appropriate given the context of the data, the problem statement, and the intended solution. Describe how the evaluation metric(s) are derived and provide an example of their mathematical representations (if applicable). Complex evaluation metrics should be clearly defined and quantifiable (can be expressed in mathematical or logical terms).

### Project Design
_(approx. 1 page)_

In this final section, summarize a theoretical workflow for approaching a solution given the problem. Provide thorough discussion for what strategies you may consider employing, what analysis of the data might be required before being used, or which algorithms will be considered for your implementation. The workflow and discussion that you provide should align with the qualities of the previous sections. Additionally, you are encouraged to include small visualizations, pseudocode, or diagrams to aid in describing the project design, but it is not required. The discussion should clearly outline your intended workflow of the capstone project.

-----------

**Before submitting your proposal, ask yourself. . .**

- Does the proposal you have written follow a well-organized structure similar to that of the project template?
- Is each section (particularly **Solution Statement** and **Project Design**) written in a clear, concise and specific fashion? Are there any ambiguous terms or phrases that need clarification?
- Would the intended audience of your project be able to understand your proposal?
- Have you properly proofread your proposal to assure there are minimal grammatical and spelling mistakes?
- Are all the resources used for this project correctly cited and referenced?
