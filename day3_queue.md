# problem   239. Sliding Window Maximum
## describle
Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right.
You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

**Example**:
```
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
**Note**  

You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

**题意**:  

给定数组，有一个大小为k的从数组最左边滑到最右边的滑动窗。在窗口中只能看到k个数字，窗口每次滑动一个位置。返回最大滑动窗。

## method 
用双端队列qmax实现滑动窗的最大值的更新。队列qmax按照降序存放数组下标，按照如下规则进行放入和弹出操作。
 - 假设遍历到 nums[i],qmax的放入规则为：
 1.若qmax为空，则直接把i放入qmax，放入结束。
 2.若qmax不为空，取出当前qmax队尾存放的下标，记为j:
 1) 若nums[j] > nums[i] ,直接把下标i放入qmax队尾，放入结束；
 2) 若nums[j] <= nums[i] ,则将j从qmax弹出，继续放入规则。
 - 假设遍历到 nums[i],qmax的弹出规则为:  
 若qmax的队头下标等于 i-w ,说明当前qmax队头的下标已过期，弹出当前队头下标即可。
  
### code
```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        if not nums or len(nums)<k or k<=0:
            return []
        qmax = []   #存放最大值下标的降序队列
        res = []    
        for i in range(len(nums)):
            # 弹出直到队列为空 或 加入nums[i]队列为降序
            while ( qmax and (nums[qmax[-1]]<=nums[i]) ):
                qmax.pop(-1)
            qmax.append(i)
            # 队头过期
            if qmax[0] == i-k:
                qmax.pop(0)
                
            if i >= k-1:
                res.append(nums[qmax[0]])
                
        return res
```
