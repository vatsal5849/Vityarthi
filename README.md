# Vityarthi
### Phase 1: Environment and Agent Design

#### 1.1 Environment Model
* **Grid Representation:** Represent the city as a 2D grid. Each cell $(x, y)$ can have different properties.
* **Static Obstacles:** Mark certain cells as impassable. These are fixed and don't change.
* **Terrain Costs:** Assign a cost to traversing each cell. For example, a "road" cell might have a cost of 1, while a "gravel" cell has a cost of 2, and a "mountain" cell has a cost of 5.
* **Dynamic Obstacles:** Implement moving obstacles that occupy a cell for a certain duration. The agent needs to be able to detect and avoid these. This can be modeled by a list of obstacle positions at each time step.

#### 1.2 Agent Model
* **State Representation:** Define the agent's state. A simple state could be $(x, y)$, but for dynamic environments, it should be $(x, y, \text{time})$. This is crucial for avoiding dynamic obstacles.
* **Actions:** Define the possible actions. For a 2D grid, these are typically moving to an adjacent cell (up, down, left, right, and optionally diagonals).
* **Costs:** The cost of an action is the terrain cost of the cell being entered. This reflects the "fuel" or "time" constraint.

### Phase 2: Search Algorithms

#### 2.1 Uninformed Search
* **Breadth-First Search (BFS):** Implement BFS to find a path from a start to a goal state. This algorithm is optimal for unweighted graphs (where all edge costs are 1). However, since we have varying terrain costs, it won't be optimal in our case. It's a good baseline.
* **Uniform-Cost Search (UCS):** Implement UCS. This is an extension of Dijkstra's algorithm and is optimal for finding the lowest-cost path on a weighted graph. It explores nodes in increasing order of their path cost ($g(n)$).

#### 2.2 Informed Search
* **A\* Search:** Implement A* search, which is an informed search algorithm that uses a heuristic to guide the search. The evaluation function is $f(n) = g(n) + h(n)$, where:
    * $g(n)$ is the cost from the start state to the current state $n$.
    * $h(n)$ is the estimated cost from state $n$ to the goal. This is the **heuristic**.
* **Admissible Heuristic:** For the 2D grid, a good and admissible heuristic is the **Manhattan distance** (or Euclidean distance). An admissible heuristic never overestimates the cost to the goal.
    * Manhattan distance: $h(n) = |x_n - x_{goal}| + |y_n - y_{goal}|$. This is admissible because the shortest path in a grid cannot be less than the sum of the horizontal and vertical distances.

#### 2.3 Local Search for Replanning
* **Problem:** Standard A\* finds a path at the beginning. If a dynamic obstacle appears on the planned path, the agent needs to replan.
* **Replanning Strategy:** Use a local search algorithm. Instead of re-running A\* from scratch, which can be computationally expensive, a local search can find a new path around the obstacle.
* **Hill-Climbing with Random Restarts:**
    * **Initial State:** The agent's current position.
    * **Heuristic:** Use the same heuristic as A\* ($h(n)$).
    * **Mechanism:** From the current state, move to the neighboring state that has the lowest heuristic value (i.e., is "closest" to the goal).
    * **Random Restarts:** If the agent gets stuck in a local minimum (a state where all neighboring states have a higher heuristic value), "restart" the search from a random new state (e.g., a few steps back from the current position) to escape it.

### Phase 3: Implementation and Experimentation

#### 3.1 Implementation
* **Programming Language:** Python is an excellent choice due to its clear syntax and powerful libraries.
* **Data Structures:**
    * Use a priority queue for A\* and UCS to efficiently retrieve the next node to expand.
    * Use a dictionary or a hash map to keep track of visited nodes and their costs to avoid cycles.
* **Modular Code:** Design separate modules for the `Environment`, `Agent`, and `Search Algorithms`.

#### 3.2 Experimental Setup
* **Map Instances:** Create several different grid maps. Vary the size, number of static obstacles, terrain costs, and the frequency/behavior of dynamic obstacles. For example, a small, dense map vs. a large, sparse map.
* **Metrics:** For each algorithm on each map, record:
    * **Path Cost:** The total cost of the path found.
    * **Nodes Expanded:** The number of states explored by the algorithm. This is a measure of computational effort.
    * **Time:** The execution time of the algorithm.

### Phase 4: Analysis and Reporting
#### 4.1 Analysis
* **BFS vs. UCS:** Explain why UCS will always find the optimal path in a weighted graph, whereas BFS will not. Show how this is reflected in your path cost results.
* **UCS vs. A\*:** Compare the number of nodes expanded. A\* should consistently expand fewer nodes than UCS because its heuristic guides it toward the goal, making it more efficient. Explain how this efficiency gain comes from the heuristic being admissible.
* **A\* vs. Local Search:** Discuss the trade-offs. A\* is optimal but can be slow to re-run. Local search is faster for replanning but is not guaranteed to find the optimal path and can get stuck in local minima. Explain how the local search strategy handles the dynamic obstacles more effectively than a full A\* re-plan.
* **Conclusion:** Summarize your findings, stating which algorithm is best suited for different scenarios (e.g., A* for finding the initial path, and local search for real-time adjustments).
