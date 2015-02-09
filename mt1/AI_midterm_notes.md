Agents
======

 - Agent perceives its **environment** therough **sensors** and acting upon that environment through **actuators**. 
 - A **percept** refers to the agent's perceptual inputs at any given instant. 
 - An agent's **percept sequence** is the complete history of everything the agent has ever perceived.
 - A **performance measure** evaluates any given sequence of environment states. It captures the notion of desirability.

Rationality
-----------

What is rational at any given time depends on four things:

 - The performance measure that defines the criterion of success.
 - The agent's prior knowledge of the environment.
 - The actions that the agent can perform.
 - The agent's percept sequence to date.

Hence, the definition of a **rational agent**: *For each possible percept sequence, a rational agent should select an action that is expected to maximize its performance measure, given the evidence provided by the percept sequence and whatever built-in knowledge the agent has.*

There is a distinction between **omniscience** and **rationality**. An **omniscient** agent knows the actual outcome of its actions and can act accordingly; but omiscience is impossible in reality. Hence a **rational** agent is not necessarily **omniscient**. It can only act based on what it knows and what it has done so far. 

A rational agent should also be **autonomous**. It should learn what it can to compensate for partial or incorrect prior-knowledge. It should also involve itself in **information gathering** and **exploration** with the aim to maximize performance and in order to modify future percepts.

PEAS
----

**P**erformance, **E**nvironment, **A**ctuators, **S**ensors. This helps us specify the task environment. For example, consider a taxi-driver agent:

 - **Performance Measure**: Safe, fast, legal, comfortable trip, maximize profits.
 - **Environment**: Roads, other traffic, pedestrians, customers.
 - **Actuators**: Steering, accelerator, brake, signal, horn, display.
 - **Sensors**: Cameras, sonar, speedometer, GPS, odometer, accelerometer, engine sensors, keyboard.

Properties of Task Environments
-------------------------------

 - **Fully observable** vs. **partially observable**
 - **Single agent** vs. **multi-agent**
 - **Deterministic** vs. **stochastic** (randomly determined; having random probability distribution).
 - **Episodic** vs. **sequential** - In episodic, agent's experience is divided into atomic episoodes. The next episode does not depend on actions taken in previous episodes (e.g. NN classifier for hand-written digits). In sequential, current decision depends on previous actions and can affect future decisions.
 - **Static** vs. **dynamic**
 - **Discrete** vs. **continuous**
 - **Known** vs. **unknown**

Turing Test
-----------

Introduced by Alan Turing in 1950. It is a test of a machine's ability to exhibit intelligent behavior equivalent to, or indistinguishable from, that of a human. A human judge engages in natural-language conversations with a human and a machine. The machine is designed to generate performance indistinguishable from that of a human being. Conversation is limited to a text-only channel. If the judge cannot reliably tell a machine from a human, the machine is said to have passed the test.

Uninformed Search
=================

**Problem formulation** is the process of deciding what actions and states to consider, given a goal. 

A **problem** can be defined formally by five components:

 - The **initial state** that the agent starts in.
 - A description of the possible **actions** available to the agent. Given a particular state `s`, `ACTIONS(s)` returns the set of actions that can be executed in `s`, i.e., each of these actions is **applicable** in `s`. 
 - A **transition model**, which is s description of what each action does. This is specified by a function `RESULT(s, a)` that returns the state that results from performing action `a` in state `s`. The term **successor** is used to refer to any state reachable from a given state by a single action. Together, the initial state, actions, and transition model implicitly define the **state space** of the problem. 
 - The **goal test**, which determines whether a given state is a goal state. 

Framing a search problem
------------------------

There are **six** components: **states**, **initial state**, **actions**, **transition model**, **goal test**, and **path cost**. Provide a mathematical description of the **transition model**, and **goal test** if possible.

Infrastructure for search algorithms
------------------------------------

For each node `n`, we have a structure that contains four components:

 - `n.STATE`: the state in the state space to which the node corresponds.
 - `n.PARENT`: the node in the search tree that generated this node.
 - `n.ACTION`: the action that was applied to the parent that generated this node (i.e., what action from the parent got me here?)
 - `n.PATH-COST`: the cost, traditionally denoted by `g(n)`, of the path from the initial state to the node, as indicated by the parent pointers.

Measuring problem-solving performance
-------------------------------------

 - **Completeness**: Is the algorithm guaranteed to find a solution when there is one?
 - **Optimality**: Does the strategy find the optimal solution? (**optimal solution**: the **solution** that has the lowest path-cost among all solutions).
 - **Time complexity**: How long does it take to find a solution?
 - **Space complexity**: How much memory is needed to perform the search?

Complexity is represented in terms of three quantities:

 - `b`: The **bgranching factor**, or maximum number of successors of any node.
 - `d`: The **depth** of the *shallowest* goal node.
 - `m`: The **maximum length** of any path in the state space. 

Comparing uninformed-search strategies
--------------------------------------

| **Criterion** | **BFS**            | **Uniform Cost**                     | **DFS**            | **Depth Limited**  | **ID-DFS**         | **Bidirectional**    |
|---------------|--------------------|--------------------------------------|--------------------|--------------------|--------------------|----------------------|
|   Complete?   | Yes<sup>a</sup>    | Yes<sup>a,b</sup>                    |   No               | Yes<sup>a</sup>    | Yes<sup>a</sup>    | Yes<sup>a,d</sup>    |
|     Time      | *O(b<sup>d</sup>)* | *O(b<sup>1+⌊C<sup>+</sup>/ϵ⌋</sup>)* | *O(b<sup>m>/sup>)* | *O(b<sup>l</sup>)* | *O(b<sup>l</sup>)* | *O(b<sup>d/2</sup>)* |
|     Space     | *O(b<sup>d</sup>)* | *O(b<sup>1+⌊C<sup>+</sup>/ϵ⌋</sup>)* | *O(bm)*            | *O(bl)*            | *O(bd)*            | *O(b<sup>d/2</sup>)* |
|    Optimal?   | Yes<sup>c</sup>    | Yes                                  |   No               | No                 | Yes<sup>c</sup>    | Yes<sup>c,d</sup>    |

(+ is actually supposed to be \*.)

Evaluation of tree-search strategies. *b* is the branching factor; *d* is the depth of the shallowest solution; *m* is the maximum depth of the search tree; *l* is the depth limit. Superscript caveats are as follows: <sup>a</sup> coplete if *b* is finite; <sup>b</sup> complete if step costs >= ϵ for positive ϵ. <sup>c</sup> optimal if step costs are all identical; <sup>d</sup> if both directions use breadth-first search. 

BFS (Breadth-first search)
--------------------------


