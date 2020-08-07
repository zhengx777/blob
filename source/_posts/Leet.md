---
title: Leetcode题解
date: 2020-07-24
tags:
---

### 1. 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

``` python

from typing import List
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                if(nums[i]+nums[j] == target):
                    return [i,j]
        return []
    def twoSum2(self, nums: List[int], target: int) -> List[int]:
        table = {}
        for i in range(len(nums)):
            table[nums[i]] = i
        for i in range(len(nums)):
            if((target - nums[i]) in table and table[target - nums[i]]!=i):
                return [i,table[target - nums[i]]]
        return []
    def towSum3(self, nums: List[int], target:int) -> List[int]:
        table = {}
        for i in range(len(nums)):
            if((target-nums[i]) in table):
                return [table[target - nums[i]],i]
            table[nums[i]] = i
        return []

```

三种方法，第一种暴力，第二种两遍hash表，第三种一遍hash表。

主要是使用hash表用空间来换取时间，这里假设hash表的查找时间很理想为O(1)。


### 2. 两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

``` python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummyNode = ListNode(None)
        cur = dummyNode
        carry = 0
        while (l1 or l2 or carry > 0):
            x = 0
            y = 0
            if (l1):
                x = l1.val
                l1 = l1.next
            if (l2):
                y = l2.val
                l2 = l2.next
            newValue = (x + y + carry )% 10
            carry = (x + y + carry )// 10
            cur.next = ListNode(newValue)
            cur = cur.next
        return dummyNode.next
```


考察链表的基本操作，考察取模和整除等用法, 需要注意最后的进位。

### 3. 无重复字符的最长子串


考虑使用hashmap少一次调用
