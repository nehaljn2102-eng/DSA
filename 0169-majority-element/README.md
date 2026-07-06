# 0169-majority-element

## 📋 Problem Description
Given an array `nums` of size `n`, the task is to find and return the *majority element*.

The majority element is defined as the element that appears more than `⌊n / 2⌋` times (i.e., strictly more than half the array's length). You are guaranteed that such a majority element always exists within the given array.

The function `majorityElement` receives a `std::vector<int>` named `nums` as input and must return an `int` representing the majority element.

## 🔍 Examples
```
Input: nums = [3,2,3]
Output: 3
Explanation: The array has 3 elements. ⌊3 / 2⌋ = 1. The majority element must appear more than 1 time. '3' appears 2 times, which is more than 1.

Input: nums = [2,2,1,1,1,2,2]
Output: 2
Explanation: The array has 7 elements. ⌊7 / 2⌋ = 3. The majority element must appear more than 3 times. '2' appears 4 times, which is more than 3.
```

## 📌 Constraints
*   `n == nums.length`
*   `1 <= n <= 5 * 10^4`
*   `-10^9 <= nums[i] <= 10^9`
*   The input is generated such that a majority element will always exist in the array.

## 🤔 Understanding the Problem
The problem asks us to identify a specific element in an array: the one that occurs most frequently, specifically more than half the time. The guarantee that a majority element *always* exists simplifies things, as we don't need to handle cases where no such element is present. The core challenge is to find this element efficiently, especially considering the follow-up question about linear time and constant space.

## 💡 Core Idea
The core idea behind the provided solution is that if an element appears more than `n/2` times, then after sorting the array, all its occurrences will be grouped together. By iterating through the sorted array and counting consecutive occurrences, we can identify when an element's frequency surpasses the `n/2` threshold.

## 🧠 Approach — Sorting
The chosen approach is **Sorting**.
This pattern fits the problem because sorting brings identical elements together. Once the array is sorted, all occurrences of a particular number will be adjacent. This makes it straightforward to count the frequency of each distinct number by simply iterating through the sorted array and checking for consecutive identical elements. Since the majority element appears more than `n/2` times, its block of consecutive occurrences will be the largest, and we can detect it as soon as its count exceeds `n/2`.

## 📝 Step-by-Step Algorithm
1.  **Sort the Array**: Begin by sorting the input array `nums` in non-decreasing order. This groups all identical elements together.
2.  **Initialize Counters**:
    *   Initialize a variable `freq` to `1` to count the frequency of the current element.
    *   Initialize a variable `ans` to `nums[0]` to store the potential majority element.
    *   Get the size of the array, `n`.
3.  **Iterate and Count**: Loop through the sorted array starting from the second element (index `1`) up to the end (`n-1`).
    *   **Check for Same Element**: If the current element `nums[i]` is the same as the previous element `nums[i-1]`, increment `freq`.
    *   **Check for Different Element**: If `nums[i]` is different from `nums[i-1]`, it means a new distinct element sequence has started. Reset `freq` to `1` and update `ans` to `nums[i]`.
    *   **Check Majority Condition**: After updating `freq` (whether incremented or reset), check if `freq` is greater than `n/2`. If it is, then `ans` holds the majority element, and we can immediately return `ans`.
4.  **Final Return**: If the loop completes without returning (which would only happen if the majority element's count crosses `n/2` exactly at the end of the array, or if the majority element is the very last element and its count is only confirmed at the end), the last `ans` stored will be the majority element. Return `ans`.

## 💻 Solution
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
       int n = nums.size(); // Get the size of the input array.

       // Step 1: Sort the array.
       // This brings all identical elements together, making it easy to count their frequencies.
       sort(nums.begin(), nums.end());

       // Step 2: Initialize counters.
       // 'freq' will store the count of consecutive occurrences of the current element.
       // 'ans' will store the current element being tracked, which might be the majority element.
       // We start with the first element and its frequency as 1.
       int freq = 1;
       int ans = nums[0];

       // Step 3: Iterate through the sorted array starting from the second element.
       for (int i = 1; i < n; i++) {
           // If the current element is the same as the previous one,
           // it means we are still counting occurrences of the same number.
           if (nums[i] == nums[i-1]) {
               freq++; // Increment its frequency.
           } else {
               // If the current element is different from the previous one,
               // a new sequence of numbers has started.
               freq = 1; // Reset frequency to 1 for the new number.
               ans = nums[i]; // Update 'ans' to this new number.
           }

           // Step 4: After updating frequency, check if it has surpassed the majority threshold.
           // If 'freq' is greater than n/2, we have found the majority element.
           // We can return it immediately because the problem guarantees a majority element exists.
           if (freq > n / 2) {
               return ans;
           }
       }

       // This line is reached if the majority element is the last element(s) in the array
       // and its frequency only crosses n/2 at the very end of the loop.
       // Since a majority element is guaranteed to exist, 'ans' will correctly hold it.
       return ans;
    }
};
```

## ⏱️ Complexity Analysis
| | Complexity | Reason |
|---|---|---|
| **Time** | O(N log N) | Dominated by the `std::sort` operation, which typically uses an algorithm like Introsort. The subsequent linear scan takes O(N) time. |
| **Space** | O(log N) | `std::sort` typically uses O(log N) auxiliary space for its recursion stack (e.g., in quicksort). If an in-place sort with O(1) auxiliary space is used, it would be O(1). |

## 🔗 Related Problems
- 229. Majority Element II
- 136. Single Number
- 215. Kth Largest Element in an Array