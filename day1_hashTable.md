<html lang="en"><head>
    <meta charset="UTF-8">
    <title></title>
<style id="system" type="text/css">h1,h2,h3,h4,h5,h6,p,blockquote {    margin: 0;    padding: 0;}body {    font-family: "Helvetica Neue", Helvetica, "Hiragino Sans GB", Arial, sans-serif;    font-size: 13px;    line-height: 18px;    color: #737373;    margin: 10px 13px 10px 13px;}a {    color: #0069d6;}a:hover {    color: #0050a3;    text-decoration: none;}a img {    border: none;}p {    margin-bottom: 9px;}h1,h2,h3,h4,h5,h6 {    color: #404040;    line-height: 36px;}h1 {    margin-bottom: 18px;    font-size: 30px;}h2 {    font-size: 24px;}h3 {    font-size: 18px;}h4 {    font-size: 16px;}h5 {    font-size: 14px;}h6 {    font-size: 13px;}hr {    margin: 0 0 19px;    border: 0;    border-bottom: 1px solid #ccc;}blockquote {    padding: 13px 13px 21px 15px;    margin-bottom: 18px;    font-family:georgia,serif;    font-style: italic;}blockquote:before {    content:"C";    font-size:40px;    margin-left:-10px;    font-family:georgia,serif;    color:#eee;}blockquote p {    font-size: 14px;    font-weight: 300;    line-height: 18px;    margin-bottom: 0;    font-style: italic;}code, pre {    font-family: Monaco, Andale Mono, Courier New, monospace;}code {    background-color: #fee9cc;    color: rgba(0, 0, 0, 0.75);    padding: 1px 3px;    font-size: 12px;    -webkit-border-radius: 3px;    -moz-border-radius: 3px;    border-radius: 3px;}pre {    display: block;    padding: 14px;    margin: 0 0 18px;    line-height: 16px;    font-size: 11px;    border: 1px solid #d9d9d9;    white-space: pre-wrap;    word-wrap: break-word;}pre code {    background-color: #fff;    color:#737373;    font-size: 11px;    padding: 0;}@media screen and (min-width: 768px) {    body {        width: 748px;        margin:10px auto;    }}</style><style id="custom" type="text/css"></style></head>
<body marginheight="0"><h1>problem 1</h1>
<h2>describle</h2>
<p>Given an array of integers, return indices of the two numbers such that they add up to a specific target.

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