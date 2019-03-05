# problem   17. Letter Combinations of a Phone Number  
## describle
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
![](https://github.com/koala7580/weekCoding/blob/master/img/200px-Telephone-keypad2.svg.png)

Example:
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.

*题意*
如上的数字键盘对应不同的字母，要求输入数字串，给出所有不同的字母组合。

### code
```python
class Solution(object):
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if len(digits) <= 0:
            return []
        
        phone_dict = {'2':'abc', '3':'def', '4':'ghi', '5':'jkl',\
                     '6':'mno', '7':'pqrs', '8':'tuv', '9':'wxyz' }
        ret = ['']
        for dig in digits:
            # 逐个数字
            tmp_ret = []
            for alpha in phone_dict[dig]:
                # 每个数字对应的一组字母
                for tmp in ret:
                    # 将单个字母逐次追加到现有字母末尾
                    tmp_ret.append(tmp + alpha)
            ret = tmp_ret
        return ret
```


# problem  	46. Permutations
## describle
Given a collection of distinct integers, return all possible permutations.

Example:
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```
## method

### code
```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        self.dfs(nums, res, [])
        return res
    
    def dfs(self, nums, res, path):
        if not nums:
            res.append(path)
        else:
            for i in range(len(nums)):
                self.dfs(nums[:i] + nums[i+1:], res, path + [nums[i]])
 
```

递归、动态规划被虐得有点惨，难受 >.< 

*参考链接*
[Permutations](https://blog.csdn.net/fuxuemingzhu/article/details/79363903#_44)
