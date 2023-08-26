# Maximum Subarray

[LeetCode Linnk](https://leetcode.com/problems/maximum-subarray/)

Level: Medium
Data Structure: Array
Approach: 
* Dynamic Programming (w/o Kadane)
* Kadane's Algorithm
* Divide & Conquer

Pre-Requisites: 
* Arrays
* Dynamic Programming
* Kadane's Algorithm

Solution code for all approaches [here](Maximum-Subarray.js)

## Problem

`Given an integer array nums, find the subarray with the largest sum, and return its sum.`

The first clue that would make you suspect that you should use dynamic programming to solve this problem is when you're asked to find the max or min of something. 

There are a few ways we can even dynamically program our solution:
* dynamic programming without the Kadane Algorithm
* dynamic programming with the Kadane Algorithm

For this walkthrough, we are approaching this problem with Kadane's algorithm.

### Algorithm - Kadane's Algorithm
We can systematically figure out where to start and where to end by doing the following: 

1. Create two integer variables and set them both to the first value of the given array
* `maxEndingHere`: This variable keeps track of the maximum subarray sum ending at the current position of the iteration. It represents the best sum achievable considering only the current element and the subarray ending just before the current element.
* `maxSoFar`: This variable keeps track of the maximum subarray sum encountered so far as the algorithm progresses through the array.

2. Since we've saved the first element of the array already in the two above variables, let's start iterating the array but at the second element. Add each number to `maxEndingHere`. If `maxEndingHere` becomes negative, we know we want to get rid of those elements of our subarray. 

3. `return maxSoFar`


``` 
var maxSubArray = function(nums) {
  let maxEndingHere = nums[0];
  let maxSoFar = maxEndingHere;

  for(let i = 1; i < nums.length; i++){
    maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }

  return maxSoFar;
} 
```


If you read step 2 and still aren't sure how to solve the problem, that's okay because we'll explain this more. From a javascript perspective, one very important operation to know that can help us here is `Math.max()`. [JS documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

`Math.max()` returns the largest of the numbers given as input parameters. 

So if our iterator looks like this: 

``` 
{
    for(let i = 1; i < nums.length; i++){
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
}
```

We know that every time we iterate, `maxEndingHere` will equal the highest number between `nums[i]` and `maxEndingHere + nums[i]`. 

For `maxSoFar` will equal the highest number between `maxSoFar` and `maxEndingHere`.


### Use Case Walkthrough

**Use Case 1:**

`nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`

First Iteration:

- `maxEndingHere = -2` (initialized)
- `maxSoFar = -2` (initialized)
- (inside of the loop)
- `i = 1`
- `nums[i] = 1`
- `maxEndingHere = Math.max(1, (-2 + 1))` (works out to `maxEndingHere = 1`)
- `maxSoFar = Math.max(-2, 1)` (works out to `maxSoFar = 1`)

Second Iteration: 

- `maxEndingHere = 1` 
- `maxSoFar = 1`
- (inside of the loop)
- `i = 2`
- `nums[i] = -3`
- `maxEndingHere = Math.max(-3, (1 + -3))` (works out to `maxEndingHere = -2`)
- `maxSoFar = Math.max(1, -2)` (works out to `maxSoFar = 1`)

Third Iteration: 

- `maxEndingHere = -2` 
- `maxSoFar = 1`
- (inside of the loop)
- `i = 3`
- `nums[i] = 4`
- `maxEndingHere = Math.max(4, (-2 + 4))` (works out to `maxEndingHere = 4`)
- `maxSoFar = Math.max(1, 4)` (works out to `maxSoFar = 4`)

Fourth Iteration: 

- `maxEndingHere = 4` 
- `maxSoFar = 4`
- (inside of the loop)
- `i = 4`
- `nums[i] = -1`
- `maxEndingHere = Math.max(-1, (4 + -1))` (works out to `maxEndingHere = 3`)
- `maxSoFar = Math.max(4, 3)` (works out to `maxSoFar = 4`)

Fifth Iteration: 

- `maxEndingHere = 3` 
- `maxSoFar = 4`
- (inside of the loop)
- `i = 5`
- `nums[i] = 2`
- `maxEndingHere = Math.max(2, (3 + 2))` (works out to `maxEndingHere = 5`)
- `maxSoFar = Math.max(4, 5)` (works out to `maxSoFar = 5`)

Sixth Iteration: 

- `maxEndingHere = 5` 
- `maxSoFar = 5`
- (inside of the loop)
- `i = 6`
- `nums[i] = 1`
- `maxEndingHere = Math.max(1, (5 + 1))` (works out to `maxEndingHere = 6`)
- `maxSoFar = Math.max(5, 6)` (works out to `maxSoFar = 6`)

Seventh Iteration: 

- `maxEndingHere = 6` 
- `maxSoFar = 6`
- (inside of the loop)
- `i = 7`
- `nums[i] = -5`
- `maxEndingHere = Math.max(-5, (6 + -5))` (works out to `maxEndingHere = 1`)
- `maxSoFar = Math.max(6, 1)` (works out to `maxSoFar = 6`)

Eighth Iteration: 

- `maxEndingHere = 1` 
- `maxSoFar = 6`
- (inside of the loop)
- `i = 8`
- `nums[i] = 4`
- `maxEndingHere = Math.max(6, (1 + 4))` (works out to `maxEndingHere = 6`)
- `maxSoFar = Math.max(6, 6)` (works out to `maxSoFar = 6`)

Final Return:
- `6`

**Explanation**

The reason this works is because as we work through the array and continue to add to `maxSoFar`, we come to find that through that process, we end up finding the max value of the subarray. The answer wanted us to return the value of that max subarray, which is what we do when we return `maxSoFar`. 

Great for us, we don't have to return the actual subarray that equals `maxSoFar`, so our work is done here. 

One common mistake is to forget while we're working through this problem that we have to know which elements make up the `maxSoFar` and if you approach the problem through that lens, you will have a hard time solving this problem. 


### Complexity Analysis

**Time Complexity**: O(n) `n = nums.length` (since we iterate through the array and interact with all elements)
**Space Complexity**: O(1) (regardless of `nums.length`, we will always only create our two variables: `maxSoFar` and `maxEndingHere`)

### Next

if(Did you solve it on the first go?) 
    [Check this problem out]()
else
    [Check this problem out]()
