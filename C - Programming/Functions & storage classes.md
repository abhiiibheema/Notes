Part 1: Core Concepts & Theoretical Notes

1. Function Fundamentals

- **Definition**: A function is an independent, self-contained block of statements designed to perform a distinct computation or task.
- **Core Advantages**:
    - **Modularity**: Breaks large, complex programs into structured, manageable units.
    - **Reusability**: Code is written once and can be invoked multiple times across the program without duplication.

The Three Structural Components of Functions

| Component                            | Purpose                                                                                      | Syntax Pattern / Example                                                               |
| ------------------------------------ | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Function Declaration (Prototype)** | Informs the compiler about the function name, parameter types, and return type before usage. | `return_type FunctionName(param_type1, param_type2);`<br>Example: `int fun(int, int);` |
| **Function Call**                    | Transfers execution control from the calling function to the target function.                | `FunctionName(arg1, arg2);`<br>Example: `fun(a, b);`                                   |
| **Function Definition**              | Provides the complete executable logic, local variables, and return statement.               | `return_type FunctionName(type var1, type var2) { ... }`                               |

---

2. Parameter Passing & Scope Mechanics

Actual vs. Formal Parameters

- **Actual Parameters (Arguments)**: The variables or literal values passed inside the function call (e.g., `a` and `b` in `fun(a, b)`).
- **Formal Parameters**: The variables declared in the function definition header that receive copies of the incoming arguments (e.g., `x` and `y` in `fun(int x, int y)`).
- **Call by Value**: C passes primitive arguments by value. Modifications made to formal parameters inside the function do **not** reflect back on the actual parameters in the caller.

Scope & Variable Shadowing

- **Global Variables**: Declared outside of all functions. Accessible across any function in the program.
- **Local Variables**: Declared inside a function block or parameter list. Accessible only within that specific scope.
- **Shadowing Rule**: If a local variable shares the same name as a global variable, the **local variable takes priority** within that block.

Function Parameter Evaluation Order

- In standard C, the order in which function call arguments are evaluated is **compiler-dependent** (unspecified behavior).
- When an exam problem specifies an evaluation order (such as _"Assume parameters are evaluated from right to left"_), arguments must be evaluated and side effects applied in that exact order before the function body begins.

Implicit Type Conversion in Parameters

- If an argument type differs from the formal parameter type, implicit type conversion occurs.
- **Example**: Passing `2.5` (a floating-point literal) to a function expecting `int x` results in truncation; `x` receives `2`.

---

3. Activation Records, Runtime Stack & Activation Trees

Activation & Stack Frames

- **Activation**: The active execution lifecycle of a function invocation.
- **Activation Record (Stack Frame)**: A dedicated memory segment allocated on the **runtime stack** whenever a function is called. It stores:
    - Local variables and formal parameters.
    - Return address back to the caller.
    - Saved machine registers and status flags.

Runtime Stack Lifecycle

1. When a function is called, its activation record is **pushed** onto the stack.
2. If that function calls another function, a new activation record is pushed on top.
3. When a function finishes execution, its activation record is **popped**, and control returns to the caller.

```
Stack Growth during: main() calls f1(), f1() calls f2()

|-------------------|
| Activation of f2  |  <-- Top of Stack (Currently Executing)
|-------------------|
| Activation of f1  |
|-------------------|
| Activation of main|  <-- Stack Base
|-------------------|
```

Activation Tree

- A hierarchical tree structure illustrating parent-to-child function call relationships.
- **Root**: Represents `main()`.
- **Nodes**: Represent function activations.
- **Edges**: Directed arrows from the caller to the callee.
- **Execution Flow**: Read from left to right as execution progresses through the tree branches.

---

Part 2: Comprehensive Problem Walkthroughs

---

Problem 1: Parameter Passing & Combined Increment Operators

```
#include <stdio.h>

int fun(int, int);

int main() {
    int a = 10, b = 20;
    printf("%d", fun(a, b));
}

int fun(int x, int y) {
    int z;
    z = x++ * ++y;
    return z;
}
```

- **Execution Trace**:
    1. `a = 10` and `b = 20` are passed by value: `x = 10`, `y = 20`.
    2. In `z = x++ * ++y;`:
        - `x++` (post-increment): Evaluates to the current value **10**, then `x` becomes 11.
        - `++y` (pre-increment): Increments `y` from 20 to 21, then evaluates to **21**.
        - Calculation: `z = 10 * 21 = 210`.
    3. `fun` returns `210` to `main()`.

**Final Output**:

```
210
```

---

Problem 2: Mutation of Global Variables across Function Scopes

```
#include <stdio.h>

int fun(int, int);
int c = 12; // Global variable

int main() {
    int a = 10, b = 20;
    c++;
    printf("%d", fun(a, b));
}

int fun(int x, int y) {
    int z;
    c = c + 20;
    z = x++ * ++y + c;
    return z;
}
```

- **Execution Trace**:
    1. Global `c` starts at `12`.
    2. In `main()`: `c++` increments global `c` to `13`.
    3. `fun(10, 20)` is called:
        - `c = c + 20` updates global `c` to `13 + 20 = 33`.
        - `x++` evaluates to `10`.
        - `++y` updates `y` to `21` and evaluates to `21`.
        - `z = (10 * 21) + 33 = 210 + 33 = 243`.
    4. Returns `243`.

**Final Output**:

```
243
```

---

Problem 3: Interleaved Function Invocations Modifying Shared Global State

```
#include <stdio.h>

int g = 5; // Global variable

int foo(int x, int y) {
    int z;
    z = ++x * y--;
    g += x;
    return z;
}

int bar(int a) {
    return a * g--;
}

int main() {
    int a = 3, b = 4;
    int r1 = foo(a, b);
    int r2 = bar(a);
    printf("%d %d", r1, r2);
}
```

- **Execution Trace**:
    1. Initial values: `g = 5`, `a = 3`, `b = 4`.
    2. Call `r1 = foo(3, 4)`:
        - `++x`: Increments formal parameter `x` from 3 to 4, evaluates to **4**.
        - `y--`: Evaluates to **4**, then `y` decrements to 3.
        - `z = 4 * 4 = 16`.
        - `g += x` updates global `g`: `g = 5 + 4 = 9`.
        - Returns `16` $\rightarrow$ `r1 = 16`.
    3. Call `r2 = bar(3)`:
        - Evaluates `a * g--`: Uses current `g` (9) $\rightarrow 3 \times 9 = 27$.
        - After multiplication, `g--` decrements `g` to 8.
        - Returns `27` $\rightarrow$ `r2 = 27`.

**Final Output**:

```
16 27
```

---

Problem 4: Multi-Step Global State Evolution

```
#include <stdio.h>

int p = 10, q = 2; // Global variables

int calc(int x, int y) {
    int r;
    r = x-- - --y;
    q *= x;
    return r;
}

int update(int a) {
    p -= a;
    return p++ * q;
}

int main() {
    int a = 6, b = 4;
    int x = calc(a, b);
    int y = update(x);
    printf("%d %d", x, y);
}
```

- **Execution Trace**:
    1. Call `x = calc(6, 4)`:
        - `x--`: Evaluates to **6**, then local `x` becomes 5.
        - `--y`: Decrements local `y` from 4 to 3, evaluates to **3**.
        - `r = 6 - 3 = 3`.
        - `q *= x`: Global `q = 2 * 5 = 10`.
        - Returns `3` $\rightarrow$ local `x` in `main()` receives `3`.
    2. Call `y = update(3)`:
        - `p -= a`: Global `p = 10 - 3 = 7`.
        - `p++ * q`: Uses `p = 7` and `q = 10` $\rightarrow 7 \times 10 = 70$.
        - (Post-increment makes `p = 8`).
        - Returns `70` $\rightarrow$ local `y` in `main()` receives `70`.

**Final Output**:

```
3 70
```

---

Problem 5: Argument Type Truncation & Switch Case Fall-Through

```
#include <stdio.h>

void foo(int x) {
    switch(x) {
        case 1: printf("*");
        case 2: printf("*");
        case 3: printf("*");
        default: printf("*");
    }
}

int main() {
    foo(2.5);
}
```

- **Execution Trace**:
    1. The float literal `2.5` is passed into `foo(int x)`.
    2. Truncation occurs: `x` becomes integer `2`.
    3. `switch(2)` jumps directly to `case 2`:
        - Prints `*`.
        - Because there are no `break` statements, execution falls through to `case 3` $\rightarrow$ prints `*`.
        - Execution falls through to `default` $\rightarrow$ prints `*`.
    4. Total number of asterisks printed: **3**.

**Final Output**:

```
***
```

---

Problem 6: Tracing Call Hierarchies Across Mutual Functions

```
#include <stdio.h>

void a();
void b();
void c();

int main() {
    a();
    b();
    return 0;
}

void a() { printf("a"); b(); }
void b() { printf("a"); c(); }
void c() { printf("a"); }
```

- **Execution Trace**:
    1. `main()` calls `a()`:
        - `a()` prints `"a"` (1st).
        - `a()` calls `b()`:
            - `b()` prints `"a"` (2nd).
            - `b()` calls `c()`:
                - `c()` prints `"a"` (3rd).
    2. `main()` calls `b()`:
        - `b()` prints `"a"` (4th).
        - `b()` calls `c()`:
            - `c()` prints `"a"` (5th).
    3. Total characters printed: **5**.

**Final Output**:

```
aaaaa
```

---

Problem 7: Hierarchical Nested Execution (Outline Method)

```
#include <stdio.h>

void a() { printf("1"); }
void b() { a(); printf("2"); }
void c() { a(); b(); printf("3"); }
void d() { a(); b(); c(); printf("4"); }

int main() {
    d();
}
```

- **Execution Trace for** **d()**:
    - Step 1: `a()` $\rightarrow$ prints `1`.
    - Step 2: `b()`:
        - Calls `a()` $\rightarrow$ prints `1`.
        - Prints `2`.
    - Step 3: `c()`:
        - Calls `a()` $\rightarrow$ prints `1`.
        - Calls `b()` $\rightarrow$ calls `a()` (prints `1`), then prints `2`.
        - Prints `3`.
    - Step 4: Prints `4`.

**Final Output**:

```
11211234
```

---

Problem 8: Local Variable Shadowing vs. Global Modification

```
#include <stdio.h>

int f1(void);
int f2(void);
int f3(void);

int x = 10; // Global variable

int main() {
    int x = 1; // Local variable in main (shadows global x)
    x += f1() + f2() + f3() + f2();
    printf("%d", x);
    return 0;
}

int f1() { int x = 25; x++; return x; }
int f2() { int x = 50; x++; return x; }
int f3() { x *= 10; return x; }
```

- **Execution Trace**:
    1. `f1()`: Has local `x = 25`. Increments to 26 and returns **26**. (Global `x` remains 10).
    2. `f2()`: Has local `x = 50`. Increments to 51 and returns **51**. (Global `x` remains 10).
    3. `f3()`: Has no local `x`. Modifies global `x`: `x = 10 * 10 = 100`. Returns **100**.
    4. Second `f2()`: Has local `x = 50`. Increments to 51 and returns **51**.
    5. Back in `main()`: $$\text{local } x = 1 + (26 + 51 + 100 + 51) = 1 + 228 = 229$$

**Final Output**:

```
229
```

---

Problem 9: Nested Invocations with Loops and `continue`

```
#include <stdio.h>

int a = 10; // Global variable

int fun(int i) {
    while (i < 20) {
        i += 2;
        if (i % 2) continue;
        i += 3;
    }
    a += i;
    return a + i;
}

int main() {
    printf("%d", fun(fun(10)));
    return 0;
}
```

- **Execution Trace**:
    - **Inner Call:** **fun(10)**:
        - Iteration 1 ($i = 10 < 20$): `i += 2` $\rightarrow 12$. Since $12 \pmod 2 == 0$ (false), `i += 3` $\rightarrow 15$.
        - Iteration 2 ($i = 15 < 20$): `i += 2` $\rightarrow 17$. Since $17 \pmod 2 == 1$ (true), `continue` skips `i += 3`.
        - Iteration 3 ($i = 17 < 20$): `i += 2` $\rightarrow 19$. Since $19 \pmod 2 == 1$ (true), `continue` skips `i += 3`.
        - Iteration 4 ($i = 19 < 20$): `i += 2` $\rightarrow 21$. Since $21 \pmod 2 == 1$ (true), `continue` skips `i += 3`.
        - Iteration 5 ($i = 21 \not< 20$): Loop terminates.
        - Updates global `a`: `a = 10 + 21 = 31`.
        - Returns: $a + i = 31 + 21 = \mathbf{52}$.
    - **Outer Call:** **fun(52)**:
        - Loop condition ($52 < 20$) is false; loop does not run.
        - Updates global `a`: `a = 31 + 52 = 83`.
        - Returns: $a + i = 83 + 52 = \mathbf{135}$.

**Final Output**:

```
135
```

---

Problem 10: Right-to-Left Parameter Evaluation Order

```
#include <stdio.h>

int g(int p) { printf("%d", p); return p; }
int h(int q) { printf("%d", q); return q; }

void f(int x, int y) {
    g(x);
    h(y);
}

int main() {
    // Assume parameters to a function are evaluated from right to left
    f(g(10), h(20));
}
```

- **Execution Trace**:
    1. Evaluating parameters of `f(...)` from **right to left**:
        - 1st: Evaluate `h(20)` $\rightarrow$ prints `"20"` and returns `20`.
        - 2nd: Evaluate `g(10)` $\rightarrow$ prints `"10"` and returns `10`.
    2. `f(10, 20)` begins execution (`x = 10`, `y = 20`):
        - Calls `g(x)` $\rightarrow$ `g(10)` prints `"10"`.
        - Calls `h(y)` $\rightarrow$ `h(20)` prints `"20"`.
    3. Concatenating all printed values: `"20"` + `"10"` + `"10"` + `"20"`.

**Final Output**:

```
20101020
```

---

Problem 11: Post-Increment in Return Expression

```
#include <stdio.h>

int fun(int i) {
    return i++;
}

int main() {
    int sum = 0;
    for (int j = 0; j < 10; j++)
        sum += fun(j);
    printf("%d", sum);
    return 0;
}
```

- **Execution Trace**:
    1. `return i++;` returns the value of `i` **before** the increment operation takes place.
    2. For $j = 0, 1, 2, \dots, 9$, `fun(j)` returns $0, 1, 2, \dots, 9$.
    3. Summation: $$\text{sum} = \sum_{j=0}^{9} j = \frac{9 \times 10}{2} = 45$$

**Final Output**:

```
45
```

---

Problem 12: Persistent Global Variable Across Iterative Calls

```
#include <stdio.h>

int i; // Global variable (uninitialized globals default to 0)

int fun(int j) {
    return ++i + j;
}

int main() {
    int sum = 0;
    for (int j = 0; j < 10; j++)
        sum += fun(j);
    printf("%d", sum);
    return 0;
}
```

- **Execution Trace**:
    - Global `i = 0`.
    - Iterations:
        - $j = 0$: `++i` makes `i = 1` $\rightarrow$ returns $1 + 0 = 1$.
        - $j = 1$: `++i` makes `i = 2` $\rightarrow$ returns $2 + 1 = 3$.
        - $j = 2$: `++i` makes `i = 3` $\rightarrow$ returns $3 + 2 = 5$.
        - $\dots$
        - $j = 9$: `++i` makes `i = 10` $\rightarrow$ returns $10 + 9 = 19$.
    - Sum of the first 10 consecutive odd integers: $$\text{sum} = 1 + 3 + 5 + \dots + 19 = 10^2 = 100$$

**Final Output**:

```
100
```

---

Problem 13: Geometric Loop Stepping Inside Repeated Calls

```
#include <stdio.h>

int fun(int num) {
    int i, count = 0;
    for (i = 1; i <= num; i = i * 2) {
        count++;
    }
    return count;
}

int main() {
    int sum = 0;
    for (int i = 1; i <= 5; i++)
        sum = sum + fun(100 + i * 10);
    printf("%d", sum);
    return 0;
}
```

- **Execution Trace**:
    - The loop in `fun(num)` doubles $i$: values produced are $1, 2, 4, 8, 16, 32, 64, 128, \dots$
    - Call evaluations for $i = 1$ to $5$:
        - `fun(110)`: Powers of 2 $\le 110$ are $\{1, 2, 4, 8, 16, 32, 64\}$ $\rightarrow$ **7 iterations**.
        - `fun(120)`: Powers of 2 $\le 120$ are $\{1, 2, 4, 8, 16, 32, 64\}$ $\rightarrow$ **7 iterations**.
        - `fun(130)`: Powers of 2 $\le 130$ include $128$ $\rightarrow$ **8 iterations**.
        - `fun(140)`: Powers of 2 $\le 140$ include $128$ $\rightarrow$ **8 iterations**.
        - `fun(150)`: Powers of 2 $\le 150$ include $128$ $\rightarrow$ **8 iterations**.
    - Total sum: $$\text{sum} = 7 + 7 + 8 + 8 + 8 = 38$$

**Final Output**:

```
38
```