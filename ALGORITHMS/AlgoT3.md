# Comprehensive Guide to Algorithms & Data Structures

## Table of Contents
1. [Huffman Encoding](#huffman-encoding)
2. [Flow Networks](#flow-networks)
   - [Ford-Fulkerson Algorithm](#ford-fulkerson-algorithm)
   - [Max-Flow Min-Cut Theorem](#max-flow-min-cut-theorem)
3. [Edge Classifications](#edge-classifications)
   - [Safe Edges](#safe-edges)
   - [Useless Edges](#useless-edges) 
   - [Undecided Edges](#undecided-edges)
4. [Solving Recurrence Relations](#solving-recurrence-relations)
   - [Tree Method](#tree-method)u
   - [Unrolling Method](#unrolling-method)

## 1. Huffman Encoding <a name="huffman-encoding"></a>

### What is Huffman Encoding?
Huffman encoding is a lossless data compression algorithm that assigns variable-length codes to input characters, with shorter codes for more frequent characters. This creates an optimal prefix code, meaning no code is a prefix of another.

### Algorithm Steps
1. Count the frequency of each character in the input
2. Create a leaf node for each character and add it to a priority queue
3. While there is more than one node in the queue:
   - Remove the two nodes with lowest frequency
   - Create a new internal node with these two nodes as children and frequency equal to the sum of the children's frequencies
   - Add the new node to the queue
4. The remaining node is the root of the Huffman tree
5. Traverse the tree to assign codes (0 for left, 1 for right)

### Example: Encoding the string "adbdcabdac"

#### Step 1: Count frequencies
- a: 3
- b: 2
- c: 2
- d: 3

#### Step 2: Create initial priority queue
| Character | Frequency |
|-----------|-----------|
| b         | 2         |
| c         | 2         |
| a         | 3         |
| d         | 3         |

#### Step 3: Build the Huffman tree
First iteration:
- Remove b (2) and c (2)
- Create new node bc (4)
- Queue: a (3), d (3), bc (4)

Second iteration:
- Remove a (3) and d (3)
- Create new node ad (6)
- Queue: bc (4), ad (6)

Third iteration:
- Remove bc (4) and ad (6)
- Create new node root (10)
- Queue: root (10)

#### Step 4: Generate codes by traversing the tree
```
         root(10)
        /        \
      /            \
    ad(6)         bc(4)
   /    \         /    \
  a(3)   d(3)   b(2)   c(2)
```

Resulting codes:
- a: 00
- d: 01
- b: 10
- c: 11

#### Step 5: Encode the string "adbdcabdac"
"adbdcabdac" → "00 01 10 01 11 00 10 01 00 11" → "0001100111001001001111"

This reduces the original 10 characters (80 bits assuming 8-bit ASCII) to 22 bits, a compression ratio of 27.5%.

### Properties of Huffman Coding
- Produces optimal prefix codes
- Time complexity: O(n log n) where n is number of unique characters
- Space complexity: O(n) for the frequency table and tree

## 2. Flow Networks <a name="flow-networks"></a>

### What is a Flow Network?
A flow network is a directed graph where each edge has a capacity, and there are two distinguished vertices:
- Source (s): where flow begins
- Sink (t): where flow ends

Flow networks model many real-world scenarios like transportation networks, communication networks, and fluid flow systems.

### Key Properties
1. **Capacity constraint**: The flow on an edge cannot exceed its capacity
   ```
   0 ≤ f(u,v) ≤ c(u,v) for all (u,v) ∈ E
   ```

2. **Flow conservation**: For all vertices except source and sink, the total flow in equals the total flow out
   ```
   ∑ f(u,v) = ∑ f(v,w) for all v ∈ V - {s,t}
   ```

3. **Skew symmetry**: The flow from u to v is the negative of the flow from v to u
   ```
   f(u,v) = -f(v,u) for all (u,v) ∈ E
   ```

### What is a Maximum Flow?
The maximum flow problem seeks to find the maximum amount of flow that can be sent from source s to sink t while respecting capacity constraints and flow conservation.

### Example Flow Network:
```
             (5)
       A ----------> B
      /|             |\
   (8) |             | (7)
    /  |             |  \
   /   |             |   \
  s    | (3)         | (2)  t
   \   |             |   /
    \  |             |  /
   (5) |             | (8)
      \|             |/
       C ----------> D
             (6)
```

### Ford-Fulkerson Algorithm <a name="ford-fulkerson-algorithm"></a>

The Ford-Fulkerson algorithm finds the maximum flow in a flow network.

#### Algorithm Steps:
1. Initialize flow on all edges to 0
2. While there is an augmenting path from s to t in the residual graph:
   - Find the minimum capacity along this path
   - Update the flow along this path by this minimum capacity
3. Return the total flow

#### Key Concepts:
- **Residual Network**: A network that indicates additional possible flow
- **Residual Capacity**: `cf(u,v) = c(u,v) - f(u,v)`
- **Augmenting Path**: A path from s to t in the residual network with positive capacity

#### Example of Ford-Fulkerson:

Given the network above, let's perform Ford-Fulkerson:

Initial flow: 0 on all edges

**Iteration 1:**
Augmenting path: s → A → B → t with min capacity 5
Updated flows:
- f(s,A) = 5
- f(A,B) = 5
- f(B,t) = 5
Current max flow: 5

**Iteration 2:**
Augmenting path: s → C → D → t with min capacity 5
Updated flows:
- f(s,C) = 5
- f(C,D) = 5
- f(D,t) = 5
Current max flow: 10

**Iteration 3:**
Augmenting path: s → A → B → D → t with min capacity 2
Updated flows:
- f(s,A) = 7
- f(A,B) = 7
- f(B,D) = 2
- f(D,t) = 7
Current max flow: 12

**Iteration 4:**
Augmenting path: s → C → A → B → t with min capacity 1
Updated flows:
- f(s,C) = 6
- f(C,A) = 1
- f(A,B) = 8
- f(B,t) = 6
Current max flow: 13

No more augmenting paths, so maximum flow is 13.

### Finding Residual Networks
Given a flow network G = (V, E) with capacity function c and flow function f, the residual network Gf = (V, Ef) is constructed as follows:

1. For each edge (u,v) ∈ E with f(u,v) < c(u,v), add edge (u,v) to Ef with residual capacity cf(u,v) = c(u,v) - f(u,v)
2. For each edge (u,v) ∈ E with f(u,v) > 0, add edge (v,u) to Ef with residual capacity cf(v,u) = f(u,v)

### Max-Flow Min-Cut Theorem <a name="max-flow-min-cut-theorem"></a>

#### What is a Cut?
A cut (S,T) in a flow network G = (V,E) is a partition of V into S and T = V-S such that s ∈ S and t ∈ T.

#### What is an s-t Cut?
An s-t cut is a cut where the source s is in set S and the sink t is in set T.

#### Capacity of an s-t Cut
The capacity of an s-t cut is the sum of capacities of edges going from S to T:
```
c(S,T) = ∑c(u,v) for all u ∈ S, v ∈ T, (u,v) ∈ E
```

#### Flow of an s-t Cut
The flow across an s-t cut is the sum of flows on edges from S to T minus the sum of flows on edges from T to S:
```
f(S,T) = ∑f(u,v) - ∑f(v,u) for all u ∈ S, v ∈ T
```

#### What is a Minimum Cut?
A minimum cut is an s-t cut with minimum capacity among all s-t cuts.

#### Max-Flow Min-Cut Theorem
The value of the maximum flow is equal to the capacity of the minimum cut.

**Example:** In our previous flow network, the minimum cut is ({s, C}, {A, B, D, t}) with capacity 13, which equals the maximum flow we calculated.

## 3. Edge Classifications <a name="edge-classifications"></a>

In the context of network flows and cuts, edges can be classified into three categories:

### Safe Edges <a name="safe-edges"></a>
An edge is "safe" if it appears in every minimum s-t cut. This means the edge must be saturated in any maximum flow.

**Properties of Safe Edges:**
- Must be saturated in any maximum flow (f(u,v) = c(u,v))
- Removing such an edge reduces the maximum flow
- In the residual network, there is no path from s to t that includes this edge

**Example:** 
In a simple network with two parallel paths from s to t:
```
       (5)
    s ----> t
    |       ^
    | (3)   | (7)
    v       |
    a ----> b
```
The edge (s,t) with capacity 5 is a safe edge because it must be saturated in any maximum flow.

### Useless Edges <a name="useless-edges"></a>
An edge is "useless" if it never appears in any minimum s-t cut. These edges do not contribute to the maximum flow.

**Properties of Useless Edges:**
- Flow is always zero in any maximum flow (f(u,v) = 0)
- Increasing the capacity of such an edge does not affect the maximum flow
- In the residual network, there is no path from s to t through this edge

**Example:** 
In the network:
```
       (10)
    s ----> a
    |       |
    | (5)   | (3)
    v       v
    b ----> t
       (7)
```
The edge (a,t) with capacity 3 is a useless edge because the maximum flow is constrained by the capacity from s to {a,b}, which is 5 + 10 = 15, but the capacity from {a,b} to t is only 7, so the edge (a,t) never contributes to any minimum cut.

### Undecided Edges <a name="undecided-edges"></a>
An edge is "undecided" if it appears in some but not all minimum s-t cuts.

**Properties of Undecided Edges:**
- May carry flow in some maximum flow solutions but not others
- Changing the capacity may or may not affect the maximum flow
- In the residual network, there might be paths from s to t that include this edge

**Example:**
In a diamond-shaped network:
```
         (10)
      s ----> a
      |       |
      |       | (10)
      |       v
      | (10)  t
      |       ^
      |       |
      v       | (10)
      b ----->
         (10)
```
The edges (s,a) and (s,b) are undecided because there are multiple maximum flows possible, and each edge could be part of different minimum cuts.

## 4. Solving Recurrence Relations <a name="solving-recurrence-relations"></a>

Recurrence relations describe problems that can be broken down into smaller subproblems of the same type. Two common methods for solving recurrence relations are the tree method and the unrolling method.

### Tree Method <a name="tree-method"></a>

The tree method visualizes the recurrence as a tree and calculates the total cost by summing the costs at each level.

**Steps:**
1. Draw the recurrence as a tree
2. Calculate the cost at each level
3. Sum the costs across all levels
4. Express the sum in closed form

**Example 1: Merge Sort Recurrence**
```
T(n) = 2T(n/2) + n, T(1) = 1
```

Using the tree method:

Level 0: Cost = n
Level 1: 2 subproblems of size n/2, Cost = 2 * (n/2) = n
Level 2: 4 subproblems of size n/4, Cost = 4 * (n/4) = n
...
Level log₂(n): n subproblems of size 1, Cost = n * 1 = n

Total cost = n + n + n + ... + n (log₂(n) + 1 times) = n log₂(n) + n = O(n log n)

**Example 2: Binary Tree Recurrence**
```
T(n) = 7T(n/3) + n², T(1) = 1
```

Using the tree method:

Level 0: Cost = n²
Level 1: 7 subproblems of size n/3, Cost = 7 * (n/3)² = 7n²/9
Level 2: 49 subproblems of size n/9, Cost = 49 * (n/9)² = 49n²/81 = 7²n²/3⁴
...
Level i: 7ⁱ subproblems of size n/3ⁱ, Cost = 7ⁱ * (n/3ⁱ)² = (7/9)ⁱ * n²

Since 7/9 < 1, the sum converges to:
T(n) = n² * (1 + 7/9 + (7/9)² + ...) = n² * (1/(1-7/9)) = n² * (9/2) = O(n²)

**Example 3: Strassen's Matrix Multiplication**
```
T(n) = 7T(n/2) + O(n²), T(1) = 1
```

Using the tree method:

Level 0: Cost = cn²
Level 1: 7 subproblems of size n/2, Cost = 7 * c(n/2)² = 7cn²/4
Level 2: 49 subproblems of size n/4, Cost = 49 * c(n/4)² = 49cn²/16 = 7²cn²/4²
...
Level i: 7ⁱ subproblems of size n/2ⁱ, Cost = 7ⁱ * c(n/2ⁱ)² = (7/4)ⁱ * cn²

Since 7/4 > 1, the sum is dominated by the last level:
Height of tree = log₂(n)
Cost at last level = (7/4)^(log₂(n)) * cn² = n^(log₂(7)-log₂(4)) * cn² = n^(log₂(7)-2) * cn²
= n^(log₂(7)) = O(n^(log₂(7))) ≈ O(n^2.81)

### Unrolling Method <a name="unrolling-method"></a>

The unrolling method repeatedly substitutes the recurrence relation into itself until a pattern emerges.

**Steps:**
1. Start with the original recurrence
2. Substitute the recurrence into itself for smaller subproblems
3. Continue until a pattern emerges
4. Express the solution in closed form

**Example 1: Binary Search Recurrence**
```
T(n) = T(n/2) + 1, T(1) = 1
```

Unrolling:
T(n) = T(n/2) + 1
     = (T(n/4) + 1) + 1 = T(n/4) + 2
     = (T(n/8) + 1) + 2 = T(n/8) + 3
     ...
     = T(n/2ᵏ) + k

When n/2ᵏ = 1, we have 2ᵏ = n, or k = log₂(n)
So, T(n) = T(1) + log₂(n) = 1 + log₂(n) = O(log n)

**Example 2: Fibonacci Recurrence**
```
T(n) = T(n-1) + T(n-2), T(0) = 0, T(1) = 1
```

This doesn't unroll cleanly, but we can use characteristic equations:
r² = r + 1
r² - r - 1 = 0
r = (1 ± √5)/2

General solution: T(n) = c₁((1+√5)/2)ⁿ + c₂((1-√5)/2)ⁿ

Using initial conditions:
T(0) = 0 = c₁ + c₂
T(1) = 1 = c₁((1+√5)/2) + c₂((1-√5)/2)

Solving: c₁ = 1/√5, c₂ = -1/√5

Thus: T(n) = (1/√5)((1+√5)/2)ⁿ - (1/√5)((1-√5)/2)ⁿ

Since |((1-√5)/2)| < 1, as n grows, this term becomes negligible:
T(n) ≈ (1/√5)((1+√5)/2)ⁿ = O(φⁿ) where φ = (1+√5)/2 ≈ 1.618 (golden ratio)

**Example 3: The Master Theorem Approach**

For recurrences of the form:
```
T(n) = aT(n/b) + f(n)
```
where a ≥ 1, b > 1, and f(n) is a polynomial:

1. If f(n) = O(n^(log_b(a)-ε)) for some ε > 0, then T(n) = Θ(n^(log_b(a)))
2. If f(n) = Θ(n^(log_b(a))), then T(n) = Θ(n^(log_b(a)) * log n)
3. If f(n) = Ω(n^(log_b(a)+ε)) for some ε > 0, and if af(n/b) ≤ kf(n) for some k < 1, then T(n) = Θ(f(n))

**Example 4: Non-constant Division Recurrence**
```
T(n) = T(n-√n) + 1, T(1) = 1
```

Unrolling:
T(n) = T(n-√n) + 1
     = T(n-√n-√(n-√n)) + 1 + 1

This doesn't simplify easily. For such recurrences, we can estimate:
Let m = √n, so n = m²
T(m²) = T(m²-m) + 1

Let S(m) = T(m²), then:
S(m) = S(m-1) + 1
     = S(m-2) + 1 + 1
     = S(m-k) + k

When m-k = 1, k = m-1, so:
S(m) = S(1) + (m-1) = 1 + (m-1) = m

Therefore: T(n) = S(√n) = √n = O(√n)

### Summary of Methods

Both methods have strengths:

**Tree Method:**
- Visual representation helps understand the problem
- Works well for dividing problems (e.g., T(n) = aT(n/b) + f(n))
- Good for recognizing patterns in level costs

**Unrolling Method:**
- More algebraic approach
- Works well for both dividing and subtracting problems
- Good for directly deriving closed-form expressions

For recurrences that fit standard forms, the Master Theorem can provide immediate results without extensive calculations.
