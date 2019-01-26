</p>
<p>You may assume that each input would have exactly one solution, and you may not use the same element twice.

</p>
<p>Example:

</p>
<pre><code>Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].</code></pre>
<h2>solution</h2>
<h3>method: hashTable</h3>
<p>用哈希表反存索引，以原数组的value为dict的key。当 目标值-当前值 在dict中时，即按顺序返回索引。
</p>
<h3>code</h3>
<pre><code class="lang-python">class Solution:
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
                keys[nums[i]] = i</code></pre>
<h1>problem 202</h1>
<h2>describle</h2>
<p>Write an algorithm to determine if a number is "happy".

</p>
<p>A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

</p>
<p>Example: 
</p>
<pre><code>Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1</code></pre>
<h2>solution</h2>
<h3>method: hashTable</h3>
<p>两种结果,和为1：return True;进入循环且不含1：return False。<br>用哈希表存放出现过的数字，进入循环即当前数字在哈希表中。
</p>
<h3>code</h3>
<pre><code class="lang-python">class Solution:  
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
            while(n&gt;0):
                tmp = n%10
                sum += tmp**2
                n //= 10
            if sum == 1:
                return True
            elif sum in num_dict:
                return False
            else:
                n = sum</code></pre>
</body></html>
