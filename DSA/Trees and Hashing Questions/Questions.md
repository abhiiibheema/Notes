**Topic 1: Binary Tree Counting & Combinatorics**

**Question 1.1: Total Labeled Trees with 4 Nodes**

- **Problem:** Calculate the total number of labeled binary trees that can be formed using $n = 4$ distinct nodes.
- **Formula:** $$\text{Total Labeled Trees} = C_n \times n! = \frac{1}{n+1} \binom{2n}{n} \times n!$$
- **Step-by-Step Solution:**
    1. Calculate the 4th Catalan number: $$C_4 = \frac{1}{5} \binom{8}{4} = \frac{1}{5} \times \frac{8 \times 7 \times 6 \times 5}{4 \times 3 \times 2 \times 1} = \frac{70}{5} = 14$$
    2. Multiply by the permutations of 4 distinct labels ($4!$): $$\text{Total} = 14 \times 4! = 14 \times 24 = \mathbf{336}$$

---

**Question 1.2: Labeled Trees with Fixed Root**

- **Problem:** Calculate the number of labeled binary trees with keys $\{1, 2, 3, 4\}$ where node **4 is fixed as the Root node**.
- **Step-by-Step Solution:**
    1. Fix node 4 at the root position.
    2. The remaining $3$ nodes ($\{1, 2, 3\}$) can be permuted in $3! = 6$ ways across the $C_4 = 14$ structural tree shapes: $$\text{Count} = C_4 \times 3! = 14 \times 6 = \mathbf{84}$$

---

**Topic 2: Tree Traversals & Tree Reconstructions**

**Question 2.1: Matching Traversals to Binary Tree Structure**

- **Problem:** Which binary tree structure matches Inorder = `B C A D` and Preorder = `A B C D`?
- **Step-by-Step Solution:**
    1. From Preorder `A B C D`, the root is **A**.
    2. Locate `A` in Inorder `B C A D`:
        - Left subtree = $\{B, C\}$
        - Right subtree = $\{D\}$
    3. Preorder for the left subtree is `B C`, so **B** is the left child of `A`.
    4. In the left subtree inorder `B C`, `C` is to the right of `B`, so **C** is the right child of `B`.
    5. The right subtree contains only **D** as the right child of `A`.
- **Reconstructed Tree Diagram:**

```
                     ( A )
                    /     \
                  ( B )   ( D )
                    \
                    ( C )
```

---

**Question 2.2: Sum of Internal Nodes from Preorder and Inorder**

- **Problem:** Given Inorder = `4, 7, 5, 6, 11, 2, 1, 5, 9` and Preorder = `2, 7, 4, 6, 5, 11, 5, 1, 9`. Construct the tree and find the sum of values of all **internal nodes**.
- **Step-by-Step Solution:**
    1. **Root:** First element in Preorder is **2**.
    2. **Inorder Split at** **2****:**
        - Left subtree inorder: `4, 7, 5, 6, 11` | Preorder: `7, 4, 6, 5, 11`
        - Right subtree inorder: `1, 5, 9` | Preorder: `5, 1, 9`
    3. **Left Subtree Construction:**
        - Root is `7`. Inorder split at `7`: Left is `4`, Right is `5, 6, 11`.
        - For `5, 6, 11`, preorder is `6, 5, 11` $\implies$ Root is `6`. Left child is `5`, Right child is `11`.
    4. **Right Subtree Construction:**
        - Root is `5`. Inorder split at `5`: Left child is `1`, Right child is `9`.
- **Reconstructed Tree Diagram:**

```
                        ( 2 )  <-- Internal
                       /     \
         Internal --> ( 7 )   ( 5 ) <-- Internal
                     /   \    /   \
                   ( 4 ) ( 6 )( 1 ) ( 9 )
                         /   \
                       ( 5 ) ( 11 )
```

```
5. **Identify Internal Nodes:** Nodes with at least one child are **`2, 7, 6, 5`**.
6. **Calculate Sum:**
   \\[\text{Sum} = 2 + 7 + 6 + 5 = \mathbf{20}\\]
```

---

**Question 2.3: Sum of Leaf Nodes from Inorder and Postorder**

- **Problem:** Given Inorder = `2, 7, 5, 6, 11, 1, 9, 5, 8` and Postorder = `2, 5, 11, 6, 7, 5, 8, 9, 1`. Construct the tree and find the **sum of all leaf nodes**.
- **Step-by-Step Solution:**
    1. **Root:** Last element in Postorder is **1**.
    2. **Inorder Split at** **1****:**
        - Left subtree: `2, 7, 5, 6, 11` | Postorder: `2, 5, 11, 6, 7`
        - Right subtree: `9, 5, 8` | Postorder: `5, 8, 9`
    3. **Left Subtree:** Root is `7`. Left child is `2`. Right child is `6` (with left child `5` and right child `11`).
    4. **Right Subtree:** Root is `9`. It has no left child. Right child is `8` (with left child `5`).
- **Reconstructed Tree Diagram:**

```
                        ( 1 )
                       /     \
                     ( 7 )   ( 9 )
                    /    \      \
                  ( 2 )  ( 6 )  ( 8 )
                         /   \   /
                       ( 5 )( 11)( 5 )
```

```
5. **Identify Leaf Nodes:** Nodes with no children are **`2, 5, 11, 5`**.
6. **Calculate Sum:**
   \\[\text{Sum} = 2 + 5 + 11 + 5 = \mathbf{23}\\]
```

---

**Question 2.4: Recursive Tree Traversal Execution Trace**

- **Problem:** Trace the output printed by the recursive function `foo(node *p)` on the given tree:

```
int foo(node *p) {
    int retval;
    if (p == NULL)
        return 0;
    else {
        retval = p->val + foo(p->left) + foo(p->right);
        printf("%d ", retval);
        return retval;
    }
}
```

- **Tree Diagram:**

```
                       ( 10 )
                      /      \
                    ( 5 )    ( 11 )
                   /    \        \
                 ( 3 )  ( 8 )    ( 13 )
```

- **Step-by-Step Execution:**
    1. `foo(3)`: $3 + 0 + 0 = 3 \implies$ Prints **3**, returns `3`.
    2. `foo(8)`: $8 + 0 + 0 = 8 \implies$ Prints **8**, returns `8`.
    3. `foo(5)`: $5 + 3 + 8 = 16 \implies$ Prints **16**, returns `16`.
    4. `foo(13)`: $13 + 0 + 0 = 13 \implies$ Prints **13**, returns `13`.
    5. `foo(11)`: $11 + 0 + 13 = 24 \implies$ Prints **24**, returns `24`.
    6. `foo(10)`: $10 + 16 + 24 = 50 \implies$ Prints **50**, returns `50`.
- **Printed Sequence:** **3 8 16 13 24 50**

---

**Topic 3: Binary Search Trees (BST)**

**Question 3.1: Valid Preorder Traversal for BST**

- **Problem:** A BST contains keys $\{1, 2, 3, 4, 5, 6, 7, 8\}$. Which sequence is a valid Preorder traversal?
    - (a) `5 3 1 2 4 7 8 6`
    - (b) `5 3 1 2 6 4 8 7`
    - (c) `5 3 2 4 1 6 7 8`
    - (d) `5 3 1 2 4 7 6 8`
- **Step-by-Step Verification:**
    - Root is `5`. Left subtree elements must be $< 5$ (`3, 1, 2, 4`), and right subtree elements must be $> 5$ (`7, 6, 8`).
    - In `(d)`:
        - Left subtree root `3`: left child `1` (which has right child `2`), right child `4`. All $< 3$ and $> 3$ boundaries are valid.
        - Right subtree root `7`: left child `6` ($< 7$), right child `8` ($> 7$). All valid.
- **Tree Diagram for (d):**

```
                       ( 5 )
                     /       \
                  ( 3 )      ( 7 )
                 /     \    /     \
               ( 1 )  ( 4 )( 6 )  ( 8 )
                 \
                 ( 2 )
```

- **Correct Answer:** **(d)**

---

**Question 3.2: BST Preorder to Postorder Conversion**

- **Problem:** The Preorder traversal of a BST is `35, 20, 10, 25, 30, 50, 45, 60`. Find the Postorder traversal.
- **Step-by-Step Reconstruction:**
    1. Root is `35`.
    2. Left subtree keys ($< 35$): `20, 10, 25, 30`
    3. Right subtree keys ($> 35$): `50, 45, 60`
    4. Subtree postorders:
        - Left: `10, 30, 25, 20`
        - Right: `45, 60, 50`
- **Tree Diagram:**

```
                         ( 35 )
                        /      \
                    ( 20 )     ( 50 )
                   /      \    /    \
                 ( 10 )  ( 25)( 45 )( 60 )
                            \
                            ( 30 )
```

- **Postorder Sequence:** **10, 30, 25, 20, 45, 60, 50, 35**

---

**Question 3.3: Complete BST Insertion Order Constraints**

- **Problem:** Let $P = \{1, 2, \dots, 15\}$. Consider any insertion order into an empty BST that results in a **complete binary tree**. Which element can **NEVER** be the 3rd element inserted?
    - (A) 4
    - (B) 2
    - (C) 10
    - (D) 5
- **Step-by-Step Solution:**
    1. To form a complete/perfect BST of 15 elements, the root must be `8`.
    2. Level 1 must contain `4` (left) and `12` (right).
    3. Level 2 contains `2, 6, 10, 14`.
    4. Level 3 contains `1, 3, 5, 7, 9, 11, 13, 15`.
- **Target Tree Structure:**

```
                            ( 8 )              <-- Level 0 (1st inserted)
                         /         \
                     ( 4 )         ( 12 )      <-- Level 1 (Can be 2nd / 3rd)
                    /     \        /     \
                  ( 2 )  ( 6 )  ( 10 )  ( 14 ) <-- Level 2 (Can be 3rd)
                  / \    / \    /  \    /  \
                 1   3  5   7  9   11  13  15  <-- Level 3 (Cannot be 3rd)
```

```
5. Element `5` is at Level 3. It cannot be inserted until its parent (`6`) and grandparent (`4`) are inserted first. Therefore, it requires at least 4 insertions.
```

- **Correct Answer:** **(D) 5**

---

**Question 3.4: Subtree Key Range Bounding**

- **Problem:** In a BST, node $n$ is the left child of node $m$. What is the maximum number of keys that can be inserted into the right subtree of node $n$?
- **Diagram:**

```
                     ( m )
                    /
                  ( n )
                    \
                    [ ? ]
```

- **Step-by-Step Solution:**
    1. Keys in the right subtree of $n$ must be strictly greater than $n$ and strictly less than $m$.
    2. Valid integer range: $[n + 1, m - 1]$.
    3. Number of keys: $$\text{Count} = (m - 1) - (n + 1) + 1 = \mathbf{m - n - 1}$$

---

**Topic 4: AVL Trees & Rebalancing Rotations**

**Question 4.1: AVL Insertion and Leaf Node Sum**

- **Problem:** Given the AVL tree below, insert key `13`, perform necessary rebalancing, and find the **sum of all leaf nodes**.
- **Initial AVL Tree:**

```
                     ( 17 )
                    /      \
                ( 12 )     ( 20 )
               /      \    /    \
             ( 5 )   ( 15)( 18) ( 24 )
             /       /  \
           ( 1 )   ( 14)( 16 )
```

- **Step-by-Step Rebalancing:**
    1. Insert `13` as the left child of `14`.
    2. The balance factor of `17` becomes $+2$ (height 4 vs 2), with an **LR imbalance** via nodes `17 -> 12 -> 15`.
    3. Double rotation (LR) makes `15` the new root of the subtree/tree.
- **Rebalanced Tree Diagram:**

```
                        ( 15 )
                      /        \
                  ( 12 )       ( 17 )
                 /      \      /    \
               ( 5 )   ( 14 ) ( 16) ( 20 )
               /       /            /    \
             ( 1 )   ( 13 )       ( 18 ) ( 24 )
```

```
4. **Leaf Nodes:** `1, 13, 16, 18, 24`
5. **Sum of Leaf Nodes:**
   \\[\text{Sum} = 1 + 13 + 16 + 18 + 24 = \mathbf{72}\\]
```

---

**Question 4.2: Comprehensive AVL Tree MSQ**

- **Problem:** Insert key `27` into the following AVL tree and perform rebalancing. Which statements are TRUE?

```
                         ( 40 )
                        /      \
                    ( 20 )     ( 60 )
                   /      \    /    \
                 ( 10 )   ( 30)( 50)( 70 )
                          /
                        ( 25 )
```

```
*   (A) The first unbalanced node encountered is 30
*   (B) The imbalance is of Left-Right (LR) type
*   (C) After rebalancing, 27 becomes parent of 25 and 30
*   (D) Sum of Leaf Nodes is 184
*   (E) Sum of Internal Nodes is 147
```

- **Step-by-Step Rebalancing:**
    1. Insert `27` as the right child of `25`.
    2. Node `30` has left subtree height 2 and right subtree height 0 $\implies \text{BF} = +2$. The first unbalanced node encountered from the bottom is **30** ((A) is TRUE).
    3. Path from 30: Left to `25`, Right to `27` $\implies$ **RL Imbalance** ((B) is FALSE).
    4. RL rotation on node 30 makes `27` the parent of `25` and `30` ((C) is TRUE).
- **Rebalanced Tree Diagram:**

```
                         ( 40 )
                        /      \
                    ( 20 )     ( 60 )
                   /      \    /    \
                 ( 10 )   ( 27)( 50)( 70 )
                          /  \
                        (25) (30)
```

```
5. **Internal Nodes:** `40, 20, 27, 60` \\(\implies \text{Sum} = 40 + 20 + 27 + 60 = \mathbf{147}\\) ((E) is TRUE).
6. **Leaf Nodes:** `10, 25, 30, 50, 70` \\(\implies \text{Sum} = 10 + 25 + 30 + 50 + 70 = \mathbf{185}\\) ((D) is FALSE).
```

- **Correct Statements:** **(A), (C), and (E)**

---

**Topic 5: Binary Heaps & Priority Queues**

**Question 5.1: Sequential Max-Heap Insertion**

- **Problem:** Keys `32, 15, 20, 30, 12, 25, 16` are inserted one by one into an initially empty Max-Heap. Find the level-order traversal of the resultant heap.
- **Step-by-Step Execution:**
    1. Insert `32`: `[32]`
    2. Insert `15`: `[32, 15]`
    3. Insert `20`: `[32, 15, 20]`
    4. Insert `30`: Placed at index 4, swaps with parent 15 $\implies$ `[32, 30, 20, 15]`
    5. Insert `12`: Placed at index 5 $\implies$ `[32, 30, 20, 15, 12]`
    6. Insert `25`: Placed at index 6, swaps with parent 20 $\implies$ `[32, 30, 25, 15, 12, 20]`
    7. Insert `16`: Placed at index 7 $\implies$ `[32, 30, 25, 15, 12, 20, 16]`
- **Final Heap Diagram:**

```
                          ( 32 )
                         /      \
                     ( 30 )    ( 25 )
                     /    \    /    \
                   ( 15 ) ( 12)( 20)( 16 )
```

- **Level-Order Array:** `[32, 30, 25, 15, 12, 20, 16]`

---

**Question 5.2: Consecutive Deletions in Max-Heap**

- **Problem:** Given the Max-Heap array `[25, 14, 16, 13, 10, 8, 12]`, find the contents of the array after **two consecutive delete operations**.
- **Step-by-Step Solution:**
    - **Initial Heap Diagram:**

```
                          ( 25 )
                         /      \
                      ( 14 )    ( 16 )
                      /   \     /   \
                    ( 13 )( 10)( 8 )( 12 )
```

```
*   **First Delete (Remove 25):**
    1. Replace root with last element `12` \\(\implies [12, 14, 16, 13, 10, 8]\\).
    2. Heapify down: Larger child of 12 is 16 \\(\implies\\) swap 12 and 16.
    3. Array becomes: `[16, 14, 12, 13, 10, 8]`.
*   **Second Delete (Remove 16):**
    1. Replace root with last element `8` \\(\implies [8, 14, 12, 13, 10]\\).
    2. Heapify down: Larger child of 8 is 14 \\(\implies\\) swap 8 and 14 \\(\implies [14, 8, 12, 13, 10]\\).
    3. Larger child of 8 is 13 \\(\implies\\) swap 8 and 13 \\(\implies [14, 13, 12, 8, 10]\\).
```

- **Final Heap Diagram:**

```
                          ( 14 )
                         /      \
                      ( 13 )    ( 12 )
                      /    \
                    ( 8 )  ( 10 )
```

- **Resulting Array:** `[14, 13, 12, 8, 10]`

---

**Question 5.3: Array Index Mapping of Complete BST in Heap Form**

- **Problem:** A BST with $N = 157$ distinct elements is a complete binary tree stored in a 1-based array heap representation. If the largest element is at index $X$ and the smallest element is at index $Y$, find $X + Y$.
- **Step-by-Step Solution:**
    1. In a BST, the largest element is at the rightmost path ($2i + 1$): $$1 \to 3 \to 7 \to 15 \to 31 \to 63 \to 127$$ At the next level, nodes start at index $128$. For $N = 157$, index $128$ exists, so the largest element is at index $X = 128$.
    2. The smallest element is at the leftmost path ($2i$): $$1 \to 2 \to 4 \to 8 \to 16 \to 32 \to 64 \to 128$$ In a complete tree of size 157, the bottom-left leaf is at index $Y = 127$.
    3. **Sum:** $$X + Y = 128 + 127 = \mathbf{255}$$

---

**Question 5.4: Min-Heapify Swap Count (GATE 2024 PYQ)**

- **Problem:** Given array `[82, 101, 90, 11, 111, 131, 44, 75, 33, 93]` of size $n = 10$. Find the total number of swaps required to build a Min-Heap using bottom-up heapification.
- **Step-by-Step Solution:**
    1. Internal nodes are at indices from $\lfloor 10/2 \rfloor = 5$ down to $1$:
        - Index 5 (value 111, child index 10 value 93): Swap 111 and 93 $\implies$ **1 swap**.
        - Index 4 (value 11, children 75, 33): No swap needed $\implies$ **0 swaps**.
        - Index 3 (value 90, children 131, 44): Swap 90 and 44 $\implies$ **1 swap**.
        - Index 2 (value 101, children 11, 93): Swap 101 and 11, then swap 101 and 33 $\implies$ **2 swaps**.
        - Index 1 (value 82, children 11, 44): Swap 82 and 11, then swap 82 and 33 $\implies$ **2 swaps**.
    2. **Total Swaps:** $$\text{Total} = 1 + 0 + 1 + 2 + 2 = \mathbf{6\text{ swaps}}$$

---

**Question 5.5: Multi-Category Data Structure Count Problem**

- **Problem:** For the set $\{1, 2, 3, 4\}$:
    - $X$ = Number of labeled binary trees
    - $Y$ = Number of distinct BSTs
    - $W$ = Number of AVL trees
    - $Z$ = Number of distinct Max-Heaps
    - Find the value of $X + Y + W + Z$.
- **Step-by-Step Solution:**
    1. $X = C_4 \times 4! = 14 \times 24 = \mathbf{336}$
    2. $Y = C_4 = \mathbf{14}$
    3. $W = \mathbf{4}$ (Only 4 balanced BST shapes satisfy $|\text{BF}| \le 1$)
    4. $Z = T(4) = \binom{3}{2} \cdot T(2) \cdot T(1) = 3 \times 1 \times 1 = \mathbf{3}$
    5. **Total Sum:** $$X + Y + W + Z = 336 + 14 + 4 + 3 = \mathbf{357}$$

---

**PART 2: HASHING & COLLISION RESOLUTION**

---

**Topic 6: Hash Functions & Uniform Distribution**

**Question 6.1: Modulo Hash Function Verification**

- **Problem:** Given keys `4322, 1334, 1471, 9679, 1989, 6171, 6173, 4199` and hash function $h(x) = x \bmod 10$. Which statements are true?
    - i. $9679, 1989, 4199$ hash to the same value
    - ii. $1471, 6171$ hash to the same value
- **Calculation:**
    - $1471 \bmod 10 = 1$ and $6171 \bmod 10 = 1$ (Statement ii is **TRUE**)
    - $9679 \bmod 10 = 9$, $1989 \bmod 10 = 9$, $4199 \bmod 10 = 9$ (Statement i is **TRUE**)
- **Correct Answer:** **Both i and ii are true**

---

**Question 6.2: Uniform Distribution Proof**

- **Problem:** Which hash function distributes keys $\{0, 1, \dots, 9\}$ most uniformly over 10 buckets?
    - (A) $h(i) = i^2 \bmod 10$
    - (B) $h(i) = i^3 \bmod 10$
- **Proof:**
    - For $i^2 \bmod 10$, outputs are $\{0, 1, 4, 9, 6, 5, 6, 9, 4, 1\}$. Buckets $2, 3, 7, 8$ remain empty (**Non-uniform**).
    - For $i^3 \bmod 10$, outputs are: $$0^3 \to 0,\; 1^3 \to 1,\; 2^3 \to 8,\; 3^3 \to 7,\; 4^3 \to 4,\; 5^3 \to 5,\; 6^3 \to 6,\; 7^3 \to 3,\; 8^3 \to 2,\; 9^3 \to 9$$ Every bucket from 0 to 9 receives exactly one key (**Strictly uniform**).
- **Correct Answer:** **(B)**

---

**Topic 7: Open Hashing (Separate Chaining)**

**Question 7.1: Chain Length Metrics (GATE 2014 PYQ)**

- **Problem:** Hash table with 9 slots, $h(k) = k \bmod 9$. Keys inserted: `5, 28, 19, 15, 20, 33, 12, 17, 10`. Find maximum, minimum, and average chain lengths.
- **Chaining Diagram:**

```
Slot 0: NULL
Slot 1: [28] -> [19] -> [10]  (Length = 3)
Slot 2: [20]                  (Length = 1)
Slot 3: [12]                  (Length = 1)
Slot 4: NULL
Slot 5: [5]                   (Length = 1)
Slot 6: [15] -> [33]          (Length = 2)
Slot 7: NULL
Slot 8: [17]                  (Length = 1)
```

- **Results:**
    - **Maximum Chain Length:** **3** (at Slot 1)
    - **Minimum Chain Length:** **0** (at Slots 0, 4, 7)
    - **Average Chain Length:** $\frac{\text{Total keys}}{\text{Slots}} = \frac{9}{9} = \mathbf{1}$

---

**Question 7.2: Probability of Unfilled Slots**

- **Problem:** In a hash table with 100 slots using chaining, what is the probability that the first 3 slots remain empty after inserting 3 keys under simple uniform hashing?
- **Step-by-Step Solution:**
    1. For each key, the probability of avoiding the first 3 slots is $\frac{100 - 3}{100} = \frac{97}{100}$.
    2. For 3 independent insertions: $$P = \frac{97}{100} \times \frac{97}{100} \times \frac{97}{100} = \mathbf{\frac{97^3}{100^3}}$$

---

**Topic 8: Closed Hashing (Linear Probing)**

**Question 8.1: Target Slot Lookup**

- **Problem:** Hash table with 10 slots (0 to 9), $h(k) = k \bmod 10$, linear probing. Keys inserted: `43, 165, 62, 123, 142`. In which slot will key `142` be inserted?
- **Step-by-Step Probing:**
    1. `43` $\to$ Slot 3
    2. `165` $\to$ Slot 5
    3. `62` $\to$ Slot 2
    4. `123`: $123 \bmod 10 = 3$ (occupied) $\to$ Slot 4
    5. `142`: $142 \bmod 10 = 2$ (occupied by 62)
        - Probe 1: Slot 3 (occupied by 43)
        - Probe 2: Slot 4 (occupied by 123)
        - Probe 3: Slot 5 (occupied by 165)
        - Probe 4: Slot 6 (empty) $\implies$ **Slot 6**
- **Table Layout Diagram:**

```
[ 0 ][ 1 ][ 62 ][ 43 ][ 123 ][ 165 ][ 142 ][ 7 ][ 8 ][ 9 ]
           Slot 2 Slot 3 Slot 4 Slot 5 Slot 6
```

- **Correct Answer:** **Slot 6**

---

**Question 8.2: Valid Insertion Sequences for Table State**

- **Problem:** A hash table of length 10 has the following state: `Slot 2: 42, Slot 3: 23, Slot 4: 34, Slot 5: 52, Slot 6: 46, Slot 7: 33`.
    1. Which sequence produces this table?
    2. How many total valid insertion sequences exist?
- **Solution:**
    1. Sequence `46, 34, 42, 23, 52, 33` is valid:
        - `46` (slot 6), `34` (slot 4), `42` (slot 2), `23` (slot 3) land without collision.
        - `52` ($52 \bmod 10 = 2$) probes $2 \to 3 \to 4 \to 5$.
        - `33` ($33 \bmod 10 = 3$) probes $3 \to 4 \to 5 \to 6 \to 7$.
    2. Permutations of $\{42, 23, 34, 46\}$ combined with valid relative arrivals of `52` and `33` yield **30 valid sequences**.

---

**Topic 9: Quadratic Probing**

**Question 9.1: Target Slot Lookup with Squared Probing**

- **Problem:** Table size $m = 20$, $h(x, i) = (x + i^2) \bmod 20$. Insert keys: `{96, 48, 63, 29, 87, 77, 47, 73, 70, 86, 166, 80, 147}`. Find the slot index for key `147`.
- **Step-by-Step Probing for 147:**
    - Initial hash: $147 \bmod 20 = 7$ (occupied by 87).
    - Probe 1 ($i=1$): $(7 + 1^2) \bmod 20 = 8$ (occupied by 48)
    - Probe 2 ($i=2$): $(7 + 2^2) \bmod 20 = 11$ (occupied by 47)
    - Probe 3 ($i=3$): $(7 + 3^2) \bmod 20 = 16$ (occupied by 96)
    - Probe 4 ($i=4$): $(7 + 4^2) \bmod 20 = 23 \bmod 20 = 3$ (occupied by 63)
    - Probe 5 ($i=5$): $(7 + 5^2) \bmod 20 = 32 \bmod 20 = \mathbf{12}$ (empty!)
- **Result:** Key `147` is placed in **Slot 12**.

---

**Question 9.2: Quadratic Probing Infinite Cycle Pitfall**

- **Problem:** Table size $m = 10$, $h(x, i) = (x + i^2) \bmod 10$. The table currently contains keys at slots `1, 2, 3, 6, 7, 8`. Trace what happens when key `12` is inserted.
- **Step-by-Step Cycle Trace:**
    - $i=0$: $(12 + 0) \bmod 10 = 2$ (occupied)
    - $i=1$: $(2 + 1) \bmod 10 = 3$ (occupied)
    - $i=2$: $(2 + 4) \bmod 10 = 6$ (occupied)
    - $i=3$: $(2 + 9) \bmod 10 = 1$ (occupied)
    - $i=4$: $(2 + 16) \bmod 10 = 8$ (occupied)
    - $i=5$: $(2 + 25) \bmod 10 = 7$ (occupied)
    - $i=6$: $(2 + 36) \bmod 10 = 8$ (occupied again!)
- **Conclusion:** The probe sequence loops endlessly through $\{2, 3, 6, 1, 8, 7\}$ and **never checks empty slots 0, 4, 5, 9**.

---

**Topic 10: Double Hashing**

**Question 10.1: Probe 1 Target Address**

- **Problem:** Double hashing with $m = 23$, primary hash $h_1(k) = k \bmod 23$, secondary hash $h_2(k) = 1 + (k \bmod 19)$. Probe formula: $[h_1(k) + i \cdot h_2(k)] \bmod 23$. Find the address checked at **Probe 1** for key $k = 90$.
- **Step-by-Step Solution:**
    1. $h_1(90) = 90 \bmod 23 = 21$
    2. $h_2(90) = 1 + (90 \bmod 19) = 1 + 14 = 15$
    3. Address for Probe 1 ($i=1$): $$\text{Address} = (21 + 1 \times 15) \bmod 23 = 36 \bmod 23 = \mathbf{13}$$

---

**Question 10.2: Double Hashing Slot Identification (GATE 2025 PYQ)**

- **Problem:** Table size $m = 11$, $h_1(k) = k \bmod 11$, $h_2(k) = 1 + (k \bmod 7)$. Keys inserted in order: `63, 50, 25, 79, 67, 24`. In which slot does key `24` get stored?
- **Step-by-Step Probing:**
    1. Prior keys:
        - `63` $\to$ Slot 8
        - `50` $\to$ Slot 6
        - `25` $\to$ Slot 3
        - `79` $\to$ Slot 2
        - `67` $\to$ Slot 1
    2. Inserting `24`:
        - Primary hash: $h_1(24) = 24 \bmod 11 = 2$ (occupied by 79).
        - Secondary hash offset: $h_2(24) = 1 + (24 \bmod 7) = 1 + 3 = 4$.
        - Probe 1 ($i=1$): $(2 + 1 \times 4) \bmod 11 = 6$ (occupied by 50).
        - Probe 2 ($i=2$): $(2 + 2 \times 4) \bmod 11 = 10 \bmod 11 = \mathbf{10}$ (empty!).
- **Table Layout Diagram:**

```
[   ][ 67 ][ 79 ][ 25 ][   ][   ][ 50 ][   ][ 63 ][   ][ 24 ]
     Slot 1 Slot 2 Slot 3         Slot 6         Slot 8   Slot 10
```

- **Correct Answer:** **Slot 10**