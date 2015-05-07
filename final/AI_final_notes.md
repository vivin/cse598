Agents
======

 - Agent perceives its **environment** therough **sensors** and acts upon that environment through **actuators**. 
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

A rational agent should also be **autonomous**. It should **learn** what it can to compensate for partial or incorrect prior-knowledge. It should also involve itself in **information gathering** and **exploration** with the aim to maximize performance and in order to modify future percepts.

*As a general rule, it is better to design performance measures according to what one actually wants in the environment, rather than according to how one things the agent should behave*.

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
 - A description of the possible **actions** available to the agent. Given a particular state $s$, $ACTIONS(s)$ returns the set of actions that can be executed in $s$, i.e., each of these actions is **applicable** in $s$. 
 - A **transition model**, which is a description of what each action does. This is specified by a function $RESULT(s, a)$ that returns the state that results from performing action $a$ in state $s$. The term **successor** is used to refer to any state reachable from a given state by a single action. Together, the initial state, actions, and transition model implicitly define the **state space** of the problem. 
 - The **goal test**, which determines whether a given state is a goal state. 

In general, *an agent with several immediate options of unknown value can decide what to do by first examining* future *actions that eventually lead to states of known value.*

Framing a search problem
------------------------

There are **six** components: **states**, **initial state**, **actions**, **transition model**, **goal test**, and **path cost**. Provide a mathematical description of the **transition model**, and **goal test** if possible.

Infrastructure for search algorithms
------------------------------------

For each node $n$, we have a structure that contains four components:

 - $n.STATE$: the state in the state space to which the node corresponds.
 - $n.PARENT$: the node in the search tree that generated this node.
 - $n.ACTION$: the action that was applied to the parent that generated this node (i.e., what action from the parent got me here?)
 - $n.PATH-COST$: the cost, traditionally denoted by $g(n)$, of the path from the initial state to the node, as indicated by the parent pointers. This would be the sum of the individual step costs in the path. The **step cost** of taking action $a$ in state $s$ to reach state $s'$ is denoted by $c(s, a, s')$. 

Measuring problem-solving performance
-------------------------------------

 - **Completeness**: Is the algorithm guaranteed to find a solution when there is one?
 - **Optimality**: Does the strategy find the optimal solution? (**optimal solution**: the **solution** that has the lowest path-cost among all solutions).
 - **Time complexity**: How long does it take to find a solution?
 - **Space complexity**: How much memory is needed to perform the search?

Complexity is represented in terms of three quantities:

 - $b$: The **branching factor**, or maximum number of successors of any node.
 - $d$: The **depth** of the *shallowest* goal node.
 - $m$: The **maximum length** of any path in the state space. 

Comparing uninformed-search strategies
--------------------------------------

| **Criterion** | **BFS**            | **Uniform Cost**                                                   | **DFS**            | **Depth Limited**  | **ID-DFS**         | **Bidirectional**    |
|---------------|--------------------|--------------------------------------------------------------------|--------------------|--------------------|--------------------|----------------------|
|   Complete?   | $\text{Yes}^{a}$   | $\text{Yes}^{a,b}$                                                 |   No               | No                 | $\text{Yes}^{a}$   | $\text{Yes}^{a,d}$   |
|     Time      | $O(b^{d})$         | $O\big(b^{1+\left\lfloor\frac{C^{*}}{\epsilon}\right\rfloor}\big)$ | $O(b^{m})$         | $O(b^{l})$         | $O(b^{d})$         | $O(b^{\frac{d}{2}})$ |
|     Space     | $O(b^{d})$         | $O\big(b^{1+\left\lfloor\frac{C^{*}}{\epsilon}\right\rfloor}\big)$ | $O(bm)$            | $O(bl)$            | $O(bd)$            | $O(b^{\frac{d}{2}})$ |
|    Optimal?   | $\text{Yes}^{c}$   | Yes                                                                |   No               | No                 | $\text{Yes}^{c}$   | $\text{Yes}^{c,d}$   |

Evaluation of tree-search strategies. $b$ is the branching factor; $d$ is the depth of the shallowest solution; *m* is the maximum depth of the search tree; *l* is the depth limit. Superscript caveats are as follows: <sup>a</sup> complete if *b* is finite; <sup>b</sup> complete if step costs >= ϵ for positive ϵ. <sup>c</sup> optimal if step costs are all identical; <sup>d</sup> if both directions use breadth-first search. 

BFS (Breadth-first search)
--------------------------

A simple strategy in which root node is expanded first and then all successors of the root node are expanded next, then *their* successors, and so on. In general, all nodes are expanded at a given depth in the search tree, before any nodes at the next level are expanded. This means that the *shallowest* unexpanded node is chosen for expansion. To do this we use a FIFO queue (i.e., regular queue). **The goal test is applied to each node when it is generated rather than when it is selected for expansion**; this is because if we applied the test when it is selected for expansion, we would have to expand the whole layer of nodes at depth $d$ before the goal was detected, which makes the runtime complexity $O(b^{d + 1})$. The algorithm discards any new path to a state already in the frontier or explored set (any such state must be *at least as deep* as the one already found). Hence BFS always has the *shallowest* path to every node on the frontier.

 - **Completeness**: BFS is complete. If the shallowest goal-node is at some finite-depth $d$, BFS will eventually find it after generating all shallower-nodes (provided branching-factor $b$ is finite). 
 - **Optimality**: BFS is optimal, assuming that the path-cost is a non-decreasing function of the depth of the node. This is easily seen if all actions have the same cost. 
 - **Time**: We generate $b^h$ nodes at each level $h$. So the root generates $b$, and then each of those generate $b$, which leads to $b^2$ at the second level, and so on. Hence in general, we have $O(b^d)$.
 - **Space**: We store every expanded node in the *explored* set. This means that every node remains in memory, which gives us $O(b^{d - 1})$ in the *explored* set. The *frontier* set then has $O(b^d)$ nodes. This means that the size of the *frontier* dominates, which gives us a space complexity of $O(b^d)$.

**Memory requirements are a bigger problem for BFS than execution time.** That is, we will face the issue of running out of memory, long before the face the issue of waiting way too long for a solution. 

Uniform-cost search
-------------------

When all step costs are equal, BFS is optimal because it always expands the *shallowest* unexpanded node. How about an algorithm that is optimal with any step-cost function? 

 - Instead of expanding the shallowest node, **uniform-cost search** expands the node $n$ with the **lowest path-cost** $\mathbf{g(n)}$
 - This is done by storing the frontier **as a priority queue** ordered by $g$. 
 - The goal test is applied to a node when it is *selected for expansion* rather than when it is first *generated* (i.e., opposite of BFS). This is because the first goal-node that is *generated* may not necessarily be on the most-optimal path.
 - Another test is also added to check for the case where a better path is found to a node currently on the frontier. 

An example:

```
                  99
[Sibiu] -----------------------[Fagaras]
   \                               |
    \                              |
     \ 80                          |
      \                            |
 [Rimnicu Vilcea]                  |
        \                          |
         \                         |
          \                        |
           \ 97                    |
            \                      |
             \                     |
          [Pitesti]                |
               \                   |
                \                  |
                 \ 101             |
                  \                | 211
                   \               |
                    \              |
                     \             |
                      \            |
                       \           |
                        \          |
                         \         |
                          \        |
                           \       |
                            \      |
                             \     |
                              \    |
                               \   |
                                \  |
                                 \ |
                                  \|
                             [Bucharest]
```

The problem is to get from **Sibiu** to **Bucharest**. The successors of **Sibiu** are **Riminicu Vilcea** and **Fagaras** with path-costs $80$ and $99$ respectively. Since **Riminicu Vilcea** is the least-cost node, it is expanded next, which gives us **Pitesti** with a total path-cost of $80 + 97 = 177$. Now **Fagaras** is the least-cost node, and so it is expanded, giving us **Bucharest** with a cost of $99 + 211 = 310$. Now although **Bucharest** is the goal node, we keep going since we don't perform the goal test on *generated* nodes. So we will next choose **Pitesti** for expansion which adds a second path to **Bucharest** with the cost $80 + 97 + 101 = 278$. The algorithm now checks to see if this new path is better than the old one; it is and so the old one is discarded. **Bucharest** with $g$-cost $278$ is now selected for expansion and then returned as a solution (because we perform the goal test when a node is selected for expansion). 

 - **Optimality**: Uniform-cost search is optimal in general. Whenever it selects a node $n$ for expansion, the optimal path to that node as been found. Why is this? Assume there exists another frontier node $n'$ on the optimal path from the start node to $n$. By definition, $n'$ would have a lower $g$-cost than $n$ and would have been selected first. Since step-costs are non-negative, paths will never get shorter as nodes are added. 
 - **Completeness**: Uniform-cost search is complete, assuming that the branching-factor $b$ is finite, and that the step costs are $\ge \epsilon$ where $\epsilon$ is some small positive-number. If the second assumption does not hold, it can get stuck in an infinite loop if there is a path with an infinite sequence of zero-cost actions (i.e., a sequence of *NoOp* actions). Completeness is therefore guaranteed provided the cost of every step exceeds some small positive-constant $\epsilon$.
 - **Time** and **Space**: The algorithm is guided by path costs rather than depth and so the runtime and space complexity cannot really be expressed in terms of $b$ and $d$. Instead, let $C^\*$ be the cost of the optimal solution, and assume that every action costs *at least* $\epsilon$. Then the worst-case time and space complexity is $O\big(b^{1+\left\lfloor\frac{C^\*}{\epsilon}\right\rfloor}\big)$, which can be much greater than $b^d$. Here we're first looking at the cost per step, and then the branching-factor is raised to that power. We add $1$ because we apply the goal test when we *select nodes for expansion*. The reason we can have runtime and space complexity greater than $b^d$ is because the algorithm can explore large trees of small steps before exploring paths that involve large, and perhaps useful steps. When all step costs are equal, uniform-cost search is similar to BFS except that BFS stops as soon as it generates a goal whereas uniform-cost examines all nodes at the goal's depth to see if any have a lower cost. Hence, uniform-cost search does more work by expanding nodes at depth $d$ unnecessarily.

DFS
---

This search algorithm always expands the *deepest* node in the current frontier of the search tree. The search goes to the deepest level of the search tree, where the nodes have no successors. As those nodes are expanded, they are removed from the frontier, so then the search "backs up" to the next deepest node that still has unexplored successors. Uses a stack (LIFO queue). This means the most-recently-generated node is selected for expansion. 

 - **Completeness**: The graph-search version (which avoids repeated states and redundant paths) is complete in finite state-spaces because it will eventually expand every node. The tree-search version is **not complete**; it can keep following a loop forever. The DFS tree-search algorithm can be modified at no extra memory-cost so that it checks new states against those on the path from the root to the current node. This avoids infinite loops in finite state-spaces but does not avoid the issue of redundant paths. In infinite state-spaces, both versions fail if an infinite non-goal path is encountered (e.g., Knuth's 4 problem; DFS will keep applying the same operator over and over again). 
 - **Optimality**: Both versions are non-optimal for similar reasons. Assuming we have a search space where we have a goal node $C$ on the right-subtree at some depth $d$, and a goal node $J$ on the left subtree at some depth $d'$ ($d' > d$). Then DFS will start by exploring the left subtree even though $C$ is a goal node. Furthermore, it would end up returning $J$ as a solution even though $C$ is a better solution. Hence DFS is not optimal. 
 - **Time**: The time complexity for DFS graph-search is bounded by the size of the state-space (which could be infinite). A DFS tree-search however, may end up generating all of the $O(b^m)$ nodes in the search tree, where $m$ is the maximum depth of any node (so this can be much bigger than the size of the state space). Also, $m$ itself can be much larger than $d$, and is infinite if the tree is unbounded. For an example, consider a binary tree where the goal node is the deepest and right-most node. In this case, DFS will generate *all* nodes before it gets to the goal node. Even if the goal node is at depth *d* which is much smaller than *m*, the time-complexity is still dominated by the fact that it is still exploring all the other nodes, and hence we still end up with $O(b^m)$.
 - **Space**: The space complexity is the reason we consider DFS. There is no advantage for a graph search, but in a DFS tree search, we only need to store a single path from the root to a leaf node, along with any remaining, unexpanded sibling-nodes for each node in the path. Once a node has been fully expanded, it can be removed from memory as soon as all of its descendants have been fully explored. Hence, the storage is only $O(bm)$ for a state space with branching-factor $b$ and maximum depth $m$. 

Depth-limited DFS
-----------------

DFS fails in infinite search-spaces. This failure can be alleviated by using a variation called depth-limited DFS. In this algorithm, DFS is supplied with a predetermined depth-limit $l$. This means that nodes at depth $l$ are treated as if they have no successors. This solves the infinite path problem. However, we have an additional source of incompleteness if we choose $l < d$ (i.e., the shallowest goal is beyond the depth limit. This usually happens when $d$ is unknown). Depth-limited DFS is also nonoptimal if we chose $l > d$ (for reasons of nonoptimality in DFS in general). 

 - **Completeness**: Incomplete if $l < d$ (but also incomplete in general).
 - **Optimality**: Nonoptimal when $l > d$.
 - **Time**: $O(b^l)$.
 - **Space**: $O(bl)$.

ID-DFS
------

This is a general strategy used in combination with DFS tree search that finds the best depth limit. The algorithm does this by gradually increasing the depth (first 0, then 1, then 2, and so on) until a node is found. This occurs when the depth limit reaches $d$, the depth of the shallowest goal-node. Iterative deepening combines the benefits of DFS and BFS.

 - **Completeness**: It is complete if $b$ is finite.
 - **Optimal**: It is optimal if all step-costs are identical. 
 - **Time**: $O(b^d)$.
 - **Space**: $O(bd)$.

This can seem wasteful because states are generated multiple times. But it turns out this is not too costly, this is because in a search tree with the same (or nearly the same) branching factor at each level, most of the nodes are in the bottom level and so it does not matter that the upper levels are generated multiple times. In an ID-DFS, the nodes at depth $d$ are generated once, the ones are depth $d - 1$ are generated twice, and so on. Hence we have:

$$
N(IDS) = (d)b + (d - 1)b^2 + \dotso + (1)b^d\text{, which is }O(b^d)
$$

There is an extra cost of generating the upper levels multiple times, but it is not too large. For example, with *b* set to $10$ and *d* set to $5$, we have:

$$
N(IDS) = 50 + 400 + 3,000 + 20,000 + 100,000 = 123,450
$$
$$
N(BFS) = 10 + 100 + 1,000 + 10,000 + 100,000 = 111,110
$$

**In general, ID-DFS is the preferred uninformed-search method when the search space is large and the depth of the solution is not known.**

Informed Search
===============

An informed-search strategy is one that uses problem-specific knowledge beyond the defintion of the problem itself. It can find solutions more efficiently than can an uninformed strategy.

The general approach that we consider is called **best-first search**. This is an instance of the general $TREE-SEARCH$ or $GRAPH-SEARCH$ algorithm in which a node is selected for expansion based on an *evaluation function* $f(n)$. The evaluation function is construed as a cost estimate, and a node with the *lowest* evaluation is expanded first. The implementation of best-first search is similar to uniform-cost search except we use $f$ instead of $g$ to order the priority queue.

**The choice of $f$ determines the search strategy.** Most best-first algorithms include as a component of $f$, a **heuristic function**, denoted as $h(n)$:

 - $h(n)$: estimated cost of the cheapest path from the state at node $n$ to a goal state.

Although $h(n)$ takes a node as input, it depends on the *state* at that node. This is unlike $g(n)$, which returns the path cost from the root to node $n$. If $n$ is a goal node, $h(n) = 0$. 

Greedy Best-First Search
------------------------

Here $f(n) = h(n)$.

 - **Optimality**: It is not optimal, and to see why, consider a path A-B-C and A-C. The heuristic cost from A-B is 50, the cost from B-C is 90, and the cost from A-C is 100. Greedy best-first search will choose B for expansion because 50 is lesser than 100. After that, it will choose to go from B to C because the cost is 90, which is lesser than 100. However, the path from A to C via B is 40 more than the path from A to C directly. Hence it is not optimal. 
 - **Completeness**: It is incomplete in a finite state-space like DFS. Consider the same situation above, but with the difference that there is no path from B-C. Then in the tree-search version, B is repeatedly expanded the path from A to C will never be taken (infinite loop). The graph-search version *is* complete in finite spaces, but not in infinite ones.
 - **Time** and **space**: The worst-case complexity for tree-search version is $O(b^m)$, where $m$ is the maximum depth of the search space. 

A\* search
----------

** A\* ** search is both **complete** and **optimal**. It is also **optimally efficient** for any given consistent-heuristic.

Here $f(n) = g(n) + h(n)$.

Since $g(n)$ is the path cost from start node to node $n$, and $h(n)$ is the estimated cost of the cheapest path from $n$ to the goal, we have:

$f(n)$ = estimated cost of the cheapest solution **through** $n$.

The algorithm is identical to uniform-cost search except it uses $g + h$ instead of $g$.

A\* has conditions for optimality. These are **admissibility** and **consistency**.

The first condition required for optimality is that $h(n)$ is an **admissible heuristic**:

**Admissible Heuristic**: A heuristic that *never overestimates* the cost to reach the goal. This means that $f(n)$ also never overestimates the true cost of a solution along the current path through $n$. Admissible heuristics are optimistic by nature because they think the cost of solving the problem is less than it actually is (e.g.: straight-line distance).

**Consistent Heuristic**: This is a stronger condition that is required only for applications of A\* to graph search. A heuristic $h(n)$ is consistent if, for every node $n$ and every successor $n'$ of $n$ generated by any action $a$, the esimated cost of reaching the goal from $n$ (i.e., $h(n)$) is no greater than the step cost of getting to $n'$ ($c(n, a, n')$) plus the estimated cost of reaching the goal from $n'$ ($h(n')$):

$h(n) \le c(n, a, n') + h(n')$

This is a form of the general **triangle inequality**. 

**Optimality of A**\*: The tree-search version of A\* is optimal if $h(n)$ is admissible, while the graph-search version is optimal if $h(n)$ is consistent. 

How is A\* optimal? First, *if $h(n)$ is consistent, then the values of $f(n)$ along any path are nondecreasing.* The proof follows directly from the definition of consistency. Supposed $n'$ is a successor of $n$; then $g(n') = g(n) + c(n, a, n')$ for some action $a$ and we have:

$$
f(n') = g(n') + h(n')
      = g(n) + c(n, a, n') + h(n') \ge g(n) + h(n) 
                                   = f(n)
$$

The next thing we have to prove is that *whenever A*\* *selects a node $n$ for expansion, the optimal path to that node has been found.* If this was not the case, it means that there would have to be another frontier node $n'$ on the optimal path from the start node to $n$. Since $f$ is nondecreasing along any path, $n'$ would have a lower $f$-cost than $n$ and would have been selected first. Hence a node like $n'$ cannot exist. 

Heuristic Development
---------------------

Heuristic accuracy has an effect on performance. The quality of a heuristic is measured by the **effective branching factor** $b^\*$. If the total number of nodes generated by A\* for any particular problem is $N$ and the solution depth is $d$, then $b^\*$ is the branching factor that a uniform tree of depth $d$ would have to have in order to contain $N + 1$ nodes. Hence:

$$
N + 1 = 1 + b^\* + (b^\*)^2 + \dotso + (b^\*)^d
$$

For example, if A\* finds a solution at depth 5 using 52 nodes, the effective branching-factor is 1.92.

It is therefore desirable to generate good heuristics. There are multiple ways to do it:

 - **Relaxed problems**: The heuristics $h\_1$ (misplaced tiles) and $h\_2$ (Manhattan distance) are fairly-good heuristics for the 8-puzzle problem. Looking at them closely, they are perfectly-accurate path-lengths for *simplified* versions of the puzzle. For the first one, the rule is changed so that a tile could move anywhere instead of just to the adjacent empty square. Then, $h\_1$ gives us the exact number of steps in the shortest solution. If we changed the rule further such that we could move the tile one square in any direction, even to an occupied square, then $h\_2$ gives us the exact number of steps in the shortest solution. A problem with fewer restrictions on its actions is called a **relaxed problem**. This makes the state-space for the relaxed problem a *supergraph* of the original state-space, because the removal of restrictions creates additional edges (i.e., there are more ways we can transition from one state to another since restrictions are removed). Any optimal solution in the original problem is, by definition, also a solution in the relaxed problem; but the relaxed problem may have *better* solutions if the added edges provide short-cuts. Hence, *the cost of an optimal solution to a relaxed problem, is an admissible heuristic for the original problem* (since it can *never* overestimate). Also, since the derived heuristic is an exact cost for the relaxed problem, it must obey the triangle inequality and is therefore **consistent**. 
 - **$ABSOLVER$**: It can generate heuristics automatically from problem definitions, using the "relaxed problem" method and various other techniques (Prieditis, 1993).
 - **Pattern databases**: Admissible heuristics can be derived from the solution cost of a **subproblem** of a given problem. The cost of an optimal solution of a subproblem is definitely a lower-bound on the cost of the complete problem. Hence it is an admissible heuristic. The idea behind **pattern databases** is to store these exact solution-costs for every-possible subproblem-instance. We can compute an admissible heuristic $h\_DB$ for each complete state encountered during a search, simply by looking up the corresponding subproblem configuration in the database. The database itself is constructed by searching back from the goal and recording the cost of each new pattern encountered; the expense of this search is amortized over mnay subsequent problem instances. 
 - **Learning heuristics from experience**: We can solve lots of problems and each optimal solution provides examples from which we can learn $h(n)$. Another way is use **features** of a state that are relevant to predicting the state's value rather than the raw state-description. For example, we can generate 100 random 8-puzzle configurations and gather statistics on their actual solution-costs. For example, assume we have a feature called "number of misplaced tiles", that is $x\_1(n)$. We might find that when this value is 5, the average solution cost is around 14 and so on. A second feature $x\_2(n)$ would be "number of pairs of adjacent tiles that are not adjacent in the goal state". These features can then be combined to generate an $h(n)$. A common approach is to use linear combination: $h(n) = c\_1x\_(n) + c\_2x\_2(n)$. The constants are adjusted to get the best fit to the actual data on solution costs. The heuristic will satisfy the condition that $h(n) = 0$ for goal states, but **is not necessarily admissible or consistent**.

What happens if one fails to get a single, "clearly-best" heuristic from the ones generated? What if a collection of admissible heuristics is available such that none of them dominates any of the others? We can have the best of both worlds by doing:

$h(n) = \max\{h\_1(n), \dotso, h\_m(n)\}$

Local Search
============

In many problems, path to the goal is irrelevant (e.g., 8 queens problem). What we usually care about is the final solution. Hence, we don't need to store the path to the solution, but can simply explore the solution space to either maximize/minimize the objective function (depending on whether we are maximizing payoff or minimizing cost). 

Hill-climbing search
--------------------

This is a simple search algorithm that simply loops and moves in the direction of increasing value (uphill). It will terminate when it reaches a peak (i.e., a point where none of the neighbors have a higher value). This algorithm is also known as **greedy local search**. Unfortunately, hill climbing will get stuck for the following reasons:

 - **Local maxima**
 - **Ridges** (sequence of local maxima that is very difficult for greedy algorithms to navigate)
 - **Plateaux** (flat area).

There are variations to get around these difficulties:

 - **Stochastic hill climbing** chooses at random from among the uphill moves; the probability of selection can vary with the steepness of the uphill move. This usually converges more slowly than steepest ascent, but in some state landscapes, it finds better solutions.
 - **First-choice hill climbing** implements stochastic hill-climbing by generating successors randomly until one is generated that is better than the current state. This is a good strategy when when a state has many (e.g., thousands) of successors.
 - **Random-restart hill climbing**: The above algorithms are incomplete; they often fail to find a goal when one exists because they can get stuck on a local maxima. In random-restart, we conduct a series of hill-climbing searches from randomly-generated initial states until a goal is found.

Simulated annealing
-------------------

The regular hill-climbing algorithm *never* makes "downhill" moves towards states with a lower value (or higher cost). Hence, it is guaranteed to be incomplete. A purely-random walk would allow us to move both uphill and downhill, but is very inefficient. How could we combine the two? We can do this with **simulated annealing**. The innermost loop of the simulated-annealing algorithm is quite similar to hill climbing, except instead of choosing the *best* move, it chooses a *random* move. If the move improves the situation, it is always accepted. Otherwise (i.e., if the move is "bad"), the algorithm accepts the move with some probability less than 1. The probability decreases exponentially with the "badness" of the move (i.e., the amount of $\Delta E$ by which the evaluation is worsened), and also decreases as the "temperature" $T$ goes down. This means that "bad" moves are more likely to be allowed at the start when $T$ is high, and they become more unlikely as $T$ decreases. If the *schedule* lowers $T$ slowly enough, the algorithm will find a global optimum with probability approaching 1.

Local beam search
-----------------

The **local beam search** algorithm keeps track of $k$ states rather than just one state. It begins with $k$ randomly-generated states. At each step, all successors of all $k$ states are generated. If any one of those is a goal, the algorithm halts. Otherwise, it selects the $k$ best successors from the complete list and repeats. This may look like we are simply running $k$ random restarts in parallel. However, in random-restart each search process runs independently of the others. *In a local beam-search, useful information is passed among the parallel search-threads*. This means the algorithm abandons unfruitful searches and moves towards the area where most progress is being made.

Adversarial Search
==================

Algorithms that cover **competitive** environments in which the agents' goals are in conflict.

MINIMAX algorithm
-----------------

Assume there are two players $MAX$ and $MIN$. $MAX$ always chooses a move that maximizes the payoff, whereas $MIN$ will always choose a move that will minimize $MAX$'s payoff. So here the strategy is built up assuming that each player plays optimally. Given a game tree, the optimal strategy can be determined from the **minimax value** of each node, which is written as $MINIMAX(n)$. 

$$
MINIMAX(s)
$$
$$
= UTILITY(s)\text{ if }TERMINAL-TEST(s)
$$
$$
= \max\_{a \in Actions(s)}\ MINIMAX(RESULT(s, a))\text{ if }PLAYER(s) = MAX
$$
$$
= \min\_{a \in Actions(s)}\ MINIMAX(RESULT(s, a))\text{ if }PLAYER(s) = MIN
$$

The **minimax algorithm** computes the minimax decision from the current state. It uses a simple, recursive computation of minimax values of each successor state, directly implementing the defining equations. The recursion proceeds all the way down to the leaves of the tree, and then the minimax values are **backed up** through the tree as the recursion unwinds. 

Assume the following tree:

![minimax](http://imgur.com/sXrSXr5.png)

The recursion proceeds all the way to three bottom-left nodes, and here it uses the $UTILITY$ function on them to discover that their values are 3, 12, and 8. Since the level above is where $MIN$ would play, it takes the minimum of the three values and returns it as the backed-up value of node B. A similar process gives the backed-up values for nodes C and D. Since the root is where $MAX$ plays, we take the maximum of the values, which gives us 3, which also ends up being the backed-up value of the root node.

The minimax algorithm performs a complete depth-first exploration of the game tree. If the maximum depth of the tree is *m* and there are *b* legal moves at each point, then the time complexity of the minimax algorithm is $O(b^m)$. The space complexity is $O(bm)$ for an algorithm that generates all actions at once, or $O(m)$ for an algorithm that generates them one at a time.

Alpha-Beta Pruning
------------------

With minimax search, the number of game states it has to examine is exponential in the depth of the tree. We cannot eliminate the exponent, but we can cut it in half by **pruning**. The idea is that we can compute the correct minimax decision without looking at every node in the game tree. The general principle is this: consider a node $n$ somewhere in the tree, such that the player has a chance of moving to that node. If the player has a better choice $m$ either at the parent of node $n$, or at any choice point further up, then $n$ will never be reached in actual play. So once we have found out enough about $n$ (by examining some of its descendants) to reach this conclusion, we can prune it. 

The algorithm gets its name from the following two parameters that describe the bounds on the backed-up values that appear anywhere along the path:

 - $\alpha$ = the value of the best (i.e., highest-value/upper-bound) choice we have found so far at any choice point along the path for $MAX$.
 - $\beta$ = the value of the best (i.e., lowest-value/lower-bound) choice we have found so far at any choice point along the path for $MIN$.

To see it in action, consider this tree:

```
                     A
                   / | \
                  /  |  \
                 /   |   \
                /    |    \
               /     |     \
              /      |      \
             /       |       \
            B        C        D
           /|\      /|\      /|\
          / | \    / | \    / | \
         /  |  \  /  |  \  /  |  \        
        11  9 13 7   5 14 2  10   3
```

First we will go all the way down the left sub-tree to find 11, which is initially the lowest-value at B (MIN). On examining other nodes, we find that 9 is actually the lowest value. So B goes through the following value changes:

 - B: [$-\infty$, 11] -> [$-\infty$, 9] -> [9, 9]

Since the MIN value at B is 9, we know that A (MAX) can currently choose a value that is at least 9. So we end up with the following values:

 - A: [9, $+\infty$]
 - B: [9, 9]

Now we will walk through the next subtree. Here, we first find 7. So the values are:

 - A: [9, $+\infty$]
 - B: [9, 9]
 - C: [$-\infty$, 7]

If we explore any more of C's children, they can only have values that are lesser than equal to 7, because MIN at C will choose the smallest value. This means that we will never get a value that is higher than 7. We also know that MAX chooses the highest value. The option that MAX has at A is 9, so it will **never** choose C! This means that we don't have to explore any other nodes under C.

Now we will explore D. Here, we first find 2. So the values are:

 - A: [9, $+\infty$]
 - B: [9, 9]
 - C: [$-\infty$, 7]
 - D: [$-\infty$, 2]

For the same reasons as in the case of C, MAX will never pick D because 2 is smaller than its best choice of 9. This means that we don't have to explore any of D's other children either. Hence, we finally end up with A: [9, 9] which means we play 9. 

Let's also see it in action with the tree rotated:

```
                     A
                   / | \
                  /  |  \
                 /   |   \
                /    |    \
               /     |     \
              /      |      \
             /       |       \
            B        C        D
           /|\      /|\      /|\
          / | \    / | \    / | \
         /  |  \  /  |  \  /  |  \        
        3  10  2 14  5  7 13  11  9
```

First we will go all the way down the left sub-tree. B ends up going through the following value changes:

 - B: [$-\infty$, 3] -> [$-\infty$, 2] -> [2, 2]

Since the MIN value at B is 2, we know that A (MAX) can only choose a value that is at least 2. So we end up with the following values:

 - A: [2, $+\infty$]
 - B: [2, 2]

Now we will walk through C's subtree. Here we first find 14. This is better than the current best-choice of 2, so we have the following values:

 - A: [2, 14]
 - B: [2, 2]
 - C: [$-\infty$, 14]

We then find 5 and 7 under C, which means we end up with the following:

 - A: [2, 14] -> [2, 5]
 - B: [2, 2]
 - C: [$-\infty$, 14] -> [$-\infty$, 5] -> [5, 5]

Now we look at D:

 - A: [2, 5] -> [2, 13] -> [2, 11] -> [2, 9] -> [9, 9]
 - B: [2, 2]
 - C: [5, 5]
 - D: [$-\infty$, 13] -> [$-\infty$, 11] -> [$-\infty$, 9] -> [9, 9]

So we play 9. What is interesting is that the rotated tree forced us to examine every-single node, which means we pretty much ended up with minimax. This will happen if we examine the children of a node in order of decreasing utility. 
Introduction and Decision Trees
===============================

 - **Unsupervised Learning**: The agent learns patterns in the input even though no explicit feedback is supplied. Most common unsupervised learning-task is clustering: detecting potentially useful clusters of input examples. For example, a taxi agent might gradually develop a concept of "good traffic days" and "bad traffic days" without ever being given labeled examples.
 - **Reinforcement learning**: The agent learns from a series of reinforcements - rewards or punishments. 
 - **Supervised learning**: The agent observes some example input - output pairs and learns a function that maps from input to output. The outputs can come from a teacher who gives the agent information about what the output is. The output can also come from the agent's percepts and the environment ends up being the teacher.

Noise and lack of labels create a continuum between supervised and unsupervised learning. 

Supervised Learning
-------------------

The task of supervised learning is this:

Given a **training set** of $N$ example input-output pairs $(x\_1, y\_1), (x\_2, y\_2),  \dotso, (x\_N, y\_N)$, where each $y\_j$ was generated by an unknown function $y = f(x)$, discover a function $h$ that approximates the true function $f$. 

$x$ and $y$ can be any value; they need not be numbers. The function $h$ is a **hypothesis**. Learning is a search through the space of possible hypotheses for one that will perform well, even on new examples beyond the training set. To measure the accuracy of a hhypothesis we give it a **test set** of examples that are distinct from the training set. We say a hypothesis **generalizes** well if it correctly predicts the value of *y* for novel examples. Sometimes the function $f$ is **stochastic** - it is not strictly a function of $x$, and what we have to learn is a **conditional probability** distribution $P(Y | x)$.

**Classification**: When the output $y$ is one of a finite set of values (such as $sunny$, $cloudy$, or $rainy$), the learning problem is called **classification**, and is called boolean or binary classification if there are only two values.

**Regression**: When $y$ is a number (such as tomorrow's temperature), the learning problem is called **regression**. (Technically, solving a regression problem is finding a conditional expectation or average value of $y$, because the probability that we have found *exactly* the right real-valued number for $y$ is 0.)

How do we choose from among multiple, consistent hypotheses? One answer is to prefer the *simplest* hypothesis consistent with the data. This principle is called **Ockham's razor**.  In general there is a tradeoff between complex hypotheses that fit the training data well, and simpler hypotheses that may generalize better (i.e., the question of overfitting). 

We say a learning problem is **realizable** if the hypothesis space contains the true function. Unfortunately we cannot always tell whether a given learning problem is realizable. 

Learning Decision Trees
-----------------------

A **decision tree** represents a function that takes as input a vector of attribute values and returns a "decision" - a single output value. The input and output values can be discrete or continuous. A decision tree reaches its decision by performing a sequence of tests. Each internal node in the tree corresponds to a test of the value of one of the input attributes, $Ai$, and the branches from the node are labled with the possible values of the attribute, $A\_i = v\_{ik}$. Each leaf node in the tree specifies a value to be returned by the function. 

Some functions cannot be represented concisely. For example, the majority function, which returns true if and only if more than half of the inputs are true, requires an exponentially large decision tree. Decision trees are therefore good for some kinds of functions and bad for others. There is no *one* representation that is efficient for all kinds of funtions. For example, consider the set of all boolean functions on $n$ attributes. How many different functions are in this set? This is the number of different truth tables we can write down. A truth table over $n$ attributes has $2^n$ rows, one for each combination of values of the attributes. We can consider the answer column of the table as a $2^n$-bit number that defines the function. Therefore there are $2^(2^n)$ different functions. 

Finding a minimal decision tree consistent with the training set is NP-hard. Constructing a minimal binary tree with respect to the expected number of tests required for classifying an unseen instance is NP-complete. Even finding the minimal equivalent decision tree for a given decision tree, or building the optimal decision tree from decision tables is known to be NP-hard. (pg 699)

**Inducing decision trees from examples**

An example for a decision tree consists of an $(x, y)$ pair where $\mathbf{x}$ is a vector of values for the input attributes, and $y$ is a single Boolean output value. 

It is guided by four cases:

 1. If the remaining examples are all positive (or all negative), then we are done: we can answer *Yes* or *No*. (e.g., see None and Some branches on pg 701). 
 2. If there are some positive and negative examples, then choose the best attribute to split them. (see Hungry being used to split on pg 701).
 3. If there are no examples left, it means that no example has been observed for this combination of attribute values, and we return a default value calculated from the plurality classification of all examples that we used in contructing the node's parent. (passed along in parent\_examples).
 4. If there are no attributes left, but both positive and negative examples, it means that these examples have exactly the same description, but different classifications. This can happen because there is an error or noise in the data. 

The set of examples is crucial for *constructing* the tree, but do not appear anywhere in the tree itself. 

We can **evaluate** the accuracy of a learning algorithms with a **learning curve**. We split the examples into a training set and test set. We learn a hypothesis *h* with the training set and measure its accuracy with the test set. For example, if we have 100 examples, we start with a training set of size 1 and increase one at a time up to size 99. For each size, we repeat the process of randomly splitting 20 times, and average the results of the 20 trials. 

**Choosing attribute tests**

We need a formal measure of "fairly good" and "really useless" to figure out which attribute we should base our test on. We can do this with the notion of information gain, which is defined in terms of **entropy**:

$$
H(V) = \sum\limits\_kP(v\_k)\log\_2\dfrac{1}{P(v\_k)} = -\sum\limits\_kP(v\_k)\log\_2P(v\_k)
$$


Hence for a boolean variable, it is just $B(q) = -(q\log\_2q + (1 - q)log\_2(1 - q))$. Since a boolean variable can only have two values. The probability of one value is always 1 minus probability of the other. So in the case of the decision tree, we can look at the entropy of the goal attribute on the whole set. We can simply look at one example, the positive example, since the formula accounts for the negative example as well $(1 - q)$. So we can simply calculate $H(Goal)$, which is just $B\big(\frac{p}{p + n}\big)$ (i.e., we see the percentage of how many times the positive example happens in the example set). 

Now testing on a single attribute will only give us part of the bits of $B\big(\frac{p}{p + n}\big)$. In the restaurant example, testing one attribute only gives us part of the information of 1 bit (of entropy; p = n = 6 so B = 1). We can measure exactly how much by looking at the entropy remaining *after* the attribute test. 

If you have an attribute $A$ with $d$ distinct values, it divides the training set of examples $E$ into subsets $E\_1, \dotso, E\_d$. Each subset $E\_k$ has $p\_k$ positive examples and $n\_k$ negative examples. If we go along that particular branch, we will need an additional $B\big(\frac{p\_k}{p\_k + n\_k}\big)$ bits of information. A randomly chosen example from the training set has the $k^{th}$ value for the attribute with probability $\frac{p\_k + n\_k}{p + n}$, and so the expected entropy remaining after testing attribute A is:

$$
Remainder(A) = \sum\limits\_{k = 1}^{d}\dfrac{p\_k + n\_k}{p + n}B\bigg(\dfrac{p\_k}{p\_k + n\_k}\bigg)
$$

The **information gain** from the attribute test on $A$ is the expected reduction in entropy:

$$
Gain(A) = B\bigg(\dfrac{p}{p + n}\bigg) - Remainder(A)
$$

**Generalization and overfitting**

An algorithm may accurately predict every case according to the test data, but it may not be general. This is called overfitting. For decision trees, we can use **decision tree pruning** to combat overfitting. Pruning works by eliminating nodes in the decision tree that are not clearly relevant. We start with a full tree, and then look at a test node that has only leaf nodes as descendants. If the test appears to be irrelevant (detecting only noise in the data), we eliminate the test, replacing it with a leaf node. This process is repeated, considering each test with only leaf descendants, until each one has either been pruned or accepted as is. 

How do we detect irrelevance? Suppose we are at a node consisting of $p$ positive and $n$ negative examples. If the attribute is irrelevant, we would expect that it would split the examples into subsets that each have roughly the same proprtion of positive examples as the whole set, $\frac{p}{p + n}$, and so the information gain will be close to zero. Hence information gain is a good clue to irrelevance. How large a gain should we require in order to split on a particular attribute?

To answer that we do a statistical **significance test**. Such a test begins by assuming there is no underlying pattern (the so-called **null hypothesis**). Then the actual data are analyzed to calculate to the extent to which they deviate from a perfect absence of pattern. If the degree of deviation is statistically unlikely (usually taken to mean a 5% probability or less), then that is considered to be good evidence for the presence of a significant pattern in the data. This is basically chi-squared.

So we need observed and expected values for each subset (i.e., for every $E\_k$ from $1$ to $d$). The expected values would then just be $\hat{p\_k} = p\times\big(\dfrac{p\_k + n\_k}{p + n}\big)$ (for positive) and $\hat{n\_k} = n\times\big(\dfrac{p\_k + n\_k}{p + n}\big)$ (for negative). The total deviation would be the overall sum for each $k$ of the sum of the positive and negative chi-squared values. (pg 706):

$$
\Delta = \sum\limits\_{k = 1}^{d}\dfrac{(p\_k - \hat{p\_k})^2}{\hat{p\_k}} + \dfrac{(n\_k - \hat{n\_k})^2}{\hat{n\_k}}
$$

This value delta is distributed according to chi-squared with $v - 1$ degress of freedom (where $v$ is the number of values for the attribute). 

**Minimum depth decision tree**

We want an easy, simple, elegant decision tree that minimizes misclassification errors. The problem is NP-hard (find min decision tree with min error). 

**Broadening the applicability of decision trees**

See pg 707

 - **Missing data** Given a complete decision tree, how should one classify an example that is missing one of the test attributes? Second, how should one modify the information-gain formula, when some examples have unknown values for the attribute? 
 - **Multivalued attributes** How do you split on something like *ExactTime* that has a bunch of different values that are effectively singletons?
 - **Continuous and integer-valued input attributes** Something like *Height* and *Weight* would have an infinite set of possible values. Decision trees usually try to find a **split point** like, *Weight* &gt; 160. 
 - **Continuous-valued output attributes** If we are trying to predict a numerical output value, we need a **regression tree** rather than a classification tree. A regression tree has at each leaf a linear function of some subset of numerical attributes rather than a single value. Learning algorithm must decide when to stop splitting and begin applying linear regression over the attributes.

Evaluating and Choosing the Best Hypothesis
-------------------------------------------

We want to learn a hypothesis that fits the future data best. We make the **stationary assumption**: that there is a probability distribution over examples that remains stationary over time (pg 708). Examples that satisfy these assumptions are called *independent and identically distributed* or **i.i.d.**. This is a definition for our "future data". 

What is "best fit"? First we define the **error rate** of a hypothesis as the proportion of mistakes it makes. That is the number of times $h(x) \ne y$ for an $(x, y)$ example. It does not mean, however, that because the hypothesis has a low error rate on the training set, it will generalize well. To get an accurate evaluation of a hypothesis, we need to test it on a set of examples it has not seen yet.

The simplest approach is **holdout cross-validation**. Randomly split the available data into a training set from which the learning algorithm produces $h$ and a test set on which the accuracy of $h$ is evaluated. This has the disadvantage that it fails to use all of the available data (pg 708). 

Another technique is *k*-**fold cross-validation**. Each example serves double duty - as training data and test data. First we split the data into *k* equal subsets. We then perform *k* rounds of learning; on each round, *1/k* of the data is held out as a test set and the remaining examples are used as training data. The average test set score of the *k* rounds should then be a better estimate than a single score. Popular values for *k* are 5 and 10. This takes us 5-10 times longer to run but is statistically more-accurate. The extreme is *k = n* or **leave-one-out cross-validation** or **LOOCV**.

**Peeking** causes the test results to be invalidated. Users can inadvertently **peek** at the test data. If you select the hypothesis *on the basis of its test set error rate*, you have peeked.

The best way is to lock your test set away and only use it to validate. 

**Model selection: Complexity versus goodness of fit**

(pg 709, 710)

Higher-degree polynomials can fit the training data better, but when the degree is too high, they will overfit and perform poorly on validation data. Choosing the degree of the polynomial is an instance of the problem of **model selection**. You can think of the task of finding the best hypothesis as two tasks: model selection defines the hypothesis space and then **optimization** finds the best hypothesis within that space.

This section talks about selecting models that are parameterizing by *size*.

Regression and Classification with Linear Models
================================================

Applies to the class of linear functions of continuous-valued inputs.

Loss functions
--------------

In the following, $o$ is output, $e$ is expected.

 - $L\_1$ is absolute value loss: $L\_1(e, o) = |e - o|$ 
 - $L\_2$ is squared error loss: $L\_2(e, o) = (e - o)^2$ (good for regression - this is a convex function, so no local minimae)
 - $L\_{0/1}$ is 0/1 loss: $L\_{0/1}(e, o) = 0$ if $o = e$ else $1$. (good for binary classification)

Univariate linear regression
----------------------------

pg(718, 719)

A univariate linear function with input $x$ and output $y$ has the form:

$$
y = w\_1x + w\_0
$$

Where $w\_0$ and $w\_1$ are real-valued coefficients to be learned. We use the letter $w$ because we can think of the coefficients as **weights**; the value of $y$ is changed by changing the relative weight of one term to another. We can define $\mathbf{w}$ to be the vector $[w\_0, w\_1]$ and define the hypothesis function:

$$
h\_{\mathbf{w}}(x) = w\_1x + w\_0
$$

The task of finding the $h\_{\mathbf{w}}$ that best fits the data is called **linear regression**. To fit a line to the data, all we have to do is find the values of the weights that minimize the empirical loss. We use the $L\_2$ loss function for this. Many forms of learning involve adjusting weights to minimize a loss, so it helps to think about a **weight space** - the space defined by all possible settings of the weights. For univariate regression, the weight space is defined by $w\_0$ and $w\_1$ is two-dimensional. Hence we can graph loss as a function of $w\_0$ and $w\_1$ in a three-dimensional plot. We can see that the loss function is **convex** and this is true for *every* linear-regression problem with an $L\_2$ loss function and thus implies that there are no local minima. 

Gradient descent
----------------

To go beyond linear models, we need to face the fact that equations defining minimum loss will often have no closed-form solution. Instead, we will face a general optimization search problem in a continuous weight space. We can address these by a hill-climbing algorithm that follows the **gradient** of the function to be optimizied. In this case, because we are trying to minimize the loss, we will use **gradient descent**. We choose any starting point in weight space (a point in the $(w\_0, w\_1)$ plane) and then move to a neighboring point that is downhill, repeating until we converge on the minimum possible loss.

The parameter $\alpha$ is usually called the **learning rate** when we are trying to minimze loss in a learning problem. It can be a fixed constant, or it can decay over time as the learning process proceeds. For univariate regression, the loss function is a quadratic function and so the partial derivative will be a linear function. (pg 719, 720)

Learning rule for the weights:

$$
w\_0 = w\_0 + \alpha(y - h\_{\mathbf{w}}(x))
$$
$$
w\_1 = w\_1 + \alpha(y - h\_{\mathbf{w}}(x)) \times x
$$

This covers only one training example. For N training examples, we want to minimize the sum of the individual losses for each example. The derivative of a sum is the sum of the derivatives and so:

$$
w\_0 = w\_0 + \alpha\sum\limits\_j(y\_j - h\_{\mathbf{w}}(x\_j))
$$
$$
w\_1 = w\_1 + \alpha\sum\limits\_j(y\_j - h\_{\mathbf{w}}(x\_j)) \times x\_j
$$

These updates constitute the **batch gradient descent** learning rule for univariate linear regression. **Convergence to a unique global minimum is guaranteed** (as long as we pick $\alpha$ small enough) but may be very slow. We have to cycle through all the training data for every step, and there may be many steps.

Another possibility is **stochastic gradient descent**, where we consider only a single training point at a time, taking a step after each one using the learning rule (i.e., not the $N$ training examples; the single-example one). It can be used in an online setting, where new data are coming one at a time, or offline, where we cycle through the same data as many times as is necessary, taking a step after considering each single example. It is often faster than batch gradient descent. With a fixed learning rate $\alpha$, however, it **does not guarantee convergence**; it can oscillate around the minimum without settling down. In some cases, as we see later, a schedule of decreasing learning rates (as in simulated annealing) does guarantee convergence. 

Multivariate linear regression
------------------------------

We can extend the univariate linear-regression to solve **multivariate linear-regression** problems. In the multivariate case, each example $\mathbf{x}$j is an n-element vector. Hence:

$$
h\_{sw}(\mathbf{x}\_j) = w\_0 + w\_1x\_{j, 1} + \dotso + w\_nx\_{j, n} = w\_0 + \sum\limits\_iw\_ix\_{j, i}
$$

The $w\_0$ term (intercept) stands out as different. We can fold it into this equation by inventing a dummy input-attribute, $x\_{j, 0}$ which is always $1$. Then $h$ is just the dot-product of the weights and the input vector, or the matrix product of the transpose of the weights and the input vector:

$$
h\_{sw}(\mathbf{x}\_j) = \mathbf{w}\cdot\mathbf{x}\_j = \mathbf{w}^T\mathbf{x}\_j = \sum\limits\_iw\_ix\_{j, i}
$$
The best vector of weights, $\mathbf{w}^\*$ minimizes the squared-error loss over the examples:

$$
\mathbf{w}^\* = \underset{\mathbf{w}}{\operatorname{argmin}}\sum\limits\_jL\_2(y\_j, \mathbf{w}\cdot\mathbf{x}\_j)
$$

Gradient descent will reach the (unique) minimum of the loss function. The update function for each weight is

$$
w\_i = w\_i + \alpha\sum\limits\_jx\_{j,i}(y\_j - h\_{\mathbf{w}}(\mathbf{x}\_j))
$$

It is possible to solve this analytically using linear algebra for the $\mathbf{w}$ that minimizes loss. If $\mathbf{y}$ is the output vector for training examples, and $\mathbf{X} is the **data matrix** (i.e., a matrix of inputs with one $n$-dimensional example per row). Then the solution that minimizes the mean squared error is:

$$
\mathbf{w}^\* = (\mathbf{X}^{\mathrm{T}}\mathbf{X})^{-1}\mathbf{X}^{\mathrm{T}}\mathbf{y}
$$

With univariate we don't have to worry about overfitting. But with multivariate linear regression in high-dimensional spaces, it is possible that some dimension that is actually irrelevant, appears by chance to be useful, resulting in **overfitting**. Hence, we can use **regularization** on multivariate linear functions to avoid overfitting. The process of explicitly penalizing complex hypothesis is called **regularization**. This is because it looks for a function that is more regular, or less complex. We do this via a cost function (pg 721):

$$
Cost(h) = EmpLoss(h) + \lambda Complexity(h)
$$

Hence the best hypothesis is obtained by finding the one with the minimum cost in the entire hypothesis-space.

The choice of regularization function depends on the hypothesis space. For polynomials, a good regularization function is the sum of the squares of the coefficients. Keeping the sum small keeps us away from wiggly polynomials. Another way to simplify models is to reduce the dimensions that models work with. A process of **feature selection** can be performed to discard attributes that appear to be irrelevant. chi-squared pruning is a kind of feature selection.

For linear functions, the complexity can be specified as a function of weights. We can consider a family of regularization functions:

$$
Complexity(h\_\mathbf{w}) = L\_q(\mathbf{w}) = \sum\limits\_i{\left\lvert w\_i \right\rvert}^q
$$

As with loss functions, with $q = 1$ we have $L\_1$ regularization, which minimizes the sum of the absolute values. With $q = 2$, we have $L\_2$ regularization which minimizes the sum of the squares. When one should you pick? It depends on the problem, but $L\_1$ regularization tends to produce a **sparse model**. That is, it often sets many weights to zero, effectively declaring the corresponding attributes to be irrelevant (like how DECISION-TREE-LEARNING does, but DTL does it by a different mechanism). Hypotheses that discard attributes can be easier for a human to understand, and may be less likely to overfit.

(pg 722) Explanation as to why $L\_1$ regularization leads to weights of zero, while $L\_2$ regularization does not. $L\_1$ prioritizes axes, where values of the weights can be zero. We are looking for an intersection between the $L\_q$ regularization space and the contours of the minimal achievable loss. At the intersection is where we have a hypothesis that minimizes cost (i.e., is not very complex and minimizes loss). $L\_2$ treats dimensional axes as arbitrary. $L\_2$ is spherical, which makes it rotationally invariant. $L\_1$ is appropriate when the axes are not interchangeable. 

Linear classifiers with a hard threshold
----------------------------------------

Linear functions can be used to do classification as well as regression. (pg 723)

A **decision boundary** is a line (or a surface, in higher dimensions) that separates the two classes. A linear decision boundary is called a **linear separator** and data that admit such a separator are called **linearly separable**.

For the earthquake vs. nuke classification problem, we have the linear classifier $-4.9 + 1.7x\_1 - x\_2$. Where when it is greater than 0, we have nuclear explosions and otherwise we have earthquakes. If we use the convention of a dummy input such that $x\_0 = 1$, we can write the classification hypothesis as follows:

$$
h\_{\mathbf{w}}(x) = 1\text{ if }\mathbf{w}\cdot\mathbf{x} \ge 0\text{ and }0\text{ otherwise}
$$

Alternatively, we can think of $h$ as the result of passing the linear function $\mathbf{w}\cdot\mathbf{x}$ through a **threshold function**:

$$
h\_\mathbf{w}(x) = Threshold(\mathbf{w}\cdot\mathbf{x})\text{ where }Threshold(z) = 1\text{ if }z \ge 0\text{ and }0\text{ otherwise}
$$

We don't have a closed-form solution that we can solve, and we cannot use gradient-descent either since the slope is 0 everywhere except at the point of discontinuity, where it does not exist at all (not differentiable). However, we can use a simple weight-update rule that converges to a solution - that is, a linear separator that classifies the data perfectly - **provided the data are linearly separable**. For a single example $(x, y)$ we have:

$$
w\_i = w\_i + \alpha(y - h\_\mathbf{w}(x))x\_i
$$

This is basically the update rule for linear regression, and is also called the **perceptron learning rule**. Since we are considering a 0/1 classification problem, the behavior is somewhat different. Both the true value (i.e., expected actual value) $y$ and the hypothesis output $h\_\mathbf{w}(x)$ can be $0$ or $1$. So there are three cases:

 - If the output is correct, i.e., $y = h\_\mathbf{w}(x)$, then the weights are not changed.
 - If $y$ is 1 but $h\_\mathbf{w}(x)$ is 0, then $w\_i$ is **increased** since we want to make $\mathbf{w}\cdot\mathbf{x}$ bigger so that $h\_\mathbf{w}(x)$ outputs 1.
 - If $y$ is 0 but $h\_\mathbf{w}(x)$ is 1, then $w\_i$ is **decreased** when the corresponding input $x\_i$ is **positive** and **increased** when $x\_i$ is **negative**. This makes sense because we want to make $\mathbf{w}\cdot\mathbf{x}$ smaller so that $h\_\mathbf{w}(x)$ outputs 0.

learning curve etc: (pg 724)

What if the data points are not linearly separable? This happens commonly in the real world. In general **the perceptron rule may not converge** to a stable solution for a **fixed learning rate**. But if the learning rate $\alpha$ decays as $O\left(\frac{1}{t}\right)$ where $t$ is the iteration number, then the rule can be shown to converge to a minimum-error solution when examples are presented in a random sequence. It can also be shown that finding the minimum-error solution is NP-hard, so one expects that many presentations of the examples will be requred for convergence to be achieved. 

Linear classification with logistic regression
----------------------------------------------

In linear classifiers, we saw that passing the output of the linear function through the threshold function creates a linear classifier. Unfortunately the hard nature of the threshold causes some problems. Mainly $hw(x)$ is no-longer differentiable since it is discontinuous. Therefore, learning the perceptron rule is very unpredictable. Another problem is that the linear classifier outputs a completely-confident prediction of $1$ or $0$, even for examples that are quite close to the boundary. In many situations, we need more gradated predictions.

We can solve this by softening the threshold function, i.e., approximating the hard threshold with a continuous, differentiable function. The function we will use is called the logistic function:

$$
Logistic(z) = \dfrac{1}{1 + \mathrm{e}^{-z}}
$$

Therefore, we now have:

$$
h\_\mathbf{w}(x) = Logistic(\mathbf{w}\cdot\mathbf{x}) = \dfrac{1}{1 + \mathrm{e}^{-\mathbf{w}\cdot\mathbf{x}}}
$$

This means the output is a number between 0 and 1 and we can interpret it as a *probability* of belonging to the class labeled 1. The hypothesis forms a soft boundary in the input space and gives a probability of 0.5 for any input at the center of the region, and approaches 0 or 1 as we move away from the boundary.

The process of fitting the weights of this model to minimize loss on a data set is called **logistic regression**. There is no easy closed-form solution to find the optimal value of $\mathbf{w}$ with this model, but we can still use gradient descent. Our hypothesis no-longer outputs just 0 or 1, so we can use the $L\_2$ loss function. (pg 726 for full derivation)

This eventually gives us the weight update rule for minimizing the loss as:

$$
w\_i = w\_i + \alpha(y - h\_\mathbf{w}(x)) \times h\_\mathbf{w}(x)(1 - h\_\mathbf{w}(x)) \times x\_i
$$

In the linearly-separable case, logistic regression is slower to convert, but is also more predictable. When data are noisy and nonseparable, logistic regression converges far more quickly and reliably.

Nonparametric models
====================

Linear regression and neural networks use the training data to estimate a **fixed** set of parameters $\mathbf{w}$, which defines our hypothesis $hw(x)$. At that point we can throw away the training data, because they are all summarized by $\mathbf{w}$. A learning model that summarizes data with a set of parameters of fixed size (independent of the number of training examples, i.e., does not depend on size of knowledge base) is called a **parametric model**.  (pg 737).

It doesn't matter how much data you throw at a parameteric model; it won't change its mind about how many parameters it needs. When data sets are small, it makes sense to have a strong restriction on the allowable hypotheses, to avoid overfitting. But when there are thousands or millions or billions of examples to learn from, it seems like a better idea to let the data speak for themselves rather than forcing them to speak through a tiny vector of parameters. If the data say that the correct answer is a very wiggly function, we shouldn't restrict ourselfs to linear or slightly wiggly functions.

A **nonparameteric model** is one that cannot be characterized by a bounded set of parameters. For example, suppose that each hypothesis we generate simply retains within itself all of the training examples and uses all of them to predict the next example. Such a hypothesis family would be nonparametric because the effective number of parameters is unbounded - it grows with the number of examples. This approach is called **instance-based learning** or **memory-based learning**. The simplest instance-based learning method is **table lookup**. That is, when asked for $h(x)$, see if $x$ is in the table; if it is,return the corresponding $y$. The problem is that it does not generalize well. If $x$ is not in the table, it can only return some default value of $y$.

Support Vector Machines
=======================

The **support vector machine** or SVM framework is currently the most-popular approach for "off-the-shelf" supervised learning: if you don't have any specialized prior-knowledge about a domain, then the SVM is an excellent method to try first. SVMs have three properties that make them attractive:

 1. SVMs construct a **maximum margin separator** - a decision boundary with the largest-possible distance to example points. This helps them generalize well.

 2. SVMs create a linear separating hyperplane, but they have the ability to embed the data into a higher-dimensional space, using the so-called **kernel trick**. Often, data that are not linearly separable in the original input space are easily separable in the higher-dimensional space. The high-dimensional linear separator is actually nonlinear in the original space. This means the hypothesis space is greatly expanded over methods that use strictly-linear representations.

 3. SVMs are a nonparametric method - they retain training examples and potentially need to store them all. On the other hand, in practice they often end up retaining **only a small fraction** of the number of examples - sometimes as few as a small constant times the number of dimensions. Thus SVMs combine the advantages of nonparametric and parametric models: they have the flexibility to represent complex functions, but they are **resistant to overfitting**. 

SVMs are successful because of one key insight and one neat trick. Assume a binary classification problem with three candidate decision boundaries, each a linear seprator. Each of them is consistent with all the examples, so from the point of view of 0/1 loss, each would be good (pg 745 has examples in figure 18.30). Logistic regression would find some separating line, and its location depends on *all* the example points. **The key insight** of SVMs is that that examples are *more important* than others, and that paying attention to them can lead to better generalization. 

If you consider a separating line which comes close to some number of examples, it certainly minimizes loss and classifies all the examples correctly. But it should make us nervous because so many examples are close to the line; it is entirely possible that other examples will turn out to fall on the other side. SVMs address this issue: instead of minimizing expected *empirical loss* on the training data, SVMs attempt to minimize expected *generalization* loss. We don't know where the as-yet-unseen points may fall, but under the *probabilistic assumption* that they are drawn from the same distribution as the previously-seen examples, there are some arguments from computational learning theory suggesting that we minimize generalization loss by choosing the separator that is farthest away from the examples we have seen so far. We call this separator, the **maximum margin separator**. The **margin** is the area bounded by lines. The distance between these lines is twice the distance from the separator to the nearest example point. 

Traditionally, SVMs use the convention that class labels are +1 and -1, instead of the +1 and 0 we have seen before. Also, where we put the intercept into the weight vector $\mathbf{w}$ (and a corresponding dummy 1 value into $x\_{j, 0}$), SVMs do not do that; they keep the intercept as a separate parameter, $b$. Hence the separator is defined as the set of points:

$$
\\{ \mathbf{x}\text{ }:\text{ }\mathbf{w}\cdot\mathbf{x} + b = 0 \\}
$$

It is possible to search the space of $\mathbf{w}$ and $b$ with gradient descent to find the parameters that maximize the margin while correctly classifying all the examples. However, it turns out there is another approach to solving this problem. The alternate representation is called the dual representation, where the optimal solution is found by solving:

$$
\underset{\mathbf{\alpha}}{\operatorname{argmax}}\sum\limits\_j\alpha\_j - \dfrac{1}{2}\sum\limits\_{j, k}\alpha\_j\alpha\_ky\_jy\_k(\mathbf{x}\_j\cdot\mathbf{x}\_k)
$$

Subject to the constraints $\alpha\_j \ge = 0$ and $\sum\nolimits\_j\alpha\_jy\_j = 0$. This is a **quadratic programming** optimization problem that can be solved by many good software packages.

Once we have found the vector $\mathbf{\alpha}$, we can get back to $\mathbf{w}$ with the equation $\mathbf{w} = \sum\nolimits\_j\alpha\_j\mathbf{x}\_j$. Or we can stay in the dual representation.

The dual representation has some important properties:

 - The expression is convex; it has a single global maximum that can be found efficiently.
 - The data enter the expression only in the form of dot products of pairs of points. This is true of the equation for the separator itself; once the optimal $\alpha\_j$ have been calculated, it is:
$$
   h(\mathbf{x}) = \operatorname{sign}\left(\sum\limits\_j\alpha\_jy\_j(\mathbf{x}\cdot\mathbf{x}\_j) - b\right)
$$
 - The weights $\alpha\_j$ associated with each data point are *zero* except for the **suport vectors** - the points closest to the separator. They are called support vectors because they "hold up" the separating plane. Because there are usually many fewer support vectors than examples, SVMs gain some of the advantages of parametric models. 

What if the examples are *not* linearly separable (pg 746, 747)? We can re-express the data - i.e., we map each input-vector $\mathbf{x}$ to a new vector of feature values, $F(\mathbf{x})$. An example:

$$
f\_1 = {x\_1}^2
$$
$$
f\_2 = {x\_2}^2
$$
$$
f\_3 = \sqrt{2}x\_1x\_2
$$

It turns out now that the data in the new, three-dimensional space defined by the three features is *linearly separable* by a plane. If the data are mapped into a space of sufficiently high dimension, then they will almost always be linearly separable. For example, four dimensions suffice for linearly separating a circle anywhere in the plane, and five dimensions suffice to linearly separate any ellipse. In general (with some special cases excepted), if we have $N$ data points, then they will always be separable in spaces of $N - 1$ dimensions or more.

Now we would not expect to find a linear separator in the input space $\mathbf{x}$, but we can find linear separators in the high-dimensional feature space F($\mathbf{x}$) simply by replacing $\mathbf{x}\_j\cdot\mathbf{x}\_k$ by $F(\mathbf{x}\_j)\cdot F(\mathbf{x}\_k)$. This is very straightforward since we can replace $\mathbf{x}$ by $F(\mathbf{x})$ in any learning algorithm. But the dot product has special properties. We can often compute $F(\mathbf{x}\_j)\cdot F(\mathbf{x}\_k)$ without first computing $F$ for each point. That is through some simple algebra we can show:

$$
F(\mathbf{x}\_j)\cdot F(\mathbf{x}\_k) = {(\mathbf{x}\_j\cdot \mathbf{x}\_k)}^2
$$

That's why the $\sqrt{2}$ is in $f\_3$. The expression ${(\mathbf{x}\_j\cdot\mathbf{x}\_k)}^2$ is called a **kernel function** and is usually written as $K(\mathbf{x}\_j, \mathbf{x}\_k)$. Hence, we can learn in the higher dimensional space, but we compute only kernel functions rather than the full list of features for each data point. (pg 747 for additional stuff on kernel functions).

This is the clever **kernel trick**. Plugging these kernels into the equation (the giant equation for SVM), *optimal linear separators can be found efficiently in feature spaces with billions of (or, in some cases, infinitely many) dimensions.* The resulting linear separators, when mapped back to the original input space, can correspond to arbitrarily wiggly, nonlinear decision boundaries between the positive and negative examples. A general kernel function is the **polynomial kernel** $K(\mathbf{x}\_j, \mathbf{x}\_k) = {(1 + \mathbf{x}\_j\cdot\mathbf{x}\_k)}^d$ and corresponds to a feature space whose dimension is exponential in $d$.

What to do with noisy data? If the data are inherently noisy, we may not want a linear separator in some high-dimensional space. Rather, we'd like a decision surface in lower-dimensional space that does not cleanly separate the classes, but reflects the reality of the noisy data. That is possible with the **soft margin** classifier, which allows examples to fall on the wrong side of the decision boundary, but assigns them a penalty proportional to the distance required to move them back on the correct side. (pg 748 last para of SVM for other places where kernel function can be applied)

Ensemble Learning
=================

Previous learning methods employ a single hypothesis, chosen from a hypothesis space, to make predictions. The idea of **ensemble learning** methods is to select a collection, or **ensemble** of hypotheses from the hypothesis space and combine their predictions. As an example, during cross-validation we might generate twenty different decision trees, and have them vote on the best classification for a new example.

The motivation for ensemble learning is simple. Consider an ensemble of $K = 5$ hypotheses and suppose that we combine their predictions using simple majority voting. For the ensemble to misclassify a new example, *at least three of the five hypotheses have to misclassify it*. The hope is that this is much less likely than a misclassification by a single hypothesis. The assumption here is that the errors are independent. 

Another way to view the ensemble idea is to think of it as a generic way of enlarging the hypothesis space. Think of the ensemble itself as a hypothesis, and the new hypothesis space as the set of all possible ensembles constructable from hypotheses in the original space.

Boosting
--------

The most widely used ensemble method is called **boosting**. First we need to understand what a **weighted training set** is. In such a training set, each example has an associated weight $w\_j \ge 0$. The higher the weight of an example, the higher is the importance attached to it during the learning of a hypothesis. We can easily modify existing algorithms to take the weight into account (where this is not possible, you can create a **replicated training set** where the $j^{th}$ example appears $w\_j$ times, using randomization to handle fractional weights).

 1. Boosting starts with $w\_j = 1$ for all the examples (normal training set). 
 2. From this, it generates the first hypothesis $h\_1$. This hypothesis will classify some of the training examples correctly and some incorrectly. 
 3. We want the next hypothesis to do better on the misclassified examples, so we **increase** their weights while **decreasing** the weights of the correctly classified examples. 
 4. From this weighted training-set, we generate the hypothesis $h\_2$. 
 5. The process continues until we have generated $K$ hypotheses. The final ensemble hypothesis is a weighted-majority combination of all $K$ hypothesis, each weighted according to how well it performed on the training set (pg 750 for a visual example). 

A specific algorithm called ADABOOST has a very important property: if the input learning algorithm $L$ is a **weak learning** algorithm - which means that $L$ always returns a hypothesis with accuracy on the training set that is slightly better than random guessing (i.e., $.5 + \epsilon$ for Boolean classification) - then ADABOOST will return a hypothesis that *classifies the training data perfectly* for **large enough** $K$. Thus, the algorithm *boosts* the accuracy of the original learning algorithm on the training data. This result holds no matter how inexpressive the original hypothesis space and no matter how complex the function being learned. For decision trees, we can employ boosting on the **decision stumps**, which are decision trees with just one test, at the root. (pg 750 for details).

The finding of the performance of boosting is a surprise since Ockham's razor tells us not to make hypotheses any more complex than necessary. But here we can see that the prediction improves as the ensemble hypothesis gets more complex. There are many explanations. One view is that boosting approximates **Bayesian learning**, which can be shown to be an optimal learning algorithm, and the approximation improves as more hypotheses are added. Another possible explanation is that the addition of further hypotheses enables the ensemble to be *more definite* in its disctinction between positive and negative examples, which helps it when it comes to classifying new examples.

Bagging
-------

**Bagging** is the first effective method with ensemble learning for improving the performance of learning algorithms. It combines hypotheses learned from multiple **bootstrap** data sets, each generated by subsampling (random) the original data set. 

Here we are given a training set of $N$ examples, and a class of learning models (e.g. decision trees, NN, etc.). We train multiple ($K$) models on different samples (data splits) and average their predictions. Prediction is done by averaging the results of $K$ models. The goal is to improve the accuracy of one model by using its multiple copies. The average of misclassification errors on different data splits gives a better estimate of the predictive ability of a learning method. For regression we average, for classification we use a majority vote. (see [here](http://people.cs.pitt.edu/~milos/courses/cs2750-Spring04/lectures/class23.pdf) for more information)

Bagging vs. Boosting
--------------------

Bagging has the advantage of being parallelizable. We can train multiple models at the same time on different samples, since no model depends on the result of the other. In contrast, Boosting and only be done sequentially, because each new hypothesis is generated by training on a data-set, whose weighted data-points have their weights set based on the performance of the *previous* hypothesis.

Precision and Recall
--------------------

 - Precision is the number of correctly classified instances for class X divided by the total number of instances the algorithm classified for class X.
 - Recall is the number of correctly classified instances for class X divided by the total number of instances of class X in the *test data*. 

Another way: recall is a measure of how many of the relevant documents were retrieved, while precision is a measure of how many of the retrieved documents were relevant.

Given,

 - D = number of documents retrieved.
 - R = number of relevant documents retrieved
 - N = number of relevant documents in the collection.

 - Recall = R / N
 - Precision = R / D

In other words: 

 - If I have high **recall**, it means out of all the documents *that are actually relevant*, a high percentage exists in my result set. However, the result set may also contain a large number of irrelevant documents. The focus here is to return *as many* of the relevant things as necessary, at the cost of also including irrelevant/misclassified things. In other words, the idea here is to *minimize false negatives* at the cost of *maximizing false positives*. 
 - If I have high **precision**, it means out of all the documents *retrieved by my query*, a high percentage are actually relevant to my query. However, it does not necessarily mean that **all** relevant documents were retrieved. The focus here is to have *as many* of the relevant things as necessary *within the search result*, at the cost of missing out on actually-relevant things (i.e., we may not return all the relevant things). In other words, the idea here is to *minimize false positives* at the cost of *maximizing false negatives* (i.e., the things that are left out). 

Let's have an example where we have 100 documents, 30 of which are relevant to our query. 

 - Algorithm #1 retrieves all 100. So Recall = 100%, and Precision = 30%. So it did retrieve all the documents, but also a bunch of irrelevant ones. A person has to manually inspect these documents.
 - Algorithm #2 retrieves 70 documents, including all 30 revelant documents. Recall = 100%, Precision = 70%. This is a little better. 
 - Algorithm #3 retrieves 60 documents, including 20 relevant. Recall = 20 / 30 = 67%, Precision = 20 / 50 = 40%. 

Another one:

 - P = N(relevant items retrieved) / N(total retrieved) = P(relevant | retrieved)
 - R = N(relevant items retrieved) / N(total relevant) = P(retrieved | relevant)

Which one is important depends on the circumstances. A typical web surfer looking for documents would like every result on the first page to be relevant (high precision) but not have the interest in knowing, let alone looking at every document that is relevant.

In contrast, various professional searchers (paralegals, intelligence analysts) are concerned with trying to get as high recall as possible, and will tolerate fairly low precision results in order to get it. 

 - You can always get a recall of 1 (but very low precision) by retrieving all documents for all queries. **Recall** is a **non-decreasing function** of the **number of documents retrieved**.
 - On the other hand, **precision** usually **decreases** as the **number of documents retrieved** is **increased**.

Learning: Terms and Statements
------------------------------

**Decision Trees**

 - **Pruning** is the process of removing irrelevant attribute tests based on their statistical significance (use chi-squared). (pg 706)
 - **Pruning** helps combat **overfitting** because the decision-tree algorithms seizes on any pattern it sees. Overfitting is more likely as the hypothesis space and number of input attributes grows.

**Linear regression**

 - **Univariate linear regression** has a single variable $x$ and has two weights $w\_0$ and $w\_1$. The function has the form $y = w\_1x + w\_0$. There is no risk of **overfitting**.
 - Finding the weights that make the hypothesis function best fit the data is called **linear regression**.
 - We can graph the $L\_2$ loss function with respect to the weights and this gives us a **convex** loss function, which means we can use **gradient descent**.
 - **Gradient descent** is a hill-climbing algorithm that follows the **gradient** of the function to be optimized to a downhill point where we minimize the loss.
 - **Learning rate** is the parameter $\alpha$ in gradient descent. It is also known as **step size**.
 - **Batch gradient descent** is when we update the weights after cycling through all the training data for every step. It can be pretty slow, but is guaranteed to converge.
 - **Stochastic gradient descent** updates the weight after considering a single training point. We can use this in an online setting. It is faster than the batch version. With a fixed-learning rate however, it *does not guarantee convergence* and can oscillate about the minimum. We will need a decreasing learning rate or simulated annealing.
 - **Multivariate linear regression** has multiple variables, where we simply represent them as a vector $\mathbf{x}\_j$. 
 - Multivariate linear regression has a risk of **overfitting**.
 - To combat **overfitting**, we use **regularization** which is a cost based on empirical loss and the complexity of the hypothesis. 
 - $L\_1$ regularization tends to produce a **spare model** where many weights are set to zero (corresponding weights are irrelevant). This is because the intersection between the minimum achievable loss space (contours) and the boundaries defined by set of weights that have complexity lesser than some $c$ intersects on the axes because the boundaries form a diamond shape.
 - $L\_2$ does not have the same result since the boundaries of the weight space (with cost less than $c$) is spherical or a circle and so the intersection is along the circumference.

**Linear classification**

 - Lets you create a linear classifier.
 - Output of linear function is passed through a hard threshold that outputs either 0 or 1.a
 - A **decision boundary** is a line or surface that separates the two classes.
 - A linear decision boundary is called a **linear separator**.
 - Data that admit a linear separator are **linearly separable**.
 - The weight-update rule for a linear classifier with a hard threshold pretty much gives us the **perceptron learning rule** which is essentially identical to the update rule for linear regression.
 - For linearly-separable data points, the perceptron learning rule *will converge* to a perfect linear separator.
 - In general, the perceptron rule *may not converge* to a stable solution for a **fixed** learning rate $\alpha$. But if we have a decaying learning rate as $O(\frac{1}{t})$ then it will converge to a minimum-error solution when examples are presented in random sequence. Finding a minimum-error solution is also NP-hard.

**Logistic Regression**

 - Lets you create a linear classifier.
 - Output of a linear function can be passed through a threshold function to create a classifier. We use a smooth threshold function here.
 - The process of fitting weights of a model to minimize loss on a data set is called **logistic regression**. 
 - We use the logistic function to have a smooth and continuous threshold so that it is differentiable.
 - The output also expresses a "belief" in the probability of the classification since the value is between 0 and 1.
 - With linearly-separable data, logistic regression is *slower* to converge but behaves more *predictably*.
 - It converges far more quickly and reliably with **noisy** and **nonseparable** data.

**Non-parametric vs Parametric**

 - **Parametric model**: A learning model that summarizes data with a set of parameters of fixed-size (independent of the number of training examples) is called a **parametric model**.
 - **Non-parametric model**: A learning model that cannot be characterized by a bounded set of parameters; it retains information about the training examples and uses *all* of them to predict the next example. An example is SVM, which retains this through the support vectors.


**SVM**

 - **Soft margin classifier**: Allows examples to fall on the wrong side, but assigns them a penalty proportional to the distance required to move them to the correct side.
 - **Kernel trick**: Plugging kernels into the optimal-solution equation for SVM (it is a quadratic programming problem), lets us efficiently find optimal linear-separators in feature spaces with billions of (or, in some cases, infinitely many) dimensions. In a lower dimensional space, these separators can appear arbitrarily wiggly. So you simply replace the dot product by this kernel function, which is applied to pairs of input data to evaluate dot products in some corresponding feature space. Using this we can find linear separates in the higher-dimensional feature space $F(\mathbf{x})$.
 - **Maximum margin separator**: A decision boundary with the largest possible distance to example points. This helps them generalize well.
 - **What a kernel does**: It lets us know how we can weight each example. A kernel function looks like a bump. For example, a parabola with intercepts on the x axis at -5 and 5, centered at 0. The weight is the highest at the center and 0 at -5 and 5. A kernal function should be symmetric around 0 and have a maximum at 0. The area must be bounded as we go to positive or negative infinity. Research suggests the shape doesn't matter much, but the width does matter. If the width is too wide, we will get underfitting and if they are too narrow we'll get overfitting.
 - **Support vectors**: The points closest to the decision boundary that "hold up" the separating hyperplane. The weights here are zero for every other data point except for the support vectors. Hence SVM retain some of the advantages of parametric models since they don't retain *all* training examples (just the ones that matter most).

Probability Theory
==================

Agents need to handle **uncertainty**, whether due to partial observability, nondeterminism, or a combination of the two. We can use probability theory for this.

The set of all possible worlds is the **sample space**. $\Omega$ is used to refer to the sample space, and $\omega$ refers to the elements of the space.

A fully specified **probability model** associates a numerical probability $P(\omega)$ with each possible world. The total probability of the set of possible worlds is 1:

$$
0 \le P(\omega) \le 1 \forall\ \omega\text{ and }\sum\limits\_{\omega \in \Omega}\omega = 1
$$

Probabilistic assertions and queries are not usually about particular possible worlds, but sets of them. For example, we might be interested in the cases where two dice add up to 11. In probability theory, these sets are called **events**. In AI, the sets are always described by **propositions** in a formal language. The probability associated with a proposition is defined to be the sum of pobabilities of the worlds in which it holds:

$$
\text{For any proposition }\Phi\text{, }P(\Phi) = \sum\limits\_{\omega \in \Phi}P(\omega)
$$

Probabilities such as $P(Total = 11)$ and $P(doubles)$ are called **unconditional** or **prior probabilities**. In other cases we are interested in the **conditional** or **posterior** probability. For example $P(doubles\ |\ Die\_1 = 5)$. Conditional probabilities are defined in terms of unconditional probabilities as follows:

**Definition of Conditional Probability**:
$$
P(a\ |\ b) = \dfrac{P(a \land b)}{P(b)}
$$

**Conditional Probability in the form of the Product Rule**
$$
P(a \land b) = P(a\ |\ b)P(b)
$$

Variables in probability theory are called **random variables** and their names begin with an uppercase letter. Every random variable has a **domain** - the set of possible values it can take on. For example, a Boolean random variable can take the values $\\{true, false\\}$. The proposition that doubles are rolled can be written as $Doubles = true$.

By convention, propositions of the form $A = true$ are simply abbreviated as $a$ whereas $A = false$ is abbreviated as $\lnot a$. 

Variables can have infinite domains as well. For any variable with an ordered domain, inequalities are allowed as well such as $NumberOfAtomsInUniverse \le 10^{70}$.

Sometimes we will want to talk about the probabilities of all the possible values of a random variable. We could write:

 - $P(Weather = sunny) = 0.6$
 - $P(Weather = rain) = 0.1$
 - $P(Weather = cloudy) = 0.29$
 - $P(Weather = snow) = 0.01$

But as an abbreviation, we can have:

$$
\mathbf{P}(Weather) = \langle 0.6, 0.1, 0.29, 0.01 \rangle
$$

The bold $\mathbf{P}$ indicates that the result is a vector of numbers, where we assume a pre-defined ordering on the domain of $Weather$. We say that the $\mathbf{P}$ statement defines a **probability distribution** for the random variable $Weather$. The $\mathbf{P}$ notation is also used for conditional distributions: $\mathbf{P}(X\ |\ Y)$ gives the values of $P(X = x\_i\ |\ Y = y\_i)$ for each possible $i, j$ pair.

For continuous variables, it is not possible to write out the entire distribution as a vector, because there are infinitely many values. Instead, we can define the probability that a random variable takes on some value $x$ as a parameterizing function of $x$. For example:

$$
P(NoonTemp = x) = Uniform\_{[18C, 26C]}(x)
$$

expresses the belief that the temperature at noon is distributed uniformly between 18 and 26. We call this function a **probability density function**.

Saying that the probability density is uniform from 18C to 26C means that there is a 100% chance that the temperature will fall somewhere in that 8C-wide region and a 50% chance that it will fall in any 4C-wide region, and so on. (pg 487)

In addition to distributions on single variables, we need notation for distributions on multiple cariables. Commas are used for this. For example, $\mathbf{P}(Weather, Cavity)$ denotes the probabilities of all combinations of the values of $Weather$ and $Cavity$ (essentially the cross product of both sets). This is called the **joint probability distribution**. We can mix variables with and without values; $\mathbf{P}(sunny, Cavity)$ would be a two-element vector giving the probabilities of a sunny day with a cavity and a sunny day with no cavity. The $\mathbf{P}$ notation makes certain expressions much more concise that they otherwise might be. For example, the product rules for all possible values of $Weather$ and $Cavity$ can be written as a single equation:

$$
\mathbf{P}(Weather, Cavity) = \mathbf{P}(Weather\ |\ Cavity)\mathbf{P}(Cavity)
$$

This is much more concise than writing 4 x 2 = 8 equations. 

As a degenerate case, $\mathbf{P}(sunny, cavity)$ has no variables and thus is a one-element vector that is the probability of a sunny day with a cavity, which could also be written as $P(sunny, cavity)$ or $P(sunny \land cavity)$. We will sometimes use $\mathbf{P}$ notation to derive results about invidividual $P$ values, and when we say $\mathbf{P}(sunny) = 0.6$, it is really an abbreviation for "$\mathbf{P}(sunny)$ is the one-element vector $\langle 0.6\rangle$, which means that $P(sunny) = 0.6$."

A probability model is completely determined by the joint distribution for all of the random variables - this is the full joint probability distribution. For example, if the random variables are $Cavity$, $Toothache$, and $Weather$, then the full joint distribution is given by $\mathbf{P}(Cavity, Toothache, Weather)$. This would be a table with 2 x 2 x 4 = 16 entries because we look at every combination (again, just the cross product of the three sets).

Probability axioms and their reasonableness
-------------------------------------------

**Inclusion-exclusion principle**
$$
P(a \lor b) = P(a) + P(b) - P(a \land b)
$$

Inference Using Full Joint Distributions
----------------------------------------

The method of **probabilistic inference** is the computation of posterior probabilities for query propositions given observed evidence. 

A particularly common task is to extract the distribution over some subset of variables or a single variable. For example, adding the entries (pg 492 fig 13.3) in the first row gives the unconditional or **marginal probability** of $cavity$:

$$
P(cavity) = 0.108 + 0.012 + 0.072 + 0.008 = 0.2
$$

This process is called **marginalization** or **summing out**, because we sum up the probabilities for each possible value of the other variables, thereby taking them out of the equation. We can write the following general marginalization rule for any sets of variables $\mathbf{Y}$ and $\mathbf{Z}$:

$$
\mathbf{P}(\mathbf{Y}) = \sum\limits\_{\mathbf{z} \in \mathbf{Z}}\mathbf{P}(\mathbf{Y}, \mathbf{z})
$$

In our example, we basically did:

$$
\mathbf{P}(Cavity) = \sum\limits\_{\mathbf{z} \in \\{Catch, Toothache\\}}\mathbf{P}(Cavity, \mathbf{z})
$$

A variant of this rule involves conditional probabilities instead of joint probabilities, using the product rule:

$$
\mathbf{P}(Y) = \sum\limits\_{\mathbf{z} \in \mathbf{Z}}\mathbf{P}(\mathbf{Y}\ |\ \mathbf{z})P(\mathbf{z})
$$

This rule is called **conditioning**. Marginalization and conditioning turn out to be useful rules for all kinds of derivations involving probability expressions.

Conditional probabilities can be found by first using the equation:

$$
P(a\ |\ b) = \dfrac{P(a \land b)}{P(b)}
$$

For $\mathbf{P}(Cavity\ |\ toothache)$, we basically have $P(cavity\ |\ toothache) + P(\lnot cavity\ |\ toothache)$. Both will have as denominator $P(toothache)$, which remains constant no matter what value of $Cavity$ we calculate. This means it ends up being a **normalization** constant for the distribution $\mathbf{P}(Cavity\ |\ toothache)$, ensuring that it adds up to 1. We will use $\alpha$ to denote such constants. Hence we can write:

$$
\mathbf{P}(Cavity\ |\ toothache) = \alpha\mathbf{P}(Cavity, toothache) = \alpha[\mathbf{P}(Cavity, toothache, catch) + \mathbf{P}(Cavity, toothache, \lnot catch)]
$$

We end up with:

$$
\alpha[\langle P(cavity, toothache, catch), P(\lnot cavity, toothache, catch)\rangle + \langle P(cavity, toothache, \lnot catch), P(\lnot cavity, toothache, \lnot catch)\rangle]
$$
$$ 
= \alpha[\langle 0.108, 0.016\rangle + \langle 0.012, 0.064\rangle] = \alpha\langle 0.12, 0.08 \rangle = \langle 0.6, 0.6 \rangle
$$

Here, even if we don't know $P(toothache)$, we can forget about $\frac{1}{P(toothache)}$ and just add up the values getting $0.12$ and $0.08$. The relative proprtions are right and so we can nromalize by dividing each of them by 0.12 + 0.08, which gives us the correct probabilities.

We can now extract a general inference procedure. If we have a query with a single variable $X$ (like $Cavity$), let $\mathbf{E}$ be the list of evidence variables (just $Toothache$ in the example), and let $\mathbf{e}$ be the observed values for them, and let $\mathbf{Y}$ be the remaining unobserved variables (just $Catch$ in the example). If the query is $\mathbf{P}(X\ |\ \mathbf{e})$, we can evaluate it as:

$$
\mathbf{P}(X\ |\ \mathbf{e}) = \alpha\mathbf{P}(X, \mathbf{e}) = \alpha\sum\limits\_{y}\mathbf{P}(X, \mathbf{e}, \mathbf{y})
$$

where the summation is over all possible $\mathbf{y}$s (i.e., all possible combinations of values of the unobserved variables $\mathbf{Y}$). Notice that $X$, $\mathbf{E}$, and $\mathbf{Y}$ constitute the complete set of variables for the domain and so $\mathbf{P}(X, \mathbf{e}, \mathbf{y})$ is simply a subset of probabilities from the full joint distribution.

Given a full joint distribution, we can answer probabilistic queries with this equation but it does not scale well. Consider a domain with $n$ boolean variables. It needs an input table of size $O(2^n)$ and takes $O(2^n)$ time to process the table. 

Independence
------------

What if we add another variable, $Weather$, to the toothache table? The full joint distribution is now $\mathbf{P}(Toothache, Catch, Cavity, Weather)$. What relationship can we infer? For example, how are $P(toothache, catch, cavity, cloudy)$ and $P(toothache, catch, cavity)$ related? Using the product rule:

$$
P(toothache, catch, cavity, cloudy) = P(cloudy\ |\ toothache, catch, cavity)P(toothache, catch, cavity)
$$

The weather has no bearing on the dental variables, so we can say:

$$
P(cloudy\ |\ toothache, catch, cavity) = P(cloudy)
$$

From this, we can deduce:

$$
P(toothache, catch, cavity, cloudy) = P(cloudy)P(toothache, catch, cavity)
$$

A similar equation exists for every entry in $\mathbf{P}(Toothache, Catch, Cavity, Weather)$ and therefore we can write the general equation:

$$
\mathbf{P}(Toothache, Catch, Cavity, Weather) = \mathbf{P}(Toothache, Catch, Cavity)\mathbf{P}(Weather)
$$

This property is called **independence** (also **marginal independence** and **absolute independence**). In particular, the weather is independent of one's dental problems. Independence between propositions $a$ and $b$ can be written as:

$$
P(a\ |\ b) = P(a) \\text{ or } P(b\ |\ a) = P(b) \\text{ or } P(a \land b) = P(a)P(b)
$$

Independence between variables $X$ and $Y$ can be written as follows:

$$
\mathbf{P}(X\ |\ Y) = \mathbf{P}(X)\text{ or }\mathbf{P}(Y\ |\ X) = \mathbf{P}(Y)\text{ or }\mathbf{P}(X, Y) = \mathbf{P}(X)\mathbf{P}(Y)
$$

Bayes' Rule and its Use
=======================

Bayes rule is:

$$
P(b\ |\ a) = \dfrac{P(a\ |\ b)P(b)}{P(a)}
$$

In the more general case, for multivalued variables:

$$
\mathbf{P}(Y\ |\ X) = \dfrac{\mathbf{P}(X\ |\ Y)\mathbf{P}(Y)}{\mathbf{P}(X)}
$$

We also have a more general version conditionalized on some background evidence $\mathbf{e}$:

$$
\mathbf{P}(Y\ |\ X, \mathbf{e}) = \dfrac{\mathbf{P}(X\ |\ Y, \mathbf{e})\mathbf{P}(Y\ |\ \mathbf{e})}{\mathbf{P}(X\ |\ \mathbf{e})}
$$

Applying Bayes' rule: The simple case
-------------------------------------

Often we perceive as evidence the *effect* of some unknown *cause* and we would like to determine that cause. (example: a stiff neck (effect) is caused by meningitis (cause)) In that case, Bayes' rule becomes:

$$
P(cause\ |\ effect) = \dfrac{P(effect\ |\ cause)P(cause)}{P(effect)}
$$

The conditional probability $P(effect\ |\ cause)$ quantifies the relationship in the **causal** direction, whereas $P(cause\ |\ effect)$ describes the **diagnostic** direction. In medical diagnosis, we often have conditional probabilities on causal relationships. For example, the doctor knows $P(symptoms\ |\ disease)$ and want to derive a diagnosis $P(disease\ |\ symptoms)$. 

For example, what if the doctor wants to know if the disease is meningitis given some symptoms? 

Let's say:

 - $P(\text{stiff neck}\ |\ \text{meningitis}) = 0.7$
 - $P(\text{meningitis}) = 1/50000$
 - $P(\text{stiff neck}) = 0.01$

Then $P(m\ |\ s) = \dfrac{P(s\ |\ m)P(m)}{P(s)} = \dfrac{0.7 \times (1/50000)}{0.01} = 0.0014$

Recall that we can avoid accessing the prior probability of the evidence ($P(s)$ here) by instead computing a posterior probability for each value of the query variable (here $m$ and $\lnot m$) and then normalizing the results. We can apply the same process using Bayes rule:

$$
\mathbf{P}(M\ |\ s) = \alpha\langle\mathbf{P}(s\ |\ m)P(m), P(s\ |\ \lnot m)P(\lnot m)\rangle
$$

To use this approach, we need to estimate $P(s\ |\ \lnot m)$ instead of $P(s)$. This information may be easier to obtain than $P(s)$ or it may be harder. The idea is that we have an alternate representation that we can use depending on how difficult it is to get some information. The general form of the Bayes' rule with normalization is:

$$
\mathbf{P}(Y\ |\ X) = \alpha\mathbf{P}(X\ |\ Y)\mathbf{P}(Y)
$$

Using Bayes' rule: Combining evidence
-------------------------------------

What if we want to answer questions based on multiple pieces of evidence?

For example, what if we wnat to answer $\mathbf{P}(Cavity\ |\ toothache \land catch)$? We know that this approach does not scale up to large numbers of variables. We can try using Bayes' rule to reformulate it:

$$
\mathbf{P}(Cavity\ |\ toothache \land catch) = \alpha\mathbf{P}(toothache \land catch\ |\ Cavity)\mathbf{P}(Cavity)
$$

For this to work we need conditional probabilities of $toothcahe$ AND $catch$ for each value of $Cavity$. However, this does not scale up. So what do we do? We can refine the earlier notion of **independence** to get the notion of **conditional independence**. 

For example, it would be nice if $Toothache$ and $Catch$ were independent. They are not, because if the probe catches in the tooth, then it is likely that the tooth has cavity, and the cavity causes a toothache. However, these variables *are* independent *given the presence or absence of a cavity*. Each is directly caused by a cavity, but neither has a direct effect on the other (toothache is caused by the nerves signalling pain, and a catch is caused by the dentist's tool catching on the tooth). 

Hence, we can say:

$$
\mathbf{P}(toothache \land catch\ |\ Cavity) = \mathbf{P}(toothache\ |\ Cavity)\mathbf{P}(catch\ |\ Cavity)
$$

This equation expresses the **conditional independence** of $toothache$ and $catch$ given $Cavity$. This means we can plug it into the earlier equation)

$$
\mathbf{P}(Cavity\ |\ toothache \land catch) = \alpha\mathbf{P}(toothache\ |\ Cavity)\mathbf{P}(catch\ |\ Cavity)\mathbf{P}(Cavity)
$$

The general definition of **conditional independence** of two variables $X$ and $Y$ given a third variable $Z$, is

$$
\mathbf{P}(X, Y\ |\ Z) = \mathbf{P}(X\ |\ Z)\mathbf{P}(Y\ |\ Z)
$$

Hence:

$$
\mathbf{P}(Tootache, Catch\ |\ Cavity) = \mathbf{P}(Toothache\ |\ Cavity)\mathbf{P}(Catch\ |\ Cavity)
$$

As with absolute independence, we can have the equivalent forms:

$$
\mathbf{P}(X\ |\ Y, Z) = \mathbf{P}(X\ |\ Z)\text{ and }\mathbf{P}(Y\ |\ X, Z) = \mathbf{P}(Y\ |\ Z)
$$


This means you can decompose the full joint distribution into smaller pieces.

For example:

$$
\mathbf{P}(Toothache, Catch, Cavity) = \mathbf{P}(Toothache, Catch\ |\ Cavity)\mathbf{P}(Cavity)\text{ (product rule)}
$$
$$
= \mathbf{P}(Toothache\ |\ Cavity)\mathbf{P}(Catch\ |\ Cavity)\mathbf{P}(Cavity)
$$

The idea is that for $n$ symptoms that are all conditionally independent given $Cavity$, the size of the representation grows as $O(n)$ instead of $O(2^n)$.

Conditional independence assertions can allow probabilistic systems to scale up; morever, they are much more commonly available than absolute independence assertions. (pg 499 for why this helps - more details). The decomposition of large probabilistic domains into weakly connected subsets through conditional independence is one of the most important developments in the recent history of AI. The dentistry example illustrates a commonly occurring pattern in which a single cause directly influences a number of effects, all of which are conditionally independent, given the cause. The full joint distribution can be written as:

$$
\mathbf{P}(Cause, Effect\_1, \dotso, Effect\_N) = \mathbf{P}(Cause)\prod\_i\mathbf{P}(Effect\_i\ |\ Cause)
$$

Such a probability distribution is called a **naive Bayes** model. This is sometimes called a **Bayesian classifier**, which has prompted true Bayesians to call it the **idiot Bayes** model. 

The Bayes classifier is the function that assigns a class label $\hat{y} = C\_k$ for some $k$ as follows:

$$
\hat{y} = \underset{k \in \\{1, \dotso, K\\}}{\operatorname{argmax}}P(C\_k)\prod\_{i = 1}^nP(x\_i\ |\ C\_k)
$$

![Bayes classifier](http://upload.wikimedia.org/math/3/5/e/35e94f179a666c4b5892a11de1b3b29e.png)

Homework Question
=================

We know in general that the definition of conditional probability is as follows:

$$
    \mathbf{P}(A | B) = \frac{\mathbf{P}(A, B)}{\mathbf{P}(B)}
$$

We have to show that the statement of conditional independence:

$$
    \mathbf{P}(X, Y | Z) = \mathbf{P}(X | Z)\mathbf{P}(Y | Z)
$$

is equivalent to the statements:

$\mathbf{P}(X | Y, Z) = \mathbf{P}(X | Z)$ and $\mathbf{P}(Y | X, Z) = \mathbf{P}(Y | Z)$

Let us first try to show that $\mathbf{P}(X | Y, Z) = \mathbf{P}(X | Z)$. We can do this by starting with the statement of conditional independence and then expanding $\mathbf{P}(X, Y | Z)$ and $\mathbf{P}(Y | Z)$ using the definition of conditional probability:

$$
        \mathbf{P}(X, Y | Z) = \mathbf{P}(X | Z)\mathbf{P}(Y | Z)
$$
$$
        \frac{\mathbf{P}(X, Y, Z)}{\mathbf{P}(Z)} = \frac{\mathbf{P}(X | Z)\mathbf{P}(Y, Z)}{\mathbf{P}(Z)}
$$
$$
        \mathbf{P}(X, Y, Z) = \mathbf{P}(X | Z)\mathbf{P}(Y, Z)
$$

We now need to get a good representation for $\mathbf{P}(X, Y, Z)$. We can do this by starting with $\mathbf{P}(X | Y, Z)$ and then expanding it by using the definition of conditional probability:

$$
        \mathbf{P}(X | Y, Z) = \frac{\mathbf{P}(X, Y, Z)}{\mathbf{P}(Y, Z)}
$$
$$
        \implies\mathbf{P}(X, Y, Z) = \mathbf{P}(X | Y, Z)\mathbf{P}(Y, Z)
$$

Substituting this back into our original equation, we get:

$$
        \mathbf{P}(X, Y, Z) = \mathbf{P}(X | Z)\mathbf{P}(Y, Z) 
$$
$$
        \mathbf{P}(X | Y, Z)\mathbf{P}(Y, Z) = \mathbf{P}(X | Z)\mathbf{P}(Y, Z)
$$
$$
        \mathbf{P}(X | Y, Z) = \mathbf{P}(X | Z)
$$

This shows that the statement of conditional independence $\mathbf{P}(X, Y | Z) = \mathbf{P}(X | Z)\mathbf{P}(Y | Z)$ is equivalent to the statement $\mathbf{P}(X | Y, Z) = \mathbf{P}(X | Z)$, since we were able to derive the latter from the former.

Now we need to show that the statement of conditional independence is also equivalent to the statement $\mathbf{P}(Y | X, Z) = \mathbf{P}(Y | Z)$. We can use a similar approach as before. However, this time we will expand $\mathbf{P}(X | Z)$ instead:

$$
        \mathbf{P}(X, Y | Z) = \mathbf{P}(X | Z)\mathbf{P}(Y | Z)
$$
$$
        \frac{\mathbf{P}(X, Y, Z)}{\mathbf{P}(Z)} = \frac{\mathbf{P}(X, Z)\mathbf{P}(Y | Z)}{\mathbf{P}(Z)}
$$
$$
        \mathbf{P}(X, Y, Z) = \mathbf{P}(X, Z)\mathbf{P}(Y | Z)
$$

Again, we need to find a good representation for $\mathbf{P}(X, Y, Z)$. This time we can get it by expanding $\mathbf{P}(Y | X, Z)$:

$$
        \mathbf{P}(Y | X, Z) = \frac{\mathbf{P}(X, Y, Z)}{\mathbf{P}(X, Z)}
$$
$$
        \implies\mathbf{P}(X, Y, Z) = \mathbf{P}(Y | X, Z)\mathbf{P}(X, Z)
$$

Substituting this back into our equation, we get:

$$
        \mathbf{P}(X, Y, Z) = \mathbf{P}(X, Z)\mathbf{P}(Y | Z)
$$
$$
        \mathbf{P}(Y | X, Z)\mathbf{P}(X, Z) = \mathbf{P}(X, Z)\mathbf{P}(Y | Z)
$$
$$
        \mathbf{P}(Y | X, Z) = \mathbf{P}(Y | Z)
$$

Hence, we were also able to derive $\mathbf{P}(Y | X, Z) = \mathbf{P}(Y | Z)$ by starting with the statement of conditional independence. Therefore, we can now say that the statement of conditional independence:

$$
    \mathbf{P}(X, Y | Z) = \mathbf{P}(X | Z)\mathbf{P}(Y | Z)
$$

is equivalent to the statements:

$\mathbf{P}(X | Y, Z) = \mathbf{P}(X | Z)$ and $\mathbf{P}(Y | X, Z) = \mathbf{P}(Y | Z)$


Probability Identities
======================

Identities you can use.

Inclusion-exclusion principle
-----------------------------

$$
P(a \lor b) = P(a) + P(b) - P(a \land b)
$$

Conditional Probability
-----------------------

$$
P(a\ |\ b) = \dfrac{P(a \land b)}{P(b)}\text{ using random-variable instances (represents a particular value)}
$$

$$
\mathbf{P}(A\ |\ B) = \dfrac{\mathbf{P}(A, B)}{\mathbf{P}(B)}\text{ using random-variables (represents an entire domain of values)}
$$


Conditional Probability as Product Rule
---------------------------------------

$$
P(a \land b) = P(a\ |\ b)P(b)\text{ using random-variable instances (represents a particular value)}
$$

$$
\mathbf{P}(A, B) = \mathbf{P}(A\ |\ B)\mathbf{P}(B)\text{ using random-variables (represents an entire domain of values)}
$$

Marginalization
---------------

This is how you compute the marginal probabilities (in general). $\mathbf{Y}$ and $\mathbf{Z}$ are *sets* of random-variables. For example, $\mathbf{Z} = \\{Catch, Toothache\\}$. A $\mathbf{z} \in \mathbf{Z}$ is actually a combination of variable-values. That is $\mathbf{z} \in z\_1 \times z\_2 \times \dotso \times z\_{\left\lvert Z \right\rvert}$, which means that $\mathbf{z}$ is an element in the set of cross products of the values of all variables in $\mathbf{Z}$.

$$
\mathbf{P}(\mathbf{Y}) = \sum\limits\_{\mathbf{z} \in \mathbf{Z}}\mathbf{P}(\mathbf{Y}, \mathbf{z})
$$

Conditioning
------------

Just use the product rule to get:

$$
\mathbf{P}(\mathbf{Y}) = \sum\limits\_{\mathbf{z}}\mathbf{P}(\mathbf{Y}\ |\ \mathbf{z})P(\mathbf{z})
$$

Independence
------------

Also known as **marginal independence** and **absolute independence**.

$$
P(a\ |\ b) = P(a) \text{ or } P(b\ |\ a) = P(b) \text{ or } P(a \land b) = P(a)P(b)
$$

For random variables instead of values (or propositions):

$$
\mathbf{P}(X\ |\ Y) = \mathbf{P}(X) \text{ or } \mathbf{P}(Y\ |\ X) = \mathbf{P}(Y) \text{ or } \mathbf{P}(X \land Y) = \mathbf{P}(X)\mathbf{P}(Y)
$$

Bayes Rule
----------

Bayes rule is:

$$
P(b\ |\ a) = \dfrac{P(a\ |\ b)P(b)}{P(a)}
$$

In the more general case, for multivalued variables:

$$
\mathbf{P}(Y\ |\ X) = \dfrac{\mathbf{P}(X\ |\ Y)\mathbf{P}(Y)}{\mathbf{P}(X)}
$$

We also have a more general version conditionalized on some background evidence $\mathbf{e}$:

$$
\mathbf{P}(Y\ |\ X, \mathbf{e}) = \dfrac{\mathbf{P}(X\ |\ Y, \mathbf{e})\mathbf{P}(Y\ |\ \mathbf{e})}{\mathbf{P}(X\ |\ \mathbf{e})}
$$

Conditional Independence
------------------------

This is when two variables are independent given a third variable.

$$
\mathbf{P}(X, Y\ |\ Z) = \mathbf{P}(X\ |\ Z)\mathbf{P}(Y\ |\ Z)
$$

As with absolute independence, we have the equivalent forms:

$$
\mathbf{P}(X\ |\ Y, Z) = \mathbf{P}(X\ |\ Z)\text{ and }\mathbf{P}(Y\ |\ X, Z) = \mathbf{P}(Y\ |\ Z)
$$

Game Theory
===========

Game Theory: Terms
------------------

 - **Agent Design**: Game theory can analyze the agent's decisions and compute the expected utility for each decision (while assuming that the other agents are acting optimally according to game theory).
 - **Mechanism Design**: When an environment is inhabited by many agents, it might be possible to define the rules of the environment (i.e., the game that the agents must play) so that the collective good of all agents is maximized when each agent adopts the game-theoretic solution that maximizes its own utility (make it so that there is a dominant strategy).
 - **Strategic Form** or **Normal Form**: A matrix representation that shows the outcome of every combination of pure-strategies of players one and two. 
 - **Pure strategy**: A deterministic policy; a strategy played with a probability of 1. 
 - **Mixed strategy**: A probability distribution over all pure-strategies. Each pure-strategy has an assigned probability.
 - **Strategy profile**: Assignment of a strategy to *each* player.
 - **Dominant strategy**: A strategy $s$ that dominates $s'$ for every choice of strategies by the other player.
 - **Strongly dominant strategy**: A strategy $s$ **strongly dominates** $s'$ if the outcome for $s$ is better for the player than the outcome for $s'$ for every choice of strategies by the other player.
 - **Weakly dominant strategy**: A strategy $s'$ **weakly dominates** $s'$ if the outcome for $s$ is better on *at least one* strategy profile (i.e., one combination of p1 and p2 pure-strategies) and no worse on any other.
 - **Pareto optimal**: An outcome **pareto optimal** if there is *no other outcome* that all players would prefer. The outcome is such that there is *no other outcome* that makes every player *at least as well off* and at least one player *strictly better off*, that is, a **pareto optimal** outcome cannot be improved upon *without hurting at least one player*. A **Nash Equilibrium** is not necessarily **pareto optimal** because player's payoffs can be increased. 
 - **Pareto dominated**: An outcome is **pareto dominated** by *another* outcome, if all players would prefer the *other* outcome.
 - **Dominant strategy equilibrium**: When each player has a dominant strategy, the combination of those strategies is called a **dominant strategy equilibrium**.
 - **Equilibrium strategy profile**: In general, a strategy profile forms an **equilibrium** if *no* player can benefit by switching strategies, given that every other player sticks with the same strategy (i.e., assume there is some strategy-profile $P = \langle p\_1, p\_2 \rangle$ that is an equilibrium. If player 2 cannot do better by switching to any other strategy while player 1 sticks with $p\_1$, and if the same situation applies if you switch the places of players 1 and 2, then the strategy profile is an **equilibrium**).
 - **Repeated games**: Games with multiple move (the simplest kind of multiple-move game). Players face the same choice repeatedly.
 - **Sequential games**: Games with a sequence of turns that need not all be the same.
 - **Extensive form**: A game tree representing a sequential game.
 - **Stochastic games**: Games that have an element of chance (like backgammon). A distinguished player called $chance$ is added, and it can take random actions. $Chance's$ "strategy" is part of the definition of the game and is specified as a probability distribution over actions.
 - **Perfect recall**: Players always remember their *own* previous actions.
 - **Belief states**: In a partially observable game, we don't know what the other player might play. Hence the game tree is created over the space of **belief states**, i.e., different situations: I have an Ace and the other guy has a King; I have an Ace and the other guy also has an Ace. These are also called **information sets**.
 - **Ascending bid** or **English auction**: Auctioneer sets a reserve bid $b\_min$. If someone bids that amount, then the auctioneer asks for $b\_min + d$ and continues. The auction ends when no one is willing to bid anymore, after which the last player wins the bid.
 - **Efficient auction**: An auction is efficient if the goods go to the agent who values them most. We can also consider revenue to the auctioneer as well (discourage collusion).
 - **Strategy-proof mechanism**: A mechanism where agents have a dominant strategy. Hence, they are incentivized to play that. 
 - **Truth-revealing**, **truthful**, or **incentive-compatible strategy**: A strategy that reveals information. For example, a bidder who reveals his true value by playing this strategy. 
 - **Revelation principle**: Any mechanism can be transformed into an equivalent truth-revealing mechanism.
 - **Sealed-bid auction**: Ascending bid is not quite truth revealing because the winning bidder only reveals that $v\_i \ge b\_o + d$ ($b\_o$ is the highest bid among all other bidders). It can also discourage competition which is a disadvantage from the point of view of the seller. For example, if everyone things one bidder is definitely going to win anyway, then no one will participate, which means that the bidder wins with just $b\_{min} + d$. Hence in this auction, each bidder makes a single bid and communicates it ot the auctioneer without the other bidders seeing it. Here, there is no single dominant-strategy. If your value is $v\_i$, and you think the maximum of all other bids is going to be $b\_o$, then you should bid $b\_o + \epsilon$ for some small $\epsilon$ if lesser than $v\_i$. So the bid depends on the estimation of other bids which requires agents to do more work. Also, an agent with the highest $v\_i$ may not win the auction. This is more competitive, however and so the auctioneer has an advantage.
 - **Sealed-bid second-price auction** or **Vickrey auction**: The winner pays the price of the *second-highest* bid $b\_o$ rather than paying his own bid. This eliminates all the complex deliberations required for **first-price sealed-bid** acutions, because the dominant strategy is just to bid $v\_i$, and the mechanism is truth-revealing. The utility for an agent $i$, $u\_i$ is $(v\_i - b\_o)$ if $b\_i > b\_o$. That is, the utility is the difference between his true value and the second-higest bid if his actual bid is greater than the second-highest bid. However, if his bid is *not* greater, then his utility is just 0. Hence playing $b\_i = v\_i$ is the **dominant strategy** because when $(v\_i - b\_o)$ is positive, any bid that wins the auction is optimal, and playing $v\_i$ wins the auction for sure. When it is negative, *any* bid that loses the auction is optimal, and bidding $v\_i$ loses the auction as well. Since he will never bid anything greater that $v\_i$, playing $v\_i$ is a **dominant strategy**.
 - **Vickrey-Clarke-Groves** or **VCG** mechanism: A way to allocate goods efficiently while maximizing global utility. The center asks each agent to tell it how much it values this free gift. Everyone has an incentive to lie and report a high value. However, the trick is that each agent pays a tax equivalent to the *loss* in global utility that occurs because of the agent's presence in the game (i.e., the agent is a winner).
   - The center asks each interested agent to report its value $b\_i$ for receiving the good. That is, $b\_i$ is how much the agent claims he values the good.
   - The center allocates the goods to a subset of agents, $A$. $b\_i(A)$ is the outcome to the $i^{th}$ agent under this particular allocation $A$. If $i \in A$ (i.e., $i$ is a winner) the outcome is $b\_i$, and otherwise it is zero. The center also chooses $A$ such that it maximizes the total reported utility $B = \sum\nolimits\_ib\_i(A)$.
   - For each $i$, the center calculates the sum of the reported utilities of *all* winners, *except* $i$. Hence $B\_{-i} = \sum\nolimits\_{j \ne i}b\_j(A)$. For each $i$, the center also calculates the allocation that would maximize the total global utility assuming $i$ was not in the game. This is $W\_{-i}$.
   - Each agent pays a tax equal to $W\_{-i} - B\_{-i}$. 

Game Theory: Statements
-----------------------

**Regarding equilibria**

 - John Nash proved that *every game has at least one equilibrium*. The general concept of equilibrium is now called **Nash Equilibrium**.
 - If a game does not have **pure-strategy Nash equilibria**, then it has an **equilibrium in mixed strategies**, which is also a **Nash Equilibrium**.
 - The value/outcome of a zero-sum game with **mixed-strategies** is within bounds obtained by the outcome for the minimax pure-strategy and the maximin pure strategy: $U(minimax) \le U(mixed) \le U(maximin)$ or $U\_{p\_1, p\_2} \le U \le U\_{p\_2, p\_1}$.
 - Every two-player zero-sum game has a maximin equilibrium when you allow mixed-strategies. In general, every two-player zero-sum game has a (maximin) solution in either pure or mixed strategies.

**Regarding strategies**

 - A dominant strategy is a Nash Equilibrium.
 - Some games have Nash equilibria, but *no* dominant strategies.
 - If one player plays a pure strategy, the second player might as well choose a pure strategy. This is because the linear combination (expected payoff) using mixed-strategies can never be better than playing the pure-strategy with the highest payoff.

Repeated Prisoner's Dilemma
---------------------------

Suppose there are going to be 100 rounds of the game. Round 100 will not be a repeated game, so both can play the dominant strategy (testify). But once round 100 is determined, round 99 cannot have any effect on subsequent rounds, and so both players can just choose (testify, testify), and so on. We can get different solutions by saying that there is a chance that they will meet again. If we assume there is $.99$ probability that they will meet again.

**Perpetual Punishment**

This strategy strats off with players playing (refuse) until one person switches to (testify) after which both players will play (testify). If everyone keeps playing (refuse), the outcome is:

$$
\sum\limits\_{t = 0}^{\infty}0.99^t \cdot (-1) = -100
$$

If a player deviates and chooses (testify) on the first round:

$$
0 + \sum\limits\_{t = 1}^{\infty}0.99^t \cdot (-5) = -495
$$

He gets a payoff of 0 in the first round because he chose (testify), but thereafter he receives -5.
