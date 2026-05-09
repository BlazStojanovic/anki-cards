<!-- note_key: 89a93dfbd9d9 -->
BF: What is RAM?
BB: **R**andom **A**ccess **M**emory - active memory for the computer. Stores data, usually measured in **Byte**s (8 bits), it holds **values** indexed by their **address (you can think of each address indexes a byte of data).
**

---

<!-- note_key: 38da15ce88fa -->
BF: **Describe basic properties of a Static Array**
BB: - In statically typed languages arrays have to have an allocated size and type when initialized, known as **static arrays**, once a static array is full **it cannot store more values**
- **Reading from an array:** Read by providing the index, **O(1) time complexity**
- **Traversing an array:** Is linear in time **O(n) time complexity**
- **Deleting an element:** Overwrite memory via "empty" value indicator **(soft delete)**, deleting from the middle makes the array non-contiguous, so we need to shift the values back and replace the last element with an EOA indicator, **worst case O(n) time complexity**
- **Inserting an element:** Like deletion we need to perform a shift operation on all indices greater than the insertion index, **worst case O(n) time complexity**

---

<!-- note_key: 2b053edc3ea7 -->
BF: **Describe basic properties of a Dynamic Array**
BB: - You don't have to specify the size (initializes to some arbitrary size), **the dynamic array gets resized at runtime**

- As we add elements to the end of the array or **push** them onto the array, they resize, **O(1) time complexity**

- You can also remove or **pop the elements**, **O(1) time complexity**
- **I**f the original size is exceeded **another array is allocated (somewhere else in memory), which is 2x the size** and the original values are recopied into the 2nd array
- This results in an **amortized time-complexity is O(1)** for a single element (last term is dominating the computation), meaning that inserting **n** elements into a dynamic array is an **O(n)** operation.
- Of course as in the static array O(n) for insertion and deletion **in the middle**

---

<!-- note_key: 90f6e47f5d40 -->
BF: How do Stacks work?
BB: Basic operations:

- push: **O(1)**
- pop: **O(1)** (need to check if empty first!)
- peek/top (look at the last element): **O(1)**

**
**

**A stack can be implemented via a dynamic array and it is a LIFO (last in first out) data structure!**

---

<!-- note_key: bdacf449596b -->
BF: Describe the basics of a singly linked list
BB: Data structure which holds elements in an ordered sequence:

- Made out of **ListNode**s which contain two attributes: **(value, next)**
- When initializing a linked list we won't know where it is stored in RAM, likely not contiguous
- To traverse start at the head of the list and use a while loop (until next -> None is reached)

Operations on a singly linked list:

1. Appending can be done in **O(1)** time complexity (**even when inserting in the middle**)
2. Deleting can also be done in **O(1)** time complexity
3. Accessing an element is **O(n)** because non-contigous in memory and traversal is needed

---

<!-- note_key: fe8984100a4e -->
BF: What is a doubly linked list?
BB: A doubly linked list is a linked list which links to both the previous and the next node in the chain. 

```
class ListNode:
 def __init__(self, val):
 self.val = val
 self.next = None
 self.prev = None

\# Implementation for Doubly Linked List
class LinkedList:
 def __init__(self):
 \# Init the list with 'dummy' head and tail nodes which makes 
 \# edge cases for insert & remove easier.
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

 \# Remove first node after dummy head (assume it exists)
 def removeFront(self):
 self.head.next.next.prev = self.head
 self.head.next = self.head.next.next

 \# Remove last node before dummy tail (assume it exists)
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

## Operations:

| Operation | Big-O Time Complexity | Notes |
| --- | --- | --- |
| Access | O(n)O(n) |  |
| Search | O(n)O(n) |  |
| Insertion | O(1)O(1)\* | Assuming you have the reference to the node at the desired position |
| Deletion | O(1)O(1)\* | Assuming you have the reference to the node at the desired position |

---

<!-- note_key: 6fb9890bdf5a -->
BF: Describe the properties of a queue
BB: Queue is a data structure implementing a FIFO principle (first in first out). In its most basic form it supports enque and deque operations (add item to queue and take item out of queue). The easiest way to implement a queue is with a linked list as adding at head/tail and taking out at head/tail are both O(1) operations in a linked list.

---

<!-- note_key: 5fd33881e885 -->
BF: What are tail calls (recursion)?
BB: **Definition:** A tail call occurs when a function's last action is calling another function and immediately returning that result

**Example Comparison:**

*Not a tail call:*

```
def factorial(n):
    if n <= 1: return 1
    return n \* factorial(n - 1)  \# Must multiply after return
```

*Tail call:*

```
def factorial(n, acc=1):
    if n <= 1: return acc
    return factorial(n - 1, n \* acc)  \# Just return the call result
```

**Why It Matters:** With Tail Call Optimization (TCO), the compiler reuses the current stack frame instead of creating a new one, effectively transforming recursion into a loop:

- Without TCO: 1000 calls = 1000 stack frames = stack overflow risk
- With TCO: 1000 calls = 1 reused frame = constant memory

**Language Support:**

- ✅ Guaranteed: Scheme, Scala, some functional languages
- ❌ Not supported: Python, Java, C\#
- ⚠️ Spec'd but rarely implemented: JavaScript (ES6)

**Key Insight:** In most mainstream languages, tail recursion doesn't actually save you from stack overflow—you'd need to convert to iteration manually.

---

<!-- note_key: 9786e6332f56 -->
BF: How does insertion sort work, and what are its time/space complexities?
BB: **How It Works:** Insertion sort builds a sorted array one element at a time by repeatedly taking the next unsorted element and inserting it into its correct position among the already-sorted elements.

Think of it like sorting playing cards in your handyou pick up cards one at a time and insert each into its proper position.

**Algorithm Steps:**

1. Start with the second element (index 1)
2. Compare it with elements to its left
3. Shift larger elements one position right
4. Insert the current element in the correct position
5. Repeat for all remaining elements

**Example:**

```
[5, 2, 4, 6, 1, 3]
[2, 5, 4, 6, 1, 3]  // Insert 2
[2, 4, 5, 6, 1, 3]  // Insert 4
[2, 4, 5, 6, 1, 3]  // 6 already in place
[1, 2, 4, 5, 6, 3]  // Insert 1
[1, 2, 3, 4, 5, 6]  // Insert 3
```

**Time Complexity:**

- Best case: **O(n)** - when array is already sorted
- Average case: **O(n²)** - random order
- Worst case: **O(n²)** - when array is reverse sorted

**Space Complexity:** **O(1)** - sorts in place, only needs constant extra space

---

<!-- note_key: 54a2ef39a913 -->
BF: How does merge sort work, and what are its time/space complexities?
BB: **How It Works:** Merge sort is a divide-and-conquer algorithm that recursively splits the array in half until each piece has one element, then merges those pieces back together in sorted order.

**Algorithm Steps:**

1. **Divide:** Split the array into two halves
2. **Conquer:** Recursively sort each half
3. **Combine:** Merge the two sorted halves into one sorted array

**Example:**

```
[38, 27, 43, 3, 9, 82, 10]

Split:
[38, 27, 43, 3] | [9, 82, 10]
[38, 27] [43, 3] | [9, 82] [10]
[38] [27] [43] [3] | [9] [82] [10]

Merge:
[27, 38] [3, 43] | [9, 82] [10]
[3, 27, 38, 43] | [9, 10, 82]
[3, 9, 10, 27, 38, 43, 82]
```

**The Merge Process:** Compare the first elements of each sorted half, take the smaller one, repeat until both halves are exhausted.

**Time Complexity:**

- Best case: **O(n log n)**
- Average case: **O(n log n)**
- Worst case: **O(n log n)**

All cases are the same! Always divides into log n levels, and merging at each level takes O(n) time.

**Space Complexity:** **O(n)** - requires additional arrays for merging (not in-place)

**
**

**When to Use:**

- Large datasets where consistent O(n log n) performance is needed
- When stability is required
- Linked lists (can be done in O(1) space)
- External sorting (data doesn't fit in memory)
- When worst-case guarantees matter

**Key Properties:**

- ✅ Stable (maintains relative order of equal elements)
- ✅ Predictable performance (always O(n log n))
- ✅ Parallelizable (each half can be sorted independently)
- ❌ Not in-place (requires O(n) extra space)
- ❌ Slower than quicksort in practice due to memory overhead

**Comparison to Other Sorts:**

- More consistent than quicksort (no O(n²) worst case)
- Much faster than insertion sort on large data
- Uses more memory than quicksort or heap sort

---

<!-- note_key: 0c135d6b9d2e -->
BF: How does quicksort work, and what are its time/space complexities?
BB: **How It Works:** Quicksort is a divide-and-conquer algorithm that picks a "pivot" element, partitions the array so all elements smaller than the pivot are on the left and larger ones on the right, then recursively sorts each partition.

**Algorithm Steps:**

1. **Choose a pivot** (various strategies: first, last, random, median-of-three)
2. **Partition:** Rearrange array so elements < pivot are left, elements > pivot are right
3. **Recursively sort** the left partition
4. **Recursively sort** the right partition

**Example:**

```
[10, 7, 8, 9, 1, 5]  // Pick pivot = 5

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

Final: [1, 5, 7, 8, 9, 10]
```

**Time Complexity:**

- Best case: **O(n log n)** - pivot always splits evenly
- Average case: **O(n log n)** - random pivot choices
- Worst case: **O(n²)** - pivot is always smallest/largest (already sorted array with bad pivot choice)

**Space Complexity:**

- **O(log n)** - recursion stack depth for balanced partitions
- **O(n)** worst case - if partitions are extremely unbalanced

**When to Use:**

- Large datasets where average-case performance matters
- When memory is limited (in-place sorting)
- General-purpose sorting (often the default in standard libraries)
- Random or unsorted data

**Key Properties:**

- ✅ In-place (uses minimal extra memory)
- ✅ Cache-efficient (good locality of reference)
- ✅ Fast in practice (lower constant factors than merge sort)
- ❌ Not stable (may change relative order of equal elements)
- ❌ Worst-case O(n²) can be triggered by poor pivot selection

**Optimizations:**

- **Randomized pivot:** Avoid worst-case on sorted data
- **Median-of-three:** Pick median of first, middle, last elements
- **Hybrid approach:** Switch to insertion sort for small subarrays (~10 elements)
- **Three-way partitioning:** Handle many duplicate values efficiently

**Comparison to Other Sorts:**

- Typically faster than merge sort in practice (better cache performance, in-place)
- Less predictable than merge sort (can degrade to O(n²))
- Much faster than insertion sort on large datasets

---

<!-- note_key: 089b510c97db -->
BF: How does bucket sort work, and what are its time/space complexities?
BB: **How It Works:** Bucket sort distributes elements into a number of "buckets," sorts each bucket individually (often using another algorithm), then concatenates the sorted buckets. It works best when input is uniformly distributed across a range.

**Algorithm Steps:**

1. **Create buckets:** Set up empty buckets to cover the range of values
2. **Distribute:** Place each element into its appropriate bucket
3. **Sort buckets:** Sort each bucket individually (insertion sort, quicksort, etc.)
4. **Concatenate:** Combine all buckets in order

**Example:**

```
Array: [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]
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

Result: [0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]
```

**Time Complexity:**

- Best case: **O(n + k)** - uniformly distributed data, k = number of buckets
- Average case: **O(n + k)** - when elements distribute evenly
- Worst case: **O(n²)** - all elements go into one bucket (degenerates to sorting algorithm used within buckets)

**Space Complexity:** **O(n + k)** - need space for n elements plus k buckets

**
**

**When to Use:**

- Data is uniformly distributed over a known range
- Floating-point numbers in a bounded range
- When linear time is needed and data distribution is favorable
- External sorting of large datasets

**Key Properties:**

- ✅ Can achieve O(n) average case with proper distribution
- ✅ Stable if the bucket sorting algorithm is stable
- ✅ Good for parallel processing (buckets can be sorted independently)
- ❌ Performance heavily depends on data distribution
- ❌ Requires knowledge of input range
- ❌ High space overhead

**Important Considerations:**

- **Bucket count:** Too few buckets --> many elements per bucket (slower). Too many buckets --> high memory overhead
- **Distribution matters:** Works best with uniform distribution; terrible with skewed data
- **Bucket sorting algorithm:** Typically use insertion sort for small buckets, quicksort for larger ones

**Comparison to Other Sorts:**

- Not comparison-based (can break O(n log n) barrier)
- Related to radix sort and counting sort
- Much faster than O(n log n) sorts when conditions are right
- Unreliable performance with poor data distribution

---

<!-- note_key: 60c3ff5956b0 -->
BF: How does radix sort work, and what are its time/space complexities?
BB: **How It Works:** Radix sort processes integers by sorting them digit by digit, starting from the least significant digit (LSD) to the most significant digit (MSD), or vice versa. It uses a stable sorting algorithm (typically counting sort) as a subroutine for each digit position.

**Algorithm Steps (LSD approach):**

1. **Find the maximum number** to determine number of digits
2. **For each digit position** (from rightmost to leftmost):
   - Use a stable sort (counting sort) to sort by that digit
   - Maintain relative order of elements with same digit value
3. After processing all digits, array is fully sorted

**Example:**

```
Array: [170, 45, 75, 90, 802, 24, 2, 66]

Sort by ones place:
[170, 90, 802, 2, 24, 45, 75, 66]

Sort by tens place:
[802, 2, 24, 45, 66, 170, 75, 90]

Sort by hundreds place:
[2, 24, 45, 66, 75, 90, 170, 802]

Final: [2, 24, 45, 66, 75, 90, 170, 802]
```

**Time Complexity:**

- Best case: **O(d × (n + k))**
- Average case: **O(d × (n + k))**
- Worst case: **O(d × (n + k))**

Where:

- d = number of digits in the largest number
- n = number of elements
- k = range of each digit (typically 10 for decimal)

Simplified: **O(d × n)** when k is constant

**Space Complexity:** **O(n + k)** - for the counting sort subroutine and output array

**When to Use:**

- Sorting integers with a limited number of digits
- Fixed-length strings or records
- When the number of digits (d) is small relative to n
- Need stable sorting with linear time

**Key Properties:**

- ✅ Stable (maintains relative order of equal elements)
- ✅ Not comparison-based (can break O(n log n) barrier)
- ✅ Predictable performance
- ✅ Works well with integers, strings, fixed-length keys
- ❌ Limited to data that can be represented as digits/keys
- ❌ Not in-place (requires extra space)
- ❌ Performance degrades if d is large

**LSD vs MSD:**

- **LSD (Least Significant Digit first):** Simpler, processes all digits, naturally stable
- **MSD (Most Significant Digit first):** Can terminate early for variable-length data, requires recursion, more complex

**Important Considerations:**

- **Digit range matters:** Binary (base-2) requires more passes but simpler counting; higher bases reduce passes but increase space
- **Not suitable for:** Floating-point numbers, negative numbers (without modification), variable-length unbounded data
- **Common modifications:** Handle negative numbers by sorting negatives and positives separately, then combining

**Comparison to Other Sorts:**

- Faster than O(n log n) comparison sorts when d is small
- Related to bucket sort and counting sort
- More predictable than bucket sort (doesn't depend on distribution)
- Better than counting sort for large ranges (only processes digits, not full range)

---

<!-- note_key: e93dd4bbb8b4 -->
BF: What is a disk and what are its key characteristics?
BB: A disk is the primary storage device in a computer. It is persistent (data remains regardless of machine state), typically stores data on the order of TBs (terabytes)

---

<!-- note_key: 3d9fe00fa78d -->
BF: What is RAM and how does it differ from disk storage?
BB: RAM (Random Access Memory) is volatile memory used for storing information. It's typically smaller (1GB-128GB) and more expensive than disk, but reading/writing is significantly faster (microseconds vs milliseconds). Data is erased when the computer shuts down.

---

<!-- note_key: da3ef57e25fa -->
BF: How fast is RAM compared to disk? Give specific examples.
BB: Writing 1 MB to RAM takes microseconds (1/10⁶ seconds), while writing the same amount to disk takes milliseconds (1/10³ seconds) - making RAM roughly 1000x faster.

---

<!-- note_key: 4cd3d3a064be -->
BF: What does volatile memory mean?
BB: Volatile memory means the data gets erased once the computer is shut down. This is why you need to save your work to disk before shutting down.

---

<!-- note_key: f27f8ee3dbbe -->
BF: What is the CPU and what is its primary role?
BB: The CPU (Central Processing Unit) is the "brain" of the computer. It acts as an intermediary between RAM and disk, reading/writing from both. It fetches instructions from RAM, decodes them, executes them, and performs all computations (addition, subtraction, multiplication, etc.).

---

<!-- note_key: b44e0eff0897 -->
BF: What is (the CPU) cache and why is it important?
BB: Cache is extremely fast memory that lies on the same die as the CPU. It stores data on the order of KBs or tens of MBs. It's checked before RAM and disk during read operations, making data access much faster when the requested data is cached.

---

<!-- note_key: 9e0a0c58aaa9 -->
BF: What are the typical cache levels in a CPU?
BB: Most CPUs have L1, L2, and L3 cache levels, which are physical components much faster than RAM.

---

<!-- note_key: 885c2ae2c0e0 -->
BF: What type of memory is cache, and why is it faster?
BB: Cache is SRAM (Static RAM). It's faster than regular RAM both because it's part of the CPU and because of its underlying technology.

---

<!-- note_key: 9e7c355874b9 -->
BF: What is the storage capacity hierarchy from largest to smallest? (CPU, RAM, Disk)
BB: 1. Disk (largest - TBs)
2. RAM (medium - GBs)
3. Cache (smallest - KBs to MBs)

---

<!-- note_key: 5ff5dd9affe1 -->
BF: What is a byte, bit, and their relationship?
BB: A bit is the smallest unit of measure in computers - a binary measure represented by 0 or 1. A byte is made up of 8 bits.

---

<!-- note_key: f850f4dbfb9f -->
BF: Define these storage units: KB, MB, GB, TB
BB: - KB (kilobyte) = 10³ bytes (thousand)
- MB (megabyte) = 10⁶ bytes (million)
- GB (gigabyte) = 10⁹ bytes (billion)
- TB (terabyte) = 10¹² bytes (trillion)

---

<!-- note_key: 8dd38402b2e4 -->
BF: What happens when you write code and run it in terms of computer architecture?
BB: Your code is translated into binary instructions stored in RAM. The CPU reads and executes these instructions, which may involve manipulating data stored elsewhere in RAM or on disk (like opening and reading a file).

---

<!-- note_key: 024e7ca22d29 -->
BF: What is vertical scaling and what are its limitations?
BB: Vertical scaling means upgrading components within the same computer (adding more RAM, upgrading CPU cores/speed). Limitations include: every computer has a maximum upgrade capacity, it's less powerful for large systems, and it creates a single point of failure (if the server goes down, everything stops).

---

<!-- note_key: 83c5560aeb3a -->
BF: What is horizontal scaling and why is it preferred for large systems despite being more complex
BB: Horizontal scaling means running multiple servers and distributing user requests among them. It's preferred because: it's more powerful, uses commodity (inexpensive) hardware, prevents single points of failure, and allows unlimited growth. However, it requires significant engineering effort to ensure servers communicate properly and requests are distributed evenly.

---

<!-- note_key: c43cb1ad8870 -->
BF: When might vertical scaling be the better choice over horizontal scaling?
BB: A: Vertical scaling is better when:

- The application is simple with predictable, moderate traffic
- Engineering resources are limited (horizontal requires significant effort for server communication and load distribution)
- The maximum capacity of a single upgraded machine is sufficient for your needs
- You want faster implementation without the complexity of distributed systems
- The cost of engineering effort outweighs the cost of better hardware

---

<!-- note_key: f1294c9d44fb -->
BF: What is a load balancer and why is it necessary in horizontally scaled systems?
BB: A load balancer determines which requests go to which server in a multi-server environment. It evenly distributes incoming requests across a group of servers, preventing any single server from becoming overwhelmed and ensuring consistent performance across the system.

---

<!-- note_key: 0ff7d86e1f5b -->
BF: What is the difference between logging services and metrics services, and why do we need both?
BB: - **Logging**: Records activity logs (what happened, errors, events before crashes) - tells you *what* occurred
- **Metrics**: Collects quantitative data (CPU usage, RAM usage, network traffic) - tells you *how well* the system is performing

Both are needed because logs don't show resource bottlenecks, and metrics don't show specific events or errors. Logs are commonly written to external servers for better reliability.

---

<!-- note_key: aab5b6a5b6a9 -->
BF: Give an example of when you'd need metrics over logs to diagnose a problem.
BB: If your server is slow but not crashing or throwing errors, logs won't reveal the issue. Metrics would show that RAM has become a bottleneck or CPU resources are maxed out, identifying the performance constraint that logs wouldn't capture.

---

<!-- note_key: 0d521698140a -->
BF: Why is external storage (database, cloud) preferred over built-in server storage?
BB: Built-in server storage has limitations in terms of size and creates a single point of failure. External storage systems connected through a network provide:

- Greater capacity and scalability
- Better reliability (data survives if server fails)
- **Statelessness**: Servers can remain stateless, meaning any server can handle any request since all persistent data lives externally
- **Shared access**: Multiple servers in horizontally scaled systems can access the same data, enabling load balancing without data inconsistency
- If a server crashes or is replaced, no data is lost since state is externalized

---

<!-- note_key: 5b1b19a1559b -->
BF: **What is vertical scaling and what are its limitations?**
BB: Vertical scaling means upgrading components within the same computer (adding more RAM, upgrading CPU cores/speed). Limitations include: every computer has a maximum upgrade capacity, it's less powerful for large systems, and it creates a single point of failure (if the server goes down, everything stops).

---

<!-- note_key: d76cd7183026 -->
BF: What is horizontal scaling and why is it preferred for large systems despite being more complex?
BB: Horizontal scaling means running multiple servers and distributing user requests among them. It's preferred because: it's more powerful, uses commodity (inexpensive) hardware, prevents single points of failure, and allows unlimited growth. However, it requires significant engineering effort to ensure servers communicate properly and requests are distributed evenly.

---

<!-- note_key: 00641ba7f43d -->
BF: What are the three fundamental operations in system design, and why are they more challenging at scale?
BB: 1. **Moving Data**: Unlike moving data between disk/RAM/CPU within one machine, distributed systems move data between clients and servers that may be geographically dispersed worldwide
2. **Storing Data**: Choosing optimal storage (databases, blob stores, file systems, distributed file systems) based on use case - like choosing the right data structure
3. **Transforming Data**: Processing data efficiently (e.g., calculating success/failure rates from logs, filtering medical records by age)

The challenge at scale is that these operations involve network latency, coordination, and complexity that don't exist in single-machine systems.

---

<!-- note_key: e3568cb35df5 -->
BF: What is availability, how is it calculated, and why is 99% availability considered poor?
BB: Availability = uptime / (uptime \+ downtime) - the percentage of time a system is operational.

99% availability means 3.65 days of downtime per year (365 × 0.01). For a global business, this translates to:

- Lost revenue during outages
- Users unable to access services
- Potential damage to reputation
- SLA violations

This is why companies target 99.9% (8.76 hours downtime/year) or higher, with mission-critical systems aiming for 99.999% ("five nines" = 5.26 minutes downtime/year).

---

<!-- note_key: 6d651d1b8b9b -->
BF: What is the significance of measuring availability in "nines" and what does "five nines" mean?
BB: Availability is measured in consecutive 9s (99%, 99.9%, 99.99%, etc.). Each additional nine reduces downtime by a factor of 10:

- 99% = 3.65 days downtime/year
- 99.9% = 8.76 hours downtime/year
- 99.99% = 52.56 minutes downtime/year
- 99.999% ("five nines") = 5.26 minutes downtime/year

"Five nines" is considered the gold standard for mission-critical systems, though it's extremely difficult and expensive to achieve.

---

<!-- note_key: 6e5780ac0ecb -->
BF: What are planned vs. unplanned downtime, and why can't we achieve 100% availability?
BB: - **Planned downtime**: Scheduled maintenance, software updates, backups, verification
- **Unplanned downtime**: Hardware/software failures, natural disasters, cyberattacks, network issues

100% availability is impossible because unplanned downtime cannot be completely eliminated - hardware fails, disasters occur, and unforeseen issues arise. The best we can do is minimize probability and impact through redundancy, monitoring, and fault tolerance.

---

<!-- note_key: 1c389f55624a -->
BF: SLA vs SLO
BB: **What is the difference between SLA and SLO, and how do they relate? Provide an example.** A:

- **SLA (Service Level Agreement)**: A contractual agreement between a company and clients guaranteeing specific uptime, responsiveness, and responsibilities. Violations may result in refunds or penalties.
- **SLO (Service Level Objective)**: Internal objectives/targets your team must hit to meet the SLA.

Example: AWS has a monthly SLA of 99.99% uptime. If they fail to meet this, they refund a percentage of service credit. AWS engineering teams would have internal SLOs (perhaps 99.995%) to ensure they stay above the SLA threshold with a buffer.

---

<!-- note_key: 4176abd31933 -->
BF: What is the difference between availability and reliability?
BB: - **Availability**: Whether the system is up and accessible (are the servers running?)
- **Reliability**: Whether the system performs correctly without failures when it IS available (do requests succeed? Does it handle load? Does it resist attacks?)

A system can be highly available (servers are up) but unreliable (requests fail, errors occur, can't handle load). Both are needed for a robust system.

---

<!-- note_key: 6a1a0f439a45 -->
BF: What is fault tolerance and how does it differ from redundancy?
BB: - **Fault Tolerance**: The system's ability to detect and heal itself from failures - disabling faulty functions, reverting to different modes, or switching to backup servers
- **Redundancy**: Having backup/additional components (servers, databases, network paths) that enable fault tolerance

**Relationship**: Redundancy is the *mechanism* that enables fault tolerance. You can't have fault tolerance without redundancy, but redundancy alone doesn't guarantee proper fault handling.

---

<!-- note_key: 3310155e0858 -->
BF: What is the difference between passive (backup) redundancy and active-active redundancy?
BB: - **Passive/Backup Redundancy**: A backup server "shadows" the primary server but remains inactive until the primary fails. The backup is unessential during normal operation.
- **Active-Active Redundancy**: Multiple servers are all actively handling requests simultaneously, sharing the load. If one fails, others continue operating.

Active-active is preferred in modern systems because it improves both availability AND throughput. When documentation mentions "redundancy," it typically means active-active unless specified otherwise.

---

<!-- note_key: 8b550a1bc8d1 -->
BF: Why isn't redundancy a perfect solution to DDoS attacks?
BB: During a DDoS attack, the primary server gets overwhelmed with requests, and legitimate users can't get responses. When the backup server takes over, it will ALSO become overloaded with the same attack traffic. Redundancy helps with hardware failures and normal load distribution, but doesn't solve the fundamental problem of malicious traffic overwhelming capacity. Proper DDoS mitigation requires additional strategies like rate limiting, traffic filtering, and CDNs.

---

<!-- note_key: 2900c68de400 -->
BF: What is throughput and what are three common ways to measure it?
BB: Throughput is the amount of data or operations a system can handle over a period of time. Three measurements:

1. **Requests/second**: Number of user requests handled (user-centric metric)
2. **Queries/second**: Number of database queries executed (database-centric metric)
3. **Bytes/second**: Maximum data transmitted over network (bandwidth-centric metric, useful for data pipelines processing large volumes of non-user data)

The appropriate metric depends on context - use requests/second for user-facing services, bytes/second for data processing pipelines.

---

<!-- note_key: efda7f4c8759 -->
BF: How do vertical and horizontal scaling each improve throughput differently?
BB: - **Vertical Scaling**: A single more powerful server can process each request faster and handle more concurrent requests due to increased CPU/RAM. But there's a ceiling - one machine has physical limits.
- **Horizontal Scaling**: Multiple servers can process requests in parallel. Total throughput = (throughput per server) × (number of servers). Theoretically unlimited scaling, though coordination overhead increases.

For high throughput systems, horizontal scaling is preferred despite added complexity.

---

<!-- note_key: ef89690b645c -->
BF: What is latency and how does it differ from throughput? Can you have high throughput but high latency?
BB: - **Latency**: Time delay for a SINGLE request to complete (client sends --> server responds)
- **Throughput**: Number of requests completed per unit time

**Yes, you can have high throughput with high latency**. Example: A server processes 1,000 requests/second (high throughput) but each request takes 5 seconds to complete (high latency). This happens when many requests are processed in parallel but each takes a long time individually.

Analogy: A highway with 10 lanes (high throughput - many cars pass per minute) but a 50 mph speed limit (high latency - each car takes longer to reach destination).

---

<!-- note_key: 9ec157753328 -->
BF: What is a CDN (Content Delivery Network) and how does it improve system performance and security?
BB: A CDN is a geographically distributed network of servers that cache and deliver content from locations closer to users.

**Performance benefits:**

- Reduced latency (users connect to nearby servers instead of distant origin servers)
- Reduced load on origin servers (CDN handles static content like images, videos, CSS, JavaScript)
- Better throughput for content delivery

**Security benefits:**

- DDoS protection: CDNs have massive capacity to absorb attack traffic before it reaches your servers
- Traffic filtering: CDN can identify and block malicious requests
- Geographic distribution: Attacks get spread across CDN's global infrastructure instead of hitting your single server

**Example**: Cloudflare, AWS CloudFront, Akamai act as a protective layer between users and your servers, serving cached content and filtering threats.

---

<!-- note_key: b839ad16eb27 -->
BF: Where does latency exist beyond network communications?
BB: Latency exists within computer components themselves:

- CPU fetching from cache vs RAM vs disk
- RAM read/write operations
- Disk seek times
- Cache lookups

Network latency (milliseconds to seconds) is typically orders of magnitude higher than internal component latency (nanoseconds to milliseconds), which is why distributed systems are more challenging than single-machine programs.

---

<!-- note_key: b32b9d4d3caa -->
BF: What is an IP address and what problem did IPv6 solve that IPv4 created?
BB: An IP address is a unique numeric identifier for every device connected to a network.

**IPv4 (32-bit):**

- Format: 0.0.0.0 to 255.255.255.255 (four octets, each 8 bits)
- Theoretically allows ~4.3 billion unique addresses
- Problem: Due to design choices and exponential internet growth, actual usable addresses are much lower, leading to address scarcity

**IPv6 (128-bit):**

- Format: xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx (8 groups of 16 bits in hexadecimal)
- Provides virtually infinite addresses, making address exhaustion highly improbable
- Created specifically to solve IPv4's scarcity problem

---

<!-- note_key: fb59a3bb36ea -->
BF: What is a data packet and what are its three main components?
BB: A data packet is the fundamental unit of data transmitted over a network. It consists of:

1. **Header**: Contains metadata including source and destination IP addresses, protocol information (called the IP Header)
2. **Data/Payload**: The actual content being transmitted (e.g., HTTP request/response, application data)
3. **Trailer**: Contains error-checking information and end-of-packet markers

Analogy: Like an envelope (header with addresses) containing a letter (payload) with a seal (trailer). Large data is typically divided into multiple packets that may arrive out of order.

---

<!-- note_key: 0326e543a8f3 -->
BF: What is the Internet Protocol (IP) and what is its primary responsibility?
BB: IP (Internet Protocol) is the fundamental protocol that governs how data packets are transmitted over the internet.

**Primary responsibilities:**

- Routing packets from source to destination using IP addresses
- Defining the structure of the IP header (source/destination addresses, packet size, etc.)
- Operating at the network layer to set the physical pathway for data transmission

**Important limitation**: IP does NOT guarantee packets arrive in order or even that they arrive at all - it's "best effort" delivery. This is why we need TCP on top of IP.

---

<!-- note_key: c8606ab361c6 -->
BF: What is TCP (Transmission Control Protocol) and how does it differ from IP? Why do we need both?
BB: **TCP** ensures reliable, ordered delivery of data packets. **IP** handles routing packets to the correct destination.

**TCP responsibilities:**

- Assigns sequence numbers to packets so they can be reassembled in correct order at destination
- Confirms packets were received (acknowledgments)
- Retransmits lost packets
- Operates at the transport layer

**Why we need both (TCP/IP together):**

- **IP alone**: Gets packets to the right place, but they might arrive out of order, duplicated, or not at all
- **TCP alone**: Can't route packets without IP addresses
- **Together**: IP routes packets to destination, TCP ensures they arrive correctly and in order

Analogy: IP is like the postal service getting mail to an address. TCP is like numbering pages of a letter and asking for confirmation they arrived, then resending missing pages.

---

<!-- note_key: d03c964079c0 -->
BF: What is the structure of network protocols in layers, and where do IP, TCP, and HTTP sit?
BB: Network protocols are organized into hierarchical layers, each with specific responsibilities:

**Network Layer (IP):**

- Sets the physical pathway for data transmission
- Routes packets using IP addresses

**Transport Layer (TCP):**

- Ensures reliable, ordered transmission of data
- Handles packet sequencing and retransmission

**Application Layer (HTTP):**

- Enables interaction between clients and servers
- Where application-specific protocols operate (HTTP, FTP, SMTP, etc.)

**Layer interaction**: Each layer communicates with adjacent layers. HTTP data becomes TCP payload, TCP packet becomes IP payload. As developers, we primarily work with the application layer, treating lower layers as abstractions.

---

<!-- note_key: 658da13fa652 -->
BF: As a developer, which part of the packet should you care about most, and how does HTTP data fit into TCP/IP packets?
BB: Developers primarily care about the **application data** portion of the packet.

**Packet nesting structure:**

```
IP Packet
└── IP Header (source/dest IP, routing info)
    └── TCP Packet (IP payload)
        └── TCP Header (sequence numbers, ports)
            └── Application Data (TCP payload)
                └── HTTP Header (method, path, status)
                    └── HTTP Payload (your actual data)
```

**Examples of application data:**

- HTTP POST request: Your data is in the HTTP payload section
- HTTP GET request: The response data is in the HTTP payload section

**Key insight**: You can treat IP and TCP headers as a "black box" - the networking stack handles them automatically. You work with HTTP requests/responses, which are encapsulated within TCP, which is encapsulated within IP.

---

<!-- note_key: 5cb5267d0c93 -->
BF: What is the difference between public and private IP addresses, and when is each used?
BB: **Public IP Address:**

- Unique identifier supplied by ISP (Internet Service Provider)
- Reachable from any device on the internet
- Used for communication between devices on separate networks
- Example: Your home router's external address, web servers

**Private IP Address:**

- Used within a confined network (home, office LAN - Local Area Network)
- NOT accessible from the broader internet
- Multiple devices across different networks can have the same private IP without conflict
- Common ranges: 192.168.x.x, 10.x.x.x, 172.16.x.x

**Key difference**: Public IPs have global accessibility; private IPs only work within their local network. This is why your laptop can have IP 192.168.1.5 at home and someone else's laptop can have the same IP - they're in different private networks.

---

<!-- note_key: 56e1954b5f87 -->
BF: What is the difference between static and dynamic IP addresses, and why are dynamic IPs typically used for clients rather than servers?
BB: **Dynamic IP Address:**

- Temporarily allocated and can change each time the device connects
- Automatically assigned (no manual configuration needed)
- Commonly used in home networks and for client devices
- Provides flexibility in managing limited available IP addresses

**Static IP Address:**

- Manually configured and never changes
- Remains the same across reboots and reconnections
- Typically used for servers

**Why servers need static IPs:**

- Clients need to know where to find the server (consistent address)
- DNS records point to specific IPs - changing IPs would break connections
- Example: If Google's servers had dynamic IPs, google.com wouldn't know which IP to point to

**Why clients use dynamic IPs:**

- Clients initiate connections (they find the server, not vice versa)
- More efficient use of limited IPv4 address space
- Easier management - no manual configuration needed
- Most home users don't need incoming connections

---

<!-- note_key: 13462a40f66c -->
BF: What are ports, why can't two applications run on the same port, and what is their numeric range?
BB: **Ports** are 16-bit numeric identifiers (0-65,535) used to distinguish between multiple applications or services running on a single device.

**Why ports are needed:** A single device (one IP address) may run multiple networked applications simultaneously. Ports allow the operating system to route incoming data to the correct application.

**Why two applications can't share a port:** When an application binds to a port, it has exclusive access. If another application tries to use the same port, it gets an "address already in use" error. The OS can't determine which application should receive incoming data on a shared port.

**Common examples:**

- Port 80: HTTP (web traffic)
- Port 443: HTTPS (secure web traffic)
- Port 4200: Angular development server (default)
- Port 3000: React/Node.js development (common default)
- Port 22: SSH
- Port 5432: PostgreSQL database

**Full address format**: IP:Port (e.g., 192.168.1.5:8080)

---

<!-- note_key: a1fee9a21d5e -->
BF: How do IP addresses and ports work together to enable communication? Provide a concrete example.
BB: An IP address identifies the **device**, while a port identifies the **specific application** on that device.

**Format**: `IP_ADDRESS:PORT`

**Concrete example:** Your computer (192.168.1.100) is running:

- Web browser connecting to Netflix
- Spotify streaming music
- Zoom video call

Netflix server (172.217.15.110):

- Web server on port 443 (HTTPS)
- API server on port 8080
- Streaming server on port 8443

**Communication flow:**

```
Your browser --> 192.168.1.100:53241 (random client port)
    ↓
Netflix web --> 172.217.15.110:443
```

Without ports, your computer couldn't run multiple network applications simultaneously, and the Netflix server couldn't differentiate between requests for web pages vs. streaming vs. API calls. The combination of IP \+ port creates a unique endpoint called a **socket**.

---

<!-- note_key: 03bc221abdca -->
BF: How does TCP ensure reliable delivery of packets, and what is the mechanism it uses to detect packet loss?
BB: TCP ensures reliability through an acknowledgment system:

**Process:**

1. Sender transmits a packet
2. Receiver sends back an acknowledgment (ACK) confirming receipt
3. If sender doesn't receive ACK within a timeout period, it assumes the packet was lost
4. Sender automatically retransmits the lost packet

**Key feature:** This is bidirectional - both client and server can send data and acknowledgments. TCP tracks every packet and guarantees all packets are received in the correct order.

**Consequence:** This reliability comes at the cost of overhead (extra acknowledgment packets) and slower transmission (waiting for confirmations).

---

<!-- note_key: 6863f80e4d36 -->
BF: What is the TCP 3-way handshake and why is it necessary before data transmission can begin?
BB: The 3-way handshake is TCP's connection establishment process that ensures both devices are ready and able to communicate reliably.

**The three steps:**

1. **SYN**: Client sends synchronization request to server ("I want to connect")
2. **SYN-ACK**: Server acknowledges and sends its own synchronization ("I acknowledge and am ready too")
3. **ACK**: Client acknowledges server's response ("Connection confirmed, let's begin")

**Why it's necessary:**

- Establishes a reliable two-way connection BEFORE any real data is sent
- Ensures both sides are ready to send and receive
- Synchronizes sequence numbers for packet ordering
- Confirms both devices can reach each other

**Trade-off:** This handshake adds latency (extra round trips) before data transmission, but guarantees a stable connection for the data exchange that follows.

---

<!-- note_key: c8c20c91ca56 -->
BF: What are the main drawbacks of TCP's reliability features?
BB: **1. Overhead:**

- Extra packets for acknowledgments (ACKs)
- Connection setup requires 3-way handshake before any data flows
- Connection teardown also requires packet exchange
- Each lost packet triggers retransmission, creating more traffic

**2. Slower speed:**

- Must wait for ACKs before continuing (or wait for timeout to retransmit)
- Packet reordering takes time
- The more reliable you make it, the more you sacrifice speed

**3. Connection state:**

- Both sides must maintain connection state/information
- Resources are consumed even when idle

**When this matters:** For applications prioritizing speed over perfect delivery (gaming, live streaming), these drawbacks make TCP unsuitable.

---

<!-- note_key: 7f8267166b3c -->
BF: What is UDP and how does it fundamentally differ from TCP in terms of reliability and connection?
BB: UDP (User Datagram Protocol) is a connectionless, "best effort" protocol that prioritizes speed over reliability.

**Key differences from TCP:**

```
| Aspect | TCP | UDP |
| --- | --- | --- |
| Connection | Requires 3-way handshake | Connectionless (just send) |
| Reliability | Guarantees delivery & order | No guarantees - packets may be lost/reordered |
| Acknowledgments | Yes - waits for ACKs | No - fire and forget |
| Retransmission | Yes - resends lost packets | No - lost packets stay lost |
| Speed | Slower (due to overhead) | Faster (minimal overhead) |
| Overhead | High | Low |
```

**Core philosophy:** UDP assumes the application will handle any errors or losses. It's just a thin wrapper around IP, adding only port numbers.

---

<!-- note_key: b1f4d6594e3d -->
BF: Why would you ever choose UDP over TCP given UDP's lack of reliability? Provide specific use cases.
BB: UDP is chosen when **speed and low latency** are more important than **guaranteed delivery**.

**Online Gaming:**

- Player position updates happen 60\+ times per second
- If one position packet is lost, the next one (arriving milliseconds later) makes it irrelevant
- Resending old position data is useless - by the time it arrives, it's outdated
- Better to skip the lost frame and move on

**Video/Audio Streaming (Live):**

- Live broadcasts can't wait for retransmission - the moment has passed
- A dropped video frame or audio blip is preferable to pausing the stream
- Users prefer minor glitches over buffering/lag
- Example: Zoom, Skype, live sports streaming

**DNS Queries:**

- Simple request/response - if no response, just resend the whole query
- Faster than TCP handshake overhead for single packets
- Most DNS queries succeed on first try

**IoT Sensor Data:**

- Temperature sensor sends readings every second
- If one reading is lost, the next reading compensates
- No need to retransmit old temperature data

**Key principle:** When new data replaces old data quickly, reliability becomes less important than speed.

---

<!-- note_key: 30957ed41702 -->
BF: Does using UDP mean the application has no reliability at all? How can applications handle UDP's unreliability?
BB: No - using UDP doesn't mean zero reliability. Applications can implement **custom reliability** at the application layer when needed.

**Application-layer solutions:**

1. **Selective reliability:** App decides which packets MUST arrive (e.g., game lobby chat) vs. which can be lost (player positions)
2. **Custom retransmission:** App can request specific missing data without TCP's strict ordering
3. **Forward error correction:** Send redundant data so losses can be reconstructed without retransmission
4. **Sequence numbers:** App adds its own numbering to detect packet loss/reordering
5. **Timeouts:** If critical data isn't acknowledged, resend it

**Examples:**

- **QUIC protocol** (used by HTTP/3): Built on UDP but adds reliability features
- **Video streaming**: Uses UDP with buffering and error concealment at application layer
- **Online games**: Use UDP for position updates but TCP for critical events (inventory changes, chat)

**Key insight:** UDP gives you control - you implement only the reliability you need, where you need it, without TCP's "all or nothing" approach.

---

<!-- note_key: dadaae6bbe19 -->
BF: Why don't software engineers typically need to choose between TCP and UDP directly?
BB: Most developers work at the **application layer**, using high-level protocols that have already made the TCP/UDP decision:

**Protocols that chose TCP for you:**

- **HTTP/HTTPS (1.1 and 2.0):** Web requests, REST APIs
- **FTP:** File transfers
- **SMTP:** Email sending
- **SSH:** Secure remote access
- **WebSockets:** Real-time web communication

**Protocols that chose UDP for you:**

- **DNS:** Domain name lookups
- **DHCP:** IP address assignment
- **HTTP/3 (QUIC):** Modern web protocol
- **WebRTC:** Video calling in browsers

**Why this matters:**

- When you make an HTTP request, TCP is handling reliability automatically
- When you use a WebRTC video library, UDP is handling speed automatically
- You only choose TCP vs UDP if you're building low-level networking code or game engines

**As a developer:** Focus on REST APIs, WebSockets, GraphQL, gRPC - let these protocols handle the transport layer decisions.

---

<!-- note_key: aeab99231441 -->
BF: What is DNS and what analogy helps explain it?
BB: DNS (Domain Name System) is a decentralized hierarchical naming system that converts human-readable domain names into numerical IP addresses. It's like the internet's phone book - just as we store contacts to avoid memorizing phone numbers, DNS helps us avoid memorizing IP addresses.

---

<!-- note_key: b7af1f75803c -->
BF: What organization manages the overall coordination, security, and operation of DNS?
BB: ICANN (Internet Corporation for Assigned Names and Numbers) manages the DNS infrastructure.

---

<!-- note_key: d52ff1f72eb2 -->
BF: Using the shopping center analogy, explain the relationship between ICANN and domain registrars.
BB: ICANN is like the shopping center's management company (ensuring smooth operation of the entire infrastructure), while domain registrars are like individual store lease providers (handling specific domain registration transactions).

---

<!-- note_key: 98359db9ba00 -->
BF: What is the role of domain registrars?
BB: Domain registrars are companies certified by ICANN to offer domain name registration services. They help search for available domain names, oversee the registration process, maintain registration records, and ensure domains are registered in the purchaser's name. (Google domains, GoDaddy, HostGator)

---

<!-- note_key: 87c65598f65c -->
BF: What is a DNS record?
BB: A DNS record stores information related to a domain or subdomain within the DNS system.

---

<!-- note_key: 7db35248f01b -->
BF: What are the two most common protocols (schemes) for web URLs?
BB: A: HTTP and HTTPS, with HTTPS now being the dominant protocol on the World Wide Web.

---

<!-- note_key: 393c7b64b592 -->
BF: What is FTP and what is its URL format?
BB: FTP (File Transfer Protocol) is used for accessing files and directories on remote servers and transferring files between systems. Format: ftp://ftp.example.com/pub/file.txt (where "pub" is the directory and "file.txt" is the file being accessed).

---

<!-- note_key: 05c8e672a4de -->
BF: What is SSH and what is its URL format?
BB: SSH (Secure Shell) is used for establishing secure remote connections to servers or computers. Format: ssh://username@example.com:22 (where "username" is for authentication, "example.com" is the server, and ":22" is the port number).

---

<!-- note_key: 7a254ca9e785 -->
BF: Break down the three components of domains.google.com
BB: 1) Subdomain: "domains", 2) Primary domain: "google", 3) Top-level domain (TLD): ".com"

---

<!-- note_key: e594f6d72441 -->
BF: What is the Client-Server Model?
BB: A computing architecture where clients (applications/systems) request services from servers (computers/software that provide resources). Roles can interchange based on context - a machine can be both client and server. The client is the 'caller' (initiates requests) and the server is the 'callee' (responds to requests).

---

<!-- note_key: c158594db07a -->
BF: What is a Remote Procedure Call (RPC)?
BB: A protocol that allows a program to execute functions on a separate machine over a network. It enables task management in distributed systems. Example: When searching YouTube, your browser calls functions on YouTube's servers (like listVideos()) even though it appears to execute locally.

---

<!-- note_key: 97ee1147ab61 -->
BF: What is HTTP and what layers does it build upon?
BB: Hypertext Transfer Protocol is a request/response protocol built on top of TCP and IP. IP delivers data packets, TCP ensures correct order, and HTTP defines how data should be formatted and transmitted over the web. It's used every time we make a call through a browser.

---

<!-- note_key: bdcff71e7bef -->
BF: What is HTTPS and why is it important?
BB: HTTPS is HTTP combined with TLS (Transport Layer Security). It provides secure, encrypted communication over networks. Regular HTTP is vulnerable to man-in-the-middle attacks, while HTTPS encrypts data, preventing unauthorized access or alteration.

---

<!-- note_key: 63fd837327c8 -->
BF: What are the main components of an HTTP Request? (four)
BB: 1. Request Method (GET, POST, PUT, DELETE)
2. Request URL/URI (specifies where request should go)
3. Headers (provide additional info about request/response)
4. Body (contains data payload for POST/PUT requests; GET requests have no body)

---

<!-- note_key: 17a52e751f6e -->
BF: What is the GET method and what makes it special?
BB: GET retrieves a resource from the server. It is idempotent, meaning making the same GET request multiple times always returns the same result. GET requests have no body since they retrieve data rather than send it.

---

<!-- note_key: 169f07d8fbe5 -->
BF: What is the POST method and is it idempotent?
BB: POST sends data to the server to create a new resource. It includes a request body (payload) with the data being sent. POST is NOT idempotent - making the same POST request multiple times can create multiple resources. Data is sent in the body (not URL) to prevent man-in-the-middle attacks.

---

<!-- note_key: 33d93ae737a0 -->
BF: What is the PUT method and why is it idempotent?
BB: PUT updates a resource with provided data. It IS idempotent because performing the same update operation multiple times with the same data leaves the resource in the same state. Example: updating a user's email to the same value repeatedly results in the same final state.

---

<!-- note_key: b47754029029 -->
BF: What is the DELETE method and why is it idempotent?
BB: DELETE removes a specified resource. It IS idempotent because while the response may differ (first call: 200 OK, subsequent calls: 404 Not Found), there is no change of state after the first deletion - the resource remains deleted.

---

<!-- note_key: e2b5ffc62414 -->
BF: What do HTTP status codes in the 1xx range indicate?
BB: Informational responses (100-199) acknowledge that the request was received and is being processed. Examples:

- 100 Continue: Everything is OK, proceed with request
- 101 Switching Protocols: Server is switching to a different protocol as requested

---

<!-- note_key: 01a980cc29b3 -->
BF: What do HTTP status codes in the 2xx range indicate?
BB: Successful responses (200-299) indicate the request succeeded. Examples:

- 200 OK: Request succeeded, returning requested data
- 201 Created: Request succeeded and a new resource was created (typically after POST or PUT)

---

<!-- note_key: b2f3e4d858b9 -->
BF: What do HTTP status codes in the 3xx range indicate?
BB: Redirection messages (300-399) indicate the resource has moved. Examples:

- 300 Multiple Choices: Request has multiple possible responses
- 301 Moved Permanently: Resource permanently moved to new location, server redirects client

---

<!-- note_key: 0a365eb6c82d -->
BF: What do HTTP status codes in the 4xx range indicate?
BB: Client error responses (400-499) indicate errors in the client's request. Examples:

- 400 Bad Request: Invalid or malformed request (e.g., incorrect parameters)
- 401 Unauthorized: Attempting to access protected resource without proper authentication
- 404 Not Found: Requested resource doesn't exist

---

<!-- note_key: 080102cf9413 -->
BF: What do HTTP status codes in the 5xx range indicate?
BB: Server error responses (500-599) indicate errors occurred on the server side while processing the request. These represent server failures or issues that prevented fulfilling the request.

- 500 Internal Server Error: Generic server error
- 503 Service Unavailable: Server temporarily unavailable

---

<!-- note_key: 6d05ef1e9ab2 -->
BF: What are HTTP Request Headers vs Response Headers?
BB: Request Headers: Provide info about the request including method, scheme, and accepted data types (e.g., Accept: text/html).

Response Headers: Provide info about the response or server, including Set-Cookie, Cache-Control, and Content-Type (e.g., Content-Type: text/html).

---

<!-- note_key: 0a053ecdd3c7 -->
BF: What is idempotency in HTTP methods?
BB: An operation is idempotent if performing it multiple times with the same parameters produces the same result/state. GET, PUT, and DELETE are idempotent. POST is NOT idempotent. This is crucial for reliability - idempotent operations can be safely retried without unintended side effects.

---

<!-- note_key: c3811e668f0f -->
BF: What is the relationship between SSL and TLS?
BB: TLS (Transport Layer Security) is the modern protocol for secure network communication. SSL (Secure Sockets Layer) is technically an outdated term that has been superseded by TLS, though SSL is still commonly used interchangeably with TLS. Together with HTTP, they form HTTPS.

---

<!-- note_key: 6a17df3ebf15 -->
BF: What is the main problem with using HTTP for real-time chat applications?
BB: HTTP is unidirectional and stateless, following a request-response model. The client must continuously poll the server at intervals to check for new messages. This creates overhead - checking too frequently (every second) overloads the server, while checking too infrequently (every minute) causes delayed message delivery. This makes HTTP suboptimal for real-time communication.

---

<!-- note_key: 851f2cb91ee4 -->
BF: What is WebSocket and what makes it different from HTTP?
BB: WebSocket is a protocol that enables bidirectional (two-way) communication between client and server. Unlike HTTP's unidirectional request-response model, WebSocket allows continuous, real-time data transmission in both directions simultaneously after an initial connection is established.

---

<!-- note_key: f955e57b332e -->
BF: What are common use cases for WebSocket?
BB: WebSocket is ideal for scenarios requiring real-time updates:

- Chat applications (instant messaging, group chats)
- Live streaming applications with real-time chat
- Real-time gaming applications
- Any application requiring instant, bidirectional communication

---

<!-- note_key: 30d75d28d106 -->
BF: How is a WebSocket connection established?
BB: - Client sends WebSocket handshake request (HTTP Upgrade request with special headers)
- Server responds with status code 101 (indicating protocol switch)
- Connection upgrades from HTTP to WebSocket
- Bidirectional data transfer begins The connection starts as HTTP then "upgrades" to WebSocket.

---

<!-- note_key: 289c8c836911 -->
BF: What is the HTTP status code 101 and what does it mean?
BB: Status code 101 means "Switching Protocols." It indicates that the server understands the Upgrade header field request and is switching to the protocol specified by the client (in this case, WebSocket). This is returned during the WebSocket handshake.

---

<!-- note_key: b6f64e0db023 -->
BF: What ports do WebSocket and WebSocket Secure use by default?
BB: - WebSocket (WS) uses port 80 (same as HTTP)
- WebSocket Secure (WSS) uses port 443 (same as HTTPS) This compatibility with existing web infrastructure makes WebSocket widely supported.

---

<!-- note_key: 08972528ce8b -->
BF: What transport protocol does WebSocket use?
BB: WebSocket uses TCP (Transmission Control Protocol). Most application-level protocols like HTTP, WebSocket, FTP, SMTP, and SSH use TCP. The exception is WebRTC, which uses UDP (User Datagram Protocol).

---

<!-- note_key: c551d8ae8513 -->
BF: What is polling and why is it problematic?
BB: Polling is when the client repeatedly checks the server at regular intervals to see if new data is available. It's problematic because:

- Too frequent polling (e.g., every second) creates excessive requests and server load
- Infrequent polling (e.g., every minute) causes delays in receiving updates
- Creates unnecessary overhead and resource consumption
- Not suitable for real-time applications

---

<!-- note_key: fe1078c19753 -->
BF: After the WebSocket handshake, how does data transfer work? (Web Sockets)
BB: After the handshake, the client and server can send data to each other in real-time over a persistent connection. This is truly bidirectional - both can initiate data transmission at any time without the client needing to constantly request updates. This is more efficient than repeatedly opening and closing HTTP connections.

---

<!-- note_key: a41701535d2a -->
BF: Does HTTP/2 multiplexing replace the need for WebSocket?
BB: No. While HTTP/2 allows multiplexing (multiple requests in parallel over a single TCP connection), it's not a perfect replacement for WebSocket. WebSocket remains prevalent because it provides true bidirectional, real-time communication, whereas HTTP/2 still follows the request-response model.

---

<!-- note_key: b85b1f3035f0 -->
BF: What is the difference between stateless and stateful protocols in the context of HTTP vs WebSocket?
BB: HTTP is stateless - each request is independent and the server doesn't maintain connection state between requests. WebSocket is stateful - it maintains a persistent connection between client and server, allowing continuous communication without re-establishing the connection for each message.

---

<!-- note_key: 3f00deffd7f0 -->
BF: What are the advantages of WebSocket over HTTP polling for real-time applications?
BB: - Lower latency: Instant message delivery without polling intervals
- Reduced overhead: Single persistent connection vs. multiple HTTP requests
- Lower server load: No constant polling requests
- True bidirectional: Server can push data without client requesting
- More efficient: No need to repeatedly open/close connections

---

<!-- note_key: c03fa1c98678 -->
BF: What is Binary Search and how does it work?
BB: Binary Search is an efficient algorithm for finding a target value in a **sorted** array. It works by:

1. Compare target with middle element
2. If target equals middle, return the index
3. If target is less than middle, search the left half
4. If target is greater than middle, search the right half
5. Repeat until target is found or search space is empty

Time Complexity: O(log n) Space Complexity: O(1) iterative, O(log n) recursive **Critical requirement: Array must be sorted**

---

<!-- note_key: f40e917dc62b -->
BF: What is the time complexity of Binary Search and why?
BB: Time Complexity: O(log n)

Why: Binary search eliminates half of the remaining elements with each comparison. For an array of size n:

- After 1st comparison: n/2 elements remain
- After 2nd comparison: n/4 elements remain
- After k comparisons: n/(2^k) elements remain

We need log₂(n) comparisons to reduce n elements to 1, hence O(log n). This is much faster than linear search O(n) for large datasets.

---

<!-- note_key: 4733e7f0a943 -->
BF: What does API stand for and what is its purpose?
BB: Application Programming Interface. It provides a way for clients to perform actions with servers over the network through a set of rules and protocols for building and interacting with software applications.

---

<!-- note_key: 04ac04ac138f -->
BF: What are the three examples of API paradigms?
BB: REST, GraphQL, and gRPC.

---

<!-- note_key: f4788158e2cc -->
BF: What does REST stand for and what is its core principle?
BB: REpresentational State Transfer. It uses straightforward HTTP for communication between client and server.

---

<!-- note_key: 13eed556b839 -->
BF: What are the two main architectural constraints of REST APIs?
BB: Client-server architecture (client and server are distinct entities that can be developed independently), and 2) Statelessness (each request must contain all necessary information; server doesn't retain details about previous requests).

---

<!-- note_key: ce2dcc895d04 -->
BF: Why is statelessness important in REST APIs?
BB: It facilitates horizontal scaling because the server doesn't need to manage or update session states or cookies.

---

<!-- note_key: ebbe4764b82f -->
BF: How does pagination work in stateless REST APIs? Give an example.
BB: The client sends state information (like offset) with each request. Example: `https://youtube.com/videos?offset=0&limit=10` for the first 10 videos, then `https://youtube.com/videos?offset=10&limit=10` for the next 10.

---

<!-- note_key: 122885d1b384 -->
BF: What is the difference between REST and RESTful?
BB: REST is an architectural paradigm, while RESTful is an adjective describing web services that follow the REST constraints.

---

<!-- note_key: 3b5ab62fbafd -->
BF: What does JSON stand for and what is it used for?
BB: JavaScript Object Notation. It's a widely used data format for transmitting information over the internet, particularly in REST APIs for both requests and responses.

---

<!-- note_key: 2c06e227a14a -->
BF: What are the key structural features of JSON?
BB: Key-value pairs, support for various levels of nesting, arrays of objects, and both keys and values must be enclosed in double quotes (not single quotes).

---

<!-- note_key: 5f3adb71e2c5 -->
BF: What data types can JSON values contain?
BB: Strings, numbers, booleans, objects (nested), arrays, and null.

---

<!-- note_key: 4b12cdabef2f -->
BF: Why is JSON considered inefficient compared to alternatives like Protocol Buffers?
BB: JSON is a text-based format that requires parsing, uses more bandwidth due to verbose syntax (property names repeated, quotation marks), and is less compact than binary formats like Protocol Buffers.

---

<!-- note_key: 7612d4a2c3b3 -->
BF: What are the two main data-fetching problems with REST APIs?
BB: Over-fetching (receiving more data than necessary) and under-fetching (receiving too little data, requiring multiple requests).

---

<!-- note_key: 84e380a0ea39 -->
BF: Give an example of over-fetching in REST APIs.
BB: When fetching user data to display comments (needing only profile picture, username, and comment), the `/user` endpoint might return unnecessary fields like country or date joined, wasting network resources.

---

<!-- note_key: fd0c476ea424 -->
BF: Give an example of under-fetching in REST APIs.
BB: When endpoints are narrowly defined and deliver only a single field per request, requiring multiple API calls to fetch each required property separately.

---

<!-- note_key: 71a6ac7fa98d -->
BF: Why was GraphQL created?
BB: To address the limitations of REST APIs, particularly to mitigate issues related to over-fetching and under-fetching.

---

<!-- note_key: c9eca4a8585a -->
BF: How does GraphQL solve the over-fetching and under-fetching problems?
BB: The client can precisely specify the required data within a single request, and the server gathers and formats all the necessary data accordingly, returning only what was requested.

---

<!-- note_key: 9954656af6e1 -->
BF: What are the two primary types of operations in GraphQL?
BB: Queries (to retrieve data) and mutations (to modify data on the server).

---

<!-- note_key: a77ebc6f7981 -->
BF: How many endpoints does GraphQL typically use?
BB: A single endpoint, typically an HTTP POST endpoint, where all queries are sent.

---

<!-- note_key: 1da91ae25086 -->
BF: What are the advantages of GraphQL over REST?
BB: Precise data specification, efficient data retrieval in a single query, ability to navigate through related objects and fields, reduced data usage (beneficial for mobile apps and slow networks), and elimination of multiple round trips.

---

<!-- note_key: 18e73c529d48 -->
BF: What are some potential drawbacks of GraphQL?
BB: More complex server implementation, harder to cache (since everything goes through one endpoint), potential for complex queries that can overload the server, and a steeper learning curve compared to REST.

---

<!-- note_key: e5a49914b38e -->
BF: What does gRPC stand for?
BB: gRPC Remote Procedure Call (recursive acronym), though most people assume it stands for Google Remote Procedure Call.

---

<!-- note_key: 0db13bf5d3ac -->
BF: What is an RPC (Remote Procedure Call)?
BB: A method that allows a program to execute a procedure in another address space, commonly on another computer on a shared network.

---

<!-- note_key: 9781599c175e -->
BF: What are the main features of gRPC?
BB: Bi-directional communication, multiplexing for multiple messages over a single TCP connection, server push, and built on top of HTTP/2.

---

<!-- note_key: 25c01e2f6b49 -->
BF: What is gRPC typically used for and why is it faster than REST?
BB: Typically used for server-to-server communication. It's much faster than REST because it sends data using Protocol Buffers instead of JSON.

---

<!-- note_key: cd2e5495b525 -->
BF: What are Protocol Buffers?
BB: A language-neutral, platform-neutral extensible mechanism for serializing structured data (binary format rather than text-based like JSON).

`.proto` example:
syntax = "proto3";message SearchRequest { string query = 1; int32 page_number = 2; int32 result_per_page = 3;}

reading in python:

```
import protobuf

syntax = "proto3"

class SearchRequest(protobuf.Message):
    query = protobuf.Field(1, protobuf.STRING)
    page_number = protobuf.Field(2, protobuf.INT32)
    result_per_page = protobuf.Field(3, protobuf.INT32)
```

---

<!-- note_key: 9c54fa020bcf -->
BF: What streaming capabilities does gRPC provide?
BB: Data can be pushed from client to server and from server to client. It's an alternative to REST APIs, not a replacement for WebSockets.

---

<!-- note_key: 61194645eaf9 -->
BF: What is an important note about error handling in gRPC?
BB: While gRPC is built on HTTP/2, it does not use HTTP error codes. You must develop your own server messages for error handling.

---

<!-- note_key: 02dc2a786962 -->
BF: In a gRPC service definition, what do the `rpc` method definitions specify?
BB: The RPC operations (methods that can be called remotely), as well as the request and response schemas (message types) that each will accept and return.

\*\*\*\*\*\* EXAMPLE \*\*\*\*\*\*

```
// The greeter service definition.
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
}
```

---

<!-- note_key: 8e1ce6ad501e -->
BF: What are "messages" in gRPC and how are they used?
BB: Messages are schemas for request and response payloads defined in .proto files. They are converted to class definitions in programming languages and used to create objects passed to RPC methods.

---

<!-- note_key: 4e64426dbd2e -->
BF: When should you choose gRPC over REST or GraphQL?
BB: Use gRPC for microservices communication, real-time streaming, low-latency requirements, internal APIs where you control both client and server, and when bandwidth efficiency is critical. Avoid for browser-based clients (limited support) or public APIs where ease of use matters more than performance.

---

<!-- note_key: 1db50e4a1f48 -->
BF: Compare the typical use cases: REST vs GraphQL vs gRPC
BB: REST: general-purpose web APIs, public APIs, simple CRUD operations. GraphQL: complex client applications with varying data needs, mobile apps requiring efficient data usage. gRPC: server-to-server communication, microservices, high-performance requirements.

---

<!-- note_key: 500131c193d2 -->
BF: How do REST, GraphQL, and gRPC differ in their endpoint structure?
BB: REST uses multiple endpoints (one per resource), GraphQL uses a single endpoint (typically POST), and gRPC uses service definitions with multiple RPC methods.

---

<!-- note_key: 6a899835272d -->
BF: What data formats do REST, GraphQL, and gRPC use?
BB: REST and GraphQL both use JSON (text-based), while gRPC uses Protocol Buffers (binary format).

---

<!-- note_key: ecea4c288123 -->
BF: What does CRUD stand for and what are the four basic operations?
BB: Create, Read, Update, Delete. These are the fundamental operations APIs provide for manipulating data.

---

<!-- note_key: 16e2a4376b5c -->
BF: How do CRUD operations map to HTTP/REST methods?
BB: Create = POST, Read = GET, Update = PUT, Delete = DELETE.

---

<!-- note_key: a3bed7a64402 -->
BF: What is an API contract (or surface area)?
BB: The defined interface that specifies method signatures, parameters, and return values that developers use to interact with the API, ensuring consistent behavior and seamless integration.

---

<!-- note_key: ee3cfc542757 -->
BF: Why is backward compatibility important in API design?
BB: APIs may be used by hundreds or thousands of applications. Breaking changes would cause crashes in dependent apps, so newer additions or changes must remain compatible with older versions.

---

<!-- note_key: 282b6fef94da -->
BF: How can you add new features to an API while maintaining backward compatibility?
BB: Make new parameters optional. For example, adding an optional `parentId` parameter to `createTweet(userId, content, parentId)`allows existing applications using `createTweet(userId, content)` to continue working without code changes.

---

<!-- note_key: ab551bd012d7 -->
BF: What are common strategies for maintaining backward compatibility?
BB: Optional parameters with default values, adding new endpoints instead of modifying existing ones, using API versioning to support multiple versions simultaneously, and providing deprecation warnings well in advance of removing old functionality.

---

<!-- note_key: 800f24f362b8 -->
BF: When and why do companies update API versions?
BB: When significant changes are made (new parameters, methods, or fundamental changes to how things work), or to address security vulnerabilities. Versioning helps distinguish between different iterations and allows developers to adapt their code.

---

<!-- note_key: 0ab47a98acf4 -->
BF: What is API deprecation?
BB: The process of announcing that a previous API version will no longer be supported, encouraging developers to update their API calls to the latest version which offers better functionality, performance, and bug fixes.

---

<!-- note_key: b279ddd0af6f -->
BF: What are API keys and why might they be regenerated in new API versions?
BB: API keys are secret tokens included in API requests to verify the identity and permissions of the requesting application. They may be regenerated in new versions to address security vulnerabilities.

---

<!-- note_key: 7935d452f4a2 -->
BF: What are the three key principles of good API design?
BB: 1) Not have conflicting endpoints, 2) Be backwards compatible, 3) Provide pagination for list operations.

---

<!-- note_key: a783b660574f -->
BF: In API design, what should developers focus on rather than internal implementation?
BB: The method signature (parameters and their types) and the return value. Developers using an API should treat it as a black box and rely on the contract, not the internal workings.

---

<!-- note_key: 99c724428ce0 -->
BF: What are other characteristics of well-designed APIs?
BB: Consistent naming conventions, clear and comprehensive documentation, appropriate error messages with proper HTTP status codes, rate limiting to prevent abuse, proper authentication and authorization, intuitive resource hierarchies, and following REST principles (if REST API).

---

<!-- note_key: 5e7bf01fb82e -->
BF: How should API endpoints be structured for retrieving resources? Give examples for single vs. multiple resources.
BB: Single resource: `https://api.twitter.com/v1.0/tweet/:tweetId`. Multiple resources with pagination: `https://api.twitter.com/v1.0/users/:id/tweets?limit=10&offset=0`. The version number (v1.0) should be included in the URL.

---

<!-- note_key: 2d05b5b4095a -->
BF: What are RESTful URL design best practices?
BB: Use nouns for resources (not verbs), use plural forms for collections (`/users` not `/user`), nest resources logically (`/users/:id/tweets`), use hyphens for multi-word resources (`/user-profiles`), keep URLs lowercase, use query parameters for filtering/sorting/pagination, and include API version in the URL path.

---

<!-- note_key: 12c0f5fe112d -->
BF: What is caching?
BB: The process of making copies of data to enable faster read and write operations. Data is duplicated in faster storage (like CPU cache or memory) even though it already exists in slower storage (like RAM or disk).

---

<!-- note_key: 775d8bce7eee -->
BF: What is the main tradeoff of using cache?
BB: Speed vs. capacity. Cache is significantly faster than RAM or disk, but has very limited capacity (usually kilobytes or megabytes), requiring careful decisions about what data to store.

---

<!-- note_key: 7d1fbe993061 -->
BF: Where is caching used beyond a single machine?
BB: Caching is widely used in distributed systems at multiple levels: CDNs (Content Delivery Networks), reverse proxy caches, application-level caches (Redis, Memcached), database query caches, and browser caches.

---

<!-- note_key: 522e1e945873 -->
BF: What is a cache hit vs. a cache miss?
BB: A cache hit occurs when requested data is found in the cache. A cache miss occurs when requested data is not found in the cache, requiring retrieval from slower storage.

---

<!-- note_key: 503b6e292038 -->
BF: Can you have a cache miss even when data is cached?
BB: Yes. The cached data might be stale (outdated) and the cache might have expired, requiring fresh data to be fetched.

---

<!-- note_key: b56ca6d0370c -->
BF: What is the cache hit ratio and how is it calculated?
BB: The percentage of content requests a cache can fulfill successfully. Formula: (cache hits / (cache hits \+ cache misses)) × 100. Higher percentages are better.

---

<!-- note_key: 25126ccb1406 -->
BF: What types of files are typically cached by browsers and why?
BB: Static elements like images, JavaScript files, and CSS files whose content remains unchanged regardless of the user or number of refreshes. These are prime candidates because making network requests repeatedly for identical content is inefficient.

---

<!-- note_key: c7ad681eeede -->
BF: What is the sequence of steps a browser follows to load a resource?
BB: Check memory cache (for current browsing session), 2) Check disk cache (persistent cache from past visits), 3) Make a network request if not found in either cache.

---

<!-- note_key: 4aab00f865b9 -->
BF: Why bother caching when the time difference might seem small (e.g., 123ms vs 11ms)?
BB: With many resources (e.g., 100 files each taking an additional 100ms), the cost adds up significantly. Small delays in page load time negatively impact user experience and can lead to users abandoning websites.

---

<!-- note_key: 90f45078191b -->
BF: What determines cache behavior in browsers?
BB: HTTP cache headers sent by servers: Cache-Control (max-age, no-cache, no-store), Expires, ETag (for validation), and Last-Modified. These headers tell browsers how long to cache resources and when to revalidate.

---

<!-- note_key: 9f6073d743fa -->
BF: What is write-around cache and when is it useful?
BB: New data is written directly to main storage (disk) rather than the cache. Useful when most new data won't be accessed frequently (like most tweets). Saves cache space for more popular content. Popular items are added to cache only after first access.

---

<!-- note_key: ab73447dc37a -->
BF: What is write-through cache and what are its advantages and disadvantages?
BB: Data is written to both cache and disk simultaneously. Advantages: avoids stale data and data inconsistency, both locations have the same version. Disadvantages: puts more load on the memory bus and fills cache with data that might not be accessed again.

---

<!-- note_key: a76516215808 -->
BF: What is write-back cache and when is data written to disk?
BB: Data is written only to cache initially. It's written to disk only when the cache is full or when the system is less busy. Provides better write performance but risks data loss if cache fails before writing to disk.

---

<!-- note_key: 3796cd566cdf -->
BF: What are the risks and benefits of each (cache) write mode?
BB: Write-around: Risk of cache miss on first read, benefit of efficient cache usage. 
Write-through: Risk of performance overhead, benefit of data consistency. 
Write-back: Risk of data loss on failure, benefit of best write performance and reduced disk I/O.

---

<!-- note_key: 80028bb09e7c -->
BF: What is an (cache) eviction policy?
BB: A system that determines which items get removed from the cache when the cache is full and new items need to be added.

---

<!-- note_key: f70b2d803203 -->
BF: What is FIFO (First In First Out) eviction policy?
BB: Similar to a queue interface. When the cache becomes full, the first piece of data that was cached is evicted first, regardless of how recently or frequently it was accessed.

---

<!-- note_key: e9efdbc44af0 -->
BF: What is LRU (Least Recently Used) eviction policy and what is its principle?
BB: Evicts items that haven't been accessed for the longest time. Principle: if an item hasn't been accessed recently, it's less likely to be accessed in the future. Useful for scenarios like popular tweets that should remain in cache.

---

<!-- note_key: daaf9d4e0f08 -->
BF: What is LFU (Least Frequently Used) eviction policy and how is it implemented?
BB: Evicts items used least frequently. Implemented using key-value pairs where the key is the item and the value is the usage frequency. When cache is full, the item with the smallest frequency is evicted.

---

<!-- note_key: c1a6dbeb3e3d -->
BF: Why is LRU often better than LFU for Twitter-like applications?
BB: LFU has limitations: old tweets from 2013 with many historical views might never be evicted despite being irrelevant now, preventing new popular tweets from being cached. LRU better reflects current relevance.

---

<!-- note_key: a5a531abb725 -->
BF: What are other common eviction policies? (than LRU, LFU)
BB: LRU-K (considers K most recent accesses), 
2Q (two queues for frequent and infrequent items), 
ARC (Adaptive Replacement Cache, balances recency and frequency), 
Random (randomly selects item to evict),
TTL (Time To Live, expires after set time period).

---

<!-- note_key: d8ca89c949fe -->
BF: What factors influence caching strategy decisions?
BB: 1. Data access patterns (read-heavy vs write-heavy), 
2. data consistency requirements (can tolerate stale data?), 
3. resource constraints (available memory), 
4. latency requirements, cost considerations (network bandwidth, compute), 
5. data volatility (how frequently does data change).

---

<!-- note_key: 4b4626bf7472 -->
BF: What is a Content Delivery Network (CDN)?
BB: A group of cache servers (or edge servers) located around the world that cache content close to end users, enabling faster data access than fetching from the origin server.

---

<!-- note_key: e4ee3c1bdd23 -->
BF: What are the main benefits of using a CDN?
BB: Speeds up content delivery for geographically distributed users, reduces load on the origin server, reduces data transmitted over long distances, saves bandwidth, improves performance, and provides redundancy/fault tolerance.

---

<!-- note_key: 1640d55c50ee -->
BF: What type of content is traditionally stored on CDNs?
BB: Static content including HTML, CSS, and static JavaScript files. Modern edge servers can also serve dynamic content through serverless JavaScript functions.

---

<!-- note_key: f440a5c70f9b -->
BF: What is a push CDN and when should you use it?
BB: New data is immediately pushed from the origin server to all cache servers. Best for static content that doesn't change frequently and is widely requested globally (e.g., video content consumed worldwide). Every request results in a cache hit, but inefficient if content isn't requested near those servers.

---

<!-- note_key: d7b09a4cf533 -->
BF: What is a pull CDN and when should you use it?
BB: The CDN retrieves content from the origin server on an "as needed" basis when users request it. Best for platforms with enormous amounts of frequently generated content (like Twitter) or content with regional popularity variations. Avoids unnecessary resource consumption but first request experiences a cache miss.

---

<!-- note_key: 532de526ae10 -->
BF: What is the key tradeoff between push and pull CDNs?
BB: Push CDN: All requests are cache hits, but wastes resources distributing unused content and requires constant maintenance. Pull CDN: More efficient resource usage and less maintenance, but first request to each CDN experiences slower load time (cache miss).

---

<!-- note_key: 1d1f5a38b37b -->
BF: What does "cache-control: public" vs "cache-control: private" mean? (CDN)
BB: "Public" means content should be cached by the CDN and shared across users (for static content like images, CSS, JS). "Private" means content should not be cached by CDNs (for user-specific or sensitive information).

---

<!-- note_key: 2fad5da671cb -->
BF: What is the primary purpose of CDNs vs horizontal scaling?
BB: CDNs minimize latency by placing servers geographically closer to users. Horizontal scaling increases throughput by adding more servers to handle more requests. They're related but have different goals - CDNs optimize for speed/distance, horizontal scaling optimizes for capacity.

---

<!-- note_key: 0c90634674bf -->
BF: What are popular CDN providers?
BB: Cloudflare, Amazon CloudFront, Akamai, Fastly, Google Cloud CDN, Microsoft Azure CDN. Each offers different features, pricing, and geographic coverage.

---

<!-- note_key: 9f5c1455c0a4 -->
BF: What is a binary tree?
BB: Binary tree is a data-structure composed of *nodes* and *pointers*. Roughly think of it as a linked list with two links for each value.

Starting node is the *root node*, if a node has no children it is considered a *leaf.*

Each node can have up to two children (left child and right child) and usually holds a value (any data-type). The *height* measures the longest path to leaf from root. The *depth* measures the shorest distance of a node from the root.

---

<!-- note_key: 69970f2ba88d -->
BF: What are Binary Search Trees?
BB: 1. Binary Search Trees (BST) are a variation of binary trees with the addition of a sorted property. The property is that every node in the left subtree is smaller than the root and every node in the right subtree is greater than the root.

2. This is a recursive property, meaning that it applies to every node in the tree. This property is analogous to having a sorted array.

3. They are useful as they allow O(log n) search just like a sorted array, but additionally allow for O(log n) insertion and deletion.

SEARCH:

\* binary comparison at teach node, eliminate half of the remaining tree at each comparison. Recursively return *true/false* up the call trace

---

<!-- note_key: bf63454d473b -->
BF: What is a forward proxy and what does it do?
BB: An intermediary server that sits between clients and the internet. It receives requests from clients and forwards them to websites on the client's behalf, masking the client's IP address from the destination server.

![forward-proxy](<media/sharpen=1-ea673b39faad9d70a8a6994505f441b8b200df89.png>)

---

<!-- note_key: 1f8f3038c177 -->
BF: What are the three main benefits of a forward proxy?
BB: 1) Privacy (hides client's IP address from destination), 
2) Efficiency through caching (stores frequently requested data), 
3) Control over network traffic (can filter/block access to certain websites).

---

<!-- note_key: 27f1e65ee6af -->
BF: In a forward proxy setup, what does the origin server see?
BB: The origin server only sees the proxy server's IP address, not the client's IP address. The client is hidden from the origin server.

---

<!-- note_key: 88f9e3e5d233 -->
BF: What are common use cases for forward proxies?
BB: Corporate networks blocking entertainment sites, schools filtering content, users bypassing geographic restrictions, anonymizing web browsing, and improving performance through caching for frequently accessed resources.

---

<!-- note_key: 01c38ff36122 -->
BF: What is a reverse proxy and how does it differ from a forward proxy?
BB: A reverse proxy sits between the internet and servers, receiving requests from clients and forwarding them to appropriate servers. It anonymizes the destination server (not the client). Clients don't know which actual server fulfilled their request.

![reverse-proxy](<media/sharpen=1.png>)

---

<!-- note_key: e0c8a20fa965 -->
BF: What are the main benefits of a reverse proxy?
BB: Protects servers by managing incoming requests, distributes load across multiple servers, provides protection against DDoS attacks, reduces latency through caching, and alleviates load on origin servers.

---

<!-- note_key: dca206411437 -->
BF: Give two examples of reverse proxies.
BB: 1) CDNs (Content Delivery Networks) - serve cached content to reduce origin server load, 
2) Load balancers - distribute traffic across multiple servers.

---

<!-- note_key: 4a7a3aa290cd -->
BF: What is a load balancer and why is it needed?
BB: A type of reverse proxy that distributes incoming network traffic across multiple horizontally scaled servers. Needed because popular websites cannot handle all traffic with a single server - load balancers ensure efficient handling and optimal resource utilization.

---

<!-- note_key: d520d4564e27 -->
BF: What is Round Robin load balancing?
BB: A strategy that distributes requests sequentially across servers in rotation. Each server receives an equal share of requests. Assumes all servers can handle equivalent workload. Simple but doesn't account for varying server capabilities or request complexity

---

<!-- note_key: b0ee2f9bef68 -->
BF: What is Weighted Round Robin (WRR) and when is it better than regular Round Robin?
BB: Distributes traffic based on each server's computational power (CPU, RAM, storage, network bandwidth). Servers with higher weights receive proportionally more requests. Better when servers have different capabilities - ensures more efficient resource utilization.

---

<!-- note_key: 374b1db37047 -->
BF: What is the Least Connections load balancing strategy and when is it useful?
BB: Assigns new requests to the server with the fewest active connections. Particularly useful when there are significant differences or unpredictable variations in request processing time (e.g., some users browsing vs. others completing checkouts). Prevents imbalances and optimizes resource utilization.

---

<!-- note_key: 80d22d4d31d5 -->
BF: What is the User Location load balancing strategy?
BB: Routes users to the geographically nearest server based on their IP address. Minimizes distance data travels, reduces latency, and provides faster user experience.

---

<!-- note_key: 342f6a6198ba -->
BF: What is the difference between Layer 4 and Layer 7 load balancing?
BB: Layer 4 (transport layer): Routes based on IP address and TCP port - fast and straightforward but less flexible. Layer 7 (application layer): Routes based on request content like HTTP headers, methods, or body - more processing overhead but offers greater flexibility and granular routing decisions.

---

<!-- note_key: 0fac85183741 -->
BF: What are the tradeoffs between different load balancing strategies?
BB: Round Robin: Simple but doesn't handle varying server capacity or request complexity. Weighted Round Robin: Better resource utilization but requires knowledge of server capabilities. Least Connections: Handles varying request times well but adds overhead tracking connections. User Location: Reduces latency but may create regional imbalances. Layer 4 vs 7: Speed vs flexibility.

---

<!-- note_key: 81006c106ad3 -->
BF: What is the key difference in what forward proxy vs reverse proxy hides?
BB: Forward proxy hides the client from the server (server only sees proxy IP). Reverse proxy hides the server from the client (client doesn't know which actual server responded). Forward proxy protects clients, reverse proxy protects servers.

---

<!-- note_key: 26b9c354ab7f -->
BF: How does basic hashing work for load balancing?
BB: **T**ake a unique identifier (IP address, user ID, or request ID), calculate its hash, and mod by the number of servers. The result assigns the request to a specific server. Example: IP 6 % 3 servers = server 0.

---

<!-- note_key: 91116e3144fe -->
BF: What is the main benefit of using hashing over Round Robin for load balancing?
BB: The same IP address/user is always directed to the same server, allowing each server to cache data specific to that user. Round Robin is inconsistent and routes the same user to different servers each time.

---

<!-- note_key: a9730990de8b -->
BF: What is the critical problem with basic hashing when a server goes down?
BB: When a server goes down, the modulo arithmetic changes (e.g., from mod 3 to mod 2), causing a complete remapping of requests. Most users get routed to different servers, leading to widespread cache misses even though their original servers may still be running.

---

<!-- note_key: e64d1f6e0e41 -->
BF: What is consistent hashing and how does it solve the server failure problem?
BB: A technique that uses a ring-based structure to map both servers and requests to points on a ring. When a server goes down, only its requests are reassigned to the next server clockwise on the ring - other mappings remain unchanged, minimizing cache misses.

---

<!-- note_key: 6041f90dddee -->
BF: How does consistent hashing assign requests to servers?
BB: Hash the request (e.g., IP address) to a value on the ring, 2) Locate the next server on the ring that is equal to or follows that hash value in the clockwise direction, 3) That server handles the request.

---

<!-- note_key: d665e0792f55 -->
BF: What happens to requests when a server fails in consistent hashing?
BB: Only the requests that were previously handled by the failed server are reassigned to the next closest server clockwise on the ring. All other request-to-server mappings remain the same.

---

<!-- note_key: 76b146e14e7c -->
BF: Why do we use modulus operation in consistent hashing?
BB: To ensure the hash value falls within the range of available positions on the ring (0 to M-1), where M is the solution space. It also prevents overflow issues since hash functions can produce very large integer values. This ensures uniform distribution of requests.

---

<!-- note_key: 6a29fc40458c -->
BF: When should you use consistent hashing vs Round Robin?
BB: Use consistent hashing when caching is important and users need to be consistently routed to the same server (CDNs, databases with user-specific data). Use Round Robin when caching is not a concern and simple, equal distribution is sufficient.

---

<!-- note_key: 2a092859c18a -->
BF: What are two main real-world applications of consistent hashing?
BB: 1) CDNs - routing specific users to the same cache servers in their regions for cache efficiency, 
2) Databases - ensuring a user's data resides on a specific server and the user is consistently routed to that server.

---

<!-- note_key: c6fb59266db4 -->
BF: What are virtual nodes in consistent hashing?
BB: A technique where each physical server is represented by multiple points (virtual nodes) on the ring. This provides better load distribution, especially when servers have different capacities, and reduces the impact when a single server fails by spreading its load across multiple other servers rather than just one.

---

<!-- note_key: 34f51176320e -->
BF: How does Rendezvous Hashing (Highest Random Weight) differ from Consistent Hashing?
BB: Rendezvous hashing computes a hash for each (request, server) pair and assigns the request to the server with the highest hash value. No ring structure needed. Consistent hashing uses a ring and assigns requests to the next clockwise server. Both minimize remapping on server failures, but rendezvous hashing provides more uniform distribution without needing virtual nodes.

---

<!-- note_key: 99a2dc17e216 -->
BF: When would you choose Rendezvous Hashing over Consistent Hashing?
BB: Choose rendezvous hashing when you need simpler implementation (no ring structure), more uniform distribution without tuning virtual nodes, or when the number of servers changes frequently. Choose consistent hashing when you need faster lookups (O(log n) vs O(n) for rendezvous) or when it's already widely adopted in your infrastructure (e.g., many CDNs use it).

---

<!-- note_key: 327ffbbcd80d -->
BF: How does insertion work in BSTs?
BB: Start at root and compare the value to insert with current node. If less, go left; if greater, go right. Repeat until finding null, then insert new node there. Time complexity: O(log n) average for balanced trees, O(n) worst case for skewed trees. Duplicates are typically rejected, inserted to right subtree, or stored with a frequency count depending on implementation.

---

<!-- note_key: 31b0d592424f -->
BF: What are the three cases for deleting a node from a BST and how do you handle each?
BB: **Case 1 - No children (leaf):** Simply remove it by setting parent's pointer to null. 
**
Case 2 - One child:** Replace node with its only child, bypassing the deleted node. 
**
Case 3 - Two children:** Find inorder successor (go right once, then left until null - smallest in right subtree) or inorder predecessor (go left once, then right until null - largest in left subtree), replace node's value with it, then delete the successor/predecessor (which has at most one child). Time complexity: O(h) where h is height.

---

<!-- note_key: 18594bed3aa6 -->
BF: What is tree balancing, what are the main self-balancing BST types, and how do rotations work?
BB: **Balancing:** Keeping height difference between subtrees minimal to ensure O(log n) operations. Unbalanced trees (e.g., from sorted insertions) degenerate to O(n). 
**
AVL Trees:** Strict balancing, height difference ≤ 1 between subtrees, uses balance factor = height(left) - height(right). Four rotation cases: Left-Left (right rotation), Right-Right (left rotation), Left-Right (left then right rotation), Right-Left (right then left rotation). Better for read-heavy workloads. 
**
Red-Black Trees:**Relaxed balancing with color properties (root is black, red nodes have black children, equal black nodes on all root-to-null paths). Height ≤ 2 log n. Fewer rotations, better for frequent insertions/deletions. 
**
Rotations:** Structural transformations preserving BST properties. Left rotation promotes right child; right rotation promotes left child. Used to rebalance after insertions/deletions.

---

<!-- note_key: acea5d45bfaf -->
BF: What is Depth-First Search (DFS) in binary trees, what are the three traversal orders, and what are their complexities?
BB: **DFS Concept:** An algorithm that traverses trees by going as deep as possible in one direction before backtracking. Pick a direction (e.g., left), follow pointers until reaching null, backtrack to parent, then go right. Best implemented recursively (or iteratively with a stack).

**Three Traversal Orders:**

1. **Inorder (Left --> Root --> Right):** Visit left subtree, then parent node, then right subtree. For BSTs, visits nodes in **sorted order**. Example output: `[2,3,4,5,6,7]`. Use case: Getting sorted data from BST.
2. **Preorder (Root --> Left --> Right):** Visit parent node first, then left subtree, then right subtree. Example output: `[4,3,2,6,5,7]`. Use case: Copying trees, prefix expression evaluation.
3. **Postorder (Left --> Right --> Root):** Visit left subtree, then right subtree, then parent node last. Example output: `[2,3,5,7,6,4]`. Use case: Deleting trees, postfix expression evaluation.

**Time Complexity:** O(n) for all three methods - must visit every node regardless of tree height.

**Space Complexity:** O(h) where h is tree height - due to recursion call stack. O(log n) for balanced trees, O(n) for skewed trees.

**Key Insight:** Only inorder traversal produces sorted output for BSTs due to the BST property (left < root < right). The leftmost node is always the smallest.

---

<!-- note_key: b74156949170 -->
BF: What is Breadth-First Search (BFS) in binary trees, how is it implemented, and what are its complexities?
BB: **BFS Concept:** Also known as level-order traversal. Visits all nodes on one level before moving to the next level, prioritizing breadth over depth. Processes tree level by level from left to right.

**Implementation:** Uses a queue (deque) data structure implemented iteratively:

1. Add root to queue
2. While queue is not empty:
   - Process all nodes at current level (loop through current queue length)
   - Dequeue node from head (popleft)
   - Add its left child, then right child to queue tail (if they exist)
   - Move to next level

**Key Detail:** The inner loop iterates exactly `len(queue)` times to process only the current level's nodes. Children added during this loop become the next level.

**Example traversal:** For a tree with root 4, level 0: `[4]`, level 1: `[3, 6]`, level 2: `[2, 5, 7]`

**
**

**Time Complexity:** O(n) - visit every node exactly once.

**
**

**Space Complexity:** O(n) - store entire level in queue. Worst case: last level can contain roughly n/2 nodes (half the tree in a complete binary tree).

**
**

**BFS vs DFS Space:** BFS uses O(n) space for the queue. DFS uses O(h) space for recursion stack where h is height (O(log n) balanced, O(n) skewed). BFS uses more space but guarantees shortest path in unweighted graphs.

---

<!-- note_key: cce78295626b -->
BF: What are Sets and Maps, how are they implemented with trees, and what are their time complexities?
BB: **TreeSet:** A set data structure implemented using a tree that ensures **unique values** and keeps elements **sorted**.

Example: Phone book with names `['Alice', 'Brad', 'Collin']` stored alphabetically. Operations (insert, delete, search) run in O(log n) time.

**
**

**TreeMap:** A map data structure implemented using a tree that stores **key-value pairs** with keys kept **sorted**.

Example: Phone book mapping names to numbers `{'Alice': 123, 'Brad': 345, 'Collin': 678}`. The key requirement: keys must be comparable to maintain order. Values can be any data type (numbers, objects, etc.). Operations run in O(log n) time.

**
**

**---**

**
**

**Key Properties:**

- Both maintain sorted order based on keys/values
- TreeSet: unique elements only
- TreeMap: unique keys only (keys map to values)
- Both use BST properties for efficiency

**Time Complexity:** O(log n) for insert, delete, and search operations when implemented with balanced trees.

**
**

**Implementation:** Java and C\+\+ have built-in TreeMap/TreeSet. Python requires external library (`SortedDict` from `sortedcontainers`). JavaScript requires external library (`treemap-js`).

**
**

**Alternative Implementation:** Sets and Maps can also be implemented using hash tables (HashSet/HashMap) which offer O(1) average time for operations but don't maintain sorted order. Choose TreeSet/TreeMap when you need sorted data; choose HashSet/HashMap when you need faster operations without ordering.

---

<!-- note_key: 64d9a6329c24 -->
BF: What is backtracking, how does it work, and what are its key characteristics (demonstrate on a Binary Tree)?
BB: **Concept:** An algorithm that operates on a brute-force approach by trying all possible solutions and backtracking when hitting a dead-end. Similar to DFS but with the ability to undo choices. Like navigating a maze: try a path, if it's a dead-end, backtrack and try another path.

**How It Works (Tree Example):**

1. Explore one direction (e.g., left subtree) recursively
2. If a valid solution is found, return true
3. If it fails, backtrack and try another direction (e.g., right subtree)
4. If all options fail, return false

**Key Pattern - Building a Path:** When tracking the path taken:

- Add current node to path when exploring: `path.append(root.val)`
- If exploration succeeds, return true (path remains)
- If exploration fails, **remove node from path before returning**: `path.pop()`
- This "undo" step is the essence of backtracking

**Example:** Find path from root to leaf without 0 values in tree `[4,0,1,null,7,3,2,null,null,null,0]`:

- Add 4 to path
- Left child is 0 (invalid), don't explore
- Go right, add 1 to path
- Try 1's left child 3, add to path
- 3 has no valid children, **pop 3** (backtrack)
- Try 1's right child 2, add to path
- 2 is leaf node, return true
- Valid path: `[4,1,2]`

**Time Complexity:** O(n) - may visit all nodes in worst case.

**Space Complexity:** O(h) where h is height - recursion call stack depth plus path storage (both limited by tree height). O(log n) for balanced trees, O(n) for skewed trees.

**Key Insight:** The `path.pop()` operation before returning false is what makes this backtracking - it undoes the decision to include that node when the path doesn't lead to a solution. Without this, failed paths would pollute the result.

---

<!-- note_key: b6e07a422374 -->
BF: What is a heap, what are its two types, and what is a priority queue?
BB: **Heap Definition:** A specialized tree-based data structure that implements a Priority Queue abstract data type (terms often used interchangeably). Unlike regular queues (FIFO), priority queues remove elements based on priority, not insertion order.

**Two Types:**

1. **Min Heap:** Smallest value at root, has highest priority to be removed. All descendants must be ≥ their ancestors.
2. **Max Heap:** Largest value at root, has highest priority to be removed. All descendants must be ≤ their ancestors.

**Key Difference from BST:** Heaps allow duplicate values. Order property is parent-child only (not left-right like BST).

---

<!-- note_key: 1debc1216cfe -->
BF: What are the two required properties for a binary heap?
BB: **1. Structure Property:** Must be a **complete binary tree** - every level is completely filled except possibly the lowest level, which is filled contiguously from left to right (no gaps).

**2. Order Property:**

- **Min Heap:** All descendants > ancestors. If root has value y, every node in left and right subtrees must have values ≥ y (recursive property).
- **Max Heap:** All descendants < ancestors. Every node in subtrees ≤ y.

**Why Complete Tree Matters:** Enables array-based implementation with simple index formulas, no pointers needed.

---

<!-- note_key: 9682ef8c1cf7 -->
BF: How are heaps implemented using arrays, and what are the parent/child index formulas?
BB: **Implementation:** Despite being drawn as trees, heaps are implemented using arrays. Nodes are stored level-by-level (BFS order) from left to right.

**One-Based Indexing:** Start filling array at index 1 (not 0) to make formulas work correctly. Index 0 is unused or set to dummy value.

**Index Formulas (where i = current node index):**

- `leftChild = 2 * i`
- `rightChild = 2 * i + 1`
- `parent = i // 2` (integer division)

**Why Index 1?** If we started at index 0, formulas break (0 \* 2 = 0, incorrectly suggesting left child is at index 0).

**Example:** Node at index 2:

- Left child: 2 \* 2 = 4
- Right child: 2 \* 2 \+ 1 = 5
- Parent: 2 // 2 = 1

**Key Advantage:** O(1) navigation to parent/children using simple arithmetic, no pointers needed. Only works because heap is a complete binary tree with contiguous array storage.

---

<!-- note_key: f9a0eba3fe3f -->
BF: Why are arrays efficient for heaps?
BB: **Why arrays are efficient for heaps:**

- Cache-friendly: contiguous memory improves CPU cache performance
- Space-efficient: no pointer overhead (saves ~16 bytes per node)
- Simple implementation: just an array and index arithmetic
- Contrast with BST: BSTs need pointers because they're not guaranteed to be complete/balanced

---

<!-- note_key: c54159dedafc -->
BF: What is the time complexity for reading the minimum (or maximum) value from a heap and why?
BB: O(1) constant time. The min/max value is always at the root (index 1 in the array), so we can simply read `heap[1]` without any searching or traversal.

---

<!-- note_key: c1350f5938bf -->
BF: How does the push operation work in a heap and what is its time complexity?
BB: **Algorithm:**

1. Append new value to end of array (maintains structure property - complete binary tree)
2. **Percolate/bubble up:** Compare with parent using `i // 2`
3. If child < parent (min heap) or child > parent (max heap), swap them
4. Repeat until node is in correct position (parent comparison satisfies order property) or reaches root

**Time Complexity:** O(log n) - in worst case, bubble up entire height of tree. Since heap is always balanced (complete binary tree), height = log n.

**Key Insight:** Always insert at next available position first (end of array) to maintain structure property, then fix order property by percolating up.

---

<!-- note_key: a41813245e6b -->
BF: How does the pop operation work in a heap, why is it more complex than push, and what is its time complexity?
BB: **Algorithm:**

1. Save root value to return (the min/max priority element)
2. Replace root with **rightmost node of last level** (last element in array)
3. Remove last element from array (maintains structure property)
4. **Percolate/bubble down:** Compare with children
   - If node has two children: swap with smaller child (min heap) or larger child (max heap)
   - If node has only left child: swap if needed
   - If node has no children or order property satisfied: stop
5. Repeat until node reaches correct position

**Why Not Swap Root with min(left, right) Child?** This violates structure property - creates gaps in the tree at level 2\+. The tree is no longer complete.

**
**

**Edge Cases:**

- Empty heap (len = 1): return None
- Single node (len = 2): just pop and return it
- Node can only have left child OR two children (never only right child, would violate complete tree property)

**Time Complexity:** O(log n) - percolate down entire height in worst case. Height = log n for balanced complete binary tree.

**Key Insight:** Always replace root with last element to maintain structure property, then fix order property by percolating down. Swap with smaller child (min heap) or larger child (max heap) to maintain order property throughout tree.

---

<!-- note_key: 32ccb0887aac -->
BF: What are the time complexities for all heap operations?
BB: - **Get Min/Max:** O(1) - root is always at index 1
- **Push:** O(log n) - insert at end, percolate up height of tree
- **Pop:** O(log n) - replace root with last element, percolate down height of tree
- **Heapify (build heap from array):** O(n) - bottom-up construction

**Why Log n for Push/Pop:** Heap is always a complete binary tree (structure property), guaranteeing balanced tree with height = log n. Both operations traverse at most the height of the tree.

---

<!-- note_key: 4831b6a14377 -->
BF: What is heapify and why is it more efficient than building a heap element-by-element?
BB: Heapify is an algorithm that builds a heap from an unsorted array in O(n) time. Much more efficient than pushing elements one-by-one, which takes O(n log n) time (n elements × O(log n) per push). When converting an existing array to a heap, always use heapify.

---

<!-- note_key: f94205182506 -->
BF: How does the heapify algorithm work?
BB: **Steps:**

1. Convert array to 1-based indexing (append first element to end)
2. Calculate first non-leaf node: `(len(heap) - 1) // 2` (index n/2)
3. Starting from first non-leaf node, percolate down (same as pop operation)
4. Decrement index by 1, repeat for next non-leaf node
5. Continue until reaching root (index 1)

**Bottom-Up Approach:** Start from lowest non-leaf level and work upward. When processing a parent, its children's subtrees are already valid heaps, so only need to percolate that parent down to correct position.

**Why Skip Leaf Nodes?** Leaves have no children, already satisfy order property. Roughly n/2 nodes are leaves, so only process n/2 non-leaf nodes.

---

<!-- note_key: 01674b29492c -->
BF: Why is heapify O(n) instead of O(n log n)?
BB: **Mathematical Reasoning:**

- Bottom level nodes (≈ n/2): percolate down 0-1 levels
- Next level up (≈ n/4): percolate down 1-2 levels
- Pattern continues: nodes halve, but percolation depth increases
- Total work: (n/2)×1 \+ (n/4)×2 \+ (n/8)×3 \+ ... = O(n)

**Key Insight:** Most nodes (the leaves) do zero work. Only a few nodes near the top do significant work. The summation converges to linear time, not O(n log n).

**Comparison:**

- Repeated push: n insertions × O(log n) each = O(n log n)
- Heapify: processes n/2 nodes with decreasing work = O(n)

---

<!-- note_key: 7e0f37020546 -->
BF: What is an RDBMS?
BB: A Relational Database Management System (RDBMS) is a database that stores persistent data using tables. It provides efficient reading and writing by structuring data into precisely defined fields that can be queried easily.

---

<!-- note_key: 1314bd54c042 -->
BF: What data structure makes SQL queries efficient?
BB: B\+ Trees make SQL queries efficient by organizing data in a multi-way tree structure with sorted leaf nodes containing all the data.

---

<!-- note_key: f49950b7b26f -->
BF: How does a B\+ Tree differ from a B-Tree?
BB: In a B\+ Tree, all data is stored in leaf nodes which are linked in sorted order, while interior nodes contain only keys and pointers. In a B-Tree, interior nodes also contain data.

---

<!-- note_key: dedd94556240 -->
BF: What is the relationship between children and keys in a B\+ Tree node?
BB: If a node has M children, it must have M-1 keys. The keys act as separation values dividing the subtrees.

---

<!-- note_key: 99f0c9eb2fbb -->
BF: Give an example of how keys divide subtrees in a B\+ Tree.
BB: A node with keys [2, 5] has three children: first child contains values < 2 (e.g., [0,1]), second child contains values between 2 and 5 (e.g., [3,4]), and third child contains values > 5 (e.g., [6,7])

---

<!-- note_key: 05a9b7829097 -->
BF: What is indexing in databases?
BB: Indexing is a way to improve the speed of data retrieval operations on a database table, at the cost of additional writes and storage space to maintain the index data structure.

---

<!-- note_key: 601320884ea2 -->
BF: Give an example of how indexing works
BB: If you have names mapped to phone numbers, you could pick the name field as the index, allowing faster retrieval of phone numbers when searching by name.

---

<!-- note_key: 9d930afab7af -->
BF: What are the trade-offs of indexing?
BB: Indexing speeds up data retrieval but requires additional writes and storage space to maintain the index data structure.

---

<!-- note_key: fc9b3505b83f -->
BF: What is a table in SQL?
BB: A table is a way to organize data where each row contains information about a single primary key. It must have a defined structure before inserting records.

---

<!-- note_key: 2b510330599a -->
BF: What is a primary key?
BB: A primary key uniquely identifies each record (row) in a table.

---

<!-- note_key: 95eb84b2c98a -->
BF: What is a record?
BB: A record is a row in a database table.

---

<!-- note_key: 44285bb1c730 -->
BF: What is varchar?
BB: Varchar (variable character) is a data type for variable length strings that can include numbers, letters, and special characters.

---

<!-- note_key: dc808608500e -->
BF: What is a foreign key constraint?
BB: A foreign key constraint ensures that a value in one table must exist in another table, establishing a relationship between the two tables and maintaining referential integrity.

---

<!-- note_key: 2917ff1d24cc -->
BF: What is a JOIN in SQL?
BB: A JOIN is a way to combine records from two or more tables based on a related column between them.

---

<!-- note_key: 7a51448e19ba -->
BF: What does ACID stand for?
BB: ACID stands for Atomicity, Consistency, Isolation, and Durability.

---

<!-- note_key: 854a10c5b12f -->
BF: What is Durability in ACID? Give an example.
BB: Durability means that after a transaction successfully completes, changes to data persist and are not undone, even in the event of a system failure. Example: If a fund transfer completes and then the power goes out, the transaction will still be recorded in the database when the system recovers.

---

<!-- note_key: 7184381dd94a -->
BF: What is Atomicity in ACID? Explain with the Alice and Bob example.
BB: Atomicity means all changes to data are performed as if they are a single operation - either all changes are performed, or none of them are. Example: If Alice sends Bob $500 and the database crashes during the transfer, atomicity ensures that money is NOT taken from Alice's account. Either the entire transaction completes (money leaves Alice and arrives at Bob), or none of it happens.

---

<!-- note_key: 619320873cd8 -->
BF: What is Isolation in ACID and what is a dirty read?
BB: Isolation means that the intermediate state of a transaction is invisible to other transactions, so concurrent transactions appear to be serialized and don't interfere with each other. A dirty read occurs when one transaction reads data from another transaction before it has committed, violating the isolation property. 

Example: If Transaction 1 is transferring $500 from Alice to Bob (not yet committed), and Transaction 2 reads Alice's balance mid-transfer, Transaction 2 has performed a dirty read.

---

<!-- note_key: 6b440f70af17 -->
BF: What is Consistency in ACID? Give an example.
BB: Consistency ensures data integrity by adhering to predefined rules and constraints that maintain data validity throughout transaction execution, bringing the database from one valid state to another. 

Example: A rule that an account balance cannot be negative is a consistency constraint that the database enforces.

---

<!-- note_key: 9b4372ccced8 -->
BF: Why are B\+ Trees particularly well-suited for disk-based storage?
BB: B\+ Trees minimize disk I/O operations because they have high fanout (many children per node), reducing tree height and the number of disk accesses needed to find data.

---

<!-- note_key: f725b4cced02 -->
BF: What is the trade-off between normalization and query performance?
BB: Normalization reduces data redundancy and improves data integrity but may require more JOINs, which can slow down queries. Denormalization can speed up reads but increases storage and risks data inconsistency.

---

<!-- note_key: f2c36125efc7 -->
BF: When would you choose a relational database over a NoSQL database?
BB: Choose a relational database when you need ACID guarantees, complex queries with JOINs, well-defined schemas, and strong data consistency requirements.

---

<!-- note_key: 7183b4da0847 -->
BF: What is referential integrity and how is it maintained?
BB: Referential integrity ensures that relationships between tables remain consistent, typically enforced through foreign key constraints so references always point to existing records. The database will typically prevent deletion of records referenced by foreign keys unless CASCADE DELETE is specified.

---

<!-- note_key: 25be664c9222 -->
BF: What does NoSQL stand for and how does it differ from SQL databases?
BB: NoSQL stands for "Not Only SQL". Unlike SQL databases, NoSQL databases don't organize data in tables and you can't use SQL syntax for complex queries and joins. They are more flexible and scalable, designed to overcome SQL limitations in scalability and performance. SQL scales vertically (limited), while NoSQL scales horizontally (unlimited).

---

<!-- note_key: 422744ad95eb -->
BF: What are the four main types of NoSQL databases?
BB: (1) Key-Value databases, 
(2) Document databases, 
(3) Wide-Column databases, and 
(4) Graph databases.

---

<!-- note_key: 13ed8ecafb57 -->
BF: What is a Key-Value database and give an example?
BB: A Key-Value database uses a simple key-value method to store data, like a hashmap. The database stores key-value pairs where the key serves as a unique identifier. It's schemaless, so different keys and values can have completely different structures. Example: Redis, which stores data in-memory (RAM) for exceptionally fast retrieval.

---

<!-- note_key: 63ec88d67570 -->
BF: What is a Document database and give an example?
BB: Document databases store data as JSON-like "documents", making it easier for developers to store and query data since it matches the format used in application code. Individual fields can be added or removed independently, providing flexibility. 

Example: MongoDB stores data in JSON-like structures with varying nesting levels.

---

<!-- note_key: aef89320ade3 -->
BF: What is a Wide-Column database and when is it useful?
BB: A Wide-Column database stores data in columns rather than rows, enabling high write throughput and optimizing reads/aggregations over specific data subsets. They excel in very large-scale scenarios like internet search, web messaging, and managing large amounts of timestamp data. 

Examples: Apache Cassandra and Google BigTable.

---

<!-- note_key: 182ae8819206 -->
BF: What is a Graph database and when should you use it?
BB: A Graph database uses nodes (entities) connected by edges (relationships), similar to graph data structures in algorithms. They're useful when data has complex relationships and interconnectedness. 

Example use case: Facebook's platform with user relationships, friends, friends-of-friends, interests, and social interactions (comments, likes, shares).

---

<!-- note_key: 2c5861faafe5 -->
BF: Why can NoSQL databases be distributed across multiple servers more easily than SQL databases?
BB: Because NoSQL databases have no foreign key or join constraints, data can be split and stored on different servers without needing to cross-reference between tables. This allows half the data to be in one part of the world and the other half elsewhere, enabling horizontal scaling.

---

<!-- note_key: d9ec9bf8e8f4 -->
BF: What does BASE stand for and how does it differ from ACID?
BB: BASE stands for "Basically Available, Soft state, Eventual consistency". While ACID focuses on strict consistency (common in SQL databases), BASE focuses on eventual consistency and is more characteristic of NoSQL databases. Both models have their use cases—one isn't inherently better than the other.

---

<!-- note_key: 6e07cbbd1bd2 -->
BF: Can NoSQL databases be ACID-compliant?
BB: es, some NoSQL databases can be ACID-compliant. MongoDB is an example of a NoSQL database that provides ACID guarantees for transactions.

---

<!-- note_key: 571034e47510 -->
BF: What is Eventual Consistency and how does it work?
BB: Eventual Consistency means that updates made to the system will eventually propagate to all nodes, but not immediately. In a leader/follower architecture, writes go to the primary (leader) node, which then eventually updates the follower nodes. During this propagation period, users might read stale data.

---

<!-- note_key: 8e3f064a365e -->
BF: Give an example of where Eventual Consistency is acceptable.
BB: Social media platforms like Twitter or Instagram, where follower counts or like counts may be slightly delayed. The exact count doesn't need to be instantly consistent across all servers—it's acceptable if it updates within seconds.

---

<!-- note_key: 2d62ae7ef391 -->
BF: What does "schemaless" mean in NoSQL databases?
BB: Schemaless means the database manages information without needing a predefined blueprint or structure. Different records can have completely different structures - one key might map to a string while another maps to a complex nested JSON object.

---

<!-- note_key: 20be9c282f52 -->
BF: What is leader/follower (master/slave) architecture?
BB: A distributed database architecture where one node is designated as the leader/primary (handles all writes and updates) and other nodes are followers/replicas (handle reads). The leader propagates changes to followers, which may result in eventual consistency rather than strict consistency.

---

<!-- note_key: 10d859a08b3d -->
BF: What are the time complexity trade-offs between Hash Maps, Tree Maps, and Sorted Arrays?
BB: **Hash Map:**

- Insert/Remove/Search: O(1) average case
- Inorder Traversal: Not possible (unordered)
- Best for: Fast lookups, counting, frequency problems

**Tree Map:**

- Insert/Remove/Search: O(log n)
- Inorder Traversal: O(n) sorted order
- Best for: When you need sorted/ordered data

**Sorted Array:**

- Insert/Remove: O(n) (requires shifting)
- Search: O(log n) (binary search)
- Inorder Traversal: O(n) already sorted
- Best for: Static data, many searches, few updates

**Key Insight:** Hash maps sacrifice ordering for speed. If you need to iterate keys in order, must first sort them O(n log n), slower than tree map's O(n) inorder traversal.

---

<!-- note_key: c6dd23726e8d -->
BF: How do you use a hash map for frequency counting and when should you use hash maps?
BB: **When to Use:** Hash maps are essential when problems mention "unique", "count", or "frequency". They're one of the most useful data structures for coding interviews.

**Frequency Counting Pattern:**

python

```
countMap = {}
for item in array:
    if item not in countMap:
        countMap[item] = 1
    else:
        countMap[item] \+= 1
```

**Example:** Array `["alice", "brad", "collin", "brad", "dylan", "kim"]` Result: `{"alice": 1, "brad": 2, "collin": 1, "dylan": 1, "kim": 1}`

**How It Works:**

- Hash maps don't allow duplicate keys (unique constraint)
- Use keys for items, values for counts
- If key exists: increment count
- If key doesn't exist: add with count = 1

**Time Complexity:** O(n) for n elements - each insert/update is O(1)

**Space Complexity:** O(k) where k = number of unique keys in the array

**Comparison:** Using tree map would take O(n log n) for same operation (each of n insertions costs O(log n)).

---

<!-- note_key: cd669e3dcb54 -->
BF: What is the difference between a Hash Set and a Hash Map?
BB: - **Hash Set:** Contains keys only, no values. Used for checking membership/existence. Example: checking if element exists, removing duplicates.
- **Hash Map:** Contains key-value pairs. Used for associations/mappings. Example: counting frequencies, storing related data.

**Both Share:**

- O(1) average time for insert/remove/search
- No duplicates allowed (keys must be unique)
- Unordered (cannot iterate in sorted order without extra work)

**Choose Hash Set when:** Only need to track presence/absence (true/false) **Choose Hash Map when:** Need to associate additional data with keys (counts, objects, etc.)

---

<!-- note_key: 77117be955f4 -->
BF: How does a hash function work and why is it needed for O(1) hash map operations?
BB: Converts a key (e.g., string) into an integer that serves as the array index where the key-value pair is stored.

**
**

**Example Process:**

1. Take each character in the key
2. Get ASCII code for each character
3. Sum all ASCII codes (pre-hashing)
4. Apply modulo with array size: `sum(ASCII codes) % array_size
`

**Example:** "Alice" has ASCII sum = 25, array size = 2

- Index = 25 % 2 = 1
- Store "Alice": "NYC" at index 1

**Key Property:** Same string always produces same integer (deterministic), enabling O(1) lookup.

**Why Modulo?** Ensures index is valid (within array bounds). Without it, large sums would be out of bounds.

---

<!-- note_key: dfdcc4d09fc5 -->
BF: What are collisions in hash maps and what are the two main strategies to handle them?
BB: **Collision:** When two different keys hash to the same array index. Inevitable as array size is limited.

**
**

**1. Chaining (Most Common):**

- Store multiple key-value pairs at same index using a linked list
- Each array position holds a linked list of nodes
- Insert/Search: O(1) average, O(n) worst case if all keys collide at one index
- Simpler to implement than open addressing

**2. Open Addressing:**

- Find next available empty slot when collision occurs
- If index i is occupied, try i\+1, i\+2, etc. (linear probing)
- Lookup: Check original index, then probe next slots until key found
- More efficient for few collisions, but limited by array size
- More complex to implement

**Trade-off:** Chaining allows unlimited entries (linked lists grow), open addressing limited by array capacity but can be more cache-friendly.

**
**

**Minimizing Collisions:** Use prime numbers for array size (only divisible by 1 and itself), distributes keys more evenly.

---

<!-- note_key: 785d1bb71fdd -->
BF: When and why do hash maps resize, and what is rehashing?
BB: **Resizing Trigger:** When array becomes **half full** (50% capacity), double the array size to `2 × capacity`. Done before insertion, not during, to minimize collisions proactively.

**Why Resize?**

- Ensure vacant spots for new keys
- Reduce collision probability
- Maintain O(1) average performance

**Rehashing Process:**

1. Create new array with doubled capacity
2. **Recompute position** for all existing key-value pairs
3. Use same hash formula but with new capacity: `sum(ASCII) % new_capacity`
4. Reinsert all pairs into new array

**Why Rehash?** Array size changed, so `key % old_size =/= key % new_size`. Keys may map to different indices.

**Example:** "Alice" at index 1 with size 2

- After doubling to size 4: 25 % 4 = 1 (happens to stay at 1)
- "Brad" (sum 27): 27 % 4 = 3, then 27 % 8 = 3 after next resize

**Cost:** Rehashing is O(n) operation but amortized O(1) per insertion (infrequent).

---

<!-- note_key: 89b149bd0bda -->
BF: What are the time complexities for hash map operations and when do they degrade?
BB: **Average Case (All Operations):** O(1) for insert, remove, and search

**Worst Case:** O(n) - occurs when:

- Poor hash function causes many collisions
- All keys hash to same index (with chaining, traverses entire linked list)
- Many collisions with open addressing (probes many slots)

**Requirements for O(1):**

- Good hash function (distributes keys evenly)
- Low collision rate
- Proper resizing (maintain load factor ≤ 0.5)

**Practical Note:** In interviews and real-world usage, assume O(1) unless specifically dealing with worst-case analysis or adversarial inputs.

---

<!-- note_key: 69d36d76fa25 -->
BF: What is a graph, what are its basic components, and what is the relationship between vertices and edges?
BB: **Definition:** A data structure made up of nodes (vertices) connected by pointers (edges). No restrictions on placement or connections - a vertex can have variable number of edges.

**Components:**

- **Vertices (V):** Nodes in the graph
- **Edges (E):** Pointers connecting vertices

**Edge Limit:** Given V vertices, maximum edges E ≤ V². Each vertex can point to every other vertex including itself. A graph with V vertices and V² edges is called a **complete graph**.

**Examples:** Trees and linked lists are actually graphs - directed graphs with specific constraints (trees: acyclic, linked lists: linear).

**Key Property:** Vertices don't need to be connected - a graph with no edges is still valid.

---

<!-- note_key: 6ba44b211e17 -->
BF: What is the difference between directed and undirected graphs?
BB: **Directed Graph:** Edges have direction (arrows). Relationship is one-way. Example: Twitter follows (A follows B doesn't mean B follows A), trees (parent --> child), linked lists (prev/next pointers).

**Undirected Graph:** Edges have no direction, represent bidirectional relationships. Example: Facebook friends (mutual), road connections between cities.

**Notation:** For directed edge from A to B, only A --> B exists. For undirected, both A --> B and B --> A exist implicitly.

---

<!-- note_key: 9f1104f51584 -->
BF: What are the three main ways to represent graphs and what are their space complexities?
BB: **1. Matrix (Grid):**

- 2D array where 0s represent vertices
- Traverse by moving up/down/left/right
- Connect adjacent 0s to form graph
- Space: O(n × m) where n = rows, m = columns
- Use: Spatial problems, pathfinding on grids

**2. Adjacency Matrix:**

- 2D array where indices represent vertices
- `adjMatrix[v1][v2] = 1` means edge from v1 to v2
- `adjMatrix[v1][v2] = 0` means no edge
- Space: O(V²) - inefficient for sparse graphs (few edges)
- Use: Dense graphs, quick edge lookup O(1)

**3. Adjacency List (Most Common):**

- Each vertex stores list of its neighbors
- Implementation: HashMap or array of lists, or GraphNode class with `neighbors` list
- Space: O(V \+ E) - only stores existing edges
- Use: Most interview problems, sparse graphs, space-efficient

**Comparison:**

- Adjacency List is most space-efficient and commonly used
- Adjacency Matrix wastes space for sparse graphs but has O(1) edge lookup
- Matrix representation used for grid-based problems

---

<!-- note_key: 36deee9b3b83 -->
BF: How do you implement a graph using an adjacency list?
BB: **Using GraphNode Class:**

python

```
class GraphNode:
    def __init__(self, val):
        self.val = val
        self.neighbors = []  \# List of adjacent vertices
```

**Using HashMap (Alternative):**

python

```
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A'],
    'D': ['B']
}
```

**Advantages:**

- Space efficient: O(V \+ E) only stores actual edges
- Easy to iterate through neighbors
- Simple to add/remove edges
- Most common in interviews

**Operations:**

- Add edge: `node1.neighbors.append(node2)` - O(1)
- Check if edge exists: search neighbors list - O(degree of vertex)
- Get all neighbors: `node.neighbors` - O(1) to access list

---

<!-- note_key: d3c5872e0f40 -->
BF: What is dynamic programming and how does it differ from regular recursion?
BB: **Definition:** Dynamic programming (DP) is an optimized version of recursion that solves big problems by breaking them into smaller subproblems, but avoids redundant calculations.

**Key Difference from Recursion:** Regular recursion recalculates the same subproblems multiple times. DP uses **memoization** (caching) to store results of subproblems, so each is calculated only once.

**
**

**Example - Fibonacci:**

- Brute force recursion: O(2^n) - calculates F(2) three times in F(5)
- DP with memoization: O(n) - calculates each F(i) once and caches it

**Core Principle:** If you're solving the same subproblem multiple times, cache the result and reuse it.

**
**

**When to Use DP:**

1. Problem has overlapping subproblems (same calculation repeated)
2. Problem has optimal substructure (optimal solution uses optimal solutions to subproblems)
3. Current problem depends on solutions to smaller versions of same problem

---

<!-- note_key: c92d60ea62b4 -->
BF: What are the two main approaches to dynamic programming and how do they differ?
BB: **1. Top-Down (Memoization):**

- **Strategy:** Recursion \+ caching
- **Direction:** Start from original problem, recurse down to base cases
- **Implementation:** Add cache parameter to recursive function, check cache before computing
- **When to use:** More intuitive, closer to recursive thinking, easier to write initially

**2. Bottom-Up (Tabulation):**

- **Strategy:** Iterative, build solution from base cases up
- **Direction:** Start from base cases, iteratively compute larger subproblems
- **Implementation:** Use array/table, fill it iteratively in correct order
- **When to use:** Often more space-efficient, avoids recursion stack overflow

**Fibonacci Example:**

- Top-down: `if n in cache: return cache[n]` then recurse
- Bottom-up: `dp = [0,1]` then iterate to build up to n

**Trade-offs:**

- Top-down: Easier to conceptualize, only computes needed subproblems, uses recursion stack space
- Bottom-up: No recursion overhead, can optimize space better, need to determine iteration order

---

<!-- note_key: d215df1afd5c -->
BF: What is 1D DP, and how can you optimize space complexity in problems like Fibonacci?
BB: **1D DP:** Problems where each subproblem depends on one variable, and results can be stored in a 1D array proportional to input size n.

**
**

**Fibonacci Example - Three Approaches:**

**1. Top-Down (Memoization):**

python

```
cache = {}
if n in cache: return cache[n]
cache[n] = fib(n-1) \+ fib(n-2)
```

Time: O(n), Space: O(n) for cache \+ O(n) recursion stack

**
**

**2. Bottom-Up (Full Array):**

python

```
dp = [0] \* (n\+1)
dp[0], dp[1] = 0, 1
for i in range(2, n\+1):
    dp[i] = dp[i-1] \+ dp[i-2]
```

Time: O(n), Space: O(n)

**
**

**3. Space-Optimized Bottom-Up:**

python

```
dp = [0, 1]  \# Only store last 2 values
for i in range(2, n\+1):
    tmp = dp[1]
    dp[1] = dp[0] \+ dp[1]
    dp[0] = tmp
```

Time: O(n), Space: O(1)

**
**

**Key Insight:** If current state only depends on fixed number of previous states (e.g., prev 2 for Fibonacci), you only need to store those states, not entire history.

**Space Optimization Pattern:** Identify how many previous values you actually need, keep only those in memory.

---

<!-- note_key: 4b295ea40dd2 -->
BF: **What is 2D DP and how do you solve the unique paths grid problem?**
BB: **A: 2D DP:** Problems where subproblems depend on two variables (e.g., row and column), requiring a 2D cache/table.

**Problem:** Count unique paths from top-left to bottom-right, moving only down or right in a grid.

**
**

**Brute Force (No DP):**

python

```
def paths(r, c, rows, cols):
    if r == rows or c == cols: return 0  \# out of bounds
    if r == rows-1 and c == cols-1: return 1  \# destination
    return paths(r\+1, c) \+ paths(r, c\+1)  \# down \+ right
```

Time: O(2^(n\+m)) - exponential, recalculates same cells

**
**

**Top-Down DP (Memoization):**

python

```
cache = {}  \# or 2D array
if (r,c) in cache: return cache[(r,c)]
cache[(r,c)] = paths(r\+1, c) \+ paths(r, c\+1)
```

Time: O(n × m), Space: O(n × m)

**
**

**Bottom-Up DP (Space Optimized):**

python

```
prevRow = [0] \* cols
for r in range(rows-1, -1, -1):
    curRow = [0] \* cols
    curRow[cols-1] = 1  \# base case
    for c in range(cols-2, -1, -1):
        curRow[c] = curRow[c\+1] \+ prevRow[c]  \# right \+ down
    prevRow = curRow
```

Time: O(n × m), Space: O(m) - only need previous row!

**
**

**Key Insights:**

- Calculate from bottom-right to top-left (or vice versa with different logic)
- Each cell's value = sum of values from cells you can reach (right \+ down)
- Space optimization: only need previous row to calculate current row, not entire 2D grid

**Common 2D DP Problems:** Longest common subsequence, edit distance, 0/1 knapsack, matrix chain multiplication

---

<!-- note_key: f8e17f660d37 -->
BF: What is replication and why is it used in distributed systems?
BB: **Definition:** Creating copies of a database (replicas) hosted on separate machines/servers, kept in sync with the original.

**Purpose:**

1. Increase data availability (if one server fails, others remain accessible)
2. Improve scalability (distribute read load across multiple servers)
3. Increase data durability (multiple copies protect against data loss)

**Architecture:** Leader/follower (master/slave)

- **Leader (master):** Original database, handles writes
- **Follower (slave):** Replica, typically handles reads
- Data flows from leader --> follower (not reverse, or leader wouldn't be fully updated)

---

<!-- note_key: f5dda534004b -->
BF: What are the differences between synchronous and asynchronous replication and their trade-offs?
BB: **Synchronous Replication:**

- Write transaction immediately replicated to follower before confirming to client
- Leader waits for follower acknowledgment
- **Pros:** Strong consistency, follower always up-to-date, can immediately replace leader if it fails
- **Cons:** Higher latency (client waits for replication), slower writes

**Asynchronous Replication:**

- Leader commits transaction and sends to follower without waiting for acknowledgment
- Client gets immediate response
- **Pros:** Lower latency, faster writes, better availability
- **Cons:** Data might be stale on follower (eventual consistency), risk of data loss if leader fails before replication

**Trade-off:** Consistency vs. Latency/Availability

- Synchronous: Prioritizes consistency, sacrifices speed
- Asynchronous: Prioritizes speed and availability, accepts temporary inconsistency

**Use Cases:**

- Synchronous: Financial transactions, critical data where consistency is mandatory
- Asynchronous: Social media feeds, analytics, where slight staleness is acceptable

---

<!-- note_key: a3e3cf307dee -->
BF: What is master-master (multi-master) replication and when is it used?
BB: **Definition:** Multiple leaders that can both read from and write to, typically serving different geographic regions.

**Use Case:** Serving data across different regions (e.g., one leader for west coast, one for east coast).

**
**

**Benefits:**

- Lower latency for users (connect to geographically closer leader)
- Better distribution of write load
- High availability (multiple leaders can handle failures)

**Challenges:**

- Synchronization latency between leaders
- Conflict resolution when same data modified in different regions
- Requires periodic updates to keep leaders in sync
- More complex than single leader setup

**Additional Knowledge:** Master-master replication requires conflict resolution strategies like "last write wins", version vectors, or CRDTs (Conflict-free Replicated Data Types) to handle concurrent writes to same data.

---

<!-- note_key: c332c472244b -->
BF: What is sharding, how does it differ from replication, and how is data partitioned?
BB: **Definition:** Dividing a database into smaller pieces (shards), each hosted on separate machines. Each shard contains only a subset of data (not a complete copy).

**
**

**Replication vs Sharding:**

- Replication: Multiple complete copies for availability/redundancy
- Sharding: Split data into pieces for performance/scalability
- Often used together: each shard can have its own replicas

**Why Shard?** When replication alone can't handle traffic volume - distributes both data AND workload.

**Shard Key:** Chosen criterion/attribute determining which shard data belongs to.

**Partitioning Strategies:**

1. **Range-Based:**
   - Split by ranges (e.g., IDs 1-25, 26-50, 51-75, 76-100)
   - Or alphabetically (A-L, M-Z)
   - Simple but can create hotspots (uneven distribution)
2. **Hash-Based:**
   - Use hash function on shard key
   - Better distribution, but range queries harder
   - Consistent hashing minimizes data movement when adding/removing shards
3. **Geographic:**
   - Partition by location (US, EU, Asia)
   - Reduces latency for regional users

**Example:** User database sharded by first letter of last name - all A-M users on shard1, N-Z on shard2.

---

<!-- note_key: 26667219c232 -->
BF: What are the main challenges with sharding, especially for SQL vs NoSQL databases?
BB: **General Challenges:**

1. **Related data placement:** Ensuring related tables/data end up in same shard (join operations across shards are expensive)
2. **Rebalancing:** Adding/removing shards requires data migration
3. **Complexity:** Application logic must know which shard to query

**SQL Database Challenges:**

- **ACID properties:** Hard to maintain atomicity/consistency across shards
- **Distributed transactions:** Complex and slow
- **No native support:** MySQL, PostgreSQL don't inherently support sharding
- **Application-level sharding:** Developers must implement sharding logic, becomes very complicated
- **Joins across shards:** Extremely expensive or impossible

**NoSQL Database Advantages:**

- **Designed for horizontal scaling:** Built with sharding in mind
- **Eventual consistency:** Relaxed consistency model works better for distributed systems
- **No relational constraints:** No complex joins, easier to distribute data
- **Native sharding support:** Many NoSQL databases handle sharding automatically

**Eventual Consistency:** After a write, replicas may temporarily have different versions, but will converge to consistent state over time. Acceptable for many use cases (social media, caching) but not for critical operations (banking).

**Additional Knowledge - Consistent Hashing for Sharding:** Minimizes data movement when shards are added/removed. Hash both data and servers onto a ring, assign data to next server clockwise. Adding a server only affects adjacent data, not entire dataset.

---

<!-- note_key: 2d5944ae7299 -->
BF: What is the CAP theorem and what does it state about distributed systems?
BB: **CAP Stands For:**

- **C**onsistency
- **A**vailability
- **P**artition Tolerance

**The Theorem:** A distributed system can only guarantee two out of three properties simultaneously. You must choose between:

- CP (Consistency \+ Partition Tolerance) - sacrifice availability
- AP (Availability \+ Partition Tolerance) - sacrifice consistency

**Key Point:** Partition Tolerance is assumed as a given (network failures will happen), so the real choice is between Consistency or Availability when partitions occur.

**Applicability:** Primarily applies to NoSQL databases and partitioned/distributed systems with replication.

**Important:** CAP consistency =/= ACID consistency. CAP consistency means all nodes see same data at same time. ACID consistency means transactional integrity.

---

<!-- note_key: 224c7dba458c -->
BF: What is partition tolerance and why is it assumed in the CAP theorem?
BB: **Definition:** A partition occurs when communication breakdown between leader and follower nodes prevents leader from updating follower.

**Causes:**

- Network failures
- Hardware issues
- Any incident preventing nodes from communicating

**Partition Tolerance Means:** System can continue functioning despite network failures, avoiding total system collapse.

**Why It's Assumed:** In distributed systems, network partitions are inevitable - they will happen. The question isn't "if" but "when" and "how do we handle them?"

**Without Partition Tolerance:** System would completely fail when any network issue occurs, making it impractical for real-world distributed systems.

---

<!-- note_key: 26707e574774 -->
BF: What does consistency mean in the CAP theorem and when should you prioritize it?
BB: **CAP Consistency Definition:** All nodes see identical data at any given moment. Every read retrieves the most recently written data, regardless of which node is queried.

**
**

**During Partition:**

- Leader has updated data
- Follower might have stale data (can't receive updates)
- Reading from follower gives outdated information

**Prioritizing Consistency (CP):** Make follower node unavailable during partition - don't serve potentially stale data. Client requests may fail, but data remains consistent.

**
**

**When to Choose Consistency:**

- **Banking systems:** Account balance must be accurate across all nodes
- **Healthcare systems:** Medical records must be up-to-date (life and death matter)
- **Financial transactions:** Incorrect data has severe consequences
- **Rule:** When data accuracy is absolutely critical, even if it means system stops responding

**Trade-off:** Sacrifice availability - clients may not get responses during partitions, but will never get incorrect data.

---

<!-- note_key: f9ae7517f2b6 -->
BF: What does availability mean in the CAP theorem and when should you prioritize it?
BB: **CAP Availability Definition:** Every system request receives a response (success or failure) regardless of system faults. System stays operational and handles requests even amid failures.

**
**

**During Partition:**

- Both leader and follower continue serving requests
- Follower may serve stale data (hasn't received updates)
- All nodes respond to valid requests

**Prioritizing Availability (AP):** Keep all nodes responding, accept that some may serve slightly outdated data (eventual consistency).

**
**

**When to Choose Availability:**

- **Learning Management Systems (LMS):** Students must submit assignments even if grade update delayed
- **Social media feeds:** Slightly outdated posts acceptable
- **Content delivery:** Better to serve cached content than fail completely
- **E-commerce product listings:** Can tolerate minor staleness in inventory counts
- **Rule:** When system must stay responsive, and temporary data inconsistency is acceptable

**Trade-off:** Sacrifice consistency - clients may get stale data during partitions, but system remains responsive.

---

<!-- note_key: 78c8e271f104 -->
BF: What is tunable consistency and how does the PACELC theorem extend CAP?
BB: **Tunable Consistency:** Modern databases don't strictly choose CP or AP. They allow dynamic adjustment between being more consistent (less available) or more available (less consistent) based on operation requirements.

**Example:** MongoDB, Cassandra allow configuring consistency levels per query - strong consistency for critical operations, eventual consistency for less critical ones.

**
**

**PACELC Theorem Extension:**

**P (Partition exists):** Choose A or C (same as CAP)

**E**lse (no partition): Choose **L**atency or **C**onsistency

**
**

**Full Statement:** "If Partition, choose Availability or Consistency. Else (normal operation), favor Latency or Consistency."

**
**

**Normal Operation Trade-off:**

- **Favor Latency:** Asynchronous replication, faster responses, risk of reading slightly stale data
- **Favor Consistency:** Synchronous replication, slower responses, always read most recent data

**Real-World:** PACELC better captures reality - systems must make trade-offs even when network is healthy, not just during partitions.

**
**

**Examples:**

- **PA/EL:** Cassandra, DynamoDB - prioritize availability and low latency
- **PC/EC:** HBase, MongoDB (strong consistency mode) - prioritize consistency even with higher latency
- **PA/EC:** Some systems choose availability during partition but consistency normally

**Key Insight:** System design is about understanding trade-offs for specific requirements, not rigid rules. Different parts of same application may make different choices.

---

<!-- note_key: 0c20586e8ee4 -->
BF: What are the main criticisms and misunderstandings of the CAP theorem?
BB: **Common Misunderstandings:**

1. **Oversimplification:** CAP is often taught as "pick 2 of 3" using Venn diagrams, but reality is more nuanced. It's not binary - there are degrees of consistency and availability.
2. **Partition Tolerance Isn't Optional:** The "pick 2" framing is misleading. In distributed systems, partitions will happen, so P is mandatory. Real choice is only C vs A during partitions.
3. **Ignores Normal Operation:** CAP only addresses behavior during network partitions, which are relatively rare. PACELC theorem fixes this by addressing latency vs consistency trade-offs during normal operation.
4. **Not All-or-Nothing:** Systems don't have to be purely CP or AP. Modern databases offer tunable consistency - different consistency levels for different operations or queries.
5. **Consistency Definition Narrow:** CAP's linearizable consistency (all nodes see same data instantly) is just one consistency model. Many weaker consistency models exist (eventual, causal, read-your-writes) that are more practical and performant.
6. **Time Aspect Ignored:** CAP doesn't account for how long inconsistency lasts or how quickly system recovers after partition heals.

**Martin Kleppmann's Critique:** CAP theorem is often misapplied and creates false dichotomy. Better to think about specific consistency guarantees your application needs and latency requirements, rather than forcing into CP/AP buckets.

**
**

**Better Approach:**

- Understand specific consistency requirements for each operation
- Consider consistency spectrum (strong --> eventual), not binary choice
- Design for partition scenarios but optimize for normal operation
- Use CAP as starting point for discussion, not rigid constraint

**Key Takeaway:** CAP theorem is a useful mental model for understanding distributed system trade-offs, but it's often over-simplified in teaching. Real systems are more nuanced with tunable parameters and multiple consistency models available.

---

<!-- note_key: cd20b1583934 -->
BF: What is object storage and what are its key components?
BB: A storage system that treats each piece of data as an object containing three components: actual data, metadata, and a unique identifier. Stored in flat address spaces with no hierarchical structure (no folders). Evolved from BLOB (Binary Large Object) storage.

---

<!-- note_key: 7f1b490ecc5b -->
BF: Why should you NOT store images and videos in databases, and what should you use instead?
BB: **Problems with Database Storage:**

- Databases not optimized for large files
- Rare to query for specific images/videos
- Hinders performance
- Increases storage requirements
- Causes frequent read/write operations on database
- Traditional RDBMSs can't handle large files efficiently

**Solution - Object Storage:**

- Specifically designed for unstructured data
- Optimized for large files
- Highly scalable flat architecture
- Examples: AWS S3, Google Cloud Storage, Azure Blob Storage

**Best Practice:** Store metadata in database (filename, size, user_id, upload_date), store actual file in object storage, database contains URL/key to retrieve file.

---

<!-- note_key: 0c9c8a8a5072 -->
BF: What are the key differences between file systems and object storage in terms of structure and scalability?
BB: **File System:**

- Hierarchical tree structure (folders within folders)
- Nested organization
- Complex structure limits scalability
- Familiar navigation with paths

**Object Storage:**

- Flat address space (no folders/hierarchy)
- All objects at same level
- Each object accessed by unique identifier
- Easy horizontal scaling (no hierarchical complexity)

**Scalability Advantage:** Flat structure allows infinite horizontal scaling without maintaining complex directory hierarchies.

---

<!-- note_key: f7aad02b5c24 -->
BF: When should you use object storage vs databases, and how do you retrieve data from object storage?
BB: **Use Object Storage For:**

- Images and videos (user uploads, profile pictures, media)
- Database backups (large backup files)
- Static assets (CSS, JavaScript files)
- Archives and long-term data retention
- Any large files (>few MB) that don't need querying

**Use Databases For:**

- Structured data needing queries/filters
- Transactional data
- Relational data requiring joins
- Data with frequent small updates

**Data Retrieval:**

- Don't read directly from object store
- Make HTTP network request to object storage using unique identifier/key
- Higher latency than database reads but optimized for large files

**System Design Pattern:**

1. User uploads image
2. Store image in S3, get back URL/key
3. Store metadata in database: `{user_id: 123, profile_pic_url: "s3://bucket/user123.jpg"}`
4. To display: fetch URL from database, make HTTP GET to S3 for actual image

**Key Trade-offs:**

- ✅ Excellent for large unstructured data, highly scalable, cost-effective
- ❌ No querying capabilities, higher latency, immutable (must replace entire object)

---

<!-- note_key: 162e562259e7 -->
BF: What is a message queue and why is it used instead of just scaling servers?
BB: A system that buffers messages between producers (app events) and consumers (application servers), allowing asynchronous processing of requests.

**Purpose:**

- Handle high volume of requests that can't be processed simultaneously
- Decouple producers from consumers
- Buffer for managing surges in data
- Process requests that don't need immediate handling

**Why Not Just Scale Servers?**

- Not always cost-effective
- Not always practical
- Many requests don't need immediate processing
- Better to queue and process later

**Key Benefit:** Decoupling - producers and consumers can operate independently, improving system flexibility and resilience.

---

<!-- note_key: 31bd2f5474fe -->
BF: What are the differences between pull-based and push-based message queue models?
BB: **Pull-Based Model:**

- **How it works:** Application server monitors queue and "pulls" messages when it has capacity
- **Pros:** More efficient for managing server-side load, server controls its own pace
- **Cons:** May introduce latency if queue is empty (wasted polling cycles)
- **Use when:** Server capacity varies or you want server to control processing rate

**Push-Based Model:**

- **How it works:** Queue pushes messages to server as they arrive
- **Pros:** Lower latency, immediate delivery
- **Cons:** Risk of overloading server if message rate is too high
- **Use when:** Predictable load or need low latency

**Acknowledgment Pattern (Both Models):**

1. Queue sends message to server
2. Server processes message
3. Server sends acknowledgment back to queue
4. If no acknowledgment within timeout, queue assumes failure and resends
5. Ensures message delivery even with temporary server issues

---

<!-- note_key: 729808e313d6 -->
BF: What is the publisher/subscriber model and what are its benefits?
BB: 1. Publishers dispatch messages to specific **topics** (categories/labels)
2. Subscribers subscribe to topics they're interested in
3. Message broker ensures all messages delivered to all subscribers of that topic
4. Subscribers process messages independently at their own pace

**Key Concept - Topics:** Categories that serve as conduits for similar messages, helping organize the vast array of messages. Multiple subscribers can subscribe to same topic.

**Example - Payment Processing:**

- **Topic:** "OrderPlaced"
- **Subscribers:**
   - Inventory Service (updates stock levels)
   - Billing Service (charges customer)
   - Shipping Service (prepares shipment)
   - Email Service (sends confirmation)
- All notified independently when order is placed

**Benefits:**

1. **Decoupling:** Publishers and subscribers don't need to know about each other
2. **Scalability:** Easy to add new subscribers without modifying publishers
3. **Flexibility:** Can introduce completely different APIs as subscribers without altering architecture
4. **Resilience:** Messages not lost if subscriber temporarily unavailable
5. **Independent Processing:** Each subscriber processes at own pace

**Adaptability:** New subscribers can be added to topic without modifying publishers, making system adaptable to changing requirements.

---

<!-- note_key: 5ff4f9f73144 -->
BF: Give a real-world example of when message queues are beneficial and explain why.
BB: **Payment Processing Example:**

**
**

**Problem Without Message Queues:**

- High usage periods (major sales) cause surge in payment requests
- Synchronous processing creates poor user experience
- Long wait times and potential timeouts
- Server overwhelmed during peaks

**Solution With Message Queues:**

**
**

**1. Handling Peak Loads:**

- Payment requests stored in queue during high traffic
- Processed asynchronously when servers have capacity
- Users get immediate confirmation (request queued)
- Actual processing happens in background

**2. Decoupling Services:**

- New order placed --> message published to "OrderPlaced" topic
- Payment service subscribes and processes payment
- Inventory service subscribes and updates stock
- Email service subscribes and sends confirmation
- All services operate independently

**Benefits:**

- Users don't wait for payment processing
- System handles traffic spikes gracefully
- Services can be updated/scaled independently
- Failed payments can be retried automatically
- No loss of requests during outages

**Other Common Use Cases:**

- Email sending (queue emails, send in background)
- Image/video processing (queue uploads, process asynchronously)
- Analytics events (queue events, process in batches)
- Notifications (queue alerts, deliver at appropriate rate)

**Popular Message Queue Services:**

- RabbitMQ
- Apache Kafka
- GCP Pub/Sub
- AWS SQS (Simple Queue Service)
- AWS SNS (Simple Notification Service)

---

<!-- note_key: 8f49bdeef1b1 -->
BF: What is MapReduce and what problem does it solve?
BB: **Definition:** A programming model and implementation for processing and generating large datasets (terabytes to petabytes) in a distributed computing environment.

**Problem It Solves:** Efficiently processing massive amounts of data across multiple machines.

**Example:** Process a billion rows containing names and SSNs - keep names, redact SSNs. Single machine would be too slow; MapReduce distributes work across many machines.

**Origin:** Introduced by Google engineers to manage large datasets efficiently, even with failures like partitions.

---

<!-- note_key: b6ae8c0d214c -->
BF: What is the difference between batch processing and stream processing?
BB: **Batch Processing:**

- Processes data in large groups/batches
- Data accumulated over period, then processed as unit
- NOT real-time - runs when batch jobs execute
- Example: Count word frequency in books, weekly/daily reports
- Frequency: Weekly, daily, hourly jobs

**Stream Processing:**

- Processes data in real-time as received
- Data processed individually, not stored first
- Immediate processing required
- Example: Redacting credit card info on payment, live fraud detection
- Cannot wait for batch - must happen instantly

**Key Difference:** Timing - batch is delayed and grouped, stream is immediate and individual.

---

<!-- note_key: 29807fdd0b2c -->
BF: What are the roles of master and worker nodes in a MapReduce system like Apache Hadoop?
BB: **Master Node:**

- Manages distribution of MapReduce job across workers
- Monitors status of each task
- Re-assigns tasks if failures occur
- Coordinates overall process

**Worker Nodes (Slave Nodes):**

- Where actual data processing happens
- Receives portion of data from master
- Receives copy of MapReduce program
- Executes Map and Reduce operations
- Multiple workers process data in parallel

**Architecture Benefit:** Fault tolerance - if worker fails, master reassigns its tasks to other workers.

---

<!-- note_key: d2575d578b2b -->
BF: What happens during the Map phase and Shuffle/Sort phase of MapReduce?
BB: **A: Map Phase:**

- Each worker executes Map operation on its assigned data portion
- Transforms input data into key-value pairs
- Example (word count): Input "hello world hello" --> Output: {("hello", 1), ("world", 1), ("hello", 1)}

**Shuffle and Sort Phase:**

- Worker nodes reorganize key-value pairs
- Groups all values with same key together
- Prepares data for Reduce phase
- Example:
   - Worker 1: "the" appears 3 times
   - Worker 2: "the" appears 7 times
   - Worker 3: "the" appears 100 times
   - Shuffle groups: "the" --> [3, 7, 100]

**Purpose:** Ensures all occurrences of same key are sent to same reducer for aggregation.

---

<!-- note_key: 7f1b3968d75d -->
BF: What happens during the Reduce phase and what is the final output?
BB: **Reduce Phase:**

- Performs Reduce operation on each group of values
- Aggregates/combines values for same key
- Produces final result for each key
- Example (word count): "the" --> [3, 7, 100] --> "the": 110

**Output:**

- Final result written to storage or database
- Example final output: {"the": 110, "hello": 5, "world": 2}

**Complete Word Count Flow:**

1. Map: "hello world hello" --> [("hello", 1), ("world", 1), ("hello", 1)]
2. Shuffle: Group by key --> {"hello": [1, 1], "world": [1]}
3. Reduce: Sum values --> {"hello": 2, "world": 1}
4. Store results

---

<!-- note_key: c1922c6400fa -->
BF: What are the main limitations of MapReduce?
BB: **Restrictive Data Processing Model:**

- Only works well with data that fits Map and Reduce steps
- Not suitable for all types of data processing tasks
- Tasks that don't align with this model need alternative approaches

**When MapReduce Doesn't Fit:**

- Iterative algorithms (machine learning)
- Graph processing (social networks)
- Real-time processing (stream processing better)
- Interactive queries (databases better)

**Better Alternatives Exist For:**

- Real-time analytics: Stream processing (Kafka, Flink)
- Interactive queries: SQL databases, data warehouses
- Iterative ML: Spark (improves on MapReduce)
- Graph algorithms: Graph databases (Neo4j)

**Key Takeaway:** MapReduce is powerful for batch processing large datasets that fit the Map-Reduce pattern, but it's not a universal solution. Understanding the concept is more important than mastering specific tools for system design interviews.
