# problem  142. Linked List Cycle II
## describle
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
Note: Do not modify the linked list.  

题意：给定一个链表，判断其中是否有环，若有则返回环开始的索引；若无，则返回None
## method1 哈希表
用哈希表存放node，遍历的同时判断是否出现在哈希表中，若出现过即有环，返回下标；若遍历结束未出现则无环。
### code
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        cycle_dict = {}
        # 空节点或单一节点，无环
        if head is None or head.next is None:
            return None
        # 遍历列表，判断是否有环
        while head is not None:
            if head.next is None:
                return None
            # 节点出现在哈希表中，有环
            if head.next in cycle_dict:
                return head.next           
            cycle_dict[head] = True
            head = head.next
        # 遍历结束，无环    
        return None
```
## method2 快慢指针
设置两个指针，fast是low的2倍速度，若有环则fast一定能追上slow。(下图参考他人博客)  

![fast-slow](https://github.com/koala7580/weekCoding/blob/master/fast_slow.png)  

图中Y点为相遇点，则必有 a+b+n*(b+c) = 2(a+b+m*(b+c)) ,化简得 (n-2m)(b+c) - b = a
由于 a>=0 ，第一次相遇时 m=0，n=1，有 c=a 。
故在判断有环之后，设置fast从head开始，那么fast和slow将在Y处相遇
### code
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        fast = slow = head 
        # 判断是否有环
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                break
        # 空节点或单节点，无环
        if fast is None or fast.next is None:
            return None
        # fast与slow相遇节点
        fast = head
        while fast != slow:
            fast = fast.next
            slow = slow.next
        return fast
```

# problem  206. Reverse Linked List
## describle
Reverse a singly linked list.
Example:
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

题意：反转链表
## method
tail保存前一个指针，cur为当前指针，head为下一个指针。  
主要是指针移动，要在head移向下一个指针之前，保存当前指针，并链接为tail的头指针。
### code
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        # 空节点和单节点直接返回
        if head is None or head.next is None:
            return head
        
        tail = head
        head = head.next
        tail.next = None
        
        while head :
            cur = head
            head = head.next
            cur.next = tail
            tail = cur
            cur = head
            
        return tail
```
