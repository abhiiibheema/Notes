## 1. Master table: Sorting algorithms

|Algorithm|Best time|Average time|Worst time|Stable?|
|---|---|---|---|---|
|Bubble Sort*|\(O(n)\)|\(O(n^2)\)|\(O(n^2)\)|Yes|
|Insertion Sort|\(O(n)\)|\(O(n^2)\)|\(O(n^2)\)|Yes|
|Selection Sort|\(O(n^2)\)|\(O(n^2)\)|\(O(n^2)\)|No|
|Merge Sort|\(O(n\log n)\)|\(O(n\log n)\)|\(O(n\log n)\)|Yes|
|Quick Sort|\(O(n\log n)\)|\(O(n\log n)\)|\(O(n^2)\)|No|
|Heap Sort|\(O(n\log n)\)|\(O(n\log n)\)|\(O(n\log n)\)|No|
|Counting Sort|\(O(n+k)\)|\(O(n+k)\)|\(O(n+k)\)|Yes**|
|Radix Sort|\(O(d(n+k))\)|\(O(d(n+k))\)|\(O(d(n+k))\)|Yes**|
|Bucket Sort|\(O(n+k)\)|\(O(n+k)\) expected|\(O(n^2)\)|Depends|

*Bubble Sort's best case assumes the optimized early-termination flag.

**Counting Sort and Radix Sort are stable when implemented with the standard stable versions.

## 2. The important table: Comparisons and swaps

Here is what GATE frequently tests.

|Algorithm|Comparisons|Swaps / Data movement|
|---|---|---|
|Bubble|Best \(n-1\), worst \(n(n-1)/2\)|Adjacent swaps; equal to inversions|
|Insertion|Best \(n-1\), worst \(n(n-1)/2\)|Shifts elements; equal to inversions|
|Selection|Always \(n(n-1)/2\)|At most \(n-1\) actual swaps|
|Merge|\(O(n\log n)\)|Merging and copying; not ordinary swaps|
|Quick|Average \(O(n\log n)\), worst \(O(n^2)\)|Depends on partition scheme|
|Heap|\(O(n\log n)\)|Heap swaps, up to \(O(n\log n)\)|
|Counting|No key comparisons|Counting and output writes|
|Radix|No key comparisons between complete values|Digit-wise counting and output writes|
|Bucket|Depends on internal sorting|Depends on bucket sorting|

Important: Insertion Sort conventionally shifts elements rather than swapping them. An adjacent-swap implementation is also possible.

## 3. Sorted vs reverse-sorted array

This is particularly important for your question.

Assume all elements are distinct.

|Algorithm|Sorted array: comparisons|Sorted array: actual swaps/shifts|Reverse array: comparisons|Reverse array: swaps/shifts|
|---|---|---|---|---|
|Bubble*|\(n-1\)|0|\(n(n-1)/2\)|\(n(n-1)/2\)|
|Insertion|\(n-1\)|0|\(n(n-1)/2\)|\(n(n-1)/2\) shifts|
|Selection|\(n(n-1)/2\)|0|\(n(n-1)/2\)|\(\lfloor n/2\rfloor\) actual swaps|

For Selection Sort, an implementation can execute \(n-1\) swap statements, but some may be self-swaps. The table counts only actual data-changing swaps.