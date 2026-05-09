# CS Basics
*305 cards · regenerated 2026-05-09 01:17 UTC from Anki via MCP · do not hand-edit*

## What is RAM?
*Basic · id 1755140815571*

<b>R</b>andom <b>A</b>ccess <b>M</b>emory - active memory for the computer. Stores data, usually measured in <b>Byte</b>s (8 bits), it holds <b>values</b> indexed by their <b>address (you can think of each address indexes a byte of data).
</b>

---

## <b>Describe basic properties of a Static Array</b>
*Basic · tags: Data::structures · id 1755141406186*

<ul><li>In statically typed languages arrays have to have an allocated size and type when initialized, known as <b>static arrays</b>, once a static array is full <b>it cannot store more values</b></li><li><b>Reading from an array: </b>Read by providing the index, <b>O(1) time complexity</b></li><li><b>Traversing an array:</b> Is linear in time <b>O(n) time complexity</b></li><li><b>Deleting an element:</b> Overwrite memory via "empty" value indicator <b>(soft delete)</b>, deleting from the middle makes the array non-contiguous, so we need to shift the values back and replace the last element with an EOA indicator, <b>worst case O(n) time complexity</b></li><li><b>Inserting an element:</b> Like deletion we need to perform a shift operation on all indices greater than the insertion index, <b>worst case O(n) time complexity</b>
</li></ul>

---

## <b>Describe basic properties of a Dynamic Array</b>
*Basic · tags: Data::structures · id 1755152863121*

<ul><li>You don't have to specify the size (initializes to some arbitrary size), <b>the dynamic array gets resized at runtime</b>
</li><li>As we add elements to the end of the array or <b>push </b>them onto the array, they resize, <b>O(1) time complexity</b>
</li><li>You can also remove or <b>pop the elements</b>, <b>O(1) time complexity</b></li><li><b>I</b>f the original size is exceeded <b>another array is allocated (somewhere else in memory), which is 2x the size</b> and the original values are recopied into the 2nd array</li><li>This results in an <b>amortized time-complexity is O(1) </b>for a single element (last term is dominating the computation), meaning that inserting <b>n</b> elements into a dynamic array is an <b>O(n)</b> operation.</li><li>Of course as in the static array O(n) for insertion and deletion <b>in the middle</b></li></ul>

---

## How do Stacks work?
*Basic · tags: Data::structures · id 1755153439594*

Basic operations:
<ul><li>push: <b>O(1)</b></li><li>pop: <b>O(1) </b>(need to check if empty first!)</li><li>peek/top (look at the last element): <b>O(1)</b></li></ul><b>
</b>
<b>A stack can be implemented via a dynamic array and it is a LIFO (last in first out) data structure!</b>

---

## Describe the basics of a singly linked list
*Basic · tags: Data::structures · id 1755227483148*

Data structure which holds elements in an ordered sequence:
<ul><li>Made out of <b>ListNode</b>s which contain two attributes: <b>(value, next)</b></li><li>When initializing a linked list we won't know where it is stored in RAM, likely not contiguous</li><li>To traverse start at the head of the list and use a while loop (until next -> None is reached)</li></ul>

Operations on a singly linked list:
<ol><li>Appending can be done in <b>O(1) </b>time complexity (<b>even when inserting in the middle</b>)</li><li>Deleting can also be done in <b>O(1)</b> time complexity</li><li>Accessing an element is <b>O(n) </b>because non-contigous in memory and traversal is needed</li></ol>

---

## What is a doubly linked list?
*Basic · id 1762587739359*

A doubly linked list is a linked list which links to both the previous and the next node in the chain. 

```
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None
        self.prev = None

# Implementation for Doubly Linked List
class LinkedList:
    def __init__(self):
        # Init the list with 'dummy' head and tail nodes which makes 
        # edge cases for insert & remove easier.
        self.head = ListNode(-1)
        self.tail = ListNode(-1)
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def insertFront(self, val):
        newNode = ListNode(val)
        newNode.prev = self.head
        newNode.next = self.head.next

        self.head.next.prev = newNode
        self.head.next = newNode

    def insertEnd(self, val):
        newNode = ListNode(val)
        newNode.next = self.tail
        newNode.prev = self.tail.prev

        self.tail.prev.next = newNode
        self.tail.prev = newNode

    # Remove first node after dummy head (assume it exists)
    def removeFront(self):
        self.head.next.next.prev = self.head
        self.head.next = self.head.next.next

    # Remove last node before dummy tail (assume it exists)
    def removeEnd(self):
        self.tail.prev.prev.next = self.tail
        self.tail.prev = self.tail.prev.prev

    def print(self):
        curr = self.head.next
        while curr != self.tail:
            print(curr.val, " -> ")
            curr = curr.next
        print()
```

<h2>Operations:</h2><table><tbody><tr><th>Operation</th><th>Big-O Time Complexity</th><th>Notes</th></tr><tr><td>Access</td><td><span style="color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">O(n)</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"></span><span style="font-style: italic; font-weight: inherit; color: inherit !important;">O</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">(</span><span style="font-style: italic; font-weight: inherit; color: inherit !important;">n</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">)</span></span></span></span></td><td></td></tr><tr><td>Search</td><td><span style="color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">O(n)</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"></span><span style="font-style: italic; font-weight: inherit; color: inherit !important;">O</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">(</span><span style="font-style: italic; font-weight: inherit; color: inherit !important;">n</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">)</span></span></span></span></td><td></td></tr><tr><td>Insertion</td><td><span style="color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">O(1)</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"></span><span style="font-style: italic; font-weight: inherit; color: inherit !important;">O</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">(</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">1</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">)</span></span></span></span>*</td><td>Assuming you have the reference to the node at the desired position</td></tr><tr><td>Deletion</td><td><span style="color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">O(1)</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"><span style="font-style: inherit; font-weight: inherit; color: inherit !important;"></span><span style="font-style: italic; font-weight: inherit; color: inherit !important;">O</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">(</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">1</span><span style="font-style: inherit; font-weight: inherit; color: inherit !important;">)</span></span></span></span>*</td><td>Assuming you have the reference to the node at the desired position</td></tr></tbody></table>

---

## Describe the properties of a queue
*Basic · id 1762909721468*

Queue is a data structure implementing a FIFO principle (first in first out). In its most basic form it supports enque and deque operations (add item to queue and take item out of queue). The easiest way to implement a queue is with a linked list as adding at head/tail and taking out at head/tail are both O(1) operations in a linked list.

---

## What are tail calls (recursion)?
*Basic · id 1763876892854*

<strong>Definition:</strong> A tail call occurs when a function's last action is calling another function and immediately returning that result

<strong>Example Comparison:</strong>
<em>Not a tail call:</em>
<pre><code><span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">factorial</span><span style="color: rgb(56, 58, 66);">(</span>n<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">if</span> n <span style="color: rgb(64, 120, 242);"><=</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(183, 107, 1);">1</span>
    <span style="color: rgb(166, 38, 164);">return</span> n <span style="color: rgb(64, 120, 242);">*</span> factorial<span style="color: rgb(56, 58, 66);">(</span>n <span style="color: rgb(64, 120, 242);">-</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Must multiply after return</span></code></pre>

<em>Tail call:</em>
<pre><code><span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">factorial</span><span style="color: rgb(56, 58, 66);">(</span>n<span style="color: rgb(56, 58, 66);">,</span> acc<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">if</span> n <span style="color: rgb(64, 120, 242);"><=</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(166, 38, 164);">return</span> acc
    <span style="color: rgb(166, 38, 164);">return</span> factorial<span style="color: rgb(56, 58, 66);">(</span>n <span style="color: rgb(64, 120, 242);">-</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> n <span style="color: rgb(64, 120, 242);">*</span> acc<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Just return the call result</span></code></pre>

<strong>Why It Matters:</strong> With Tail Call Optimization (TCO), the compiler reuses the current stack frame instead of creating a new one, effectively transforming recursion into a loop:
<ul><li>Without TCO: 1000 calls = 1000 stack frames = stack overflow risk</li><li>With TCO: 1000 calls = 1 reused frame = constant memory</li></ul><strong>Language Support:</strong>
<ul><li>✅ Guaranteed: Scheme, Scala, some functional languages</li><li>❌ Not supported: Python, Java, C#</li><li>⚠️ Spec'd but rarely implemented: JavaScript (ES6)</li></ul><strong>Key Insight:</strong> In most mainstream languages, tail recursion doesn't actually save you from stack overflow—you'd need to convert to iteration manually.

---

## How does insertion sort work, and what are its time/space complexities?
*Basic · id 1763877060292*

<strong>How It Works:</strong> Insertion sort builds a sorted array one element at a time by repeatedly taking the next unsorted element and inserting it into its correct position among the already-sorted elements.
Think of it like sorting playing cards in your handyou pick up cards one at a time and insert each into its proper position.

<strong>Algorithm Steps:</strong>
<ol><li>Start with the second element (index 1)</li><li>Compare it with elements to its left</li><li>Shift larger elements one position right</li><li>Insert the current element in the correct position</li><li>Repeat for all remaining elements</li></ol><strong>Example:</strong>

<pre><code>[5, 2, 4, 6, 1, 3]
[2, 5, 4, 6, 1, 3]  // Insert 2
[2, 4, 5, 6, 1, 3]  // Insert 4
[2, 4, 5, 6, 1, 3]  // 6 already in place
[1, 2, 4, 5, 6, 3]  // Insert 1
[1, 2, 3, 4, 5, 6]  // Insert 3</code></pre>

<strong>Time Complexity:</strong>
<ul><li>Best case: <strong>O(n)</strong> - when array is already sorted</li><li>Average case: <strong>O(n²)</strong> - random order</li><li>Worst case: <strong>O(n²)</strong> - when array is reverse sorted</li></ul><strong>Space Complexity:</strong> <strong>O(1)</strong> - sorts in place, only needs constant extra space

---

## How does merge sort work, and what are its time/space complexities?
*Basic · id 1763878006554*

<strong>How It Works:</strong> Merge sort is a divide-and-conquer algorithm that recursively splits the array in half until each piece has one element, then merges those pieces back together in sorted order.
<strong>Algorithm Steps:</strong>
<ol><li><strong>Divide:</strong> Split the array into two halves</li><li><strong>Conquer:</strong> Recursively sort each half</li><li><strong>Combine:</strong> Merge the two sorted halves into one sorted array</li></ol><strong>Example:</strong>

<pre><code>[38, 27, 43, 3, 9, 82, 10]

Split:
[38, 27, 43, 3] | [9, 82, 10]
[38, 27] [43, 3] | [9, 82] [10]
[38] [27] [43] [3] | [9] [82] [10]

Merge:
[27, 38] [3, 43] | [9, 82] [10]
[3, 27, 38, 43] | [9, 10, 82]
[3, 9, 10, 27, 38, 43, 82]</code></pre>

<strong>The Merge Process:</strong> Compare the first elements of each sorted half, take the smaller one, repeat until both halves are exhausted.

<strong>Time Complexity:</strong>
<ul><li>Best case: <strong>O(n log n)</strong></li><li>Average case: <strong>O(n log n)</strong></li><li>Worst case: <strong>O(n log n)</strong></li></ul>All cases are the same! Always divides into log n levels, and merging at each level takes O(n) time.
<strong>Space Complexity:</strong> <strong>O(n)</strong> - requires additional arrays for merging (not in-place)
<strong>
</strong>
<strong>When to Use:</strong>
<ul><li>Large datasets where consistent O(n log n) performance is needed</li><li>When stability is required</li><li>Linked lists (can be done in O(1) space)</li><li>External sorting (data doesn't fit in memory)</li><li>When worst-case guarantees matter</li></ul><strong>Key Properties:</strong>
<ul><li>✅ Stable (maintains relative order of equal elements)</li><li>✅ Predictable performance (always O(n log n))</li><li>✅ Parallelizable (each half can be sorted independently)</li><li>❌ Not in-place (requires O(n) extra space)</li><li>❌ Slower than quicksort in practice due to memory overhead</li></ul><strong>Comparison to Other Sorts:</strong>
<ul><li>More consistent than quicksort (no O(n²) worst case)</li><li>Much faster than insertion sort on large data</li><li>Uses more memory than quicksort or heap sort</li></ul>

---

## How does quicksort work, and what are its time/space complexities?
*Basic · id 1763878470326*

<strong>How It Works:</strong> Quicksort is a divide-and-conquer algorithm that picks a "pivot" element, partitions the array so all elements smaller than the pivot are on the left and larger ones on the right, then recursively sorts each partition.

<strong>Algorithm Steps:</strong>
<ol><li><strong>Choose a pivot</strong> (various strategies: first, last, random, median-of-three)</li><li><strong>Partition:</strong> Rearrange array so elements < pivot are left, elements > pivot are right</li><li><strong>Recursively sort</strong> the left partition</li><li><strong>Recursively sort</strong> the right partition</li></ol><strong>Example:</strong>

<pre><code>[10, 7, 8, 9, 1, 5]  // Pick pivot = 5

Partition around 5:
[1, 5, 8, 9, 10, 7]  // 1 left of pivot, rest right

Recursively sort left: [1]  // Already sorted
Recursively sort right: [8, 9, 10, 7]
  Pivot = 7
  [7, 9, 10, 8]
  Left: [] Right: [9, 10, 8]
    Pivot = 8
    [8, 10, 9]
    Continue...

Final: [1, 5, 7, 8, 9, 10]</code></pre>

<strong>Time Complexity:</strong>
<ul><li>Best case: <strong>O(n log n)</strong> - pivot always splits evenly</li><li>Average case: <strong>O(n log n)</strong> - random pivot choices</li><li>Worst case: <strong>O(n²)</strong> - pivot is always smallest/largest (already sorted array with bad pivot choice)</li></ul><strong>Space Complexity:</strong>
<ul><li><strong>O(log n)</strong> - recursion stack depth for balanced partitions</li><li><strong>O(n)</strong> worst case - if partitions are extremely unbalanced</li></ul><strong>When to Use:</strong>
<ul><li>Large datasets where average-case performance matters</li><li>When memory is limited (in-place sorting)</li><li>General-purpose sorting (often the default in standard libraries)</li><li>Random or unsorted data</li></ul><strong>Key Properties:</strong>
<ul><li>✅ In-place (uses minimal extra memory)</li><li>✅ Cache-efficient (good locality of reference)</li><li>✅ Fast in practice (lower constant factors than merge sort)</li><li>❌ Not stable (may change relative order of equal elements)</li><li>❌ Worst-case O(n²) can be triggered by poor pivot selection</li></ul><strong>Optimizations:</strong>
<ul><li><strong>Randomized pivot:</strong> Avoid worst-case on sorted data</li><li><strong>Median-of-three:</strong> Pick median of first, middle, last elements</li><li><strong>Hybrid approach:</strong> Switch to insertion sort for small subarrays (~10 elements)</li><li><strong>Three-way partitioning:</strong> Handle many duplicate values efficiently</li></ul><strong>Comparison to Other Sorts:</strong>
<ul><li>Typically faster than merge sort in practice (better cache performance, in-place)</li><li>Less predictable than merge sort (can degrade to O(n²))</li><li>Much faster than insertion sort on large datasets</li></ul>

---

## How does bucket sort work, and what are its time/space complexities?
*Basic · id 1763879284670*

<strong>How It Works:</strong> Bucket sort distributes elements into a number of "buckets," sorts each bucket individually (often using another algorithm), then concatenates the sorted buckets. It works best when input is uniformly distributed across a range.

<strong>Algorithm Steps:</strong>
<ol><li><strong>Create buckets:</strong> Set up empty buckets to cover the range of values</li><li><strong>Distribute:</strong> Place each element into its appropriate bucket</li><li><strong>Sort buckets:</strong> Sort each bucket individually (insertion sort, quicksort, etc.)</li><li><strong>Concatenate:</strong> Combine all buckets in order</li></ol><strong>Example:</strong>

<pre><code>Array: [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]
Range: 0.0 to 1.0, 5 buckets

Bucket 0 [0.0-0.2): [0.17, 0.12]
Bucket 1 [0.2-0.4): [0.39, 0.26, 0.21, 0.23]
Bucket 2 [0.4-0.6): []
Bucket 3 [0.6-0.8): [0.78, 0.72, 0.68]
Bucket 4 [0.8-1.0): [0.94]

Sort each bucket:
Bucket 0: [0.12, 0.17]
Bucket 1: [0.21, 0.23, 0.26, 0.39]
Bucket 2: []
Bucket 3: [0.68, 0.72, 0.78]
Bucket 4: [0.94]

Result: [0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]</code></pre>

<strong>Time Complexity:</strong>
<ul><li>Best case: <strong>O(n + k)</strong> - uniformly distributed data, k = number of buckets</li><li>Average case: <strong>O(n + k)</strong> - when elements distribute evenly</li><li>Worst case: <strong>O(n²)</strong> - all elements go into one bucket (degenerates to sorting algorithm used within buckets)</li></ul><strong>Space Complexity:</strong> <strong>O(n + k)</strong> - need space for n elements plus k buckets
<strong>
</strong>
<strong>When to Use:</strong>
<ul><li>Data is uniformly distributed over a known range</li><li>Floating-point numbers in a bounded range</li><li>When linear time is needed and data distribution is favorable</li><li>External sorting of large datasets</li></ul><strong>Key Properties:</strong>
<ul><li>✅ Can achieve O(n) average case with proper distribution</li><li>✅ Stable if the bucket sorting algorithm is stable</li><li>✅ Good for parallel processing (buckets can be sorted independently)</li><li>❌ Performance heavily depends on data distribution</li><li>❌ Requires knowledge of input range</li><li>❌ High space overhead</li></ul><strong>Important Considerations:</strong>
<ul><li><strong>Bucket count:</strong> Too few buckets → many elements per bucket (slower). Too many buckets → high memory overhead</li><li><strong>Distribution matters:</strong> Works best with uniform distribution; terrible with skewed data</li><li><strong>Bucket sorting algorithm:</strong> Typically use insertion sort for small buckets, quicksort for larger ones</li></ul><strong>Comparison to Other Sorts:</strong>
<ul><li>Not comparison-based (can break O(n log n) barrier)</li><li>Related to radix sort and counting sort</li><li>Much faster than O(n log n) sorts when conditions are right</li><li>Unreliable performance with poor data distribution</li></ul>

---

## How does radix sort work, and what are its time/space complexities?
*Basic · id 1763879973979*

<strong>How It Works:</strong> Radix sort processes integers by sorting them digit by digit, starting from the least significant digit (LSD) to the most significant digit (MSD), or vice versa. It uses a stable sorting algorithm (typically counting sort) as a subroutine for each digit position.
<strong>Algorithm Steps (LSD approach):</strong>
<ol><li><strong>Find the maximum number</strong> to determine number of digits</li><li><strong>For each digit position</strong> (from rightmost to leftmost):<ul><li>Use a stable sort (counting sort) to sort by that digit</li><li>Maintain relative order of elements with same digit value</li></ul></li><li>After processing all digits, array is fully sorted</li></ol><strong>Example:</strong>

<pre><code>Array: [170, 45, 75, 90, 802, 24, 2, 66]

Sort by ones place:
[170, 90, 802, 2, 24, 45, 75, 66]

Sort by tens place:
[802, 2, 24, 45, 66, 170, 75, 90]

Sort by hundreds place:
[2, 24, 45, 66, 75, 90, 170, 802]

Final: [2, 24, 45, 66, 75, 90, 170, 802]</code></pre>

<strong>Time Complexity:</strong>
<ul><li>Best case: <strong>O(d × (n + k))</strong></li><li>Average case: <strong>O(d × (n + k))</strong></li><li>Worst case: <strong>O(d × (n + k))</strong></li></ul>Where:
<ul><li>d = number of digits in the largest number</li><li>n = number of elements</li><li>k = range of each digit (typically 10 for decimal)</li></ul>Simplified: <strong>O(d × n)</strong> when k is constant
<strong>Space Complexity:</strong> <strong>O(n + k)</strong> - for the counting sort subroutine and output array
<strong>When to Use:</strong>
<ul><li>Sorting integers with a limited number of digits</li><li>Fixed-length strings or records</li><li>When the number of digits (d) is small relative to n</li><li>Need stable sorting with linear time</li></ul><strong>Key Properties:</strong>
<ul><li>✅ Stable (maintains relative order of equal elements)</li><li>✅ Not comparison-based (can break O(n log n) barrier)</li><li>✅ Predictable performance</li><li>✅ Works well with integers, strings, fixed-length keys</li><li>❌ Limited to data that can be represented as digits/keys</li><li>❌ Not in-place (requires extra space)</li><li>❌ Performance degrades if d is large</li></ul><strong>LSD vs MSD:</strong>
<ul><li><strong>LSD (Least Significant Digit first):</strong> Simpler, processes all digits, naturally stable</li><li><strong>MSD (Most Significant Digit first):</strong> Can terminate early for variable-length data, requires recursion, more complex</li></ul><strong>Important Considerations:</strong>
<ul><li><strong>Digit range matters:</strong> Binary (base-2) requires more passes but simpler counting; higher bases reduce passes but increase space</li><li><strong>Not suitable for:</strong> Floating-point numbers, negative numbers (without modification), variable-length unbounded data</li><li><strong>Common modifications:</strong> Handle negative numbers by sorting negatives and positives separately, then combining</li></ul><strong>Comparison to Other Sorts:</strong>
<ul><li>Faster than O(n log n) comparison sorts when d is small</li><li>Related to bucket sort and counting sort</li><li>More predictable than bucket sort (doesn't depend on distribution)</li><li>Better than counting sort for large ranges (only processes digits, not full range)</li></ul>

---

## What is a disk and what are its key characteristics?
*Basic · id 1764126021882*

A disk is the primary storage device in a computer. It is persistent (data remains regardless of machine state), typically stores data on the order of TBs (terabytes)

---

## What is RAM and how does it differ from disk storage?
*Basic · id 1764126041424*

RAM (Random Access Memory) is volatile memory used for storing information. It's typically smaller (1GB-128GB) and more expensive than disk, but reading/writing is significantly faster (microseconds vs milliseconds). Data is erased when the computer shuts down.

---

## How fast is RAM compared to disk? Give specific examples.
*Basic · id 1764126070708*

Writing 1 MB to RAM takes microseconds (1/10⁶ seconds), while writing the same amount to disk takes milliseconds (1/10³ seconds) - making RAM roughly 1000x faster.

---

## What does volatile memory mean?
*Basic · id 1764126084996*

Volatile memory means the data gets erased once the computer is shut down. This is why you need to save your work to disk before shutting down.

---

## What is the CPU and what is its primary role?
*Basic · id 1764126103128*

The CPU (Central Processing Unit) is the "brain" of the computer. It acts as an intermediary between RAM and disk, reading/writing from both. It fetches instructions from RAM, decodes them, executes them, and performs all computations (addition, subtraction, multiplication, etc.).

---

## What is (the CPU) cache and why is it important?
*Basic · id 1764126138098*

Cache is extremely fast memory that lies on the same die as the CPU. It stores data on the order of KBs or tens of MBs. It's checked before RAM and disk during read operations, making data access much faster when the requested data is cached.

---

## What are the typical cache levels in a CPU?
*Basic · id 1764126147030*

Most CPUs have L1, L2, and L3 cache levels, which are physical components much faster than RAM.

---

## What type of memory is cache, and why is it faster?
*Basic · id 1764126174696*

Cache is SRAM (Static RAM). It's faster than regular RAM both because it's part of the CPU and because of its underlying technology.

---

## What is the storage capacity hierarchy from largest to smallest? (CPU, RAM, Disk)
*Basic · id 1764126206081*

<ol><li>Disk (largest - TBs)</li><li>RAM (medium - GBs)</li><li>Cache (smallest - KBs to MBs)</li></ol>

---

## What is a byte, bit, and their relationship?
*Basic · id 1764126217703*

A bit is the smallest unit of measure in computers - a binary measure represented by 0 or 1. A byte is made up of 8 bits.

---

## Define these storage units: KB, MB, GB, TB
*Basic · id 1764126232538*

<ul><li>KB (kilobyte) = 10³ bytes (thousand)</li><li>MB (megabyte) = 10⁶ bytes (million)</li><li>GB (gigabyte) = 10⁹ bytes (billion)</li><li>TB (terabyte) = 10¹² bytes (trillion)</li></ul>

---

## What happens when you write code and run it in terms of computer architecture?
*Basic · id 1764126254510*

Your code is translated into binary instructions stored in RAM. The CPU reads and executes these instructions, which may involve manipulating data stored elsewhere in RAM or on disk (like opening and reading a file).

---

## What is vertical scaling and what are its limitations?
*Basic · id 1764127013976*

Vertical scaling means upgrading components within the same computer (adding more RAM, upgrading CPU cores/speed). Limitations include: every computer has a maximum upgrade capacity, it's less powerful for large systems, and it creates a single point of failure (if the server goes down, everything stops).

---

## What is horizontal scaling and why is it preferred for large systems despite being more complex
*Basic · id 1764127031960*

Horizontal scaling means running multiple servers and distributing user requests among them. It's preferred because: it's more powerful, uses commodity (inexpensive) hardware, prevents single points of failure, and allows unlimited growth. However, it requires significant engineering effort to ensure servers communicate properly and requests are distributed evenly.

---

## When might vertical scaling be the better choice over horizontal scaling?
*Basic · id 1764127105246*

<strong></strong>A: Vertical scaling is better when:
<ul><li>The application is simple with predictable, moderate traffic</li><li>Engineering resources are limited (horizontal requires significant effort for server communication and load distribution)</li><li>The maximum capacity of a single upgraded machine is sufficient for your needs</li><li>You want faster implementation without the complexity of distributed systems</li><li>The cost of engineering effort outweighs the cost of better hardware</li></ul>

---

## What is a load balancer and why is it necessary in horizontally scaled systems?
*Basic · id 1764127129119*

A load balancer determines which requests go to which server in a multi-server environment. It evenly distributes incoming requests across a group of servers, preventing any single server from becoming overwhelmed and ensuring consistent performance across the system.

---

## What is the difference between logging services and metrics services, and why do we need both?
*Basic · id 1764127150302*

<ul><li><strong>Logging</strong>: Records activity logs (what happened, errors, events before crashes) - tells you <em>what</em> occurred</li><li><strong>Metrics</strong>: Collects quantitative data (CPU usage, RAM usage, network traffic) - tells you <em>how well</em> the system is performing</li></ul>Both are needed because logs don't show resource bottlenecks, and metrics don't show specific events or errors. Logs are commonly written to external servers for better reliability.

---

## Give an example of when you'd need metrics over logs to diagnose a problem.
*Basic · id 1764127166690*

If your server is slow but not crashing or throwing errors, logs won't reveal the issue. Metrics would show that RAM has become a bottleneck or CPU resources are maxed out, identifying the performance constraint that logs wouldn't capture.

---

## Why is external storage (database, cloud) preferred over built-in server storage?
*Basic · id 1764127240004*

Built-in server storage has limitations in terms of size and creates a single point of failure. External storage systems connected through a network provide:
<ul><li>Greater capacity and scalability</li><li>Better reliability (data survives if server fails)</li><li><strong>Statelessness</strong>: Servers can remain stateless, meaning any server can handle any request since all persistent data lives externally</li><li><strong>Shared access</strong>: Multiple servers in horizontally scaled systems can access the same data, enabling load balancing without data inconsistency</li><li>If a server crashes or is replaced, no data is lost since state is externalized</li></ul>

---

## <strong>What is vertical scaling and what are its limitations?</strong>
*Basic · id 1764128038974*

Vertical scaling means upgrading components within the same computer (adding more RAM, upgrading CPU cores/speed). Limitations include: every computer has a maximum upgrade capacity, it's less powerful for large systems, and it creates a single point of failure (if the server goes down, everything stops).

---

## What is horizontal scaling and why is it preferred for large systems despite being more complex?
*Basic · id 1764128049922*

Horizontal scaling means running multiple servers and distributing user requests among them. It's preferred because: it's more powerful, uses commodity (inexpensive) hardware, prevents single points of failure, and allows unlimited growth. However, it requires significant engineering effort to ensure servers communicate properly and requests are distributed evenly.

---

## What are the three fundamental operations in system design, and why are they more challenging at scale?
*Basic · id 1764128082766*

<ol><li><strong>Moving Data</strong>: Unlike moving data between disk/RAM/CPU within one machine, distributed systems move data between clients and servers that may be geographically dispersed worldwide</li><li><strong>Storing Data</strong>: Choosing optimal storage (databases, blob stores, file systems, distributed file systems) based on use case - like choosing the right data structure</li><li><strong>Transforming Data</strong>: Processing data efficiently (e.g., calculating success/failure rates from logs, filtering medical records by age)</li></ol>The challenge at scale is that these operations involve network latency, coordination, and complexity that don't exist in single-machine systems.

---

## What is availability, how is it calculated, and why is 99% availability considered poor?
*Basic · id 1764128109279*

Availability = uptime / (uptime + downtime) - the percentage of time a system is operational.
99% availability means 3.65 days of downtime per year (365 × 0.01). For a global business, this translates to:
<ul><li>Lost revenue during outages</li><li>Users unable to access services</li><li>Potential damage to reputation</li><li>SLA violations</li></ul>This is why companies target 99.9% (8.76 hours downtime/year) or higher, with mission-critical systems aiming for 99.999% ("five nines" = 5.26 minutes downtime/year).

---

## What is the significance of measuring availability in "nines" and what does "five nines" mean?
*Basic · id 1764128121706*

Availability is measured in consecutive 9s (99%, 99.9%, 99.99%, etc.). Each additional nine reduces downtime by a factor of 10:
<ul><li>99% = 3.65 days downtime/year</li><li>99.9% = 8.76 hours downtime/year</li><li>99.99% = 52.56 minutes downtime/year</li><li>99.999% ("five nines") = 5.26 minutes downtime/year</li></ul>"Five nines" is considered the gold standard for mission-critical systems, though it's extremely difficult and expensive to achieve.

---

## What are planned vs. unplanned downtime, and why can't we achieve 100% availability?
*Basic · id 1764128137813*

<ul><li><strong>Planned downtime</strong>: Scheduled maintenance, software updates, backups, verification</li><li><strong>Unplanned downtime</strong>: Hardware/software failures, natural disasters, cyberattacks, network issues</li></ul>100% availability is impossible because unplanned downtime cannot be completely eliminated - hardware fails, disasters occur, and unforeseen issues arise. The best we can do is minimize probability and impact through redundancy, monitoring, and fault tolerance.

---

## SLA vs SLO
*Basic · id 1764128157599*

<strong>What is the difference between SLA and SLO, and how do they relate? Provide an example.</strong> A:
<ul><li><strong>SLA (Service Level Agreement)</strong>: A contractual agreement between a company and clients guaranteeing specific uptime, responsiveness, and responsibilities. Violations may result in refunds or penalties.</li><li><strong>SLO (Service Level Objective)</strong>: Internal objectives/targets your team must hit to meet the SLA.</li></ul>Example: AWS has a monthly SLA of 99.99% uptime. If they fail to meet this, they refund a percentage of service credit. AWS engineering teams would have internal SLOs (perhaps 99.995%) to ensure they stay above the SLA threshold with a buffer.

---

## What is the difference between availability and reliability?
*Basic · id 1764128173213*

<ul><li><strong>Availability</strong>: Whether the system is up and accessible (are the servers running?)</li><li><strong>Reliability</strong>: Whether the system performs correctly without failures when it IS available (do requests succeed? Does it handle load? Does it resist attacks?)</li></ul>A system can be highly available (servers are up) but unreliable (requests fail, errors occur, can't handle load). Both are needed for a robust system.

---

## What is fault tolerance and how does it differ from redundancy?
*Basic · id 1764128196169*

<ul><li><strong>Fault Tolerance</strong>: The system's ability to detect and heal itself from failures - disabling faulty functions, reverting to different modes, or switching to backup servers</li><li><strong>Redundancy</strong>: Having backup/additional components (servers, databases, network paths) that enable fault tolerance</li></ul><strong>Relationship</strong>: Redundancy is the <em>mechanism</em> that enables fault tolerance. You can't have fault tolerance without redundancy, but redundancy alone doesn't guarantee proper fault handling.

---

## What is the difference between passive (backup) redundancy and active-active redundancy?
*Basic · id 1764128220082*

<ul><li><strong>Passive/Backup Redundancy</strong>: A backup server "shadows" the primary server but remains inactive until the primary fails. The backup is unessential during normal operation.</li><li><strong>Active-Active Redundancy</strong>: Multiple servers are all actively handling requests simultaneously, sharing the load. If one fails, others continue operating.</li></ul>Active-active is preferred in modern systems because it improves both availability AND throughput. When documentation mentions "redundancy," it typically means active-active unless specified otherwise.

---

## Why isn't redundancy a perfect solution to DDoS attacks?
*Basic · id 1764128258885*

During a DDoS attack, the primary server gets overwhelmed with requests, and legitimate users can't get responses. When the backup server takes over, it will ALSO become overloaded with the same attack traffic. Redundancy helps with hardware failures and normal load distribution, but doesn't solve the fundamental problem of malicious traffic overwhelming capacity. Proper DDoS mitigation requires additional strategies like rate limiting, traffic filtering, and CDNs.

---

## What is throughput and what are three common ways to measure it?
*Basic · id 1764128269733*

Throughput is the amount of data or operations a system can handle over a period of time. Three measurements:
<ol><li><strong>Requests/second</strong>: Number of user requests handled (user-centric metric)</li><li><strong>Queries/second</strong>: Number of database queries executed (database-centric metric)</li><li><strong>Bytes/second</strong>: Maximum data transmitted over network (bandwidth-centric metric, useful for data pipelines processing large volumes of non-user data)</li></ol>The appropriate metric depends on context - use requests/second for user-facing services, bytes/second for data processing pipelines.

---

## How do vertical and horizontal scaling each improve throughput differently?
*Basic · id 1764128326077*

<ul><li><strong>Vertical Scaling</strong>: A single more powerful server can process each request faster and handle more concurrent requests due to increased CPU/RAM. But there's a ceiling - one machine has physical limits.</li><li><strong>Horizontal Scaling</strong>: Multiple servers can process requests in parallel. Total throughput = (throughput per server) × (number of servers). Theoretically unlimited scaling, though coordination overhead increases.</li></ul>For high throughput systems, horizontal scaling is preferred despite added complexity.

---

## What is latency and how does it differ from throughput? Can you have high throughput but high latency?
*Basic · id 1764128362139*

<ul><li><strong>Latency</strong>: Time delay for a SINGLE request to complete (client sends → server responds)</li><li><strong>Throughput</strong>: Number of requests completed per unit time</li></ul><strong>Yes, you can have high throughput with high latency</strong>. Example: A server processes 1,000 requests/second (high throughput) but each request takes 5 seconds to complete (high latency). This happens when many requests are processed in parallel but each takes a long time individually.
Analogy: A highway with 10 lanes (high throughput - many cars pass per minute) but a 50 mph speed limit (high latency - each car takes longer to reach destination).

---

## What is a CDN (Content Delivery Network) and how does it improve system performance and security?
*Basic · id 1764128369485*

A CDN is a geographically distributed network of servers that cache and deliver content from locations closer to users.
<strong>Performance benefits:</strong>
<ul><li>Reduced latency (users connect to nearby servers instead of distant origin servers)</li><li>Reduced load on origin servers (CDN handles static content like images, videos, CSS, JavaScript)</li><li>Better throughput for content delivery</li></ul><strong>Security benefits:</strong>
<ul><li>DDoS protection: CDNs have massive capacity to absorb attack traffic before it reaches your servers</li><li>Traffic filtering: CDN can identify and block malicious requests</li><li>Geographic distribution: Attacks get spread across CDN's global infrastructure instead of hitting your single server</li></ul><strong>Example</strong>: Cloudflare, AWS CloudFront, Akamai act as a protective layer between users and your servers, serving cached content and filtering threats.

---

## Where does latency exist beyond network communications?
*Basic · id 1764128391370*

Latency exists within computer components themselves:
<ul><li>CPU fetching from cache vs RAM vs disk</li><li>RAM read/write operations</li><li>Disk seek times</li><li>Cache lookups</li></ul>Network latency (milliseconds to seconds) is typically orders of magnitude higher than internal component latency (nanoseconds to milliseconds), which is why distributed systems are more challenging than single-machine programs.

---

## What is an IP address and what problem did IPv6 solve that IPv4 created?
*Basic · id 1764129391694*

An IP address is a unique numeric identifier for every device connected to a network.
<strong>IPv4 (32-bit):</strong>
<ul><li>Format: 0.0.0.0 to 255.255.255.255 (four octets, each 8 bits)</li><li>Theoretically allows ~4.3 billion unique addresses</li><li>Problem: Due to design choices and exponential internet growth, actual usable addresses are much lower, leading to address scarcity</li></ul><strong>IPv6 (128-bit):</strong>
<ul><li>Format: xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx (8 groups of 16 bits in hexadecimal)</li><li>Provides virtually infinite addresses, making address exhaustion highly improbable</li><li>Created specifically to solve IPv4's scarcity problem</li></ul>

---

## What is a data packet and what are its three main components?
*Basic · id 1764129435748*

A data packet is the fundamental unit of data transmitted over a network. It consists of:
<ol><li><strong>Header</strong>: Contains metadata including source and destination IP addresses, protocol information (called the IP Header)</li><li><strong>Data/Payload</strong>: The actual content being transmitted (e.g., HTTP request/response, application data)</li><li><strong>Trailer</strong>: Contains error-checking information and end-of-packet markers</li></ol>Analogy: Like an envelope (header with addresses) containing a letter (payload) with a seal (trailer). Large data is typically divided into multiple packets that may arrive out of order.

---

## What is the Internet Protocol (IP) and what is its primary responsibility?
*Basic · id 1764129480893*

IP (Internet Protocol) is the fundamental protocol that governs how data packets are transmitted over the internet.
<strong>Primary responsibilities:</strong>
<ul><li>Routing packets from source to destination using IP addresses</li><li>Defining the structure of the IP header (source/destination addresses, packet size, etc.)</li><li>Operating at the network layer to set the physical pathway for data transmission</li></ul><strong>Important limitation</strong>: IP does NOT guarantee packets arrive in order or even that they arrive at all - it's "best effort" delivery. This is why we need TCP on top of IP.

---

## What is TCP (Transmission Control Protocol) and how does it differ from IP? Why do we need both?
*Basic · id 1764129517101*

<strong>TCP</strong> ensures reliable, ordered delivery of data packets. <strong>IP</strong> handles routing packets to the correct destination.
<strong>TCP responsibilities:</strong>
<ul><li>Assigns sequence numbers to packets so they can be reassembled in correct order at destination</li><li>Confirms packets were received (acknowledgments)</li><li>Retransmits lost packets</li><li>Operates at the transport layer</li></ul><strong>Why we need both (TCP/IP together):</strong>
<ul><li><strong>IP alone</strong>: Gets packets to the right place, but they might arrive out of order, duplicated, or not at all</li><li><strong>TCP alone</strong>: Can't route packets without IP addresses</li><li><strong>Together</strong>: IP routes packets to destination, TCP ensures they arrive correctly and in order</li></ul>Analogy: IP is like the postal service getting mail to an address. TCP is like numbering pages of a letter and asking for confirmation they arrived, then resending missing pages.

---

## What is the structure of network protocols in layers, and where do IP, TCP, and HTTP sit?
*Basic · id 1764130025528*

Network protocols are organized into hierarchical layers, each with specific responsibilities:
<strong>Network Layer (IP):</strong>
<ul><li>Sets the physical pathway for data transmission</li><li>Routes packets using IP addresses</li></ul><strong>Transport Layer (TCP):</strong>
<ul><li>Ensures reliable, ordered transmission of data</li><li>Handles packet sequencing and retransmission</li></ul><strong>Application Layer (HTTP):</strong>
<ul><li>Enables interaction between clients and servers</li><li>Where application-specific protocols operate (HTTP, FTP, SMTP, etc.)</li></ul><strong>Layer interaction</strong>: Each layer communicates with adjacent layers. HTTP data becomes TCP payload, TCP packet becomes IP payload. As developers, we primarily work with the application layer, treating lower layers as abstractions.

---

## As a developer, which part of the packet should you care about most, and how does HTTP data fit into TCP/IP packets?
*Basic · id 1764130059800*

Developers primarily care about the <strong>application data</strong> portion of the packet.
<strong>Packet nesting structure:</strong>

<pre><code>IP Packet
└── IP Header (source/dest IP, routing info)
    └── TCP Packet (IP payload)
        └── TCP Header (sequence numbers, ports)
            └── Application Data (TCP payload)
                └── HTTP Header (method, path, status)
                    └── HTTP Payload (your actual data)</code></pre>

<strong>Examples of application data:</strong>
<ul><li>HTTP POST request: Your data is in the HTTP payload section</li><li>HTTP GET request: The response data is in the HTTP payload section</li></ul><strong>Key insight</strong>: You can treat IP and TCP headers as a "black box" - the networking stack handles them automatically. You work with HTTP requests/responses, which are encapsulated within TCP, which is encapsulated within IP.

---

## What is the difference between public and private IP addresses, and when is each used?
*Basic · id 1764130100321*

<strong>Public IP Address:</strong>
<ul><li>Unique identifier supplied by ISP (Internet Service Provider)</li><li>Reachable from any device on the internet</li><li>Used for communication between devices on separate networks</li><li>Example: Your home router's external address, web servers</li></ul><strong>Private IP Address:</strong>
<ul><li>Used within a confined network (home, office LAN - Local Area Network)</li><li>NOT accessible from the broader internet</li><li>Multiple devices across different networks can have the same private IP without conflict</li><li>Common ranges: 192.168.x.x, 10.x.x.x, 172.16.x.x</li></ul><strong>Key difference</strong>: Public IPs have global accessibility; private IPs only work within their local network. This is why your laptop can have IP 192.168.1.5 at home and someone else's laptop can have the same IP - they're in different private networks.

---

## What is the difference between static and dynamic IP addresses, and why are dynamic IPs typically used for clients rather than servers?
*Basic · id 1764130152678*

<strong>Dynamic IP Address:</strong>
<ul><li>Temporarily allocated and can change each time the device connects</li><li>Automatically assigned (no manual configuration needed)</li><li>Commonly used in home networks and for client devices</li><li>Provides flexibility in managing limited available IP addresses</li></ul><strong>Static IP Address:</strong>
<ul><li>Manually configured and never changes</li><li>Remains the same across reboots and reconnections</li><li>Typically used for servers</li></ul><strong>Why servers need static IPs:</strong>
<ul><li>Clients need to know where to find the server (consistent address)</li><li>DNS records point to specific IPs - changing IPs would break connections</li><li>Example: If Google's servers had dynamic IPs, google.com wouldn't know which IP to point to</li></ul><strong>Why clients use dynamic IPs:</strong>
<ul><li>Clients initiate connections (they find the server, not vice versa)</li><li>More efficient use of limited IPv4 address space</li><li>Easier management - no manual configuration needed</li><li>Most home users don't need incoming connections</li></ul>

---

## What are ports, why can't two applications run on the same port, and what is their numeric range?
*Basic · id 1764130188201*

<strong>Ports</strong> are 16-bit numeric identifiers (0-65,535) used to distinguish between multiple applications or services running on a single device.
<strong>Why ports are needed:</strong> A single device (one IP address) may run multiple networked applications simultaneously. Ports allow the operating system to route incoming data to the correct application.
<strong>Why two applications can't share a port:</strong> When an application binds to a port, it has exclusive access. If another application tries to use the same port, it gets an "address already in use" error. The OS can't determine which application should receive incoming data on a shared port.
<strong>Common examples:</strong>
<ul><li>Port 80: HTTP (web traffic)</li><li>Port 443: HTTPS (secure web traffic)</li><li>Port 4200: Angular development server (default)</li><li>Port 3000: React/Node.js development (common default)</li><li>Port 22: SSH</li><li>Port 5432: PostgreSQL database</li></ul><strong>Full address format</strong>: IP:Port (e.g., 192.168.1.5:8080)

---

## How do IP addresses and ports work together to enable communication? Provide a concrete example.
*Basic · id 1764130255001*

An IP address identifies the <strong>device</strong>, while a port identifies the <strong>specific application</strong> on that device.
<strong>Format</strong>: <code>IP_ADDRESS:PORT</code>
<strong>Concrete example:</strong> Your computer (192.168.1.100) is running:
<ul><li>Web browser connecting to Netflix</li><li>Spotify streaming music</li><li>Zoom video call</li></ul>Netflix server (172.217.15.110):
<ul><li>Web server on port 443 (HTTPS)</li><li>API server on port 8080</li><li>Streaming server on port 8443</li></ul><strong>Communication flow:</strong>

<pre><code>Your browser → 192.168.1.100:53241 (random client port)
    ↓
Netflix web → 172.217.15.110:443</code></pre>

Without ports, your computer couldn't run multiple network applications simultaneously, and the Netflix server couldn't differentiate between requests for web pages vs. streaming vs. API calls. The combination of IP + port creates a unique endpoint called a <strong>socket</strong>.

---

## How does TCP ensure reliable delivery of packets, and what is the mechanism it uses to detect packet loss?
*Basic · id 1764134223648*

TCP ensures reliability through an acknowledgment system:
<strong>Process:</strong>
<ol><li>Sender transmits a packet</li><li>Receiver sends back an acknowledgment (ACK) confirming receipt</li><li>If sender doesn't receive ACK within a timeout period, it assumes the packet was lost</li><li>Sender automatically retransmits the lost packet</li></ol><strong>Key feature:</strong> This is bidirectional - both client and server can send data and acknowledgments. TCP tracks every packet and guarantees all packets are received in the correct order.
<strong>Consequence:</strong> This reliability comes at the cost of overhead (extra acknowledgment packets) and slower transmission (waiting for confirmations).

---

## What is the TCP 3-way handshake and why is it necessary before data transmission can begin?
*Basic · id 1764134256050*

The 3-way handshake is TCP's connection establishment process that ensures both devices are ready and able to communicate reliably.
<strong>The three steps:</strong>
<ol><li><strong>SYN</strong>: Client sends synchronization request to server ("I want to connect")</li><li><strong>SYN-ACK</strong>: Server acknowledges and sends its own synchronization ("I acknowledge and am ready too")</li><li><strong>ACK</strong>: Client acknowledges server's response ("Connection confirmed, let's begin")</li></ol><strong>Why it's necessary:</strong>
<ul><li>Establishes a reliable two-way connection BEFORE any real data is sent</li><li>Ensures both sides are ready to send and receive</li><li>Synchronizes sequence numbers for packet ordering</li><li>Confirms both devices can reach each other</li></ul><strong>Trade-off:</strong> This handshake adds latency (extra round trips) before data transmission, but guarantees a stable connection for the data exchange that follows.

---

## What are the main drawbacks of TCP's reliability features?
*Basic · id 1764134295692*

<strong>1. Overhead:</strong>
<ul><li>Extra packets for acknowledgments (ACKs)</li><li>Connection setup requires 3-way handshake before any data flows</li><li>Connection teardown also requires packet exchange</li><li>Each lost packet triggers retransmission, creating more traffic</li></ul><strong>2. Slower speed:</strong>
<ul><li>Must wait for ACKs before continuing (or wait for timeout to retransmit)</li><li>Packet reordering takes time</li><li>The more reliable you make it, the more you sacrifice speed</li></ul><strong>3. Connection state:</strong>
<ul><li>Both sides must maintain connection state/information</li><li>Resources are consumed even when idle</li></ul><strong>When this matters:</strong> For applications prioritizing speed over perfect delivery (gaming, live streaming), these drawbacks make TCP unsuitable.

---

## What is UDP and how does it fundamentally differ from TCP in terms of reliability and connection?
*Basic · id 1764134329494*

UDP (User Datagram Protocol) is a connectionless, "best effort" protocol that prioritizes speed over reliability.
<strong>Key differences from TCP:</strong>
<pre><table><tbody><tr><th>Aspect</th><th>TCP</th><th>UDP</th></tr><tr><td>Connection</td><td>Requires 3-way handshake</td><td>Connectionless (just send)</td></tr><tr><td>Reliability</td><td>Guarantees delivery & order</td><td>No guarantees - packets may be lost/reordered</td></tr><tr><td>Acknowledgments</td><td>Yes - waits for ACKs</td><td>No - fire and forget</td></tr><tr><td>Retransmission</td><td>Yes - resends lost packets</td><td>No - lost packets stay lost</td></tr><tr><td>Speed</td><td>Slower (due to overhead)</td><td>Faster (minimal overhead)</td></tr><tr><td>Overhead</td><td>High</td><td>Low</td></tr></tbody></table></pre><strong>Core philosophy:</strong> UDP assumes the application will handle any errors or losses. It's just a thin wrapper around IP, adding only port numbers.

---

## Why would you ever choose UDP over TCP given UDP's lack of reliability? Provide specific use cases.
*Basic · id 1764134396292*

UDP is chosen when <strong>speed and low latency</strong> are more important than <strong>guaranteed delivery</strong>.
<strong>Online Gaming:</strong>
<ul><li>Player position updates happen 60+ times per second</li><li>If one position packet is lost, the next one (arriving milliseconds later) makes it irrelevant</li><li>Resending old position data is useless - by the time it arrives, it's outdated</li><li>Better to skip the lost frame and move on</li></ul><strong>Video/Audio Streaming (Live):</strong>
<ul><li>Live broadcasts can't wait for retransmission - the moment has passed</li><li>A dropped video frame or audio blip is preferable to pausing the stream</li><li>Users prefer minor glitches over buffering/lag</li><li>Example: Zoom, Skype, live sports streaming</li></ul><strong>DNS Queries:</strong>
<ul><li>Simple request/response - if no response, just resend the whole query</li><li>Faster than TCP handshake overhead for single packets</li><li>Most DNS queries succeed on first try</li></ul><strong>IoT Sensor Data:</strong>
<ul><li>Temperature sensor sends readings every second</li><li>If one reading is lost, the next reading compensates</li><li>No need to retransmit old temperature data</li></ul><strong>Key principle:</strong> When new data replaces old data quickly, reliability becomes less important than speed.

---

## Does using UDP mean the application has no reliability at all? How can applications handle UDP's unreliability?
*Basic · id 1764134465184*

No - using UDP doesn't mean zero reliability. Applications can implement <strong>custom reliability</strong> at the application layer when needed.
<strong>Application-layer solutions:</strong>
<ol><li><strong>Selective reliability:</strong> App decides which packets MUST arrive (e.g., game lobby chat) vs. which can be lost (player positions)</li><li><strong>Custom retransmission:</strong> App can request specific missing data without TCP's strict ordering</li><li><strong>Forward error correction:</strong> Send redundant data so losses can be reconstructed without retransmission</li><li><strong>Sequence numbers:</strong> App adds its own numbering to detect packet loss/reordering</li><li><strong>Timeouts:</strong> If critical data isn't acknowledged, resend it</li></ol><strong>Examples:</strong>
<ul><li><strong>QUIC protocol</strong> (used by HTTP/3): Built on UDP but adds reliability features</li><li><strong>Video streaming</strong>: Uses UDP with buffering and error concealment at application layer</li><li><strong>Online games</strong>: Use UDP for position updates but TCP for critical events (inventory changes, chat)</li></ul><strong>Key insight:</strong> UDP gives you control - you implement only the reliability you need, where you need it, without TCP's "all or nothing" approach.

---

## Why don't software engineers typically need to choose between TCP and UDP directly?
*Basic · id 1764134509286*

Most developers work at the <strong>application layer</strong>, using high-level protocols that have already made the TCP/UDP decision:
<strong>Protocols that chose TCP for you:</strong>
<ul><li><strong>HTTP/HTTPS (1.1 and 2.0):</strong> Web requests, REST APIs</li><li><strong>FTP:</strong> File transfers</li><li><strong>SMTP:</strong> Email sending</li><li><strong>SSH:</strong> Secure remote access</li><li><strong>WebSockets:</strong> Real-time web communication</li></ul><strong>Protocols that chose UDP for you:</strong>
<ul><li><strong>DNS:</strong> Domain name lookups</li><li><strong>DHCP:</strong> IP address assignment</li><li><strong>HTTP/3 (QUIC):</strong> Modern web protocol</li><li><strong>WebRTC:</strong> Video calling in browsers</li></ul><strong>Why this matters:</strong>
<ul><li>When you make an HTTP request, TCP is handling reliability automatically</li><li>When you use a WebRTC video library, UDP is handling speed automatically</li><li>You only choose TCP vs UDP if you're building low-level networking code or game engines</li></ul><strong>As a developer:</strong> Focus on REST APIs, WebSockets, GraphQL, gRPC - let these protocols handle the transport layer decisions.

---

## What is DNS and what analogy helps explain it?
*Basic · id 1764134861531*

DNS (Domain Name System) is a decentralized hierarchical naming system that converts human-readable domain names into numerical IP addresses. It's like the internet's phone book - just as we store contacts to avoid memorizing phone numbers, DNS helps us avoid memorizing IP addresses.

---

## What organization manages the overall coordination, security, and operation of DNS?
*Basic · id 1764134880868*

ICANN (Internet Corporation for Assigned Names and Numbers) manages the DNS infrastructure.

---

## Using the shopping center analogy, explain the relationship between ICANN and domain registrars.
*Basic · id 1764134904519*

ICANN is like the shopping center's management company (ensuring smooth operation of the entire infrastructure), while domain registrars are like individual store lease providers (handling specific domain registration transactions).

---

## What is the role of domain registrars?
*Basic · id 1764134941791*

Domain registrars are companies certified by ICANN to offer domain name registration services. They help search for available domain names, oversee the registration process, maintain registration records, and ensure domains are registered in the purchaser's name. (Google domains, GoDaddy, HostGator)

---

## What is a DNS record?
*Basic · id 1764134963164*

A DNS record stores information related to a domain or subdomain within the DNS system.

---

## What are the two most common protocols (schemes) for web URLs?
*Basic · id 1764135011617*

A: HTTP and HTTPS, with HTTPS now being the dominant protocol on the World Wide Web.

---

## What is FTP and what is its URL format?
*Basic · id 1764135025349*

FTP (File Transfer Protocol) is used for accessing files and directories on remote servers and transferring files between systems. Format: ftp://ftp.example.com/pub/file.txt (where "pub" is the directory and "file.txt" is the file being accessed).

---

## What is SSH and what is its URL format?
*Basic · id 1764135039432*

SSH (Secure Shell) is used for establishing secure remote connections to servers or computers. Format: ssh://username@example.com:22 (where "username" is for authentication, "example.com" is the server, and ":22" is the port number).

---

## Break down the three components of domains.google.com
*Basic · id 1764135098448*

1) Subdomain: "domains", 2) Primary domain: "google", 3) Top-level domain (TLD): ".com"

---

## What is the Client-Server Model?
*Basic · id 1764227004699*

A computing architecture where clients (applications/systems) request services from servers (computers/software that provide resources). Roles can interchange based on context - a machine can be both client and server. The client is the 'caller' (initiates requests) and the server is the 'callee' (responds to requests).

---

## What is a Remote Procedure Call (RPC)?
*Basic · id 1764227023688*

A protocol that allows a program to execute functions on a separate machine over a network. It enables task management in distributed systems. Example: When searching YouTube, your browser calls functions on YouTube's servers (like listVideos()) even though it appears to execute locally.

---

## What is HTTP and what layers does it build upon?
*Basic · id 1764227042249*

Hypertext Transfer Protocol is a request/response protocol built on top of TCP and IP. IP delivers data packets, TCP ensures correct order, and HTTP defines how data should be formatted and transmitted over the web. It's used every time we make a call through a browser.

---

## What is HTTPS and why is it important?
*Basic · id 1764227106681*

HTTPS is HTTP combined with TLS (Transport Layer Security). It provides secure, encrypted communication over networks. Regular HTTP is vulnerable to man-in-the-middle attacks, while HTTPS encrypts data, preventing unauthorized access or alteration.

---

## What are the main components of an HTTP Request? (four)
*Basic · id 1764227303526*

<strong></strong>
<ol><li>Request Method (GET, POST, PUT, DELETE)</li><li>Request URL/URI (specifies where request should go)</li><li>Headers (provide additional info about request/response)</li><li>Body (contains data payload for POST/PUT requests; GET requests have no body)</li></ol>

---

## <strong></strong>What is the GET method and what makes it special?
*Basic · id 1764227319415*

GET retrieves a resource from the server. It is idempotent, meaning making the same GET request multiple times always returns the same result. GET requests have no body since they retrieve data rather than send it.

---

## What is the POST method and is it idempotent?
*Basic · id 1764227349144*

POST sends data to the server to create a new resource. It includes a request body (payload) with the data being sent. POST is NOT idempotent - making the same POST request multiple times can create multiple resources. Data is sent in the body (not URL) to prevent man-in-the-middle attacks.

---

## What is the PUT method and why is it idempotent?
*Basic · id 1764227364505*

PUT updates a resource with provided data. It IS idempotent because performing the same update operation multiple times with the same data leaves the resource in the same state. Example: updating a user's email to the same value repeatedly results in the same final state.

---

## What is the DELETE method and why is it idempotent?
*Basic · id 1764227377679*

DELETE removes a specified resource. It IS idempotent because while the response may differ (first call: 200 OK, subsequent calls: 404 Not Found), there is no change of state after the first deletion - the resource remains deleted.

---

## What do HTTP status codes in the 1xx range indicate?
*Basic · id 1764227394313*

Informational responses (100-199) acknowledge that the request was received and is being processed. Examples:
<ul><li>100 Continue: Everything is OK, proceed with request</li><li>101 Switching Protocols: Server is switching to a different protocol as requested</li></ul>

---

## What do HTTP status codes in the 2xx range indicate?
*Basic · id 1764227405740*

Successful responses (200-299) indicate the request succeeded. Examples:
<ul><li>200 OK: Request succeeded, returning requested data</li><li>201 Created: Request succeeded and a new resource was created (typically after POST or PUT)</li></ul>

---

## What do HTTP status codes in the 3xx range indicate?
*Basic · id 1764227418312*

Redirection messages (300-399) indicate the resource has moved. Examples:
<ul><li>300 Multiple Choices: Request has multiple possible responses</li><li>301 Moved Permanently: Resource permanently moved to new location, server redirects client</li></ul>

---

## What do HTTP status codes in the 4xx range indicate?
*Basic · id 1764227433109*

Client error responses (400-499) indicate errors in the client's request. Examples:
<ul><li>400 Bad Request: Invalid or malformed request (e.g., incorrect parameters)</li><li>401 Unauthorized: Attempting to access protected resource without proper authentication</li><li>404 Not Found: Requested resource doesn't exist</li></ul>

---

## What do HTTP status codes in the 5xx range indicate?
*Basic · id 1764227449791*

Server error responses (500-599) indicate errors occurred on the server side while processing the request. These represent server failures or issues that prevented fulfilling the request.
<ul><li>500 Internal Server Error: Generic server error</li><li>503 Service Unavailable: Server temporarily unavailable</li></ul>

---

## What are HTTP Request Headers vs Response Headers?
*Basic · id 1764227478844*

Request Headers: Provide info about the request including method, scheme, and accepted data types (e.g., Accept: text/html).
Response Headers: Provide info about the response or server, including Set-Cookie, Cache-Control, and Content-Type (e.g., Content-Type: text/html).

---

## What is idempotency in HTTP methods?
*Basic · id 1764227492985*

An operation is idempotent if performing it multiple times with the same parameters produces the same result/state. GET, PUT, and DELETE are idempotent. POST is NOT idempotent. This is crucial for reliability - idempotent operations can be safely retried without unintended side effects.

---

## What is the relationship between SSL and TLS?
*Basic · id 1764227512971*

TLS (Transport Layer Security) is the modern protocol for secure network communication. SSL (Secure Sockets Layer) is technically an outdated term that has been superseded by TLS, though SSL is still commonly used interchangeably with TLS. Together with HTTP, they form HTTPS.

---

## What is the main problem with using HTTP for real-time chat applications?
*Basic · id 1764228488688*

HTTP is unidirectional and stateless, following a request-response model. The client must continuously poll the server at intervals to check for new messages. This creates overhead - checking too frequently (every second) overloads the server, while checking too infrequently (every minute) causes delayed message delivery. This makes HTTP suboptimal for real-time communication.

---

## What is WebSocket and what makes it different from HTTP?
*Basic · id 1764228525780*

WebSocket is a protocol that enables bidirectional (two-way) communication between client and server. Unlike HTTP's unidirectional request-response model, WebSocket allows continuous, real-time data transmission in both directions simultaneously after an initial connection is established.

---

## What are common use cases for WebSocket?
*Basic · id 1764228541318*

WebSocket is ideal for scenarios requiring real-time updates:
<ul><li>Chat applications (instant messaging, group chats)</li><li>Live streaming applications with real-time chat</li><li>Real-time gaming applications</li><li>Any application requiring instant, bidirectional communication</li></ul>

---

## How is a WebSocket connection established?
*Basic · id 1764228631224*

<li>Client sends WebSocket handshake request (HTTP Upgrade request with special headers)</li><li>Server responds with status code 101 (indicating protocol switch)</li><li>Connection upgrades from HTTP to WebSocket</li><li>Bidirectional data transfer begins The connection starts as HTTP then "upgrades" to WebSocket.</li>

---

## What is the HTTP status code 101 and what does it mean?
*Basic · id 1764228651727*

Status code 101 means "Switching Protocols." It indicates that the server understands the Upgrade header field request and is switching to the protocol specified by the client (in this case, WebSocket). This is returned during the WebSocket handshake.

---

## What ports do WebSocket and WebSocket Secure use by default?
*Basic · id 1764228667614*

<strong></strong>
<ul><li>WebSocket (WS) uses port 80 (same as HTTP)</li><li>WebSocket Secure (WSS) uses port 443 (same as HTTPS) This compatibility with existing web infrastructure makes WebSocket widely supported.</li></ul>

---

## What transport protocol does WebSocket use?
*Basic · id 1764228711926*

WebSocket uses TCP (Transmission Control Protocol). Most application-level protocols like HTTP, WebSocket, FTP, SMTP, and SSH use TCP. The exception is WebRTC, which uses UDP (User Datagram Protocol).

---

## What is polling and why is it problematic?
*Basic · id 1764228726838*

Polling is when the client repeatedly checks the server at regular intervals to see if new data is available. It's problematic because:
<ul><li>Too frequent polling (e.g., every second) creates excessive requests and server load</li><li>Infrequent polling (e.g., every minute) causes delays in receiving updates</li><li>Creates unnecessary overhead and resource consumption</li><li>Not suitable for real-time applications</li></ul>

---

## After the WebSocket handshake, how does data transfer work? (Web Sockets)
*Basic · id 1764228757357*

After the handshake, the client and server can send data to each other in real-time over a persistent connection. This is truly bidirectional - both can initiate data transmission at any time without the client needing to constantly request updates. This is more efficient than repeatedly opening and closing HTTP connections.

---

## Does HTTP/2 multiplexing replace the need for WebSocket?
*Basic · id 1764228794487*

No. While HTTP/2 allows multiplexing (multiple requests in parallel over a single TCP connection), it's not a perfect replacement for WebSocket. WebSocket remains prevalent because it provides true bidirectional, real-time communication, whereas HTTP/2 still follows the request-response model.

---

## What is the difference between stateless and stateful protocols in the context of HTTP vs WebSocket?
*Basic · id 1764228811564*

HTTP is stateless - each request is independent and the server doesn't maintain connection state between requests. WebSocket is stateful - it maintains a persistent connection between client and server, allowing continuous communication without re-establishing the connection for each message.

---

## What are the advantages of WebSocket over HTTP polling for real-time applications?
*Basic · id 1764228827909*

<li>Lower latency: Instant message delivery without polling intervals</li><li>Reduced overhead: Single persistent connection vs. multiple HTTP requests</li><li>Lower server load: No constant polling requests</li><li>True bidirectional: Server can push data without client requesting</li><li>More efficient: No need to repeatedly open/close connections</li>

---

## What is Binary Search and how does it work?
*Basic · id 1764229882483*

Binary Search is an efficient algorithm for finding a target value in a <strong>sorted</strong> array. It works by:
<ol><li>Compare target with middle element</li><li>If target equals middle, return the index</li><li>If target is less than middle, search the left half</li><li>If target is greater than middle, search the right half</li><li>Repeat until target is found or search space is empty</li></ol>Time Complexity: O(log n) Space Complexity: O(1) iterative, O(log n) recursive <strong>Critical requirement: Array must be sorted</strong>

---

## What is the time complexity of Binary Search and why?
*Basic · id 1764229894385*

Time Complexity: O(log n)
Why: Binary search eliminates half of the remaining elements with each comparison. For an array of size n:
<ul><li>After 1st comparison: n/2 elements remain</li><li>After 2nd comparison: n/4 elements remain</li><li>After k comparisons: n/(2^k) elements remain</li></ul>We need log₂(n) comparisons to reduce n elements to 1, hence O(log n). This is much faster than linear search O(n) for large datasets.

---

## What does API stand for and what is its purpose?
*Basic · id 1764311634088*

Application Programming Interface. It provides a way for clients to perform actions with servers over the network through a set of rules and protocols for building and interacting with software applications.

---

## What are the three examples of API paradigms?
*Basic · id 1764311651919*

REST, GraphQL, and gRPC.

---

## What does REST stand for and what is its core principle?
*Basic · id 1764311666375*

REpresentational State Transfer. It uses straightforward HTTP for communication between client and server.

---

## What are the two main architectural constraints of REST APIs?
*Basic · id 1764311685052*

Client-server architecture (client and server are distinct entities that can be developed independently), and 2) Statelessness (each request must contain all necessary information; server doesn't retain details about previous requests).

---

## Why is statelessness important in REST APIs?
*Basic · id 1764311700908*

It facilitates horizontal scaling because the server doesn't need to manage or update session states or cookies.

---

## How does pagination work in stateless REST APIs? Give an example.
*Basic · id 1764311723051*

The client sends state information (like offset) with each request. Example: <code>https://youtube.com/videos?offset=0&limit=10</code> for the first 10 videos, then <code>https://youtube.com/videos?offset=10&limit=10</code> for the next 10.

---

## What is the difference between REST and RESTful?
*Basic · id 1764311738748*

REST is an architectural paradigm, while RESTful is an adjective describing web services that follow the REST constraints.

---

## What does JSON stand for and what is it used for?
*Basic · id 1764311978225*

JavaScript Object Notation. It's a widely used data format for transmitting information over the internet, particularly in REST APIs for both requests and responses.

---

## What are the key structural features of JSON?
*Basic · id 1764311998283*

Key-value pairs, support for various levels of nesting, arrays of objects, and both keys and values must be enclosed in double quotes (not single quotes).

---

## What data types can JSON values contain?
*Basic · id 1764312010824*

Strings, numbers, booleans, objects (nested), arrays, and null.

---

## Why is JSON considered inefficient compared to alternatives like Protocol Buffers?
*Basic · id 1764312029606*

JSON is a text-based format that requires parsing, uses more bandwidth due to verbose syntax (property names repeated, quotation marks), and is less compact than binary formats like Protocol Buffers.

---

## What are the two main data-fetching problems with REST APIs?
*Basic · id 1764312053893*

Over-fetching (receiving more data than necessary) and under-fetching (receiving too little data, requiring multiple requests).

---

## Give an example of over-fetching in REST APIs.
*Basic · id 1764312069192*

When fetching user data to display comments (needing only profile picture, username, and comment), the <code>/user</code> endpoint might return unnecessary fields like country or date joined, wasting network resources.

---

## Give an example of under-fetching in REST APIs.
*Basic · id 1764312080707*

When endpoints are narrowly defined and deliver only a single field per request, requiring multiple API calls to fetch each required property separately.

---

## Why was GraphQL created?
*Basic · id 1764312127219*

To address the limitations of REST APIs, particularly to mitigate issues related to over-fetching and under-fetching.

---

## How does GraphQL solve the over-fetching and under-fetching problems?
*Basic · id 1764312145793*

The client can precisely specify the required data within a single request, and the server gathers and formats all the necessary data accordingly, returning only what was requested.

---

## What are the two primary types of operations in GraphQL?
*Basic · id 1764312165773*

Queries (to retrieve data) and mutations (to modify data on the server).

---

## How many endpoints does GraphQL typically use?
*Basic · id 1764312171355*

A single endpoint, typically an HTTP POST endpoint, where all queries are sent.

---

## What are the advantages of GraphQL over REST?
*Basic · id 1764312182495*

Precise data specification, efficient data retrieval in a single query, ability to navigate through related objects and fields, reduced data usage (beneficial for mobile apps and slow networks), and elimination of multiple round trips.

---

## What are some potential drawbacks of GraphQL?
*Basic · id 1764312201039*

More complex server implementation, harder to cache (since everything goes through one endpoint), potential for complex queries that can overload the server, and a steeper learning curve compared to REST.

---

## What does gRPC stand for?
*Basic · id 1764313118271*

gRPC Remote Procedure Call (recursive acronym), though most people assume it stands for Google Remote Procedure Call.

---

## What is an RPC (Remote Procedure Call)?
*Basic · id 1764313132700*

A method that allows a program to execute a procedure in another address space, commonly on another computer on a shared network.

---

## What are the main features of gRPC?
*Basic · id 1764313149661*

Bi-directional communication, multiplexing for multiple messages over a single TCP connection, server push, and built on top of HTTP/2.

---

## What is gRPC typically used for and why is it faster than REST?
*Basic · id 1764313163697*

Typically used for server-to-server communication. It's much faster than REST because it sends data using Protocol Buffers instead of JSON.

---

## What are Protocol Buffers?
*Basic · id 1764313289465*

A language-neutral, platform-neutral extensible mechanism for serializing structured data (binary format rather than text-based like JSON).

`.proto` example:
<span style="white-space-collapse: preserve;">syntax = "proto3";
</span><span style="white-space-collapse: preserve;">message SearchRequest {
</span><span style="white-space-collapse: preserve;">  string query = 1;
</span><span style="white-space-collapse: preserve;">  int32 page_number = 2;
</span><span style="white-space-collapse: preserve;">  int32 result_per_page = 3;
</span><span style="white-space-collapse: preserve;">}

reading in python:
</span><pre><code><span style="font-style: inherit; font-weight: inherit; color: rgb(86, 156, 214);">import</span> protobuf

syntax <span style="font-style: inherit; font-weight: inherit; color: rgb(220, 218, 218);">=</span> <span style="font-style: inherit; font-weight: inherit; color: rgb(206, 145, 120);">"proto3"</span>

<span style="font-style: inherit; font-weight: inherit; color: rgb(86, 156, 214);">class</span> <span style="font-style: inherit; font-weight: inherit; color: rgb(78, 201, 176);">SearchRequest</span><span style="font-style: inherit; font-weight: inherit;">(</span>protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>Message<span style="font-style: inherit; font-weight: inherit;">)</span><span style="font-style: inherit; font-weight: inherit;">:</span>
    query <span style="font-style: inherit; font-weight: inherit; color: rgb(220, 218, 218);">=</span> protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>Field<span style="font-style: inherit; font-weight: inherit;">(</span><span style="font-style: inherit; font-weight: inherit; color: rgb(181, 206, 168);">1</span><span style="font-style: inherit; font-weight: inherit;">,</span> protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>STRING<span style="font-style: inherit; font-weight: inherit;">)</span>
    page_number <span style="font-style: inherit; font-weight: inherit; color: rgb(220, 218, 218);">=</span> protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>Field<span style="font-style: inherit; font-weight: inherit;">(</span><span style="font-style: inherit; font-weight: inherit; color: rgb(181, 206, 168);">2</span><span style="font-style: inherit; font-weight: inherit;">,</span> protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>INT32<span style="font-style: inherit; font-weight: inherit;">)</span>
    result_per_page <span style="font-style: inherit; font-weight: inherit; color: rgb(220, 218, 218);">=</span> protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>Field<span style="font-style: inherit; font-weight: inherit;">(</span><span style="font-style: inherit; font-weight: inherit; color: rgb(181, 206, 168);">3</span><span style="font-style: inherit; font-weight: inherit;">,</span> protobuf<span style="font-style: inherit; font-weight: inherit;">.</span>INT32<span style="font-style: inherit; font-weight: inherit;">)</span></code></pre>

---

## What streaming capabilities does gRPC provide?
*Basic · id 1764313298228*

Data can be pushed from client to server and from server to client. It's an alternative to REST APIs, not a replacement for WebSockets.

---

## What is an important note about error handling in gRPC?
*Basic · id 1764313314495*

While gRPC is built on HTTP/2, it does not use HTTP error codes. You must develop your own server messages for error handling.

---

## In a gRPC service definition, what do the <code>rpc</code> method definitions specify?
*Basic · id 1764313389103*

The RPC operations (methods that can be called remotely), as well as the request and response schemas (message types) that each will accept and return.

****** EXAMPLE ******
<pre><code>// The greeter service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}</code></pre>

---

## What are "messages" in gRPC and how are they used?
*Basic · id 1764313474661*

Messages are schemas for request and response payloads defined in .proto files. They are converted to class definitions in programming languages and used to create objects passed to RPC methods.

---

## When should you choose gRPC over REST or GraphQL?
*Basic · id 1764313494403*

Use gRPC for microservices communication, real-time streaming, low-latency requirements, internal APIs where you control both client and server, and when bandwidth efficiency is critical. Avoid for browser-based clients (limited support) or public APIs where ease of use matters more than performance.

---

## Compare the typical use cases: REST vs GraphQL vs gRPC
*Basic · id 1764313515797*

REST: general-purpose web APIs, public APIs, simple CRUD operations. GraphQL: complex client applications with varying data needs, mobile apps requiring efficient data usage. gRPC: server-to-server communication, microservices, high-performance requirements.

---

## How do REST, GraphQL, and gRPC differ in their endpoint structure?
*Basic · id 1764313531333*

REST uses multiple endpoints (one per resource), GraphQL uses a single endpoint (typically POST), and gRPC uses service definitions with multiple RPC methods.

---

## What data formats do REST, GraphQL, and gRPC use?
*Basic · id 1764313543165*

REST and GraphQL both use JSON (text-based), while gRPC uses Protocol Buffers (binary format).

---

## What does CRUD stand for and what are the four basic operations?
*Basic · id 1764314643886*

Create, Read, Update, Delete. These are the fundamental operations APIs provide for manipulating data.

---

## How do CRUD operations map to HTTP/REST methods?
*Basic · id 1764314656090*

Create = POST, Read = GET, Update = PUT, Delete = DELETE.

---

## What is an API contract (or surface area)?
*Basic · id 1764314669762*

The defined interface that specifies method signatures, parameters, and return values that developers use to interact with the API, ensuring consistent behavior and seamless integration.

---

## Why is backward compatibility important in API design?
*Basic · id 1764314684376*

APIs may be used by hundreds or thousands of applications. Breaking changes would cause crashes in dependent apps, so newer additions or changes must remain compatible with older versions.

---

## How can you add new features to an API while maintaining backward compatibility?
*Basic · id 1764314715745*

Make new parameters optional. For example, adding an optional <code>parentId</code> parameter to <code>createTweet(userId, content, parentId)</code>allows existing applications using <code>createTweet(userId, content)</code> to continue working without code changes.

---

## What are common strategies for maintaining backward compatibility?
*Basic · id 1764314740009*

Optional parameters with default values, adding new endpoints instead of modifying existing ones, using API versioning to support multiple versions simultaneously, and providing deprecation warnings well in advance of removing old functionality.

---

## When and why do companies update API versions?
*Basic · id 1764314760961*

When significant changes are made (new parameters, methods, or fundamental changes to how things work), or to address security vulnerabilities. Versioning helps distinguish between different iterations and allows developers to adapt their code.

---

## What is API deprecation?
*Basic · id 1764314800294*

The process of announcing that a previous API version will no longer be supported, encouraging developers to update their API calls to the latest version which offers better functionality, performance, and bug fixes.

---

## What are API keys and why might they be regenerated in new API versions?
*Basic · id 1764314816805*

API keys are secret tokens included in API requests to verify the identity and permissions of the requesting application. They may be regenerated in new versions to address security vulnerabilities.

---

## What are the three key principles of good API design?
*Basic · id 1764314861490*

1) Not have conflicting endpoints, 2) Be backwards compatible, 3) Provide pagination for list operations.

---

## In API design, what should developers focus on rather than internal implementation?
*Basic · id 1764314883668*

The method signature (parameters and their types) and the return value. Developers using an API should treat it as a black box and rely on the contract, not the internal workings.

---

## What are other characteristics of well-designed APIs?
*Basic · id 1764314903111*

Consistent naming conventions, clear and comprehensive documentation, appropriate error messages with proper HTTP status codes, rate limiting to prevent abuse, proper authentication and authorization, intuitive resource hierarchies, and following REST principles (if REST API).

---

## How should API endpoints be structured for retrieving resources? Give examples for single vs. multiple resources.
*Basic · id 1764314930326*

Single resource: <code>https://api.twitter.com/v1.0/tweet/:tweetId</code>. Multiple resources with pagination: <code>https://api.twitter.com/v1.0/users/:id/tweets?limit=10&offset=0</code>. The version number (v1.0) should be included in the URL.

---

## What are RESTful URL design best practices?
*Basic · id 1764315159869*

Use nouns for resources (not verbs), use plural forms for collections (<code>/users</code> not <code>/user</code>), nest resources logically (<code>/users/:id/tweets</code>), use hyphens for multi-word resources (<code>/user-profiles</code>), keep URLs lowercase, use query parameters for filtering/sorting/pagination, and include API version in the URL path.

---

## What is caching?
*Basic · id 1764401096249*

The process of making copies of data to enable faster read and write operations. Data is duplicated in faster storage (like CPU cache or memory) even though it already exists in slower storage (like RAM or disk).

---

## What is the main tradeoff of using cache?
*Basic · id 1764401110590*

Speed vs. capacity. Cache is significantly faster than RAM or disk, but has very limited capacity (usually kilobytes or megabytes), requiring careful decisions about what data to store.

---

## Where is caching used beyond a single machine?
*Basic · id 1764401137128*

Caching is widely used in distributed systems at multiple levels: CDNs (Content Delivery Networks), reverse proxy caches, application-level caches (Redis, Memcached), database query caches, and browser caches.

---

## What is a cache hit vs. a cache miss?
*Basic · id 1764401160100*

A cache hit occurs when requested data is found in the cache. A cache miss occurs when requested data is not found in the cache, requiring retrieval from slower storage.

---

## Can you have a cache miss even when data is cached?
*Basic · id 1764401165882*

Yes. The cached data might be stale (outdated) and the cache might have expired, requiring fresh data to be fetched.

---

## What is the cache hit ratio and how is it calculated?
*Basic · id 1764401181632*

The percentage of content requests a cache can fulfill successfully. Formula: (cache hits / (cache hits + cache misses)) × 100. Higher percentages are better.

---

## What types of files are typically cached by browsers and why?
*Basic · id 1764479100850*

Static elements like images, JavaScript files, and CSS files whose content remains unchanged regardless of the user or number of refreshes. These are prime candidates because making network requests repeatedly for identical content is inefficient.

---

## What is the sequence of steps a browser follows to load a resource?
*Basic · id 1764479111785*

Check memory cache (for current browsing session), 2) Check disk cache (persistent cache from past visits), 3) Make a network request if not found in either cache.

---

## Why bother caching when the time difference might seem small (e.g., 123ms vs 11ms)?
*Basic · id 1764479138780*

With many resources (e.g., 100 files each taking an additional 100ms), the cost adds up significantly. Small delays in page load time negatively impact user experience and can lead to users abandoning websites.

---

## What determines cache behavior in browsers?
*Basic · id 1764479166083*

HTTP cache headers sent by servers: Cache-Control (max-age, no-cache, no-store), Expires, ETag (for validation), and Last-Modified. These headers tell browsers how long to cache resources and when to revalidate.

---

## What is write-around cache and when is it useful?
*Basic · id 1764479198883*

New data is written directly to main storage (disk) rather than the cache. Useful when most new data won't be accessed frequently (like most tweets). Saves cache space for more popular content. Popular items are added to cache only after first access.

---

## What is write-through cache and what are its advantages and disadvantages?
*Basic · id 1764479217519*

Data is written to both cache and disk simultaneously. Advantages: avoids stale data and data inconsistency, both locations have the same version. Disadvantages: puts more load on the memory bus and fills cache with data that might not be accessed again.

---

## What is write-back cache and when is data written to disk?
*Basic · id 1764479235680*

Data is written only to cache initially. It's written to disk only when the cache is full or when the system is less busy. Provides better write performance but risks data loss if cache fails before writing to disk.

---

## What are the risks and benefits of each (cache) write mode?
*Basic · id 1764479273135*

Write-around: Risk of cache miss on first read, benefit of efficient cache usage. 
Write-through: Risk of performance overhead, benefit of data consistency. 
Write-back: Risk of data loss on failure, benefit of best write performance and reduced disk I/O.

---

## What is an (cache) eviction policy?
*Basic · id 1764479288171*

A system that determines which items get removed from the cache when the cache is full and new items need to be added.
<strong></strong>

---

## What is FIFO (First In First Out) eviction policy?
*Basic · id 1764479302714*

Similar to a queue interface. When the cache becomes full, the first piece of data that was cached is evicted first, regardless of how recently or frequently it was accessed.

---

## What is LRU (Least Recently Used) eviction policy and what is its principle?
*Basic · id 1764479331210*

Evicts items that haven't been accessed for the longest time. Principle: if an item hasn't been accessed recently, it's less likely to be accessed in the future. Useful for scenarios like popular tweets that should remain in cache.

---

## What is LFU (Least Frequently Used) eviction policy and how is it implemented?
*Basic · id 1764479348915*

Evicts items used least frequently. Implemented using key-value pairs where the key is the item and the value is the usage frequency. When cache is full, the item with the smallest frequency is evicted.

---

## Why is LRU often better than LFU for Twitter-like applications?
*Basic · id 1764479370175*

LFU has limitations: old tweets from 2013 with many historical views might never be evicted despite being irrelevant now, preventing new popular tweets from being cached. LRU better reflects current relevance.

---

## What are other common eviction policies? (than LRU, LFU)
*Basic · id 1764479409050*

LRU-K (considers K most recent accesses), 
2Q (two queues for frequent and infrequent items), 
ARC (Adaptive Replacement Cache, balances recency and frequency), 
Random (randomly selects item to evict),
TTL (Time To Live, expires after set time period).

---

## What factors influence caching strategy decisions?
*Basic · id 1764479470104*

1. Data access patterns (read-heavy vs write-heavy), 
2. data consistency requirements (can tolerate stale data?), 
3. resource constraints (available memory), 
4. latency requirements, cost considerations (network bandwidth, compute), 
5. data volatility (how frequently does data change).

---

## What is a Content Delivery Network (CDN)?
*Basic · id 1764480249143*

A group of cache servers (or edge servers) located around the world that cache content close to end users, enabling faster data access than fetching from the origin server.

---

## What are the main benefits of using a CDN?
*Basic · id 1764480261977*

Speeds up content delivery for geographically distributed users, reduces load on the origin server, reduces data transmitted over long distances, saves bandwidth, improves performance, and provides redundancy/fault tolerance.

---

## What type of content is traditionally stored on CDNs?
*Basic · id 1764480274018*

Static content including HTML, CSS, and static JavaScript files. Modern edge servers can also serve dynamic content through serverless JavaScript functions.

---

## What is a push CDN and when should you use it?
*Basic · id 1764480297165*

New data is immediately pushed from the origin server to all cache servers. Best for static content that doesn't change frequently and is widely requested globally (e.g., video content consumed worldwide). Every request results in a cache hit, but inefficient if content isn't requested near those servers.

---

## What is a pull CDN and when should you use it?
*Basic · id 1764480317072*

The CDN retrieves content from the origin server on an "as needed" basis when users request it. Best for platforms with enormous amounts of frequently generated content (like Twitter) or content with regional popularity variations. Avoids unnecessary resource consumption but first request experiences a cache miss.

---

## What is the key tradeoff between push and pull CDNs?
*Basic · id 1764480327436*

Push CDN: All requests are cache hits, but wastes resources distributing unused content and requires constant maintenance. Pull CDN: More efficient resource usage and less maintenance, but first request to each CDN experiences slower load time (cache miss).

---

## What does "cache-control: public" vs "cache-control: private" mean? (CDN)
*Basic · id 1764480352481*

"Public" means content should be cached by the CDN and shared across users (for static content like images, CSS, JS). "Private" means content should not be cached by CDNs (for user-specific or sensitive information).

---

## What is the primary purpose of CDNs vs horizontal scaling?
*Basic · id 1764480371764*

CDNs minimize latency by placing servers geographically closer to users. Horizontal scaling increases throughput by adding more servers to handle more requests. They're related but have different goals - CDNs optimize for speed/distance, horizontal scaling optimizes for capacity.

---

## What are popular CDN providers?
*Basic · id 1764480387494*

Cloudflare, Amazon CloudFront, Akamai, Fastly, Google Cloud CDN, Microsoft Azure CDN. Each offers different features, pricing, and geographic coverage.

---

## What is a binary tree?
*Basic · id 1764562693140*

Binary tree is a data-structure composed of <i>nodes</i> and <i>pointers</i>. Roughly think of it as a linked list with two links for each value.

Starting node is the <i>root node</i>, if a node has no children it is considered a <i>leaf.</i>

Each node can have up to two children (left child and right child) and usually holds a value (any data-type). The <i>height </i>measures the longest path to leaf from root. The <i>depth </i>measures the shorest distance of a node from the root.

---

## What are Binary Search Trees?
*Basic · id 1764563118319*

1. Binary Search Trees (BST) are a variation of binary trees with the addition of a sorted property. The property is that every node in the left subtree is smaller than the root and every node in the right subtree is greater than the root.

2. This is a recursive property, meaning that it applies to every node in the tree. This property is analogous to having a sorted array.

3. They are useful as they allow O(log n) search just like a sorted array, but additionally allow for O(log n) insertion and deletion.

SEARCH:
* binary comparison at teach node, eliminate half of the remaining tree at each comparison. Recursively return <i>true/false</i> up the call trace

---

## What is a forward proxy and what does it do?
*Basic · id 1764573079055*

An intermediary server that sits between clients and the internet. It receives requests from clients and forwards them to websites on the client's behalf, masking the client's IP address from the destination server.

<img alt="forward-proxy" src="sharpen=1-ea673b39faad9d70a8a6994505f441b8b200df89.png">

---

## What are the three main benefits of a forward proxy?
*Basic · id 1764573101160*

1) Privacy (hides client's IP address from destination), 
2) Efficiency through caching (stores frequently requested data), 
3) Control over network traffic (can filter/block access to certain websites).

---

## In a forward proxy setup, what does the origin server see?
*Basic · id 1764573113569*

The origin server only sees the proxy server's IP address, not the client's IP address. The client is hidden from the origin server.

---

## What are common use cases for forward proxies?
*Basic · id 1764573130957*

Corporate networks blocking entertainment sites, schools filtering content, users bypassing geographic restrictions, anonymizing web browsing, and improving performance through caching for frequently accessed resources.

---

## What is a reverse proxy and how does it differ from a forward proxy?
*Basic · id 1764573179377*

A reverse proxy sits between the internet and servers, receiving requests from clients and forwarding them to appropriate servers. It anonymizes the destination server (not the client). Clients don't know which actual server fulfilled their request.

<img alt="reverse-proxy" src="sharpen=1.png">

---

## What are the main benefits of a reverse proxy?
*Basic · id 1764573220653*

Protects servers by managing incoming requests, distributes load across multiple servers, provides protection against DDoS attacks, reduces latency through caching, and alleviates load on origin servers.

---

## Give two examples of reverse proxies.
*Basic · id 1764573235564*

1) CDNs (Content Delivery Networks) - serve cached content to reduce origin server load, 
2) Load balancers - distribute traffic across multiple servers.

---

## What is a load balancer and why is it needed?
*Basic · id 1764573249718*

A type of reverse proxy that distributes incoming network traffic across multiple horizontally scaled servers. Needed because popular websites cannot handle all traffic with a single server - load balancers ensure efficient handling and optimal resource utilization.

---

## What is Round Robin load balancing?
*Basic · id 1764573267555*

A strategy that distributes requests sequentially across servers in rotation. Each server receives an equal share of requests. Assumes all servers can handle equivalent workload. Simple but doesn't account for varying server capabilities or request complexity

---

## What is Weighted Round Robin (WRR) and when is it better than regular Round Robin?
*Basic · id 1764573283756*

Distributes traffic based on each server's computational power (CPU, RAM, storage, network bandwidth). Servers with higher weights receive proportionally more requests. Better when servers have different capabilities - ensures more efficient resource utilization.

---

## What is the Least Connections load balancing strategy and when is it useful?
*Basic · id 1764573300171*

Assigns new requests to the server with the fewest active connections. Particularly useful when there are significant differences or unpredictable variations in request processing time (e.g., some users browsing vs. others completing checkouts). Prevents imbalances and optimizes resource utilization.

---

## What is the User Location load balancing strategy?
*Basic · id 1764573314263*

Routes users to the geographically nearest server based on their IP address. Minimizes distance data travels, reduces latency, and provides faster user experience.

---

## What is the difference between Layer 4 and Layer 7 load balancing?
*Basic · id 1764573333592*

Layer 4 (transport layer): Routes based on IP address and TCP port - fast and straightforward but less flexible. Layer 7 (application layer): Routes based on request content like HTTP headers, methods, or body - more processing overhead but offers greater flexibility and granular routing decisions.

---

## What are the tradeoffs between different load balancing strategies?
*Basic · id 1764573358891*

Round Robin: Simple but doesn't handle varying server capacity or request complexity. Weighted Round Robin: Better resource utilization but requires knowledge of server capabilities. Least Connections: Handles varying request times well but adds overhead tracking connections. User Location: Reduces latency but may create regional imbalances. Layer 4 vs 7: Speed vs flexibility.

---

## What is the key difference in what forward proxy vs reverse proxy hides?
*Basic · id 1764573374467*

Forward proxy hides the client from the server (server only sees proxy IP). Reverse proxy hides the server from the client (client doesn't know which actual server responded). Forward proxy protects clients, reverse proxy protects servers.

---

## How does basic hashing work for load balancing?
*Basic · id 1764643974142*

<b>T</b>ake a unique identifier (IP address, user ID, or request ID), calculate its hash, and mod by the number of servers. The result assigns the request to a specific server. Example: IP 6 % 3 servers = server 0.

---

## What is the main benefit of using hashing over Round Robin for load balancing?
*Basic · id 1764643989351*

The same IP address/user is always directed to the same server, allowing each server to cache data specific to that user. Round Robin is inconsistent and routes the same user to different servers each time.

---

## What is the critical problem with basic hashing when a server goes down?
*Basic · id 1764644007147*

When a server goes down, the modulo arithmetic changes (e.g., from mod 3 to mod 2), causing a complete remapping of requests. Most users get routed to different servers, leading to widespread cache misses even though their original servers may still be running.

---

## What is consistent hashing and how does it solve the server failure problem?
*Basic · id 1764644029037*

A technique that uses a ring-based structure to map both servers and requests to points on a ring. When a server goes down, only its requests are reassigned to the next server clockwise on the ring - other mappings remain unchanged, minimizing cache misses.

---

## How does consistent hashing assign requests to servers?
*Basic · id 1764644045706*

Hash the request (e.g., IP address) to a value on the ring, 2) Locate the next server on the ring that is equal to or follows that hash value in the clockwise direction, 3) That server handles the request.

---

## What happens to requests when a server fails in consistent hashing?
*Basic · id 1764644058183*

Only the requests that were previously handled by the failed server are reassigned to the next closest server clockwise on the ring. All other request-to-server mappings remain the same.

---

## Why do we use modulus operation in consistent hashing?
*Basic · id 1764644077667*

To ensure the hash value falls within the range of available positions on the ring (0 to M-1), where M is the solution space. It also prevents overflow issues since hash functions can produce very large integer values. This ensures uniform distribution of requests.

---

## When should you use consistent hashing vs Round Robin?
*Basic · id 1764644098940*

Use consistent hashing when caching is important and users need to be consistently routed to the same server (CDNs, databases with user-specific data). Use Round Robin when caching is not a concern and simple, equal distribution is sufficient.

---

## What are two main real-world applications of consistent hashing?
*Basic · id 1764644114762*

1) CDNs - routing specific users to the same cache servers in their regions for cache efficiency, 
2) Databases - ensuring a user's data resides on a specific server and the user is consistently routed to that server.

---

## What are virtual nodes in consistent hashing?
*Basic · id 1764644128542*

A technique where each physical server is represented by multiple points (virtual nodes) on the ring. This provides better load distribution, especially when servers have different capacities, and reduces the impact when a single server fails by spreading its load across multiple other servers rather than just one.

---

## How does Rendezvous Hashing (Highest Random Weight) differ from Consistent Hashing?
*Basic · id 1764644505108*

Rendezvous hashing computes a hash for each (request, server) pair and assigns the request to the server with the highest hash value. No ring structure needed. Consistent hashing uses a ring and assigns requests to the next clockwise server. Both minimize remapping on server failures, but rendezvous hashing provides more uniform distribution without needing virtual nodes.

---

## When would you choose Rendezvous Hashing over Consistent Hashing?
*Basic · id 1764644537184*

Choose rendezvous hashing when you need simpler implementation (no ring structure), more uniform distribution without tuning virtual nodes, or when the number of servers changes frequently. Choose consistent hashing when you need faster lookups (O(log n) vs O(n) for rendezvous) or when it's already widely adopted in your infrastructure (e.g., many CDNs use it).

---

## How does insertion work in BSTs?
*Basic · id 1764645366294*

Start at root and compare the value to insert with current node. If less, go left; if greater, go right. Repeat until finding null, then insert new node there. Time complexity: O(log n) average for balanced trees, O(n) worst case for skewed trees. Duplicates are typically rejected, inserted to right subtree, or stored with a frequency count depending on implementation.

---

## What are the three cases for deleting a node from a BST and how do you handle each?
*Basic · id 1764645557412*

<strong>Case 1 - No children (leaf):</strong> Simply remove it by setting parent's pointer to null. 
<strong>
Case 2 - One child:</strong> Replace node with its only child, bypassing the deleted node. 
<strong>
Case 3 - Two children:</strong> Find inorder successor (go right once, then left until null - smallest in right subtree) or inorder predecessor (go left once, then right until null - largest in left subtree), replace node's value with it, then delete the successor/predecessor (which has at most one child). Time complexity: O(h) where h is height.

---

## What is tree balancing, what are the main self-balancing BST types, and how do rotations work?
*Basic · id 1764645596501*

<strong>Balancing:</strong> Keeping height difference between subtrees minimal to ensure O(log n) operations. Unbalanced trees (e.g., from sorted insertions) degenerate to O(n). 
<strong>
AVL Trees:</strong> Strict balancing, height difference ≤ 1 between subtrees, uses balance factor = height(left) - height(right). Four rotation cases: Left-Left (right rotation), Right-Right (left rotation), Left-Right (left then right rotation), Right-Left (right then left rotation). Better for read-heavy workloads. 
<strong>
Red-Black Trees:</strong>Relaxed balancing with color properties (root is black, red nodes have black children, equal black nodes on all root-to-null paths). Height ≤ 2 log n. Fewer rotations, better for frequent insertions/deletions. 
<strong>
Rotations:</strong> Structural transformations preserving BST properties. Left rotation promotes right child; right rotation promotes left child. Used to rebalance after insertions/deletions.

---

## What is Depth-First Search (DFS) in binary trees, what are the three traversal orders, and what are their complexities?
*Basic · id 1764647914740*

<strong>DFS Concept:</strong> An algorithm that traverses trees by going as deep as possible in one direction before backtracking. Pick a direction (e.g., left), follow pointers until reaching null, backtrack to parent, then go right. Best implemented recursively (or iteratively with a stack).
<strong>Three Traversal Orders:</strong>
<ol><li><strong>Inorder (Left → Root → Right):</strong> Visit left subtree, then parent node, then right subtree. For BSTs, visits nodes in <strong>sorted order</strong>. Example output: <code>[2,3,4,5,6,7]</code>. Use case: Getting sorted data from BST.</li><li><strong>Preorder (Root → Left → Right):</strong> Visit parent node first, then left subtree, then right subtree. Example output: <code>[4,3,2,6,5,7]</code>. Use case: Copying trees, prefix expression evaluation.</li><li><strong>Postorder (Left → Right → Root):</strong> Visit left subtree, then right subtree, then parent node last. Example output: <code>[2,3,5,7,6,4]</code>. Use case: Deleting trees, postfix expression evaluation.</li></ol><strong>Time Complexity:</strong> O(n) for all three methods - must visit every node regardless of tree height.
<strong>Space Complexity:</strong> O(h) where h is tree height - due to recursion call stack. O(log n) for balanced trees, O(n) for skewed trees.
<strong>Key Insight:</strong> Only inorder traversal produces sorted output for BSTs due to the BST property (left < root < right). The leftmost node is always the smallest.

---

## What is Breadth-First Search (BFS) in binary trees, how is it implemented, and what are its complexities?
*Basic · id 1764649098542*

<strong>BFS Concept:</strong> Also known as level-order traversal. Visits all nodes on one level before moving to the next level, prioritizing breadth over depth. Processes tree level by level from left to right.
<strong>Implementation:</strong> Uses a queue (deque) data structure implemented iteratively:
<ol><li>Add root to queue</li><li>While queue is not empty:<ul><li>Process all nodes at current level (loop through current queue length)</li><li>Dequeue node from head (popleft)</li><li>Add its left child, then right child to queue tail (if they exist)</li><li>Move to next level</li></ul></li></ol><strong>Key Detail:</strong> The inner loop iterates exactly <code>len(queue)</code> times to process only the current level's nodes. Children added during this loop become the next level.

<strong>Example traversal:</strong> For a tree with root 4, level 0: <code>[4]</code>, level 1: <code>[3, 6]</code>, level 2: <code>[2, 5, 7]</code>
<strong>
</strong>
<strong>Time Complexity:</strong> O(n) - visit every node exactly once.
<strong>
</strong>
<strong>Space Complexity:</strong> O(n) - store entire level in queue. Worst case: last level can contain roughly n/2 nodes (half the tree in a complete binary tree).
<strong>
</strong>
<strong>BFS vs DFS Space:</strong> BFS uses O(n) space for the queue. DFS uses O(h) space for recursion stack where h is height (O(log n) balanced, O(n) skewed). BFS uses more space but guarantees shortest path in unweighted graphs.

---

## What are Sets and Maps, how are they implemented with trees, and what are their time complexities?
*Basic · id 1764649251419*

<strong>TreeSet:</strong> A set data structure implemented using a tree that ensures <strong>unique values</strong> and keeps elements <strong>sorted</strong>. 

Example: Phone book with names <code>['Alice', 'Brad', 'Collin']</code> stored alphabetically. Operations (insert, delete, search) run in O(log n) time.
<strong>
</strong>
<strong>TreeMap:</strong> A map data structure implemented using a tree that stores <strong>key-value pairs</strong> with keys kept <strong>sorted</strong>. 

Example: Phone book mapping names to numbers <code>{'Alice': 123, 'Brad': 345, 'Collin': 678}</code>. The key requirement: keys must be comparable to maintain order. Values can be any data type (numbers, objects, etc.). Operations run in O(log n) time.
<strong>
</strong>
<strong>---</strong>
<strong>
</strong>
<strong>Key Properties:</strong>
<ul><li>Both maintain sorted order based on keys/values</li><li>TreeSet: unique elements only</li><li>TreeMap: unique keys only (keys map to values)</li><li>Both use BST properties for efficiency</li></ul><strong>Time Complexity:</strong> O(log n) for insert, delete, and search operations when implemented with balanced trees.
<strong>
</strong>
<strong>Implementation:</strong> Java and C++ have built-in TreeMap/TreeSet. Python requires external library (<code>SortedDict</code> from <code>sortedcontainers</code>). JavaScript requires external library (<code>treemap-js</code>).
<strong>
</strong>
<strong>Alternative Implementation:</strong> Sets and Maps can also be implemented using hash tables (HashSet/HashMap) which offer O(1) average time for operations but don't maintain sorted order. Choose TreeSet/TreeMap when you need sorted data; choose HashSet/HashMap when you need faster operations without ordering.

---

## What is backtracking, how does it work, and what are its key characteristics (demonstrate on a Binary Tree)?
*Basic · id 1764734592850*

<strong>Concept:</strong> An algorithm that operates on a brute-force approach by trying all possible solutions and backtracking when hitting a dead-end. Similar to DFS but with the ability to undo choices. Like navigating a maze: try a path, if it's a dead-end, backtrack and try another path.
<strong>How It Works (Tree Example):</strong>
<ol><li>Explore one direction (e.g., left subtree) recursively</li><li>If a valid solution is found, return true</li><li>If it fails, backtrack and try another direction (e.g., right subtree)</li><li>If all options fail, return false</li></ol><strong>Key Pattern - Building a Path:</strong> When tracking the path taken:
<ul><li>Add current node to path when exploring: <code>path.append(root.val)</code></li><li>If exploration succeeds, return true (path remains)</li><li>If exploration fails, <strong>remove node from path before returning</strong>: <code>path.pop()</code></li><li>This "undo" step is the essence of backtracking</li></ul><strong>Example:</strong> Find path from root to leaf without 0 values in tree <code>[4,0,1,null,7,3,2,null,null,null,0]</code>:
<ul><li>Add 4 to path</li><li>Left child is 0 (invalid), don't explore</li><li>Go right, add 1 to path</li><li>Try 1's left child 3, add to path</li><li>3 has no valid children, <strong>pop 3</strong> (backtrack)</li><li>Try 1's right child 2, add to path</li><li>2 is leaf node, return true</li><li>Valid path: <code>[4,1,2]</code></li></ul><strong>Time Complexity:</strong> O(n) - may visit all nodes in worst case.
<strong>Space Complexity:</strong> O(h) where h is height - recursion call stack depth plus path storage (both limited by tree height). O(log n) for balanced trees, O(n) for skewed trees.
<strong>Key Insight:</strong> The <code>path.pop()</code> operation before returning false is what makes this backtracking - it undoes the decision to include that node when the path doesn't lead to a solution. Without this, failed paths would pollute the result.

---

## What is a heap, what are its two types, and what is a priority queue?
*Basic · id 1764735526429*

<strong>Heap Definition:</strong> A specialized tree-based data structure that implements a Priority Queue abstract data type (terms often used interchangeably). Unlike regular queues (FIFO), priority queues remove elements based on priority, not insertion order.

<strong>Two Types:</strong>
<ol><li><strong>Min Heap:</strong> Smallest value at root, has highest priority to be removed. All descendants must be ≥ their ancestors.</li><li><strong>Max Heap:</strong> Largest value at root, has highest priority to be removed. All descendants must be ≤ their ancestors.</li></ol>

<strong>Key Difference from BST:</strong> Heaps allow duplicate values. Order property is parent-child only (not left-right like BST).

---

## What are the two required properties for a binary heap?
*Basic · id 1764735565898*

<strong>1. Structure Property:</strong> Must be a <strong>complete binary tree</strong> - every level is completely filled except possibly the lowest level, which is filled contiguously from left to right (no gaps).

<strong>2. Order Property:</strong>
<ul><li><strong>Min Heap:</strong> All descendants > ancestors. If root has value y, every node in left and right subtrees must have values ≥ y (recursive property).</li><li><strong>Max Heap:</strong> All descendants < ancestors. Every node in subtrees ≤ y.</li></ul>

<strong>Why Complete Tree Matters:</strong> Enables array-based implementation with simple index formulas, no pointers needed.

---

## How are heaps implemented using arrays, and what are the parent/child index formulas?
*Basic · id 1764735607511*

<strong>Implementation:</strong> Despite being drawn as trees, heaps are implemented using arrays. Nodes are stored level-by-level (BFS order) from left to right.

<strong>One-Based Indexing:</strong> Start filling array at index 1 (not 0) to make formulas work correctly. Index 0 is unused or set to dummy value.

<strong>Index Formulas (where i = current node index):</strong>
<ul><li><code>leftChild = 2 * i</code></li><li><code>rightChild = 2 * i + 1</code></li><li><code>parent = i // 2</code> (integer division)</li></ul>

<strong>Why Index 1?</strong> If we started at index 0, formulas break (0 * 2 = 0, incorrectly suggesting left child is at index 0).

<strong>Example:</strong> Node at index 2:
<ul><li>Left child: 2 * 2 = 4</li><li>Right child: 2 * 2 + 1 = 5</li><li>Parent: 2 // 2 = 1</li></ul>

<strong>Key Advantage:</strong> O(1) navigation to parent/children using simple arithmetic, no pointers needed. Only works because heap is a complete binary tree with contiguous array storage.

---

## Why are arrays efficient for heaps?
*Basic · id 1764735623714*

<strong>Why arrays are efficient for heaps:</strong>
<ul><li>Cache-friendly: contiguous memory improves CPU cache performance</li><li>Space-efficient: no pointer overhead (saves ~16 bytes per node)</li><li>Simple implementation: just an array and index arithmetic</li><li>Contrast with BST: BSTs need pointers because they're not guaranteed to be complete/balanced</li></ul>

---

## What is the time complexity for reading the minimum (or maximum) value from a heap and why?
*Basic · id 1764736403516*

O(1) constant time. The min/max value is always at the root (index 1 in the array), so we can simply read <code>heap[1]</code> without any searching or traversal.

---

## How does the push operation work in a heap and what is its time complexity?
*Basic · id 1764736481247*

<strong>Algorithm:</strong>
<ol><li>Append new value to end of array (maintains structure property - complete binary tree)</li><li><strong>Percolate/bubble up:</strong> Compare with parent using <code>i // 2</code></li><li>If child < parent (min heap) or child > parent (max heap), swap them</li><li>Repeat until node is in correct position (parent comparison satisfies order property) or reaches root</li></ol><strong>Time Complexity:</strong> O(log n) - in worst case, bubble up entire height of tree. Since heap is always balanced (complete binary tree), height = log n.

<strong>Key Insight:</strong> Always insert at next available position first (end of array) to maintain structure property, then fix order property by percolating up.

---

## How does the pop operation work in a heap, why is it more complex than push, and what is its time complexity?
*Basic · id 1764736636982*

<strong>Algorithm:</strong>
<ol><li>Save root value to return (the min/max priority element)</li><li>Replace root with <strong>rightmost node of last level</strong> (last element in array)</li><li>Remove last element from array (maintains structure property)</li><li><strong>Percolate/bubble down:</strong> Compare with children<ul><li>If node has two children: swap with smaller child (min heap) or larger child (max heap)</li><li>If node has only left child: swap if needed</li><li>If node has no children or order property satisfied: stop</li></ul></li><li>Repeat until node reaches correct position</li></ol><strong>Why Not Swap Root with min(left, right) Child?</strong> This violates structure property - creates gaps in the tree at level 2+. The tree is no longer complete.
<strong>
</strong>
<strong>Edge Cases:</strong>

<ul><li>Empty heap (len = 1): return None</li><li>Single node (len = 2): just pop and return it</li><li>Node can only have left child OR two children (never only right child, would violate complete tree property)</li></ul><strong>Time Complexity:</strong> O(log n) - percolate down entire height in worst case. Height = log n for balanced complete binary tree.

<strong>Key Insight:</strong> Always replace root with last element to maintain structure property, then fix order property by percolating down. Swap with smaller child (min heap) or larger child (max heap) to maintain order property throughout tree.

---

## What are the time complexities for all heap operations?
*Basic · id 1764736685055*

<ul><li><strong>Get Min/Max:</strong> O(1) - root is always at index 1</li><li><strong>Push:</strong> O(log n) - insert at end, percolate up height of tree</li><li><strong>Pop:</strong> O(log n) - replace root with last element, percolate down height of tree</li><li><strong>Heapify (build heap from array):</strong> O(n) - bottom-up construction</li></ul><strong>Why Log n for Push/Pop:</strong> Heap is always a complete binary tree (structure property), guaranteeing balanced tree with height = log n. Both operations traverse at most the height of the tree.

---

## What is heapify and why is it more efficient than building a heap element-by-element?
*Basic · id 1764737334102*

Heapify is an algorithm that builds a heap from an unsorted array in O(n) time. Much more efficient than pushing elements one-by-one, which takes O(n log n) time (n elements × O(log n) per push). When converting an existing array to a heap, always use heapify.

---

## How does the heapify algorithm work?
*Basic · id 1764737374331*

<strong>Steps:</strong>
<ol><li>Convert array to 1-based indexing (append first element to end)</li><li>Calculate first non-leaf node: <code>(len(heap) - 1) // 2</code> (index n/2)</li><li>Starting from first non-leaf node, percolate down (same as pop operation)</li><li>Decrement index by 1, repeat for next non-leaf node</li><li>Continue until reaching root (index 1)</li></ol><strong>Bottom-Up Approach:</strong> Start from lowest non-leaf level and work upward. When processing a parent, its children's subtrees are already valid heaps, so only need to percolate that parent down to correct position.
<strong>Why Skip Leaf Nodes?</strong> Leaves have no children, already satisfy order property. Roughly n/2 nodes are leaves, so only process n/2 non-leaf nodes.

---

## Why is heapify O(n) instead of O(n log n)?
*Basic · id 1764737413474*

<strong>Mathematical Reasoning:</strong>
<ul><li>Bottom level nodes (≈ n/2): percolate down 0-1 levels</li><li>Next level up (≈ n/4): percolate down 1-2 levels</li><li>Pattern continues: nodes halve, but percolation depth increases</li><li>Total work: (n/2)×1 + (n/4)×2 + (n/8)×3 + ... = O(n)</li></ul><strong>Key Insight:</strong> Most nodes (the leaves) do zero work. Only a few nodes near the top do significant work. The summation converges to linear time, not O(n log n).
<strong>Comparison:</strong>
<ul><li>Repeated push: n insertions × O(log n) each = O(n log n)</li><li>Heapify: processes n/2 nodes with decreasing work = O(n)</li></ul>

---

## What is an RDBMS?
*Basic · id 1764738548340*

A Relational Database Management System (RDBMS) is a database that stores persistent data using tables. It provides efficient reading and writing by structuring data into precisely defined fields that can be queried easily.

---

## What data structure makes SQL queries efficient?
*Basic · id 1764738561746*

B+ Trees make SQL queries efficient by organizing data in a multi-way tree structure with sorted leaf nodes containing all the data.

---

## How does a B+ Tree differ from a B-Tree?
*Basic · id 1764738572200*

In a B+ Tree, all data is stored in leaf nodes which are linked in sorted order, while interior nodes contain only keys and pointers. In a B-Tree, interior nodes also contain data.

---

## What is the relationship between children and keys in a B+ Tree node?
*Basic · id 1764738583910*

If a node has M children, it must have M-1 keys. The keys act as separation values dividing the subtrees.

---

## Give an example of how keys divide subtrees in a B+ Tree.
*Basic · id 1764738604346*

A node with keys [2, 5] has three children: first child contains values < 2 (e.g., [0,1]), second child contains values between 2 and 5 (e.g., [3,4]), and third child contains values > 5 (e.g., [6,7])

---

## What is indexing in databases?
*Basic · id 1764738619448*

Indexing is a way to improve the speed of data retrieval operations on a database table, at the cost of additional writes and storage space to maintain the index data structure.

---

## Give an example of how indexing works
*Basic · id 1764738632624*

If you have names mapped to phone numbers, you could pick the name field as the index, allowing faster retrieval of phone numbers when searching by name.

---

## What are the trade-offs of indexing?
*Basic · id 1764738640715*

Indexing speeds up data retrieval but requires additional writes and storage space to maintain the index data structure.

---

## What is a table in SQL?
*Basic · id 1764738650938*

A table is a way to organize data where each row contains information about a single primary key. It must have a defined structure before inserting records.

---

## What is a primary key?
*Basic · id 1764738660349*

A primary key uniquely identifies each record (row) in a table.

---

## What is a record?
*Basic · id 1764738669132*

A record is a row in a database table.

---

## What is varchar?
*Basic · id 1764738677628*

Varchar (variable character) is a data type for variable length strings that can include numbers, letters, and special characters.

---

## What is a foreign key constraint?
*Basic · id 1764738692397*

A foreign key constraint ensures that a value in one table must exist in another table, establishing a relationship between the two tables and maintaining referential integrity.

---

## What is a JOIN in SQL?
*Basic · id 1764738709309*

A JOIN is a way to combine records from two or more tables based on a related column between them.

---

## What does ACID stand for?
*Basic · id 1764738800957*

ACID stands for Atomicity, Consistency, Isolation, and Durability.

---

## What is Durability in ACID? Give an example.
*Basic · id 1764738814102*

Durability means that after a transaction successfully completes, changes to data persist and are not undone, even in the event of a system failure. Example: If a fund transfer completes and then the power goes out, the transaction will still be recorded in the database when the system recovers.

---

## What is Atomicity in ACID? Explain with the Alice and Bob example.
*Basic · id 1764738834040*

Atomicity means all changes to data are performed as if they are a single operation - either all changes are performed, or none of them are. Example: If Alice sends Bob $500 and the database crashes during the transfer, atomicity ensures that money is NOT taken from Alice's account. Either the entire transaction completes (money leaves Alice and arrives at Bob), or none of it happens.

---

## What is Isolation in ACID and what is a dirty read?
*Basic · id 1764738862809*

Isolation means that the intermediate state of a transaction is invisible to other transactions, so concurrent transactions appear to be serialized and don't interfere with each other. A dirty read occurs when one transaction reads data from another transaction before it has committed, violating the isolation property. 

Example: If Transaction 1 is transferring $500 from Alice to Bob (not yet committed), and Transaction 2 reads Alice's balance mid-transfer, Transaction 2 has performed a dirty read.

---

## What is Consistency in ACID? Give an example.
*Basic · id 1764738881797*

Consistency ensures data integrity by adhering to predefined rules and constraints that maintain data validity throughout transaction execution, bringing the database from one valid state to another. 

Example: A rule that an account balance cannot be negative is a consistency constraint that the database enforces.

---

## Why are B+ Trees particularly well-suited for disk-based storage?
*Basic · id 1764738901044*

B+ Trees minimize disk I/O operations because they have high fanout (many children per node), reducing tree height and the number of disk accesses needed to find data.

---

## What is the trade-off between normalization and query performance?
*Basic · id 1764738916665*

Normalization reduces data redundancy and improves data integrity but may require more JOINs, which can slow down queries. Denormalization can speed up reads but increases storage and risks data inconsistency.

---

## When would you choose a relational database over a NoSQL database?
*Basic · id 1764738928055*

Choose a relational database when you need ACID guarantees, complex queries with JOINs, well-defined schemas, and strong data consistency requirements.

---

## What is referential integrity and how is it maintained?
*Basic · id 1764738948961*

Referential integrity ensures that relationships between tables remain consistent, typically enforced through foreign key constraints so references always point to existing records. The database will typically prevent deletion of records referenced by foreign keys unless CASCADE DELETE is specified.

---

## What does NoSQL stand for and how does it differ from SQL databases?
*Basic · id 1764740958644*

NoSQL stands for "Not Only SQL". Unlike SQL databases, NoSQL databases don't organize data in tables and you can't use SQL syntax for complex queries and joins. They are more flexible and scalable, designed to overcome SQL limitations in scalability and performance. SQL scales vertically (limited), while NoSQL scales horizontally (unlimited).
<strong></strong>

---

## What are the four main types of NoSQL databases?
*Basic · id 1764740979609*

(1) Key-Value databases, 
(2) Document databases, 
(3) Wide-Column databases, and 
(4) Graph databases.

---

## What is a Key-Value database and give an example?
*Basic · id 1764740997509*

A Key-Value database uses a simple key-value method to store data, like a hashmap. The database stores key-value pairs where the key serves as a unique identifier. It's schemaless, so different keys and values can have completely different structures. Example: Redis, which stores data in-memory (RAM) for exceptionally fast retrieval.

---

## What is a Document database and give an example?
*Basic · id 1764741018485*

Document databases store data as JSON-like "documents", making it easier for developers to store and query data since it matches the format used in application code. Individual fields can be added or removed independently, providing flexibility. 

Example: MongoDB stores data in JSON-like structures with varying nesting levels.

---

## What is a Wide-Column database and when is it useful?
*Basic · id 1764741048245*

A Wide-Column database stores data in columns rather than rows, enabling high write throughput and optimizing reads/aggregations over specific data subsets. They excel in very large-scale scenarios like internet search, web messaging, and managing large amounts of timestamp data. 

Examples: Apache Cassandra and Google BigTable.

---

## What is a Graph database and when should you use it?
*Basic · id 1764741070045*

A Graph database uses nodes (entities) connected by edges (relationships), similar to graph data structures in algorithms. They're useful when data has complex relationships and interconnectedness. 

Example use case: Facebook's platform with user relationships, friends, friends-of-friends, interests, and social interactions (comments, likes, shares).

---

## Why can NoSQL databases be distributed across multiple servers more easily than SQL databases?
*Basic · id 1764741086249*

Because NoSQL databases have no foreign key or join constraints, data can be split and stored on different servers without needing to cross-reference between tables. This allows half the data to be in one part of the world and the other half elsewhere, enabling horizontal scaling.

---

## What does BASE stand for and how does it differ from ACID?
*Basic · id 1764741112416*

BASE stands for "Basically Available, Soft state, Eventual consistency". While ACID focuses on strict consistency (common in SQL databases), BASE focuses on eventual consistency and is more characteristic of NoSQL databases. Both models have their use cases—one isn't inherently better than the other.

---

## Can NoSQL databases be ACID-compliant?
*Basic · id 1764741122713*

es, some NoSQL databases can be ACID-compliant. MongoDB is an example of a NoSQL database that provides ACID guarantees for transactions.

---

## What is Eventual Consistency and how does it work?
*Basic · id 1764741137277*

Eventual Consistency means that updates made to the system will eventually propagate to all nodes, but not immediately. In a leader/follower architecture, writes go to the primary (leader) node, which then eventually updates the follower nodes. During this propagation period, users might read stale data.

---

## Give an example of where Eventual Consistency is acceptable.
*Basic · id 1764741152575*

Social media platforms like Twitter or Instagram, where follower counts or like counts may be slightly delayed. The exact count doesn't need to be instantly consistent across all servers—it's acceptable if it updates within seconds.

---

## What does "schemaless" mean in NoSQL databases?
*Basic · id 1764741169738*

Schemaless means the database manages information without needing a predefined blueprint or structure. Different records can have completely different structures - one key might map to a string while another maps to a complex nested JSON object.

---

## What is leader/follower (master/slave) architecture?
*Basic · id 1764741183866*

A distributed database architecture where one node is designated as the leader/primary (handles all writes and updates) and other nodes are followers/replicas (handle reads). The leader propagates changes to followers, which may result in eventual consistency rather than strict consistency.

---

## What are the time complexity trade-offs between Hash Maps, Tree Maps, and Sorted Arrays?
*Basic · id 1764819216495*

<strong>Hash Map:</strong>
<ul><li>Insert/Remove/Search: O(1) average case</li><li>Inorder Traversal: Not possible (unordered)</li><li>Best for: Fast lookups, counting, frequency problems</li></ul><strong>Tree Map:</strong>
<ul><li>Insert/Remove/Search: O(log n)</li><li>Inorder Traversal: O(n) sorted order</li><li>Best for: When you need sorted/ordered data</li></ul><strong>Sorted Array:</strong>
<ul><li>Insert/Remove: O(n) (requires shifting)</li><li>Search: O(log n) (binary search)</li><li>Inorder Traversal: O(n) already sorted</li><li>Best for: Static data, many searches, few updates</li></ul><strong>Key Insight:</strong> Hash maps sacrifice ordering for speed. If you need to iterate keys in order, must first sort them O(n log n), slower than tree map's O(n) inorder traversal.

---

## How do you use a hash map for frequency counting and when should you use hash maps?
*Basic · id 1764819260832*

<strong>When to Use:</strong> Hash maps are essential when problems mention "unique", "count", or "frequency". They're one of the most useful data structures for coding interviews.
<strong>Frequency Counting Pattern:</strong>

python
<pre><code>countMap <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">{</span><span style="color: rgb(56, 58, 66);">}</span>
<span style="color: rgb(166, 38, 164);">for</span> item <span style="color: rgb(166, 38, 164);">in</span> array<span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">if</span> item <span style="color: rgb(166, 38, 164);">not</span> <span style="color: rgb(166, 38, 164);">in</span> countMap<span style="color: rgb(56, 58, 66);">:</span>
        countMap<span style="color: rgb(56, 58, 66);">[</span>item<span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">1</span>
    <span style="color: rgb(166, 38, 164);">else</span><span style="color: rgb(56, 58, 66);">:</span>
        countMap<span style="color: rgb(56, 58, 66);">[</span>item<span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">+=</span> <span style="color: rgb(183, 107, 1);">1</span></code></pre>

<strong>Example:</strong> Array <code>["alice", "brad", "collin", "brad", "dylan", "kim"]</code> Result: <code>{"alice": 1, "brad": 2, "collin": 1, "dylan": 1, "kim": 1}</code>
<strong>How It Works:</strong>
<ul><li>Hash maps don't allow duplicate keys (unique constraint)</li><li>Use keys for items, values for counts</li><li>If key exists: increment count</li><li>If key doesn't exist: add with count = 1</li></ul><strong>Time Complexity:</strong> O(n) for n elements - each insert/update is O(1)
<strong>Space Complexity:</strong> O(k) where k = number of unique keys in the array
<strong>Comparison:</strong> Using tree map would take O(n log n) for same operation (each of n insertions costs O(log n)).

---

## What is the difference between a Hash Set and a Hash Map?
*Basic · id 1764819282932*

<ul><li><strong>Hash Set:</strong> Contains keys only, no values. Used for checking membership/existence. Example: checking if element exists, removing duplicates.</li><li><strong>Hash Map:</strong> Contains key-value pairs. Used for associations/mappings. Example: counting frequencies, storing related data.</li></ul><strong>Both Share:</strong>
<ul><li>O(1) average time for insert/remove/search</li><li>No duplicates allowed (keys must be unique)</li><li>Unordered (cannot iterate in sorted order without extra work)</li></ul><strong>Choose Hash Set when:</strong> Only need to track presence/absence (true/false) <strong>Choose Hash Map when:</strong> Need to associate additional data with keys (counts, objects, etc.)

---

## How does a hash function work and why is it needed for O(1) hash map operations?
*Basic · id 1764819680899*

Converts a key (e.g., string) into an integer that serves as the array index where the key-value pair is stored.

<strong>
</strong>
<strong>Example Process:</strong>

<ol><li>Take each character in the key</li><li>Get ASCII code for each character</li><li>Sum all ASCII codes (pre-hashing)</li><li>Apply modulo with array size: <code>sum(ASCII codes) % array_size
</code></li></ol>
<strong>Example:</strong> "Alice" has ASCII sum = 25, array size = 2
<ul><li>Index = 25 % 2 = 1</li><li>Store "Alice": "NYC" at index 1</li></ul>

<strong>Key Property:</strong> Same string always produces same integer (deterministic), enabling O(1) lookup.

<strong>Why Modulo?</strong> Ensures index is valid (within array bounds). Without it, large sums would be out of bounds.

---

## What are collisions in hash maps and what are the two main strategies to handle them?
*Basic · id 1764819896344*

<strong>Collision:</strong> When two different keys hash to the same array index. Inevitable as array size is limited.
<strong>
</strong>
<strong>1. Chaining (Most Common):</strong>

<ul><li>Store multiple key-value pairs at same index using a linked list</li><li>Each array position holds a linked list of nodes</li><li>Insert/Search: O(1) average, O(n) worst case if all keys collide at one index</li><li>Simpler to implement than open addressing</li></ul><strong>2. Open Addressing:</strong>
<ul><li>Find next available empty slot when collision occurs</li><li>If index i is occupied, try i+1, i+2, etc. (linear probing)</li><li>Lookup: Check original index, then probe next slots until key found</li><li>More efficient for few collisions, but limited by array size</li><li>More complex to implement</li></ul><strong>Trade-off:</strong> Chaining allows unlimited entries (linked lists grow), open addressing limited by array capacity but can be more cache-friendly.
<strong>
</strong>
<strong>Minimizing Collisions:</strong> Use prime numbers for array size (only divisible by 1 and itself), distributes keys more evenly.

---

## When and why do hash maps resize, and what is rehashing?
*Basic · id 1764819938160*

<strong>Resizing Trigger:</strong> When array becomes <strong>half full</strong> (50% capacity), double the array size to <code>2 × capacity</code>. Done before insertion, not during, to minimize collisions proactively.
<strong>Why Resize?</strong>
<ul><li>Ensure vacant spots for new keys</li><li>Reduce collision probability</li><li>Maintain O(1) average performance</li></ul><strong>Rehashing Process:</strong>
<ol><li>Create new array with doubled capacity</li><li><strong>Recompute position</strong> for all existing key-value pairs</li><li>Use same hash formula but with new capacity: <code>sum(ASCII) % new_capacity</code></li><li>Reinsert all pairs into new array</li></ol><strong>Why Rehash?</strong> Array size changed, so <code>key % old_size ≠ key % new_size</code>. Keys may map to different indices.

<strong>Example:</strong> "Alice" at index 1 with size 2
<ul><li>After doubling to size 4: 25 % 4 = 1 (happens to stay at 1)</li><li>"Brad" (sum 27): 27 % 4 = 3, then 27 % 8 = 3 after next resize</li></ul><strong>Cost:</strong> Rehashing is O(n) operation but amortized O(1) per insertion (infrequent).

---

## What are the time complexities for hash map operations and when do they degrade?
*Basic · id 1764819971093*

<strong>Average Case (All Operations):</strong> O(1) for insert, remove, and search
<strong>Worst Case:</strong> O(n) - occurs when:
<ul><li>Poor hash function causes many collisions</li><li>All keys hash to same index (with chaining, traverses entire linked list)</li><li>Many collisions with open addressing (probes many slots)</li></ul><strong>Requirements for O(1):</strong>
<ul><li>Good hash function (distributes keys evenly)</li><li>Low collision rate</li><li>Proper resizing (maintain load factor ≤ 0.5)</li></ul><strong>Practical Note:</strong> In interviews and real-world usage, assume O(1) unless specifically dealing with worst-case analysis or adversarial inputs.

---

## What is a graph, what are its basic components, and what is the relationship between vertices and edges?
*Basic · id 1764822146127*

<strong>Definition:</strong> A data structure made up of nodes (vertices) connected by pointers (edges). No restrictions on placement or connections - a vertex can have variable number of edges.

<strong>Components:</strong>
<ul><li><strong>Vertices (V):</strong> Nodes in the graph</li><li><strong>Edges (E):</strong> Pointers connecting vertices</li></ul>

<strong>Edge Limit:</strong> Given V vertices, maximum edges E ≤ V². Each vertex can point to every other vertex including itself. A graph with V vertices and V² edges is called a <strong>complete graph</strong>.

<strong>Examples:</strong> Trees and linked lists are actually graphs - directed graphs with specific constraints (trees: acyclic, linked lists: linear).

<strong>Key Property:</strong> Vertices don't need to be connected - a graph with no edges is still valid.

---

## What is the difference between directed and undirected graphs?
*Basic · id 1764822157439*

<strong>Directed Graph:</strong> Edges have direction (arrows). Relationship is one-way. Example: Twitter follows (A follows B doesn't mean B follows A), trees (parent → child), linked lists (prev/next pointers).
<strong>Undirected Graph:</strong> Edges have no direction, represent bidirectional relationships. Example: Facebook friends (mutual), road connections between cities.
<strong>Notation:</strong> For directed edge from A to B, only A → B exists. For undirected, both A → B and B → A exist implicitly.

---

## What are the three main ways to represent graphs and what are their space complexities?
*Basic · id 1764822220609*

<strong>1. Matrix (Grid):</strong>
<ul><li>2D array where 0s represent vertices</li><li>Traverse by moving up/down/left/right</li><li>Connect adjacent 0s to form graph</li><li>Space: O(n × m) where n = rows, m = columns</li><li>Use: Spatial problems, pathfinding on grids</li></ul><strong>2. Adjacency Matrix:</strong>
<ul><li>2D array where indices represent vertices</li><li><code>adjMatrix[v1][v2] = 1</code> means edge from v1 to v2</li><li><code>adjMatrix[v1][v2] = 0</code> means no edge</li><li>Space: O(V²) - inefficient for sparse graphs (few edges)</li><li>Use: Dense graphs, quick edge lookup O(1)</li></ul><strong>3. Adjacency List (Most Common):</strong>
<ul><li>Each vertex stores list of its neighbors</li><li>Implementation: HashMap or array of lists, or GraphNode class with <code>neighbors</code> list</li><li>Space: O(V + E) - only stores existing edges</li><li>Use: Most interview problems, sparse graphs, space-efficient</li></ul><strong>Comparison:</strong>
<ul><li>Adjacency List is most space-efficient and commonly used</li><li>Adjacency Matrix wastes space for sparse graphs but has O(1) edge lookup</li><li>Matrix representation used for grid-based problems</li></ul>

---

## How do you implement a graph using an adjacency list?
*Basic · id 1764822250536*

<strong>Using GraphNode Class:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">GraphNode</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> val<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        self<span style="color: rgb(56, 58, 66);">.</span>val <span style="color: rgb(64, 120, 242);">=</span> val
        self<span style="color: rgb(56, 58, 66);">.</span>neighbors <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># List of adjacent vertices</span></code></pre>

<strong>Using HashMap (Alternative):</strong>

python
<pre><code>graph <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">{</span>
    <span style="color: rgb(80, 161, 79);">'A'</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(80, 161, 79);">'B'</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(80, 161, 79);">'C'</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">,</span>
    <span style="color: rgb(80, 161, 79);">'B'</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(80, 161, 79);">'A'</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(80, 161, 79);">'D'</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">,</span>
    <span style="color: rgb(80, 161, 79);">'C'</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(80, 161, 79);">'A'</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">,</span>
    <span style="color: rgb(80, 161, 79);">'D'</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(80, 161, 79);">'B'</span><span style="color: rgb(56, 58, 66);">]</span>
<span style="color: rgb(56, 58, 66);">}</span></code></pre>

<strong>Advantages:</strong>
<ul><li>Space efficient: O(V + E) only stores actual edges</li><li>Easy to iterate through neighbors</li><li>Simple to add/remove edges</li><li>Most common in interviews</li></ul><strong>Operations:</strong>
<ul><li>Add edge: <code>node1.neighbors.append(node2)</code> - O(1)</li><li>Check if edge exists: search neighbors list - O(degree of vertex)</li><li>Get all neighbors: <code>node.neighbors</code> - O(1) to access list</li></ul>

---

## What is dynamic programming and how does it differ from regular recursion?
*Basic · id 1764833432179*

<strong>Definition:</strong> Dynamic programming (DP) is an optimized version of recursion that solves big problems by breaking them into smaller subproblems, but avoids redundant calculations.

<strong>Key Difference from Recursion:</strong> Regular recursion recalculates the same subproblems multiple times. DP uses <strong>memoization</strong> (caching) to store results of subproblems, so each is calculated only once.
<strong>
</strong>
<strong>Example - Fibonacci:</strong>
<ul><li>Brute force recursion: O(2^n) - calculates F(2) three times in F(5)</li><li>DP with memoization: O(n) - calculates each F(i) once and caches it</li></ul><strong>Core Principle:</strong> If you're solving the same subproblem multiple times, cache the result and reuse it.
<strong>
</strong>
<strong>When to Use DP:</strong>
<ol><li>Problem has overlapping subproblems (same calculation repeated)</li><li>Problem has optimal substructure (optimal solution uses optimal solutions to subproblems)</li><li>Current problem depends on solutions to smaller versions of same problem</li></ol>

---

## What are the two main approaches to dynamic programming and how do they differ?
*Basic · id 1764833478549*

<strong>1. Top-Down (Memoization):</strong>

<ul><li><strong>Strategy:</strong> Recursion + caching</li><li><strong>Direction:</strong> Start from original problem, recurse down to base cases</li><li><strong>Implementation:</strong> Add cache parameter to recursive function, check cache before computing</li><li><strong>When to use:</strong> More intuitive, closer to recursive thinking, easier to write initially</li></ul><strong>2. Bottom-Up (Tabulation):</strong>
<ul><li><strong>Strategy:</strong> Iterative, build solution from base cases up</li><li><strong>Direction:</strong> Start from base cases, iteratively compute larger subproblems</li><li><strong>Implementation:</strong> Use array/table, fill it iteratively in correct order</li><li><strong>When to use:</strong> Often more space-efficient, avoids recursion stack overflow</li></ul><strong>Fibonacci Example:</strong>
<ul><li>Top-down: <code>if n in cache: return cache[n]</code> then recurse</li><li>Bottom-up: <code>dp = [0,1]</code> then iterate to build up to n</li></ul><strong>Trade-offs:</strong>
<ul><li>Top-down: Easier to conceptualize, only computes needed subproblems, uses recursion stack space</li><li>Bottom-up: No recursion overhead, can optimize space better, need to determine iteration order</li></ul>

---

## What is 1D DP, and how can you optimize space complexity in problems like Fibonacci?
*Basic · id 1764833549825*

<strong>1D DP:</strong> Problems where each subproblem depends on one variable, and results can be stored in a 1D array proportional to input size n.
<strong>
</strong>
<strong>Fibonacci Example - Three Approaches:</strong>
<strong>1. Top-Down (Memoization):</strong>

python
<pre><code>cache <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">{</span><span style="color: rgb(56, 58, 66);">}</span>
<span style="color: rgb(166, 38, 164);">if</span> n <span style="color: rgb(166, 38, 164);">in</span> cache<span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(166, 38, 164);">return</span> cache<span style="color: rgb(56, 58, 66);">[</span>n<span style="color: rgb(56, 58, 66);">]</span>
cache<span style="color: rgb(56, 58, 66);">[</span>n<span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> fib<span style="color: rgb(56, 58, 66);">(</span>n<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">+</span> fib<span style="color: rgb(56, 58, 66);">(</span>n<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

Time: O(n), Space: O(n) for cache + O(n) recursion stack
<strong>
</strong>
<strong>2. Bottom-Up (Full Array):</strong>

python
<pre><code>dp <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">*</span> <span style="color: rgb(56, 58, 66);">(</span>n<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">,</span> dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1</span>
<span style="color: rgb(166, 38, 164);">for</span> i <span style="color: rgb(166, 38, 164);">in</span> <span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> n<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    dp<span style="color: rgb(56, 58, 66);">[</span>i<span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> dp<span style="color: rgb(56, 58, 66);">[</span>i<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">+</span> dp<span style="color: rgb(56, 58, 66);">[</span>i<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">]</span></code></pre>

Time: O(n), Space: O(n)
<strong>
</strong>
<strong>3. Space-Optimized Bottom-Up:</strong>

python
<pre><code>dp <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Only store last 2 values</span>
<span style="color: rgb(166, 38, 164);">for</span> i <span style="color: rgb(166, 38, 164);">in</span> <span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> n<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    tmp <span style="color: rgb(64, 120, 242);">=</span> dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span>
    dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">+</span> dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span>
    dp<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> tmp</code></pre>

Time: O(n), Space: O(1)
<strong>
</strong>
<strong>Key Insight:</strong> If current state only depends on fixed number of previous states (e.g., prev 2 for Fibonacci), you only need to store those states, not entire history.
<strong>Space Optimization Pattern:</strong> Identify how many previous values you actually need, keep only those in memory.

---

## <strong>What is 2D DP and how do you solve the unique paths grid problem?</strong>
*Basic · id 1764833621476*

<strong>A: 2D DP:</strong> Problems where subproblems depend on two variables (e.g., row and column), requiring a 2D cache/table.

<strong>Problem:</strong> Count unique paths from top-left to bottom-right, moving only down or right in a grid.
<strong>
</strong>
<strong>Brute Force (No DP):</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">paths</span><span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(56, 58, 66);">,</span> c<span style="color: rgb(56, 58, 66);">,</span> rows<span style="color: rgb(56, 58, 66);">,</span> cols<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">if</span> r <span style="color: rgb(64, 120, 242);">==</span> rows <span style="color: rgb(166, 38, 164);">or</span> c <span style="color: rgb(64, 120, 242);">==</span> cols<span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(183, 107, 1);">0</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># out of bounds</span>
    <span style="color: rgb(166, 38, 164);">if</span> r <span style="color: rgb(64, 120, 242);">==</span> rows<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(166, 38, 164);">and</span> c <span style="color: rgb(64, 120, 242);">==</span> cols<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(183, 107, 1);">1</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># destination</span>
    <span style="color: rgb(166, 38, 164);">return</span> paths<span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> c<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">+</span> paths<span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(56, 58, 66);">,</span> c<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># down + right</span></code></pre>

Time: O(2^(n+m)) - exponential, recalculates same cells
<strong>
</strong>
<strong>Top-Down DP (Memoization):</strong>

python
<pre><code>cache <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">{</span><span style="color: rgb(56, 58, 66);">}</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># or 2D array</span>
<span style="color: rgb(166, 38, 164);">if</span> <span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(56, 58, 66);">,</span>c<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(166, 38, 164);">in</span> cache<span style="color: rgb(56, 58, 66);">:</span> <span style="color: rgb(166, 38, 164);">return</span> cache<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(56, 58, 66);">,</span>c<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">]</span>
cache<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(56, 58, 66);">,</span>c<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> paths<span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> c<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">+</span> paths<span style="color: rgb(56, 58, 66);">(</span>r<span style="color: rgb(56, 58, 66);">,</span> c<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

Time: O(n × m), Space: O(n × m)
<strong>
</strong>
<strong>Bottom-Up DP (Space Optimized):</strong>

python
<pre><code>prevRow <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">*</span> cols
<span style="color: rgb(166, 38, 164);">for</span> r <span style="color: rgb(166, 38, 164);">in</span> <span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span>rows<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    curRow <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">*</span> cols
    curRow<span style="color: rgb(56, 58, 66);">[</span>cols<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">1</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># base case</span>
    <span style="color: rgb(166, 38, 164);">for</span> c <span style="color: rgb(166, 38, 164);">in</span> <span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span>cols<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        curRow<span style="color: rgb(56, 58, 66);">[</span>c<span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> curRow<span style="color: rgb(56, 58, 66);">[</span>c<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">+</span> prevRow<span style="color: rgb(56, 58, 66);">[</span>c<span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># right + down</span>
    prevRow <span style="color: rgb(64, 120, 242);">=</span> curRow</code></pre>

Time: O(n × m), Space: O(m) - only need previous row!
<strong>
</strong>
<strong>Key Insights:</strong>
<ul><li>Calculate from bottom-right to top-left (or vice versa with different logic)</li><li>Each cell's value = sum of values from cells you can reach (right + down)</li><li>Space optimization: only need previous row to calculate current row, not entire 2D grid</li></ul><strong>Common 2D DP Problems:</strong> Longest common subsequence, edit distance, 0/1 knapsack, matrix chain multiplication

---

## What is replication and why is it used in distributed systems?
*Basic · id 1764834942032*

<strong>Definition:</strong> Creating copies of a database (replicas) hosted on separate machines/servers, kept in sync with the original.

<strong>Purpose:</strong>
<ol><li>Increase data availability (if one server fails, others remain accessible)</li><li>Improve scalability (distribute read load across multiple servers)</li><li>Increase data durability (multiple copies protect against data loss)</li></ol>

<strong>Architecture:</strong> Leader/follower (master/slave)
<ul><li><strong>Leader (master):</strong> Original database, handles writes</li><li><strong>Follower (slave):</strong> Replica, typically handles reads</li><li>Data flows from leader → follower (not reverse, or leader wouldn't be fully updated)</li></ul>

---

## What are the differences between synchronous and asynchronous replication and their trade-offs?
*Basic · id 1764834999134*

<strong>Synchronous Replication:</strong>
<ul><li>Write transaction immediately replicated to follower before confirming to client</li><li>Leader waits for follower acknowledgment</li><li><strong>Pros:</strong> Strong consistency, follower always up-to-date, can immediately replace leader if it fails</li><li><strong>Cons:</strong> Higher latency (client waits for replication), slower writes</li></ul><strong>Asynchronous Replication:</strong>
<ul><li>Leader commits transaction and sends to follower without waiting for acknowledgment</li><li>Client gets immediate response</li><li><strong>Pros:</strong> Lower latency, faster writes, better availability</li><li><strong>Cons:</strong> Data might be stale on follower (eventual consistency), risk of data loss if leader fails before replication</li></ul><strong>Trade-off:</strong> Consistency vs. Latency/Availability
<ul><li>Synchronous: Prioritizes consistency, sacrifices speed</li><li>Asynchronous: Prioritizes speed and availability, accepts temporary inconsistency</li></ul><strong>Use Cases:</strong>
<ul><li>Synchronous: Financial transactions, critical data where consistency is mandatory</li><li>Asynchronous: Social media feeds, analytics, where slight staleness is acceptable</li></ul>

---

## What is master-master (multi-master) replication and when is it used?
*Basic · id 1764835100394*

<strong>Definition:</strong> Multiple leaders that can both read from and write to, typically serving different geographic regions.
<strong>Use Case:</strong> Serving data across different regions (e.g., one leader for west coast, one for east coast).

<strong>
</strong>
<strong>Benefits:</strong>
<ul><li>Lower latency for users (connect to geographically closer leader)</li><li>Better distribution of write load</li><li>High availability (multiple leaders can handle failures)</li></ul><strong>Challenges:</strong>
<ul><li>Synchronization latency between leaders</li><li>Conflict resolution when same data modified in different regions</li><li>Requires periodic updates to keep leaders in sync</li><li>More complex than single leader setup</li></ul><strong>Additional Knowledge:</strong> Master-master replication requires conflict resolution strategies like "last write wins", version vectors, or CRDTs (Conflict-free Replicated Data Types) to handle concurrent writes to same data.

---

## What is sharding, how does it differ from replication, and how is data partitioned?
*Basic · id 1764835166528*

<strong>Definition:</strong> Dividing a database into smaller pieces (shards), each hosted on separate machines. Each shard contains only a subset of data (not a complete copy).
<strong>
</strong>
<strong>Replication vs Sharding:</strong>
<ul><li>Replication: Multiple complete copies for availability/redundancy</li><li>Sharding: Split data into pieces for performance/scalability</li><li>Often used together: each shard can have its own replicas</li></ul><strong>Why Shard?</strong> When replication alone can't handle traffic volume - distributes both data AND workload.
<strong>Shard Key:</strong> Chosen criterion/attribute determining which shard data belongs to.
<strong>Partitioning Strategies:</strong>
<ol><li><strong>Range-Based:</strong><ul><li>Split by ranges (e.g., IDs 1-25, 26-50, 51-75, 76-100)</li><li>Or alphabetically (A-L, M-Z)</li><li>Simple but can create hotspots (uneven distribution)</li></ul></li><li><strong>Hash-Based:</strong><ul><li>Use hash function on shard key</li><li>Better distribution, but range queries harder</li><li>Consistent hashing minimizes data movement when adding/removing shards</li></ul></li><li><strong>Geographic:</strong><ul><li>Partition by location (US, EU, Asia)</li><li>Reduces latency for regional users</li></ul></li></ol><strong>Example:</strong> User database sharded by first letter of last name - all A-M users on shard1, N-Z on shard2.

---

## What are the main challenges with sharding, especially for SQL vs NoSQL databases?
*Basic · id 1764835253123*

<strong>General Challenges:</strong>
<ol><li><strong>Related data placement:</strong> Ensuring related tables/data end up in same shard (join operations across shards are expensive)</li><li><strong>Rebalancing:</strong> Adding/removing shards requires data migration</li><li><strong>Complexity:</strong> Application logic must know which shard to query</li></ol><strong>SQL Database Challenges:</strong>
<ul><li><strong>ACID properties:</strong> Hard to maintain atomicity/consistency across shards</li><li><strong>Distributed transactions:</strong> Complex and slow</li><li><strong>No native support:</strong> MySQL, PostgreSQL don't inherently support sharding</li><li><strong>Application-level sharding:</strong> Developers must implement sharding logic, becomes very complicated</li><li><strong>Joins across shards:</strong> Extremely expensive or impossible</li></ul><strong>NoSQL Database Advantages:</strong>
<ul><li><strong>Designed for horizontal scaling:</strong> Built with sharding in mind</li><li><strong>Eventual consistency:</strong> Relaxed consistency model works better for distributed systems</li><li><strong>No relational constraints:</strong> No complex joins, easier to distribute data</li><li><strong>Native sharding support:</strong> Many NoSQL databases handle sharding automatically</li></ul><strong>Eventual Consistency:</strong> After a write, replicas may temporarily have different versions, but will converge to consistent state over time. Acceptable for many use cases (social media, caching) but not for critical operations (banking).

<strong>Additional Knowledge - Consistent Hashing for Sharding:</strong> Minimizes data movement when shards are added/removed. Hash both data and servers onto a ring, assign data to next server clockwise. Adding a server only affects adjacent data, not entire dataset.

---

## What is the CAP theorem and what does it state about distributed systems?
*Basic · id 1764835820636*

<strong>CAP Stands For:</strong>
<ul><li><strong>C</strong>onsistency</li><li><strong>A</strong>vailability</li><li><strong>P</strong>artition Tolerance</li></ul>

<strong>The Theorem:</strong> A distributed system can only guarantee two out of three properties simultaneously. You must choose between:
<ul><li>CP (Consistency + Partition Tolerance) - sacrifice availability</li><li>AP (Availability + Partition Tolerance) - sacrifice consistency</li></ul>

<strong>Key Point:</strong> Partition Tolerance is assumed as a given (network failures will happen), so the real choice is between Consistency or Availability when partitions occur.

<strong>Applicability:</strong> Primarily applies to NoSQL databases and partitioned/distributed systems with replication.

<strong>Important:</strong> CAP consistency ≠ ACID consistency. CAP consistency means all nodes see same data at same time. ACID consistency means transactional integrity.

---

## What is partition tolerance and why is it assumed in the CAP theorem?
*Basic · id 1764835856046*

<strong>Definition:</strong> A partition occurs when communication breakdown between leader and follower nodes prevents leader from updating follower.

<strong>Causes:</strong>
<ul><li>Network failures</li><li>Hardware issues</li><li>Any incident preventing nodes from communicating</li></ul>

<strong>Partition Tolerance Means:</strong> System can continue functioning despite network failures, avoiding total system collapse.

<strong>Why It's Assumed:</strong> In distributed systems, network partitions are inevitable - they will happen. The question isn't "if" but "when" and "how do we handle them?"

<strong>Without Partition Tolerance:</strong> System would completely fail when any network issue occurs, making it impractical for real-world distributed systems.

---

## What does consistency mean in the CAP theorem and when should you prioritize it?
*Basic · id 1764835916821*

<strong>CAP Consistency Definition:</strong> All nodes see identical data at any given moment. Every read retrieves the most recently written data, regardless of which node is queried.

<strong>
</strong>
<strong>During Partition:</strong>
<ul><li>Leader has updated data</li><li>Follower might have stale data (can't receive updates)</li><li>Reading from follower gives outdated information</li></ul>

<strong>Prioritizing Consistency (CP):</strong> Make follower node unavailable during partition - don't serve potentially stale data. Client requests may fail, but data remains consistent.

<strong>
</strong>
<strong>When to Choose Consistency:</strong>
<ul><li><strong>Banking systems:</strong> Account balance must be accurate across all nodes</li><li><strong>Healthcare systems:</strong> Medical records must be up-to-date (life and death matter)</li><li><strong>Financial transactions:</strong> Incorrect data has severe consequences</li><li><strong>Rule:</strong> When data accuracy is absolutely critical, even if it means system stops responding</li></ul>

<strong>Trade-off:</strong> Sacrifice availability - clients may not get responses during partitions, but will never get incorrect data.

---

## What does availability mean in the CAP theorem and when should you prioritize it?
*Basic · id 1764835966720*

<strong>CAP Availability Definition:</strong> Every system request receives a response (success or failure) regardless of system faults. System stays operational and handles requests even amid failures.

<strong>
</strong>
<strong>During Partition:</strong>
<ul><li>Both leader and follower continue serving requests</li><li>Follower may serve stale data (hasn't received updates)</li><li>All nodes respond to valid requests</li></ul>

<strong>Prioritizing Availability (AP):</strong> Keep all nodes responding, accept that some may serve slightly outdated data (eventual consistency).

<strong>
</strong>
<strong>When to Choose Availability:</strong>
<ul><li><strong>Learning Management Systems (LMS):</strong> Students must submit assignments even if grade update delayed</li><li><strong>Social media feeds:</strong> Slightly outdated posts acceptable</li><li><strong>Content delivery:</strong> Better to serve cached content than fail completely</li><li><strong>E-commerce product listings:</strong> Can tolerate minor staleness in inventory counts</li><li><strong>Rule:</strong> When system must stay responsive, and temporary data inconsistency is acceptable</li></ul>

<strong>Trade-off:</strong> Sacrifice consistency - clients may get stale data during partitions, but system remains responsive.

---

## What is tunable consistency and how does the PACELC theorem extend CAP?
*Basic · id 1764836011460*

<strong>Tunable Consistency:</strong> Modern databases don't strictly choose CP or AP. They allow dynamic adjustment between being more consistent (less available) or more available (less consistent) based on operation requirements.

<strong>Example:</strong> MongoDB, Cassandra allow configuring consistency levels per query - strong consistency for critical operations, eventual consistency for less critical ones.

<strong>
</strong>
<strong>PACELC Theorem Extension:</strong>

<strong>P (Partition exists):</strong> Choose A or C (same as CAP)

<strong>E</strong>lse (no partition): Choose <strong>L</strong>atency or <strong>C</strong>onsistency

<strong>
</strong>
<strong>Full Statement:</strong> "If Partition, choose Availability or Consistency. Else (normal operation), favor Latency or Consistency."

<strong>
</strong>
<strong>Normal Operation Trade-off:</strong>
<ul><li><strong>Favor Latency:</strong> Asynchronous replication, faster responses, risk of reading slightly stale data</li><li><strong>Favor Consistency:</strong> Synchronous replication, slower responses, always read most recent data</li></ul>

<strong>Real-World:</strong> PACELC better captures reality - systems must make trade-offs even when network is healthy, not just during partitions.

<strong>
</strong>
<strong>Examples:</strong>
<ul><li><strong>PA/EL:</strong> Cassandra, DynamoDB - prioritize availability and low latency</li><li><strong>PC/EC:</strong> HBase, MongoDB (strong consistency mode) - prioritize consistency even with higher latency</li><li><strong>PA/EC:</strong> Some systems choose availability during partition but consistency normally</li></ul>

<strong>Key Insight:</strong> System design is about understanding trade-offs for specific requirements, not rigid rules. Different parts of same application may make different choices.

---

## What are the main criticisms and misunderstandings of the CAP theorem?
*Basic · id 1764836167483*

<strong>Common Misunderstandings:</strong>
<ol><li><strong>Oversimplification:</strong> CAP is often taught as "pick 2 of 3" using Venn diagrams, but reality is more nuanced. It's not binary - there are degrees of consistency and availability.</li><li><strong>Partition Tolerance Isn't Optional:</strong> The "pick 2" framing is misleading. In distributed systems, partitions will happen, so P is mandatory. Real choice is only C vs A during partitions.</li><li><strong>Ignores Normal Operation:</strong> CAP only addresses behavior during network partitions, which are relatively rare. PACELC theorem fixes this by addressing latency vs consistency trade-offs during normal operation.</li><li><strong>Not All-or-Nothing:</strong> Systems don't have to be purely CP or AP. Modern databases offer tunable consistency - different consistency levels for different operations or queries.</li><li><strong>Consistency Definition Narrow:</strong> CAP's linearizable consistency (all nodes see same data instantly) is just one consistency model. Many weaker consistency models exist (eventual, causal, read-your-writes) that are more practical and performant.</li><li><strong>Time Aspect Ignored:</strong> CAP doesn't account for how long inconsistency lasts or how quickly system recovers after partition heals.</li></ol><strong>Martin Kleppmann's Critique:</strong> CAP theorem is often misapplied and creates false dichotomy. Better to think about specific consistency guarantees your application needs and latency requirements, rather than forcing into CP/AP buckets.
<strong>
</strong>
<strong>Better Approach:</strong>
<ul><li>Understand specific consistency requirements for each operation</li><li>Consider consistency spectrum (strong → eventual), not binary choice</li><li>Design for partition scenarios but optimize for normal operation</li><li>Use CAP as starting point for discussion, not rigid constraint</li></ul><strong>Key Takeaway:</strong> CAP theorem is a useful mental model for understanding distributed system trade-offs, but it's often over-simplified in teaching. Real systems are more nuanced with tunable parameters and multiple consistency models available.

---

## What is object storage and what are its key components?
*Basic · id 1764836530775*

A storage system that treats each piece of data as an object containing three components: actual data, metadata, and a unique identifier. Stored in flat address spaces with no hierarchical structure (no folders). Evolved from BLOB (Binary Large Object) storage.

---

## Why should you NOT store images and videos in databases, and what should you use instead?
*Basic · id 1764836562159*

<strong>Problems with Database Storage:</strong>
<ul><li>Databases not optimized for large files</li><li>Rare to query for specific images/videos</li><li>Hinders performance</li><li>Increases storage requirements</li><li>Causes frequent read/write operations on database</li><li>Traditional RDBMSs can't handle large files efficiently</li></ul><strong>Solution - Object Storage:</strong>
<ul><li>Specifically designed for unstructured data</li><li>Optimized for large files</li><li>Highly scalable flat architecture</li><li>Examples: AWS S3, Google Cloud Storage, Azure Blob Storage</li></ul><strong>Best Practice:</strong> Store metadata in database (filename, size, user_id, upload_date), store actual file in object storage, database contains URL/key to retrieve file.

---

## What are the key differences between file systems and object storage in terms of structure and scalability?
*Basic · id 1764836591350*

<strong>File System:</strong>
<ul><li>Hierarchical tree structure (folders within folders)</li><li>Nested organization</li><li>Complex structure limits scalability</li><li>Familiar navigation with paths</li></ul><strong>Object Storage:</strong>
<ul><li>Flat address space (no folders/hierarchy)</li><li>All objects at same level</li><li>Each object accessed by unique identifier</li><li>Easy horizontal scaling (no hierarchical complexity)</li></ul><strong>Scalability Advantage:</strong> Flat structure allows infinite horizontal scaling without maintaining complex directory hierarchies.

---

## When should you use object storage vs databases, and how do you retrieve data from object storage?
*Basic · id 1764836639593*

<strong>Use Object Storage For:</strong>
<ul><li>Images and videos (user uploads, profile pictures, media)</li><li>Database backups (large backup files)</li><li>Static assets (CSS, JavaScript files)</li><li>Archives and long-term data retention</li><li>Any large files (>few MB) that don't need querying</li></ul><strong>Use Databases For:</strong>
<ul><li>Structured data needing queries/filters</li><li>Transactional data</li><li>Relational data requiring joins</li><li>Data with frequent small updates</li></ul><strong>Data Retrieval:</strong>
<ul><li>Don't read directly from object store</li><li>Make HTTP network request to object storage using unique identifier/key</li><li>Higher latency than database reads but optimized for large files</li></ul><strong>System Design Pattern:</strong>
<ol><li>User uploads image</li><li>Store image in S3, get back URL/key</li><li>Store metadata in database: <code>{user_id: 123, profile_pic_url: "s3://bucket/user123.jpg"}</code></li><li>To display: fetch URL from database, make HTTP GET to S3 for actual image</li></ol><strong>Key Trade-offs:</strong>
<ul><li>✅ Excellent for large unstructured data, highly scalable, cost-effective</li><li>❌ No querying capabilities, higher latency, immutable (must replace entire object)</li></ul>

---

## What is a message queue and why is it used instead of just scaling servers?
*Basic · id 1764899368453*

A system that buffers messages between producers (app events) and consumers (application servers), allowing asynchronous processing of requests.

<strong>Purpose:</strong>
<ul><li>Handle high volume of requests that can't be processed simultaneously</li><li>Decouple producers from consumers</li><li>Buffer for managing surges in data</li><li>Process requests that don't need immediate handling</li></ul><strong>Why Not Just Scale Servers?</strong>
<ul><li>Not always cost-effective</li><li>Not always practical</li><li>Many requests don't need immediate processing</li><li>Better to queue and process later</li></ul><strong>Key Benefit:</strong> Decoupling - producers and consumers can operate independently, improving system flexibility and resilience.

<strong></strong>

---

## What are the differences between pull-based and push-based message queue models?
*Basic · id 1764899760048*

<strong>Pull-Based Model:</strong>
<ul><li><strong>How it works:</strong> Application server monitors queue and "pulls" messages when it has capacity</li><li><strong>Pros:</strong> More efficient for managing server-side load, server controls its own pace</li><li><strong>Cons:</strong> May introduce latency if queue is empty (wasted polling cycles)</li><li><strong>Use when:</strong> Server capacity varies or you want server to control processing rate</li></ul><strong>Push-Based Model:</strong>
<ul><li><strong>How it works:</strong> Queue pushes messages to server as they arrive</li><li><strong>Pros:</strong> Lower latency, immediate delivery</li><li><strong>Cons:</strong> Risk of overloading server if message rate is too high</li><li><strong>Use when:</strong> Predictable load or need low latency</li></ul><strong>Acknowledgment Pattern (Both Models):</strong>
<ol><li>Queue sends message to server</li><li>Server processes message</li><li>Server sends acknowledgment back to queue</li><li>If no acknowledgment within timeout, queue assumes failure and resends</li><li>Ensures message delivery even with temporary server issues</li></ol>

---

## What is the publisher/subscriber model and what are its benefits?
*Basic · id 1764899840976*

<ol><li>Publishers dispatch messages to specific <strong>topics</strong> (categories/labels)</li><li>Subscribers subscribe to topics they're interested in</li><li>Message broker ensures all messages delivered to all subscribers of that topic</li><li>Subscribers process messages independently at their own pace</li></ol><strong>Key Concept - Topics:</strong> Categories that serve as conduits for similar messages, helping organize the vast array of messages. Multiple subscribers can subscribe to same topic.
<strong>Example - Payment Processing:</strong>
<ul><li><strong>Topic:</strong> "OrderPlaced"</li><li><strong>Subscribers:</strong><ul><li>Inventory Service (updates stock levels)</li><li>Billing Service (charges customer)</li><li>Shipping Service (prepares shipment)</li><li>Email Service (sends confirmation)</li></ul></li><li>All notified independently when order is placed</li></ul><strong>Benefits:</strong>
<ol><li><strong>Decoupling:</strong> Publishers and subscribers don't need to know about each other</li><li><strong>Scalability:</strong> Easy to add new subscribers without modifying publishers</li><li><strong>Flexibility:</strong> Can introduce completely different APIs as subscribers without altering architecture</li><li><strong>Resilience:</strong> Messages not lost if subscriber temporarily unavailable</li><li><strong>Independent Processing:</strong> Each subscriber processes at own pace</li></ol><strong>Adaptability:</strong> New subscribers can be added to topic without modifying publishers, making system adaptable to changing requirements.

---

## Give a real-world example of when message queues are beneficial and explain why.
*Basic · id 1764900198974*

<strong>Payment Processing Example:</strong>

<strong>
</strong>
<strong>Problem Without Message Queues:</strong>
<ul><li>High usage periods (major sales) cause surge in payment requests</li><li>Synchronous processing creates poor user experience</li><li>Long wait times and potential timeouts</li><li>Server overwhelmed during peaks</li></ul><strong>Solution With Message Queues:</strong>
<strong>
</strong>
<strong>1. Handling Peak Loads:</strong>
<ul><li>Payment requests stored in queue during high traffic</li><li>Processed asynchronously when servers have capacity</li><li>Users get immediate confirmation (request queued)</li><li>Actual processing happens in background</li></ul><strong>2. Decoupling Services:</strong>
<ul><li>New order placed → message published to "OrderPlaced" topic</li><li>Payment service subscribes and processes payment</li><li>Inventory service subscribes and updates stock</li><li>Email service subscribes and sends confirmation</li><li>All services operate independently</li></ul><strong>Benefits:</strong>
<ul><li>Users don't wait for payment processing</li><li>System handles traffic spikes gracefully</li><li>Services can be updated/scaled independently</li><li>Failed payments can be retried automatically</li><li>No loss of requests during outages</li></ul><strong>Other Common Use Cases:</strong>
<ul><li>Email sending (queue emails, send in background)</li><li>Image/video processing (queue uploads, process asynchronously)</li><li>Analytics events (queue events, process in batches)</li><li>Notifications (queue alerts, deliver at appropriate rate)</li></ul><strong>Popular Message Queue Services:</strong>
<ul><li>RabbitMQ</li><li>Apache Kafka</li><li>GCP Pub/Sub</li><li>AWS SQS (Simple Queue Service)</li><li>AWS SNS (Simple Notification Service)</li></ul>

---

## What is MapReduce and what problem does it solve?
*Basic · id 1764900234063*

<strong>Definition:</strong> A programming model and implementation for processing and generating large datasets (terabytes to petabytes) in a distributed computing environment.
<strong>Problem It Solves:</strong> Efficiently processing massive amounts of data across multiple machines.
<strong>Example:</strong> Process a billion rows containing names and SSNs - keep names, redact SSNs. Single machine would be too slow; MapReduce distributes work across many machines.
<strong>Origin:</strong> Introduced by Google engineers to manage large datasets efficiently, even with failures like partitions.

---

## What is the difference between batch processing and stream processing?
*Basic · id 1764900245511*

<strong>Batch Processing:</strong>
<ul><li>Processes data in large groups/batches</li><li>Data accumulated over period, then processed as unit</li><li>NOT real-time - runs when batch jobs execute</li><li>Example: Count word frequency in books, weekly/daily reports</li><li>Frequency: Weekly, daily, hourly jobs</li></ul><strong>Stream Processing:</strong>
<ul><li>Processes data in real-time as received</li><li>Data processed individually, not stored first</li><li>Immediate processing required</li><li>Example: Redacting credit card info on payment, live fraud detection</li><li>Cannot wait for batch - must happen instantly</li></ul><strong>Key Difference:</strong> Timing - batch is delayed and grouped, stream is immediate and individual.

---

## What are the roles of master and worker nodes in a MapReduce system like Apache Hadoop?
*Basic · id 1764900258776*

<strong>Master Node:</strong>
<ul><li>Manages distribution of MapReduce job across workers</li><li>Monitors status of each task</li><li>Re-assigns tasks if failures occur</li><li>Coordinates overall process</li></ul><strong>Worker Nodes (Slave Nodes):</strong>
<ul><li>Where actual data processing happens</li><li>Receives portion of data from master</li><li>Receives copy of MapReduce program</li><li>Executes Map and Reduce operations</li><li>Multiple workers process data in parallel</li></ul><strong>Architecture Benefit:</strong> Fault tolerance - if worker fails, master reassigns its tasks to other workers.

---

## What happens during the Map phase and Shuffle/Sort phase of MapReduce?
*Basic · id 1764900302082*

<strong>A: Map Phase:</strong>

<ul><li>Each worker executes Map operation on its assigned data portion</li><li>Transforms input data into key-value pairs</li><li>Example (word count): Input "hello world hello" → Output: {("hello", 1), ("world", 1), ("hello", 1)}</li></ul><strong>Shuffle and Sort Phase:</strong>
<ul><li>Worker nodes reorganize key-value pairs</li><li>Groups all values with same key together</li><li>Prepares data for Reduce phase</li><li>Example:<ul><li>Worker 1: "the" appears 3 times</li><li>Worker 2: "the" appears 7 times</li><li>Worker 3: "the" appears 100 times</li><li>Shuffle groups: "the" → [3, 7, 100]</li></ul></li></ul><strong>Purpose:</strong> Ensures all occurrences of same key are sent to same reducer for aggregation.

---

## What happens during the Reduce phase and what is the final output?
*Basic · id 1764900328609*

<strong>Reduce Phase:</strong>
<ul><li>Performs Reduce operation on each group of values</li><li>Aggregates/combines values for same key</li><li>Produces final result for each key</li><li>Example (word count): "the" → [3, 7, 100] → "the": 110</li></ul><strong>Output:</strong>
<ul><li>Final result written to storage or database</li><li>Example final output: {"the": 110, "hello": 5, "world": 2}</li></ul><strong>Complete Word Count Flow:</strong>
<ol><li>Map: "hello world hello" → [("hello", 1), ("world", 1), ("hello", 1)]</li><li>Shuffle: Group by key → {"hello": [1, 1], "world": [1]}</li><li>Reduce: Sum values → {"hello": 2, "world": 1}</li><li>Store results</li></ol>

---

## What are the main limitations of MapReduce?
*Basic · id 1764900358049*

<strong>Restrictive Data Processing Model:</strong>
<ul><li>Only works well with data that fits Map and Reduce steps</li><li>Not suitable for all types of data processing tasks</li><li>Tasks that don't align with this model need alternative approaches</li></ul><strong>When MapReduce Doesn't Fit:</strong>
<ul><li>Iterative algorithms (machine learning)</li><li>Graph processing (social networks)</li><li>Real-time processing (stream processing better)</li><li>Interactive queries (databases better)</li></ul><strong>Better Alternatives Exist For:</strong>
<ul><li>Real-time analytics: Stream processing (Kafka, Flink)</li><li>Interactive queries: SQL databases, data warehouses</li><li>Iterative ML: Spark (improves on MapReduce)</li><li>Graph algorithms: Graph databases (Neo4j)</li></ul><strong>Key Takeaway:</strong> MapReduce is powerful for batch processing large datasets that fit the Map-Reduce pattern, but it's not a universal solution. Understanding the concept is more important than mastering specific tools for system design interviews.
