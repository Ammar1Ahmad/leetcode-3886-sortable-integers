# leetcode-3886-sortable-integers
Python solution for LeetCode 3886 – Sortable Integers, featuring an algorithm explanation, correctness analysis, time and space complexity, and test cases.
# LeetCode 3886 — Sortable Integers

<p align="center">
  <img src="https://img.shields.io/badge/LeetCode-3886-orange?style=for-the-badge&logo=leetcode" alt="LeetCode Problem 3886">
  <img src="https://img.shields.io/badge/Language-Python-blue?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Topic-Algorithms-green?style=for-the-badge" alt="Algorithms">
</p>

A Python solution to **LeetCode 3886 — Sortable Integers**, with an explanation of the algorithm, key observations, correctness proof, complexity analysis, and example test cases.

---

## 📌 Problem Statement

You are given an integer array `nums` of length `n`.

An integer `k` is called **sortable** if:

1. `k` divides `n`.
2. The array can be partitioned into consecutive subarrays of length `k`.
3. Each subarray can be cyclically rotated independently any number of times to make the entire array sorted in non-decreasing order.

Return the **sum of all possible sortable integers `k`**.

### Examples

**Example 1**

```python
Input:  nums = [3, 1, 2]
Output: 3
```

Explanation: Only `k = 3` is sortable. The subarray `[3, 1, 2]` can be rotated to `[1, 2, 3]`.

**Example 2**

```python
Input:  nums = [7, 6, 5]
Output: 0
```

Explanation: Neither `k = 1` nor `k = 3` can make the array sorted using the permitted rotations.

**Example 3**

```python
Input:  nums = [5, 8]
Output: 3
```

Explanation: Both `k = 1` and `k = 2` are sortable. The answer is `1 + 2 = 3`.

### Constraints

- `1 <= n == nums.length <= 10^5`
- `1 <= nums[i] <= 10^5`

---

## 💡 Approach

The key observation is that **a block can be cyclically rotated into non-decreasing order if and only if its circular sequence contains at most one descent**.

A descent occurs when an element is greater than the next element. The circular comparison also includes the last element compared with the first.

For every divisor `k` of `n`:

1. Divide the array into consecutive blocks of length `k`.
2. Count the descents in each block, including the circular boundary.
3. Reject `k` if any block has more than one descent.
4. Check that the blocks can be concatenated in sorted order: the maximum of each block must be less than or equal to the minimum of the next block.
5. If all conditions hold, add `k` to the answer.

This approach avoids generating rotations or sorting individual blocks.

---

## 🐍 Python Solution

```python
class Solution:
    def sortableIntegers(self, nums: list[int]) -> int:
        n = len(nums)
        answer = 0

        for k in range(1, n + 1):
            if n % k != 0:
                continue

            valid = True
            previous_block_max = None

            for start in range(0, n, k):
                end = start + k
                first = nums[start]

                block_min = first
                block_max = first
                descents = 0

                for i in range(start + 1, end):
                    if nums[i - 1] > nums[i]:
                        descents += 1

                    block_min = min(block_min, nums[i])
                    block_max = max(block_max, nums[i])

                # Include the circular last-to-first comparison.
                if nums[end - 1] > first:
                    descents += 1

                if descents > 1:
                    valid = False
                    break

                # Consecutive sorted blocks must join correctly.
                if (
                    previous_block_max is not None
                    and previous_block_max > block_min
                ):
                    valid = False
                    break

                previous_block_max = block_max

            if valid:
                answer += k

        return answer
```

---

## 🔍 Dry Run

Input:

```python
nums = [3, 1, 2]
```

The length is `n = 3`, so its divisors are `1` and `3`.

| `k` | Blocks | Result |
|---:|---|---|
| 1 | `[3]`, `[1]`, `[2]` | Invalid because the blocks cannot be rearranged |
| 3 | `[3, 1, 2]` | Valid; rotate to `[1, 2, 3]` |

Therefore:

```text
Answer = 3
```

---

## ✅ Correctness Proof

For a fixed divisor `k`, consider each block in order.

**1. Internal feasibility**

A circular block can be rotated into non-decreasing order exactly when it has at most one strict descent around its circular sequence. The algorithm counts these descents and rejects the block if the count exceeds one.

**2. Boundary feasibility**

After rotation into sorted order, each block begins with its minimum and ends with its maximum. The concatenation of sorted blocks is non-decreasing exactly when the maximum of every block is no greater than the minimum of the next block. The algorithm verifies this condition.

**3. Correct sum**

The algorithm adds `k` only if every block is internally sortable and every boundary between consecutive blocks is valid. Thus, it includes every sortable divisor and excludes every non-sortable divisor.

Therefore, the returned sum is correct.

---

## ⏱️ Complexity Analysis

Let `d(n)` denote the number of positive divisors of `n`.

- **Time Complexity:** `O(n · d(n))` — each divisor requires at most one linear scan of the array.
- **Auxiliary Space Complexity:** `O(1)` — only a fixed number of variables are used.

---

## 🧪 Test Cases

| Input | Expected Output |
|---|---:|
| `[3, 1, 2]` | `3` |
| `[7, 6, 5]` | `0` |
| `[5, 8]` | `3` |
| `[1, 2, 3, 4]` | `7` |
| `[9]` | `1` |
| `[2, 2, 2, 2]` | `7` |

---

## 🚀 How to Use

1. Open [LeetCode](https://leetcode.com/).
2. Find problem **3886 — Sortable Integers**.
3. Select Python as the programming language.
4. Copy the solution above into the editor.
5. Submit to run the judge's test cases.

**Note:** Keep the method name `sortableIntegers` exactly as required by the judge.

---

## 📚 Key Takeaways

- Generate only divisors of `n` as candidates.
- Use circular descent counting to identify blocks sortable by rotation.
- Check block boundaries to guarantee global non-decreasing order.
- Avoid unnecessary sorting and rotation simulation.

---

## 👨‍💻 Author

**Ammar Ahmad**

Computer Science & Engineering | Python | Data Structures & Algorithms | Problem Solving

---

## 📄 License

This repository's solution and documentation are available under the [MIT License](https://opensource.org/license/mit).

If you reuse or adapt this work, attribution is appreciated.
