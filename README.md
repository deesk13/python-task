# python-task
# DSA 10 Problem Patterns – Python

This repository contains 10 Data Structures and Algorithms problems implemented in Python.

The problems are designed to identify and practice common DSA patterns such as:

* Sliding Window
* HashSet
* Kadane's Algorithm
* Two Pointers
* Maximum Product Subarray
* Prefix Sum + HashMap
* Anagram Grouping
* HashSet Sequence
* Sorting + Intervals

---

## 📌 Problem List

| #  | Problem                         | Main Pattern             |
| -- | ------------------------------- | ------------------------ |
| 1  | Student Attendance Analysis     | Sliding Window + Set     |
| 2  | Online Shopping Price Analysis  | Kadane's Algorithm       |
| 3  | Rainwater Collection            | Two Pointers             |
| 4  | Employee Performance            | Kadane's Algorithm       |
| 5  | Product Sales                   | Maximum Product Subarray |
| 6  | Customer Purchase History       | Sliding Window + Set     |
| 7  | Bank Transaction Analysis       | Prefix Sum + HashMap     |
| 8  | Employee Skill Grouping         | HashMap + Sorting        |
| 9  | Network Packet Analysis         | HashSet                  |
| 10 | Hospital Appointment Scheduling | Sorting + Intervals      |

---

# 1. Student Attendance Analysis

### Pattern

**Sliding Window + Set**

### Problem

Find the length of the longest continuous sequence containing no repeated attendance value.

### Example

```text
Input:
[1, 2, 3, 1, 4]

Output:
4
```

### Code

```python
def longest_unique(arr):
    seen = set()
    left = 0
    maximum = 0

    for right in range(len(arr)):

        while arr[right] in seen:
            seen.remove(arr[left])
            left += 1

        seen.add(arr[right])

        maximum = max(maximum, right - left + 1)

    return maximum


arr = [1, 2, 3, 1, 4]

print(longest_unique(arr))
```

### Complexity

```text
Time  : O(n)
Space : O(n)
```

---

# 2. Online Shopping Price Analysis

### Pattern

**Kadane's Algorithm**

### Problem

Find the maximum sum of a continuous sequence.

### Example

```text
Input:
[-2, 3, -1, 5, -6, 4]

Output:
7

Subarray:
[3, -1, 5]
```

### Code

```python
def max_subarray_sum(arr):
    current = arr[0]
    maximum = arr[0]

    for i in range(1, len(arr)):
        current = max(arr[i], current + arr[i])
        maximum = max(maximum, current)

    return maximum


arr = [-2, 3, -1, 5, -6, 4]

print(max_subarray_sum(arr))
```

### Complexity

```text
Time  : O(n)
Space : O(1)
```

---

# 3. Rainwater Collection

### Pattern

**Two Pointers**

### Problem

Given heights of bars, calculate how much rainwater can be trapped.

### Example

```text
Input:
[0, 1, 0, 2, 1, 0, 1, 3]

Output:
5
```

### Code

```python
def trap_water(height):
    left = 0
    right = len(height) - 1

    left_max = 0
    right_max = 0

    water = 0

    while left < right:

        if height[left] <= height[right]:

            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]

            left += 1

        else:

            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right]

            right -= 1

    return water


arr = [0, 1, 0, 2, 1, 0, 1, 3]

print(trap_water(arr))
```

### Complexity

```text
Time  : O(n)
Space : O(1)
```

---

# 4. Employee Performance

### Pattern

**Kadane's Algorithm**

### Problem

Find the maximum continuous performance score.

### Example

```text
Input:
[-2, 3, -1, 5, -6, 4]

Output:
7
```

### Code

```python
def max_performance(arr):
    current = arr[0]
    maximum = arr[0]

    for i in range(1, len(arr)):
        current = max(arr[i], current + arr[i])
        maximum = max(maximum, current)

    return maximum


arr = [-2, 3, -1, 5, -6, 4]

print(max_performance(arr))
```

### Complexity

```text
Time  : O(n)
Space : O(1)
```

---

# 5. Product Sales

### Pattern

**Maximum Product Subarray**

### Problem

Find the maximum product of a continuous subarray.

### Example

```text
Input:
[2, 3, -2, 4]

Output:
6

Subarray:
[2, 3]
```

### Code

```python
def max_product(arr):
    current_max = arr[0]
    current_min = arr[0]
    answer = arr[0]

    for i in range(1, len(arr)):

        if arr[i] < 0:
            current_max, current_min = current_min, current_max

        current_max = max(arr[i], current_max * arr[i])
        current_min = min(arr[i], current_min * arr[i])

        answer = max(answer, current_max)

    return answer


arr = [2, 3, -2, 4]

print(max_product(arr))
```

### Why `current_min`?

Because:

```text
negative × negative = positive
```

A very small negative product can become the maximum product when multiplied by another negative number.

### Complexity

```text
Time  : O(n)
Space : O(1)
```

---

# 6. Customer Purchase History

### Pattern

**Sliding Window + Set**

### Problem

Find the longest continuous sequence of purchases without repeating an item.

### Example

```text
Input:
[10, 20, 10, 30, 40, 20]

Output:
4

Longest unique sequence:
[10, 30, 40, 20]
```

### Code

```python
def longest_unique(arr):
    seen = set()
    left = 0
    maximum = 0

    for right in range(len(arr)):

        while arr[right] in seen:
            seen.remove(arr[left])
            left += 1

        seen.add(arr[right])

        maximum = max(maximum, right - left + 1)

    return maximum


arr = [10, 20, 10, 30, 40, 20]

print(longest_unique(arr))
```

### Complexity

```text
Time  : O(n)
Space : O(n)
```

### Sliding Window

```text
left  → shrink
right → expand
```

---

# 7. Bank Transaction Analysis

### Pattern

**Prefix Sum + HashMap**

### Problem

Count the number of continuous subarrays whose sum equals a given target.

### Example

```text
Input:
arr = [1, 2, 3, 2]
target = 5

Output:
2
```

The valid subarrays are:

```text
[2, 3]
[3, 2]
```

### Code

```python
def count_subarrays(arr, target):
    prefix_sum = 0
    count = 0

    frequency = {0: 1}

    for num in arr:

        prefix_sum += num

        required = prefix_sum - target

        if required in frequency:
            count += frequency[required]

        frequency[prefix_sum] = frequency.get(prefix_sum, 0) + 1

    return count


arr = [1, 2, 3, 2]
target = 5

print(count_subarrays(arr, target))
```

### Complexity

```text
Time  : O(n)
Space : O(n)
```

### Core Formula

```text
required = current_prefix_sum - target
```

---

# 8. Employee Skill Grouping

### Pattern

**HashMap + Sorting**

### Problem

Group words that are anagrams of each other.

### Example

```text
Input:
["eat", "tea", "tan", "ate", "nat", "bat"]
```

Output:

```text
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

### Code

```python
def group_anagrams(words):
    groups = {}

    for word in words:

        key = ''.join(sorted(word))

        if key not in groups:
            groups[key] = []

        groups[key].append(word)

    return list(groups.values())


words = ["eat", "tea", "tan", "ate", "nat", "bat"]

print(group_anagrams(words))
```

### Why sorting?

```text
eat → aet
tea → aet
ate → aet
```

Same sorted key means they are anagrams.

### Complexity

Approximately:

```text
Time  : O(n × k log k)
Space : O(n × k)
```

where `k` is the average word length.

---

# 9. Network Packet Analysis

### Pattern

**HashSet**

### Problem

Find the length of the longest consecutive numerical sequence, regardless of input order.

### Example

```text
Input:
[100, 4, 200, 1, 3, 2]

Output:
4
```

Sequence:

```text
1 → 2 → 3 → 4
```

### Code

```python
def longest_consecutive(arr):
    numbers = set(arr)
    maximum = 0

    for num in numbers:

        if num - 1 not in numbers:

            current = num
            length = 1

            while current + 1 in numbers:
                current += 1
                length += 1

            maximum = max(maximum, length)

    return maximum


arr = [100, 4, 200, 1, 3, 2]

print(longest_consecutive(arr))
```

### Important idea

We start counting only when:

```python
num - 1 not in numbers
```

That means `num` is the **beginning** of a sequence.

### Complexity

```text
Time  : O(n)
Space : O(n)
```

---

# 10. Hospital Appointment Scheduling

### Pattern

**Sorting + Intervals**

### Problem

Merge overlapping appointment intervals.

### Example

```text
Input:
[[1, 3], [2, 6], [8, 10], [9, 12]]

Output:
[[1, 6], [8, 12]]
```

### Code

```python
def merge_intervals(intervals):

    intervals.sort()

    merged = []

    for interval in intervals:

        if not merged or interval[0] > merged[-1][1]:
            merged.append(interval)

        else:
            merged[-1][1] = max(
                merged[-1][1],
                interval[1]
            )

    return merged


appointments = [[1, 3], [2, 6], [8, 10], [9, 12]]

print(merge_intervals(appointments))
```

### Complexity

```text
Time  : O(n log n)
Space : O(n)
```

---

# 🧠 Pattern Recognition Cheat Sheet

```text
┌────────────────────────────────────────────────┐
│ Problem wording                  Pattern       │
├────────────────────────────────────────────────┤
│ Longest unique continuous        Sliding Window│
│ Maximum continuous SUM           Kadane        │
│ Maximum continuous PRODUCT       Product Kadane│
│ Rainwater                        Two Pointers  │
│ Subarray sum = target            Prefix Sum    │
│ Group anagrams                   HashMap       │
│ Consecutive numbers              HashSet       │
│ Overlapping intervals            Sorting       │
└────────────────────────────────────────────────┘
```

## DSA Thinking Process

Don't memorize the complete code.

For every question, first ask:

```text
┌──────────────────────────────────┐
│ 1. What is the question asking? │
│                                  │
│ 2. Continuous?                   │
│       ↓                          │
│    Sliding Window / Kadane       │
│                                  │
│ 3. Maximum SUM?                  │
│       ↓                          │
│    Kadane                        │
│                                  │
│ 4. Maximum PRODUCT?              │
│       ↓                          │
│    Max + Min Product             │
│                                  │
│ 5. Need fast lookup?             │
│       ↓                          │
│    Set / HashMap                 │
│                                  │
│ 6. Overlapping ranges?           │
│       ↓                          │
│    Sorting + Intervals           │
└──────────────────────────────────┘
```

## Complexity Summary

| #  | Pattern              |           Time |    Space |
| -- | -------------------- | -------------: | -------: |
| 1  | Sliding Window + Set |           O(n) |     O(n) |
| 2  | Kadane               |           O(n) |     O(1) |
| 3  | Two Pointers         |           O(n) |     O(1) |
| 4  | Kadane               |           O(n) |     O(1) |
| 5  | Maximum Product      |           O(n) |     O(1) |
| 6  | Sliding Window + Set |           O(n) |     O(n) |
| 7  | Prefix Sum + HashMap |           O(n) |     O(n) |
| 8  | HashMap + Sorting    | O(n × k log k) | O(n × k) |
| 9  | HashSet              |           O(n) |     O(n) |
| 10 | Sorting + Intervals  |     O(n log n) |     O(n) |

---

## 🎯 Learning Order

Recommended order for understanding these problems:

```text
1. Sliding Window
       ↓
2. Kadane
       ↓
3. Two Pointers
       ↓
4. Maximum Product Subarray
       ↓
5. Prefix Sum
       ↓
6. HashMap
       ↓
7. HashSet
       ↓
8. Sorting + Intervals
```

The important goal is **not memorizing these 10 programs**. The goal is to look at a new question and identify which pattern it belongs to.
****
