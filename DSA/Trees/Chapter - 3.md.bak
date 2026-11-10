**Diagram Notes: Module 3 — Binary Search Trees (BST) & Height-Balanced AVL Trees**

---

**1. Binary Search Tree (BST) Fundamentals**

**A. Core BST Property**

A **Binary Search Tree** is a binary tree in which for **every node** $x$1:

- All keys stored in the **Left Subtree** of $x$ are **strictly less than** the key of $x$1.
- All keys stored in the **Right Subtree** of $x$ are **strictly greater than** the key of $x$1.

```
                             ( 56 )
                            /      \
                        ( 26 )     ( 200 )
                       /      \    /     \
                     (18)    (28)(190)   (213)
                     /  \    /
                   (12)(24)(27)
```

---

**B. Time Complexities & Traversal Properties**

- **Inorder Traversal Property**: The Inorder traversal of **ANY** valid Binary Search Tree produces a sequence of keys in **strictly increasing (sorted) order**2.
    - _Example Inorder_: `12, 18, 24, 26, 27, 28, 56, 190, 200, 213`2.
- **Tree Reconstruction Rule**: A BST can be **uniquely constructed using ONLY its Preorder traversal** or **ONLY its Postorder traversal**34 (since sorting the traversal sequence gives the Inorder traversal automatically)2.
- **Time Complexity**:
    - **Search Complexity**: Worst Case = $O(n)$ (for skewed trees)5more_horiz; Best/Average Case = $O(\log_2 n)$5.

---

**C. Minimum, Maximum, Predecessor & Successor**

- **Minimum Element**: Found by repeatedly following left pointers to the **leftmost node** of the tree8. Time Complexity = $O(h)$8.
- **Maximum Element**: Found by repeatedly following right pointers to the **rightmost node** of the tree9. Time Complexity = $O(h)$9.
- **Inorder Predecessor of** $x$: The node that immediately precedes $x$ in Inorder traversal (i.e., the **maximum element in the left subtree** of $x$)10.
- **Inorder Successor of** $x$: The node that immediately follows $x$ in Inorder traversal (i.e., the **minimum element in the right subtree** of $x$)10.

---

**2. BST Operations: Searching, Insertion & Deletion**

**A. Insertion Mechanics**

Newly inserted elements in a standard BST are **always added as Leaf Nodes** by traversing down from the root based on key comparisons11.

---

**B. Deletion Mechanics (The 3 Cases)**

```
                      ┌─────────────────────────────────┐
                      │    DELETING NODE X FROM A BST   │
                      └────────────────┬────────────────┘
                                       │
            ┌──────────────────────────┼──────────────────────────┐
            ▼                          ▼                          ▼
 ┌──────────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐
 │ Case 1: 0 Children   │   │ Case 2: 1 Child      │   │ Case 3: 2 Children   │
 │ (Leaf Node)          │   │ (Single Subtree)     │   │ (Dual Subtrees)      │
 ├──────────────────────┤   ├──────────────────────┤   ├──────────────────────┤
 │ Directly remove node │   │ Replace node X with  │   │ Replace X with either│
 │ X from tree.         │   │ its single child.    │   │ Inorder Predecessor  │
 │                      │   │                      │   │ OR Inorder Successor.│
 └──────────────────────┘   └──────────────────────┘   └──────────────────────┘
```

**Case 1: Deleting a Leaf Node (0 Children)**

Directly unlink node $X$ from its parent pointer12.

```
          (8)                      (8)
         /   \                    /   \
       (4)   (12)   ──►         (4)   (12)
      /   \  /   \             /      /   \
    (2)  (6)(10) (14)        (2)    (10)  (14)
           ^ Delete 6
```

**Case 2: Deleting a Node with 1 Child**

Bypass node $X$ by linking its parent directly to its only child13.

**Case 3: Deleting a Node with 2 Children**

1. Locate either the **Inorder Predecessor** (Max of Left Subtree) or **Inorder Successor** (Min of Right Subtree) of node $X$14.
2. Copy the key value of that predecessor/successor into node $X$13.
3. Recursively delete the predecessor/successor node (which is guaranteed to fall under Case 1 or Case 2)13.

---

**3. Height-Balanced Trees: AVL Trees & Rotations**

**A. Definition & Balance Factor (BF)**

An **AVL Tree** is a self-balancing Binary Search Tree where the **Balance Factor (BF)** of **every single node** satisfies15: $$\mathbf{\text{Balance Factor (BF)} = \text{Height of Left Subtree} - \text{Height of Right Subtree}} \in \{-1, 0, +1\}$$15

- **Search, Insertion, & Deletion Complexity**: Guaranteed $O(\log_2 n)$ in worst case5.

```
        Not an AVL Tree (BF = +2)                   Valid AVL Tree
                 ( 4 )  [BF = 1 - 3 = -2]              ( 41 ) [BF = 2 - 2 = 0]
                /     \                               /      \
             (1)      (10) [BF = 2 - 1 = +1]      (30)       (79)
                      /  \                        /  \       /  \
                    (9)  (11)                   (25) (32)  (65) (80)
                    /
                  (8)
```

---

**B. Critical Node Concept**

Upon inserting a new key, if the AVL property is violated, the **Critical Node** is defined as the **first node along the path from the newly inserted leaf up to the root whose** $|\text{BF}| > 1$16.

---

**C. The 4 AVL Rebalancing Rotations**

```
                            ┌────────────────────────┐
                            │  AVL REBALANCING TYPE  │
                            └───────────┬────────────┘
                                        │
             ┌──────────────────────────┴──────────────────────────┐
             ▼                                                     ▼
┌─────────────────────────┐                           ┌─────────────────────────┐
│  Single Rotations       │                           │  Double Rotations       │
├─────────────────────────┤                           ├─────────────────────────┤
│ Left-Left (LL):         │                           │ Left-Right (LR):        │
│ Insert in Left child of │ ──► Single Right Rotation │ Rotate Left child Left, │
│ Left subtree            │     on Critical Node [17] │ then Critical Node Right│
├─────────────────────────┤                           ├─────────────────────────┤
│ Right-Right (RR):       │                           │ Right-Left (RL):        │
│ Insert in Right child of│ ──► Single Left Rotation  │ Rotate Right child Right│
│ Right subtree           │     on Critical Node [18] │ then Critical Node Left │
└─────────────────────────┘                           └─────────────────────────┘
```

**1. Single Rotations (LL & RR)**

- **LL Rotation (Single Right Rotation)**:

```
     (3) [Critical Node]                  (2)
    /                                    /   \
  (2)                   ──►            (1)   (3)
  /
(1)
```

- **RR Rotation (Single Left Rotation)**:

```
(1) [Critical Node]                       (2)
  \                                      /   \
  (2)                   ──►            (1)   (3)
    \
    (3)
```

---

**2. Double Rotations (LR & RL)**

- **LR Rotation (Rotate Left, then Right)**:

```
     (3) [Critical]             (3) [Critical]             (2)
    /                          /                          /   \
  (1)           ──►          (2)           ──►          (1)   (3)
    \                        /
    (2)                    (1)
```

- **RL Rotation (Rotate Right, then Left)**:

```
(1) [Critical]               (1) [Critical]                (2)
  \                            \                          /   \
  (3)           ──►            (2)         ──►          (1)   (3)
  /                              \
(2)                              (3)
```

---

**4. Step-by-Step Solved Problems**

**Problem 1: Minimum Comparisons to Construct a BST**

- **Question**: Keys `45, 25, 65, 15, 35, 55, 75, 30` are inserted in order into an empty BST17. Assuming each comparison with a node's key is counted during insertion, find the total number of comparisons17.
- **Step-by-Step Calculation**:
    1. Insert `45`: Root node (0 comparisons)17.
    2. Insert `25`: Compared with `45` (1 comparison)17.
    3. Insert `65`: Compared with `45` (1 comparison)17.
    4. Insert `15`: Compared with `45`, `25` (2 comparisons)17.
    5. Insert `35`: Compared with `45`, `25` (2 comparisons)17.
    6. Insert `55`: Compared with `45`, `65` (2 comparisons)17.
    7. Insert `75`: Compared with `45`, `65` (2 comparisons)17.
    8. Insert `30`: Compared with `45`, `25`, `35` (3 comparisons)17.
- **Total Comparisons**: $$0 + 1 + 1 + 2 + 2 + 2 + 2 + 3 = \mathbf{13}\text{ comparisons}$$17

---

**Problem 2: Valid Preorder Traversal Verification for BST**

- **Question**: A BST contains keys `{1, 2, 3, 4, 5, 6, 7, 8}`18. Which of the following is a valid Preorder traversal sequence?
    - **(a)** `5 3 1 2 4 7 8 6`
    - **(b)** `5 3 1 2 6 4 8 7`
    - **(c)** `5 3 2 4 1 6 7 8`
    - **(d)** `5 3 1 2 4 7 6 8`
- **Step-by-Step Verification**:
    - Root is `5`18. Left subtree keys must be `< 5` (`3, 1, 2, 4`), Right subtree keys must be `> 5` (`7, 6, 8`)18.
    - Option (d) splits into: Left `3, 1, 2, 4` (Root 3, Left 1->Right 2, Right 4) and Right `7, 6, 8` (Root 7, Left 6, Right 8)18. All subtrees satisfy the BST ordering property18.
- **Correct Answer**: **(d)**18

---

**Problem 3: Converting Preorder to Postorder in BST**

- **Question**: Preorder traversal of a BST is `35, 20, 10, 25, 30, 50, 45, 60`19. What is the valid Postorder traversal?
- **Step-by-Step Reconstruction**:
    1. Root = `35`19.
    2. Left Subtree Preorder (`< 35`): `20, 10, 25, 30`19.
    3. Right Subtree Preorder (`> 35`): `50, 45, 60`19.
    4. Postorder for Left Subtree: `10, 30, 25, 20`19.
    5. Postorder for Right Subtree: `45, 60, 50`19.
- **Final Postorder Sequence**: $$\mathbf{10, 30, 25, 20, 45, 60, 50, 35}$$19

---

**Problem 4: Complete BST Insertion Sequence Problem**

- **Question**: Let $P = \{1, 2, \dots, 15\}$20. Consider any order of insertion into an empty BST that forms a **complete binary tree**20. Which element can **NEVER** be the 3rd element inserted?
    - **(A)** 4
    - **(B)** 2
    - **(C)** 10
    - **(D)** 5
- **Step-by-Step Solution**:
    1. To form a complete/perfect BST of 15 elements, Root must be `8`, Level 1 nodes must be `4` and `12`, Level 2 nodes must be `2, 6, 10, 14`20.
    2. The 1st element MUST be `8`20.
    3. The 2nd element can be `4` or `12`20.
    4. The 3rd element can be `12` or `4`, or Level-2 nodes `2, 6, 10, 14`20.
    5. Element `5` lies at Level 320. It can only be inserted after its parent (`4` or `6`) is already placed (requiring at least 4 insertions)20.
- **Correct Answer**: **(D) 5**20

---

**Problem 5: Key Range Calculation on Subtrees**

- **Question**: Consider an intermediate BST representation where node $n$ is the left child of node $m$21. What is the maximum number of keys that can be inserted on the right-hand side of node $n$21?

```
                         ( m )
                        /
                      ( n )
                        \
                        [ ? ]
```

- **Step-by-Step Derivation**:
    1. Keys in the right subtree of $n$ must be **strictly greater than** $n$ and **strictly less than** $m$21.
    2. Integer range of valid keys = $[n + 1, m - 1]$21.
    3. Total key count = $(m - 1) - (n + 1) + 1 = \mathbf{m - n - 1}$21.

---

**Problem 6: Key Counting between Root and Left Child**

- **Question**: A BST is constructed with key values $1 \dots 1000$22. The root is `550`, left child is `190`, right child is `750`22. How many elements can be present in the right subtree of the left child (`190`)22?
- **Step-by-Step Solution**:
    1. Keys in the right subtree of `190` must be `> 190` and `< 550`22.
    2. Key range = 22.
    3. Number of elements = $\text{UB} - \text{LB} + 1 = 549 - 191 + 1 = \mathbf{359}$22.

---

**Problem 7: AVL Tree Rebalancing (LR Rotation) & Leaf Sum**

- **Question**: Given an AVL tree with Root `17`, left child `12` (children `5` and `15`), right child `20` (children `18` and `24`)23. Insert key `13` into the tree, perform necessary rebalancing, and find the **sum of elements in the leaf nodes**23.

```
                     ( 17 )
                    /      \
                ( 12 )     ( 20 )
               /      \    /    \
             (5)     (15)(18)   (24)
             /       /  \
           (1)     (14) (16)
```

- **Step-by-Step Rebalancing**:
    1. Insert `13` as left child of `14`23.
    2. Critical node is `17` with $\text{BF} = +2$23. Imbalance pattern is **LR type** (Left to 12, Right to 15)23.
    3. Double rotation (LR) makes `15` the new Root of the tree23.
    4. Leaves in rebalanced tree: `1, 13, 16, 18, 24`23.
- **Sum of Leaf Nodes**: $$1 + 13 + 16 + 18 + 24 = \mathbf{72}$$23

---

**Problem 8: Comprehensive AVL Tree MSQ**

- **Question**: Insert `27` into the given AVL tree and perform rebalancing24. Which of the following statements are correct24?

```
                         ( 40 )
                        /      \
                    ( 20 )     ( 60 )
                   /      \    /    \
                 (10)     (30)(50)  (70)
                          /
                        (25)
```

- **(A)** The first unbalanced node encountered is 3024
- **(B)** The imbalance is of Left-Right (LR) type24
- **(C)** After rebalancing, 27 becomes parent of 25 and 3024
- **(D)** Sum of Leaf Nodes is 18424
- **(E)** Sum of Internal Nodes is 14724
- **Step-by-Step Verification**:
    1. Insert `27` as right child of `25`24.
    2. First node with $|\text{BF}| > 1$ from bottom-up is **30** ($\text{BF} = +2$) $\implies$ **Statement A is TRUE**24.
    3. Imbalance path from 30 goes Left (25) then Right (27) $\implies$ **RL Imbalance** (Statement B is FALSE)24.
    4. Rebalancing node 30 via RL rotation places `27` as parent of `25` and `30` $\implies$ **Statement C is TRUE**24.
    5. Internal nodes after rebalancing: `40, 20, 27, 60` $\implies \text{Sum} = 40 + 20 + 27 + 60 = \mathbf{147} \implies$ **Statement E is TRUE**24.