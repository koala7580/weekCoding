# problem 1
## describle

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## solution
### method: hashTable
用哈希表反存索引，以原数组的value为dict的key。当 目标值-当前值 在dict中时，即按顺序返回索引。
### code

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        keys = {}
        for i in range(len(nums)):
            if (target - nums[i]) in keys:
                return [keys[target - nums[i]], i]
            if nums[i] not in keys:
                keys[nums[i]] = i
```

# problem 202
## describle
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 
```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```
## solution
### method: hashTable  
两种结果,和为1：return True;进入循环且不含1：return False。
用哈希表存放出现过的数字，进入循环即当前数字在哈希表中。  
### code
```python
class Solution:  
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        loop_flag = False
        num_dict = {}
        while True:
            num_dict[n] = True
            sum = 0
            while(n>0):
                tmp = n%10
                sum += tmp**2
                n //= 10
            if sum == 1:
                return True
            elif sum in num_dict:
                return False
            else:
                n = sum
```
