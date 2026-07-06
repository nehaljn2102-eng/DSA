# 0169-majority-element

## 📋 Problem Description
Given an array `nums` of size `n`, the task is to find and return the *majority element*.

The majority element is defined as the element that appears more than `⌊n / 2⌋` times in the array. You are guaranteed that such a majority element always exists within the given array.

**Input:** An integer array `nums`.
**Output:** The majority element (an integer).

## 🔍 Examples
```
Input: nums = [3,2,3]
Output: 3
Explanation: The array has 3 elements. ⌊3 / 2⌋ = 1. The element '3' appears 2 times, which is more than 1.
```

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
Explanation: The array has 7 elements. ⌊7 / 2⌋ = 3. The element '2' appears 4 times, which is more than 3.
```

## 📌 Constraints
*   `n == nums.length`
*   `1 <= n <= 5 * 10^4`
*   `-10^9 <= nums[i] <= 10^9`
*   The input is generated such that a majority element will always exist in the array.

## 🤔 Understanding the Problem
The problem asks us to identify a specific element in an array: one that occurs more frequently than half the total number of elements. For instance, if an array has 7 elements, the majority element must appear at least 4 times. The problem guarantees that such an element will always be present, simplifying the task as we don't need to handle cases where no majority element exists. This guarantee is crucial because it means we don't have to worry about returning a "not found" value.

## 💡 Core Idea
If an array is sorted, all occurrences of the same element will be grouped together consecutively. This property allows us to easily count the frequency of each distinct element by iterating through the sorted array and tracking consecutive identical values. Since a majority element is guaranteed to exist and appears more than `n/2` times, it will necessarily maintain its majority status even after sorting.

## 🧠 Approach — Sorting and Counting
This problem can be effectively solved using a **Sorting** approach.
The reason sorting fits this problem well is due to the definition of a majority element. By sorting the array, all identical elements are placed next to each other. This makes it straightforward to count the occurrences of each element. We can then iterate through the sorted array, keeping a running count of consecutive identical elements. As soon as this count exceeds `n/2`, we've found our majority element. The problem's guarantee that a majority element always exists ensures that this process will eventually yield a result.

## 📝 Step-by-Step Algorithm
1.  **Get Array Size**: Determine the number of elements `n` in the input array `nums`.
2.  **Sort the Array**: Sort the `nums` array in non-decreasing order. This groups all identical elements together.
3.  **Initialize Counters**:
    *   Initialize `freq` to `1` (to count the first element).
    *   Initialize `ans` to `nums[0]` (our current candidate for the majority element).
4.  **Iterate and Count**: Loop through the sorted array starting from the second element (index `1`) up to `n-1`.
    *   **Check for Consecutive Elements**: If the current element `nums[i]` is the same as the previous element `nums[i-1]`:
        *   Increment `freq`.
    *   **Handle New Element**: If `nums[i]` is different from `nums[i-1]`:
        *   Reset `freq` to `1` (as we're starting to count a new distinct element).
        *   Update `ans` to `nums[i]` (this new element becomes our current candidate).
5.  **Check Majority Condition**: After each increment or reset of `freq`, check if `freq` is greater than `n / 2`.
    *   If `freq > n / 2`, then `ans` is the majority element. Return `ans` immediately.
6.  **Final Return**: If the loop completes without returning (which might happen if the majority element is the very last element or sequence of elements), the last `ans` value will be the majority element due to the problem's guarantee. Return `ans`.

## 💻 Solution
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
       int n = nums.size(); // Get the total number of elements in the array.

       // The commented-out section below represents a brute-force approach.
       // It iterates through each unique element and then counts its frequency
       // by iterating through the entire array again. This results in O(N^2) time complexity.
       /*
       for(int val : nums){
             int freq = 0;
             for(int el : nums){
                 if(el == val){
                     freq++;
                 }
             }
             if(freq > n / 2){
                 return val;
             }
         }
         return -1; // Should not be reached due to problem constraints
       */

        // Step 1: Sort the array.
        // This groups all identical elements together, making it easy to count their occurrences.
        // For example, [2,2,1,1,1,2,2] becomes [1,1,1,2,2,2,2].
        sort(nums.begin(), nums.end());

        // Step 2: Initialize variables for tracking frequency and the current candidate.
        int freq = 1;      // 'freq' will store the count of consecutive identical elements.
                           // It starts at 1 because we are considering nums[0] initially.
        int ans = nums[0]; // 'ans' stores the current element being counted.
                           // We start with the first element of the sorted array.

        // Step 3: Iterate through the sorted array starting from the second element.
        for (int i = 1; i < n; i++) {
            // If the current element is the same as the previous one, it's part of the same group.
            if (nums[i] == nums[i-1]) {
                freq++; // Increment its frequency count.
            } else {
                // If the current element is different from the previous one,
                // we've encountered a new distinct element.
                freq = 1;      // Reset frequency to 1 for this new element.
                ans = nums[i]; // Update 'ans' to this new element.
            }

            // Step 4: After updating frequency, check if it exceeds n/2.
            // If it does, we have found the majority element.
            if (freq > n / 2) {
                return ans; // Return the majority element immediately.
            }
        }

        // Step 5: If the loop completes without returning, it means the majority element
        // was the last sequence of elements encountered, or the only element.
        // Due to the problem guarantee that a majority element always exists,
        // the last 'ans' value will be the correct majority element.
        return ans;
    }
};
```

## ⏱️ Complexity Analysis
| | Complexity | Reason |
|---|---|---|
| **Time** | O(N log N) | The dominant operation is sorting the array, which typically takes O(N log N) time for comparison-based sorts like `std::sort`. The subsequent loop iterates through the array once, taking O(N) time. |
| **Space** | O(log N) or O(1) | `std::sort` typically uses O(log N) auxiliary space for its recursive calls (e.g., for introsort). In some highly optimized or specific cases (like heapsort), it might be O(1) auxiliary space. The additional variables used (`n`, `freq`, `ans`, `i`) consume O(1) space. |

## 🔗 Related Problems
*   229. Majority Element II
*   451. Sort Characters By Frequency
*   164. Maximum Gap