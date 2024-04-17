# GMF-swarm-robot
**Gravitational Mass-Field (GMF) Framework for Swarm Robotics and Multi-Robot Systems**

GMF is a parametric distributed control framework that follows the gravitational force equation below.

GMF principle: 
$` F_{ij} = \frac{g*m_i*m_j}{r_{ij}^2} `$
for all robots i and j that share a connectivity graph (i.e., robot j is in the (immediate) neighborhood of robot i)

Expected distance to be maintained following GMF principle: 
$` r_{ij}^{GMF}(t)  = \sqrt{\frac{{g}_i(t) m_i (t) m_j(t)}{F_{ij}(t)}} `$

Each robot i's control policy: 
$` u_i(t)  = K_p\sum\limits_{j \in {\mathcal{N}_{i}}}  (\|\tilde{r}_{ij} (t) \|^2 - \|r_{ij}^{GMF} (t)\|^2)  \tilde{r}_{ij}(t) `$, 
where $` \tilde{r}_{ij}(t) = (p_j(t) - p_i(t)) `$ is the vector towards robot j from robot i based on their current positions and $`\|\tilde{r}_{ij} (t) \| = \|p_j - p_i\| `$ is the Euclidean distance between robots i and j.

GMF can execute control policies in three domains: control of the multi-agent system through the current gravity 'g' value agreed in the system, local control of a node pair through the link force ($`F_{ij}`$) agreed among robots i and j, and the quantification of the agent’s capabilities using the mass $`m_i`$ values. Modifying these parameters causes private, local, and global-level changes and forces the inter-agent distances to satisfy the force equation. The low-level (primary) goal of GMF-based distributed control is to maintain the link force between it and its neighbors.

At any instant t, each robot maintains a local copy (time-stamped self-versions of) three sets of parameters: 
1) global $`g`$ value at robot i, which needs to be globally consistent (i.e., periodically achieve consensus),
2) local mass $`m_{i}`$ value, which can be changed based on any task robot i wants to perform (see boundary tracking for an example)
3) private link force $`F_{ij}`$ that robot i want to maintain with robot j based on their tasks (or to maintain personalized attraction and repulsive forces)

 
# Experiment Demonstrations

## Demonstrations on HeRoSwarm Robot Testbed

**Cooperative Object Pushing Demo** 
Robots execute formation control and cage an object. One of the robots is given a goal point to navigate to, while other robots coordinate and satisfy the formation control objective using the GMF principle.

![Cooperative Object Pushing Hardware Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/object_pushing.gif)

**Sequential Task Execution**
Some robots get specific goals (Tasks), while others coordinate their g values and adjust their positions to satisfy the GMF principle.

![Sequential Task Hardware Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/task_allocation_seq_7robots_4goal.gif)

## Demonstrations on Robotarium Simulator Testbed

**Global Rendezvous**
On a line graph, Robot 4 initiates rendezvous by decrementing the gravity (g) value over time. Other robots achieve this consensus on g, affecting all robots meeting (rendezvous) together at a global level. 

![Global Rendezvous Robotarium Simulator Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/global_rendezvous_line.gif)


**Local Rendezvous**
On a line graph, Robot 5 initiates rendezvous by decrementing its mass ($`m_{5}`$) value over time. Other robots react to this, affecting the robots in the neighborhood of robot 5 rendezvous together at a local level (since it is a line graph).

![Local Rendezvous Robotarium Simulator Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/local_rendezvous_line.gif)

**Private Rendezvous**
On a line graph, Robots 4 and 5 initiate rendezvous by incrementing their Force ($`F_{45}`$) value over time. To satisfy the GMF principle, robots 4 and 5 will have to reduce their distance to each other, resulting in rendezvous at a private level (to robots 4 and 5 only). However, others adjust their positions to satisfy the GMF principle based on the new changes to the system.

![Private Rendezvous Robotarium Simulator Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/private_rendezvous_line.gif)

**Boundary Tracking**
On a line graph, robots form a circular formation around a physical source initially. Tobots set their mass values based on the luminosity (signal strength) measurement at their position. When the intensity of the light source increases gradually, the robot's mass value increases, increasing its inter-robot distances and forming a larger circular shape. Vice versa, when the intensity decreases over time. This effect is seen as tracking the boundary of a target/source based on measurements. 

![Boundary Tracking Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/sensor_control_new-unimodal.gif)

**Task Execution (Sequential)**
On a line graph, out of 10 robots, four robots are given specific goal points in a sequential manner (one goal after another when the first robot reaches its goal point). The non-goal-reaching robots adjust their positions to satisfy the GMF principle. When they cannot satisfy (because there is no configuration allowing them to move to), they propose changes to the 'g' values and achieve consensus on this value globally. 

![Task Execution Sequential](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/task_allocation_ten_sequential.gif)

**Task Execution (Parallel/Dynamic)**
On a line graph, out of 7 robots, three robots are given specific goal points simultaneously, and these robots go to their goals. Other robots adjust their positions to satisfy the GMF principle. When they cannot satisfy (because there is no configuration allowing them to move to), they propose changes to the 'g' values and achieve consensus on this value globally. 

![Task Execution Parallel](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/task_allocation_parallel.gif)

## Demonstrations on Robotarium Hardware Testbed

**Multi-Robot Rendezvous**
On a complete graph, Robot 5 initiates the rendezvous objective by reducing its mass ($`m_5`$) value over time. This effect impacts the global level (whole group).

![Global Rendezvous Robotarium Hardware Demo](https://github.com/herolab-uga/GMF-swarm-robot/blob/main/gifs/global-rendezvous-robotarium-hardware.gif)
