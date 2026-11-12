### CS TOPIC WISE TEST - 4

### Q.1

### 

Consider the MadeEasy Knapsack called MEKnapack

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip40-1774260319864.png)

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip41-1774260326466.png)

What is maximum profit using greedy strategy based on profit/weight ratio?

Backspace7894561230.-

←→

Clear All

### Q.2

### 

Let X = {x1, x2 ..... xn} be a set of n real valued points on the number line (not necessarily sorted).  
An internal [x, x + 2] can be placed starting at any point x ∈ X, and covers all points within that  
range. We have to cover all points in X using the minimum number of such internals.  
Greedy algorithm works:  
While there are uncovered points.  
Choose the leftmost uncovered point xi  
Place an internal starting at xi → covers [xi, xi + 2].  
S1 : X = [0.9, 1.1, 2.5, 2.7, 4.2, 4.9, 6.1, 6.8], the algorithm places 3 intervals to cover all points.  
S2 : T(n) = Ο(n log n) for unsorted array, Ο(n) for sorted array.  
S3 : If we replace [x, x + 2] with a symmetric interval [x – 1, x + 1], the greedy strategy needs to be  
changed to pick rightmost point within range instead of leftmost.

- 1.
    
    Only S1 and S3 are true
    

- 2.
    
    Only S1 is true
    

- 3.
    
    S2 and S3 are true
    

- 4.
    
    S1, S2 and S3 are true
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.3

### 

Consider an array arr[ ] = {10, 7, 15, 17, 2, 20, 6, 3}, array index start with ‘3’.  
After building a min heap using bottom-up approach, which element at location arr[6].

Backspace7894561230.-

←→

Clear All

### Q.4

### 

Suppose you implement priority queue using sorted and unsorted array. Worst case time  
complexity of insert and extract-min is [assume ascending order of sorted array extract-min is  
delete minimum]

- 1.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip34-1774260071899.png)
    

- 2.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip35-1774260076550.png)
    

- 3.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip36-1774260081717.png)
    

- 4.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip37-1774260085860.png)
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.5

### 

Let G = (V, E) be a directed graph with non-negative edge weights, let s and t be two vertices  
such that path exist from s → t.  
Which of the following conditions, such that unique shortest path exists from s to t.

- 1.
    
    When all edges are distinct positive integers.
    

- 2.
    
    When all edges are distinct positive integers and the graph contains no directed cycles.
    

- 3.
    
    When all edge length are distinct power of 2.
    

- 4.
    
    None of the above
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.6

### 

Which of the following statement is true for given graph G(n, m) with m = θ(n2) where n is  
number of vertices and m is number of edges.  
(i) Time complexity to test edge (u, v) exists or not in graph.  
(ii) Space complexity to store graph.

- 1.
    
    If graph represented in adjacency matrix from (i) is θ(1) (ii) is θ(n).
    

- 2.
    
    If graph represented in adjacency matrix from (i) is θ(1) (ii) is θ(n2).
    

- 3.
    
    If graph represented in adjacency list from (i) is θ(1) (ii) is θ(n).
    

- 4.
    
    If graph represented in adjacency list from (i) is θ(n) (ii) is θ(n).
    

##### Question Type: **SCQ**; Marks for correct answer: **2**; Negative Marks: **0.66**

### Q.7

### 

Consider the following directed graph _G_ is source vertex, ‘_A_’ is DFS traversal tree. _F_, _B_, _C_, _T_ are forward, back, cross and tree edges.

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip31-1774259920339.png)

Which of the following option is/are correctly possible?

- 1.
    
    There exist DFS sequence from vertex A with at most two forward edges.
    

- 2.
    
    There exist DFS sequence from vertex A with no forward edges.
    

- 3.
    
    For every sequence of DFS from source A, always no back edges.
    

- 4.
    
    There exists DFS sequence from source A, with no cross edge.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.8

### 

Consider the adjacency list representation of graph, each vertex maintain list for outgoing edges  
only. Let n and m denote the number of vertices and edges respectively in graph and k is the  
maximum in-degree of a vertex.  
Worst case to compute the in-degree of a given vertex

- 1.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip27-1774259858627.png)
    

- 2.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip28-1774259863403.png)
    

- 3.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip29-1774259867564.png)
    

- 4.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip30-1774259871501.png)
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.9

### 

Consider the following graph:  
Number of different BFS sequence possible from source ‘S’.

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip25-1774259814187.png)

Backspace7894561230.-

←→

Clear All

### Q.10

### 

Consider the following G = (V, E) undirected connected weighted graph, given in form of matrix.

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip22-1774259771238.png)

where ‘0’ entry in matrix represent no edge.  
Non-zero entry in matrix represent undirected edge and its weights.  
Value of x for which graph have maximum MST is y.  
Value of x * y is ________?

Backspace7894561230.-

←→

Clear All

### Q.11

### 

Let G = (V, E) be a connected, undirected, weighted graph with distinct positive edge weights.  
Suppose T ⊆ G is the unique minimum spanning tree (MST) of G and for a given real value λ  
defined the set.

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip19-1774259554272.png)

Let G≤ λ = (V, E≤ λ) be the threshold subgraph induced by these edges.  
Which of the following statements is/are always true?

- 1.
    
    If G≤ λ is disconnected, then no edge from any component of G≤ λ can be part of the MST.
    

- 2.
    
    The number of connected components in G≤ λ decrease if λ → ∞.
    

- 3.
    
    For any value λ, all edges in T ∩ G≤ λ form a forest in G.
    

- 4.
    
    The first![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip20-1774259712901.png)– 1 edges added as λ increase will always be the edges of MST T.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.12

### 

Consider the following weighted graph.

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip17-1774259488140.png)

Which of the following is/are true?

- 1.
    
    Dijkstra fails to find the shortest path in graph.
    

- 2.
    
    Edge [D, E] [D, F], [A, B] are not part of Dijkstra tree.
    

- 3.
    
    There exist a pair of vertex have 2 different shortest path.
    

- 4.
    
    None of these
    

##### Question Type: **SCQ**; Marks for correct answer: **2**; Negative Marks: **0.66**

### Q.13

### 

Consider the following function:

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip16-1774259430735.png)

What is the function computes?

- 1.
    
    Number of partitions of n into distinct positive integers.
    

- 2.
    
    Number of ordered sequence of positive integers that sum to n.
    

- 3.
    
    Number of subsets of {1, 2, ..... n} that sum to n.
    

- 4.
    
    None of these
    

##### Question Type: **SCQ**; Marks for correct answer: **2**; Negative Marks: **0.66**

### Q.14

### 

Consider the following connected graph:

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip14-1774259387171.png)

Suppose x always be part of minimum spanning tree and y can’t be part of MST.  
What is the sum of Maxvalue(x) + Minvalue(y) = ?

Backspace7894561230.-

←→

Clear All

### Q.15

### 

Consider, G = (V, E) be connected undirected graph with distinct positive edge weights. Let T be  
the MST of G.  
Consider the following statements:  
I. For any two vertices u and v, the path between u and v in T is not necessarily the shortest path in G.  
II. If an edge ∈ E is not present in any cycle in G, then e must in every MST of G.  
III. If e ∈ T and replacing it with a lighter edge e′ ∉ T then new tree is still a MST.  
Which of the above statements is/are always true?

- 1.
    
    Only 1
    

- 2.
    
    1 and 2
    

- 3.
    
    2 and 3
    

- 4.
    
    All 1, 2, and 3
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.16

### Determine the number of ways to fill below tree from [1 to 10] values such that each parent is  
greater than its children?

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip12-1774259295933.png) 

Backspace7894561230.-

←→

Clear All

### Q.17

### 

Consider the following code for an array [1 ... n]?

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip11-1774259242422.png)

Let C(n) be the total number of comparison done during unknown loop(A, n). (S, n) be the total number of swaps done.

- 1.
    
    If ∃ i ∈ [1, n] such that all A[i] = k (constant) ten C(n) = 0, S(n) = 0
    

- 2.
    
    The final structure of A cannot be used for in place heap sort because it violets parent child ordering.
    

- 3.
    
    C(n) ∉ Ω (n log n) and S(n) ∈ Ο(n) regardless of input values.
    

- 4.
    
    Both (a) and (c)
    

##### Question Type: **SCQ**; Marks for correct answer: **2**; Negative Marks: **0.66**