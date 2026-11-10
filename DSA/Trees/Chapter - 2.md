**Diagram Notes: Module 2 — Tree Traversals & Combinatorial Tree Counting**

---

**1. Core Traversal Mechanics & Visual Tracing**

**A. Definitions of Standard Traversals**

- **Inorder Traversal**: Visit **Left Subtree** $\rightarrow$ **Root** $\rightarrow$ **Right Subtree**.
- **Preorder Traversal**: Visit **Root** $\rightarrow$ **Left Subtree** $\rightarrow$ **Right Subtree**.
- **Postorder Traversal**: Visit **Left Subtree** $\rightarrow$ **Right Subtree** $\rightarrow$ **Root**.

```
                   ( Root )
                  /        \
         ( Left Subtree )  ( Right Subtree )
```

---

**B. Outer Contour (Boundary Tracing) Shortcut**

Draw a continuous outer contour line wrapping around the entire tree starting from the top-left of the root node going counter-clockwise.

```
                      ( A )   <--- 1. PREORDER: Print value when line touches LEFT edge
                     /     \
                   (B)     (C) <--- 2. INORDER:  Print value when line passes UNDER bottom
                  /   \
                (D)   (E)      <--- 3. POSTORDER: Print value when line touches RIGHT edge
```

- **Preorder**: Record the node label when the outer line touches its **LEFT** side.
- **Inorder**: Record the node label when the outer line passes **UNDER** its bottom edge.
- **Postorder**: Record the node label when the outer line touches its **RIGHT** side.

---

**C. Quick Root Identification Rule**

- **Preorder**: The **FIRST** element in the preorder sequence is ALWAYS the **Root**.
- **Postorder**: The **LAST** element in the postorder sequence is ALWAYS the **Root**.
- **Inorder**: Dividing the inorder sequence at the identified root separates all **Left Subtree elements** (to the left of root) from all **Right Subtree elements** (to the right of root).

---

**2. Unique Tree Reconstruction Rules**

```
                ┌──────────────────────────────────────────────┐
                │        CAN A UNIQUE TREE BE BUILT?           │
                └──────────────────────┬───────────────────────┘
                                       │
            ┌──────────────────────────┴──────────────────────────┐
            ▼                                                     ▼
┌───────────────────────┐                             ┌───────────────────────┐
│ Inorder + Preorder?   │ ──► YES (Unique Tree)   │ Preorder + Postorder? │
│ Inorder + Postorder?  │ ──► YES (Unique Tree)   │ ──► NO (Ambiguous)    │
└───────────────────────┘                             └───────────────────────┘
```

**Why Preorder + Postorder is NOT Unique**

Given Preorder = `a b c` and Postorder = `c b a`:

- `a` is the root, but `b` could be either a left child or a right child.
- Both a **left-skewed tree** and a **right-skewed tree** yield the exact same Preorder (`a b c`) and Postorder (`c b a`).

---

**3. Combinatorial Tree Counting & Catalan Numbers**

**A. Unlabeled Binary Trees**

An **unlabeled binary tree** considers only the geometric structure/shape of the tree (nodes are indistinguishable).

- **Recurrence Relation**: $$T(n) = \sum_{i=1}^{n} T(i - 1) \cdot T(n - i)$$ _Where_ $i - 1$ _nodes go to the left subtree and_ $n - i$ _nodes go to the right subtree._
- **Catalan Number Formula**: $$C_n = \frac{1}{n + 1} \binom{2n}{n} = \frac{(2n)!}{(n + 1)! \, n!}$$

**First 6 Catalan Number Values**:

|$n$ (Nodes)|Formula Calculation|$C_n$ (Unlabeled Trees / BSTs)|
|---|---|---|
|**1**|$\frac{1}{2}\binom{2}{1} = \frac{2}{2}$|**1**|
|**2**|$\frac{1}{3}\binom{4}{2} = \frac{6}{3}$|**2**|
|**3**|$\frac{1}{4}\binom{6}{3} = \frac{20}{4}$|**5**|
|**4**|$\frac{1}{5}\binom{8}{4} = \frac{70}{5}$|**14**|
|**5**|$\frac{1}{6}\binom{10}{5} = \frac{252}{6}$|**42**|
|**6**|$\frac{1}{7}\binom{12}{6} = \frac{924}{7}$|**132**|

---

**B. Labeled Binary Trees**

When $n$ nodes are distinct/labeled with unique keys (e.g., $1, 2, \dots, n$): $$\text{Total Labeled Binary Trees} = C_n \times n! = \frac{1}{n + 1} \binom{2n}{n} \times n!$$

---

**C. Distinct Binary Search Trees (BSTs)**

Given $n$ distinct keys, the number of structurally valid Binary Search Trees that can be formed is **equal to the Catalan Number** $C_n$: $$\text{Total Distinct BSTs} = C_n = \frac{1}{n + 1} \binom{2n}{n}$$

---

**4. Step-by-Step Solved Problems**

**Problem 1: Tracing Traversals on a Given Binary Tree**

- **Question**: For the binary tree shown below, find the Inorder, Preorder, and Postorder traversals.

```
                      (1)
                     /   \
                   (7)   (9)
                  /   \     \
                (2)   (6)   (8)
                      /     /
                    (11)  (5)
```

- **Step-by-Step Solution**:
    1. **Inorder (Left** $\rightarrow$ **Root** $\rightarrow$ **Right)**:
        - Left subtree of 1: Node 7 has left child 2 and right child 6 (which has left child 11) $\implies$ `2, 7, 11, 6`.
        - Root: `1`.
        - Right subtree of 1: Node 9 has right child 8 (which has left child 5) $\implies$ `9, 5, 8`.
        - **Inorder Output**: `2, 7, 11, 6, 1, 9, 5, 8`
    2. **Preorder (Root** $\rightarrow$ **Left** $\rightarrow$ **Right)**:
        - Root: `1`
        - Left subtree: `7, 2, 6, 11`
        - Right subtree: `9, 8, 5`
        - **Preorder Output**: `1, 7, 2, 6, 11, 9, 8, 5`
    3. **Postorder (Left** $\rightarrow$ **Right** $\rightarrow$ **Root)**:
        - Left subtree: `2, 11, 6, 7`
        - Right subtree: `5, 8, 9`
        - Root: `1`
        - **Postorder Output**: `2, 11, 6, 7, 5, 8, 9, 1`

---

**Problem 2: Matching Traversals to Binary Tree Structure**

- **Question**: Which binary tree has Inorder = `B C A D` and Preorder = `A B C D`?
    - **(A)** Root `A`, Left child `B`, Right child `C` with left child `D`
    - **(B)** Root `A`, Left child `C` with left child `B`, Right child `D`
    - **(C)** Root `A`, Right child `B` with left child `C` and right child `D`
    - **(D)** Root `A`, Left child `B` with right child `C`, Right child `D`
- **Step-by-Step Solution**:
    1. From Preorder `A B C D`, the root is **A**.
    2. Locate `A` in Inorder `B C A D`:
        - Left subtree consists of `{B, C}`.
        - Right subtree consists of `{D}`.
    3. Preorder for Left Subtree is `B C` $\implies$ Root of Left Subtree is **B**.
    4. Locate `B` in Left Subtree Inorder `B C`:
        - Nothing is to the left of `B` $\implies$ `B` has no left child.
        - `C` is to the right of `B` $\implies$ **C** **is the right child of** **B**.
    5. Right subtree contains only **D** as the right child of `A`.

```
                        ( A )
                       /     \
                     (B)     (D)
                       \
                       (C)
```

- **Correct Option**: **(D)**

---

**Problem 3: Finding Internal Nodes Sum from Preorder & Inorder**

- **Question**: Given Inorder = `4, 7, 5, 6, 11, 2, 1, 5, 9` and Preorder = `2, 7, 4, 6, 5, 11, 5, 1, 9`. Construct the binary tree and find the sum of values of all **internal nodes**.
- **Step-by-Step Solution**:
    1. **Identify Root**: First element in Preorder is **2**.
    2. **Split Inorder at** **2**:
        - Left Subtree Inorder: `4, 7, 5, 6, 11` | Preorder: `7, 4, 6, 5, 11`
        - Right Subtree Inorder: `1, 5, 9` | Preorder: `5, 1, 9`
    3. **Construct Left Subtree**:
        - Root of Left Subtree is **7**.
        - Inorder split at `7`: Left is `4`, Right is `5, 6, 11`.
        - Root of `{5, 6, 11}` is **6**. Inorder split at `6`: Left is `5`, Right is `11`.
    4. **Construct Right Subtree**:
        - Root of Right Subtree is **5**.
        - Inorder split at `5`: Left is `1`, Right is `9`.

```
                        (2)   <--- Internal
                       /   \
           Internal ->(7)   (5) <--- Internal
                     /  \   / \
                   (4)  (6) (1)(9)
                        / \
                      (5) (11)
```

1. **Identify Internal Nodes**:
    - Nodes with at least 1 child: **2, 7, 6, 5**.
2. **Calculate Sum**: $$\text{Sum} = 2 + 7 + 6 + 5 = \mathbf{20}$$

---

**Problem 4: Finding Leaf Nodes Sum from Inorder & Postorder**

- **Question**: Given Inorder = `2, 7, 5, 6, 11, 1, 9, 5, 8` and Postorder = `2, 5, 11, 6, 7, 5, 8, 9, 1`. Construct the binary tree and calculate the **sum of all leaf nodes**.
- **Step-by-Step Solution**:
    1. **Identify Root**: Last element in Postorder is **1**.
    2. **Split Inorder at** **1**:
        - Left Subtree Inorder: `2, 7, 5, 6, 11` | Postorder: `2, 5, 11, 6, 7`
        - Right Subtree Inorder: `9, 5, 8` | Postorder: `5, 8, 9`
    3. **Construct Subtrees**:
        - Left Subtree Root = **7**. Left child is `2`. Right child is **6** (with left child `5`, right child `11`).
        - Right Subtree Root = **9**. Right child is **8** (with left child `5`).

```
                        (1)
                       /   \
                     (7)   (9)
                    /   \     \
                  (2)   (6)   (8)
                        / \   /
                      (5)(11)(5)
```

1. **Identify Leaf Nodes**:
    - Nodes with zero children: **2, 5, 11, 5**.
2. **Calculate Sum**: $$\text{Sum} = 2 + 5 + 11 + 5 = \mathbf{23}$$

---

**Problem 5: Labeled Tree Counting & Fixed Root Constraints**

- **Question**:
    1. Calculate the total number of labeled binary trees that can be formed using $n = 4$ distinct nodes.
    2. Calculate the number of labeled binary trees using keys $\{1, 2, 3, 4\}$ where node `4` is fixed as the **Root node**.
- **Step-by-Step Solution**:
    1. **Part 1 (Total Labeled Trees)**: $$\text{Total} = C_4 \times 4! = 14 \times 24 = \mathbf{336}$$
    2. **Part 2 (Fixed Root)**:
        - Fix node `4` at the root position.
        - The remaining $3$ nodes $\{1, 2, 3\}$ can be arranged in $3! = 6$ ways across the $C_4 = 14$ valid binary tree structural shapes. $$\text{Count} = C_4 \times 3! = 14 \times 6 = \mathbf{84}$$

---

**Problem 6: Recursive Tree Function Execution Trace**

- **Question**: Consider the function `foo(node *p)` below and the given binary tree. What sequence of values is printed?

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

**Tree Structure**:

- Root `10`
- Left child `5` (with left child `3` and right child `8`)
- Right child `11` (with right child `13`)

```
                       (10)
                      /    \
                    (5)    (11)
                   /   \      \
                 (3)   (8)    (13)
```

- **Step-by-Step Execution Trace**:
    1. Call `foo(10)` $\rightarrow$ calls `foo(5)` $\rightarrow$ calls `foo(3)`.
    2. `foo(3)`: Left/Right are NULL $\rightarrow$ `retval = 3 + 0 + 0 = 3`. Prints **3**. Returns `3`.
    3. `foo(8)`: Left/Right are NULL $\rightarrow$ `retval = 8 + 0 + 0 = 8`. Prints **8**. Returns `8`.
    4. `foo(5)`: Left returned 3, Right returned 8 $\rightarrow$ `retval = 5 + 3 + 8 = 16`. Prints **16**. Returns `16`.
    5. Back to `foo(10)` $\rightarrow$ calls `foo(11)` $\rightarrow$ `foo(11->left)` returns 0 $\rightarrow$ calls `foo(13)`.
    6. `foo(13)`: Left/Right are NULL $\rightarrow$ `retval = 13 + 0 + 0 = 13`. Prints **13**. Returns `13`.
    7. `foo(11)`: Left returned 0, Right returned 13 $\rightarrow$ `retval = 11 + 0 + 13 = 24`. Prints **24**. Returns `24`.
    8. `foo(10)`: Left returned 16, Right returned 24 $\rightarrow$ `retval = 10 + 16 + 24 = 50`. Prints **50**. Returns `50`.
- **Final Printed Sequence**: $$\mathbf{3 \quad 8 \quad 16 \quad 13 \quad 24 \quad 50}$$