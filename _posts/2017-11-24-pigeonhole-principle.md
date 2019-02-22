---
layout: post
title: "LeetCode 523 — DP与鸽巢原理"
tags: [dna, oj, math, leetcode, 鸽巢原理, dynamic programming, dp]
comments: true
date: 2017-11-23 00:00:00
modified: 2017-11-24 00:00:00
---

## 鸽巢原理

> 鸽巢原理的一种简单的表述法为：
> 若有n个笼子和n+1只鸽子，所有的鸽子都被关在鸽笼里，那么至少有一个笼子有至少2只鸽子。
> 另一种为：
> 若有n个笼子和kn+1只鸽子，所有的鸽子都被关在鸽笼里，那么至少有一个笼子有至少k+1只鸽子。
> 一种推广表达是这样的：
> 如果要把`n`个物件分配到`m`个容器中，必有至少一个容器容纳至少\\( \lceil\frac{n}{m}\rceil \\)个物件。
>
> 鸽巢原理经常在计算机领域得到真正的应用。比如：哈希表的重复问题（冲突）是不可避免的，因为Keys的数目总是比Indices的数目多，不管是多么高明的算法都不可能解决这个问题。这个原理，还证明任何无损压缩算法，在把一个文件变小的同时，一定有其他文件增大来辅助，否则某些信息必然会丢失。
<p align="right">———维基百科</p>  

鸽巢原理的思想很简单，对应于每个具体问题就是每次选择都有`n`种情形供选择，若要选择`kn+1`次的话，那么必定存在一种选择是被选择了`k+1`次的。
<!--more-->

## Continuous Subarray Sum

[523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/#/description)的问题是：在非负整数列表中寻找是否至少有2个数的和等于n倍的k，k是给定的整数，n是任意整数。

问题看上去就很DP，我在最初设法解的时候，是先固定某一位，再在该位数字的右边寻找可能的解。这样复杂度是`O(n^2)`，虽然能AC，但是显然不是出题人想要的解：

```py
class Solution(object):
    def checkSubarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        Input: [23, 2, 4, 6, 7],  k=6
        Output: True
        """
        k *= cmp(k,0)
        for i in range(1, len(nums)):
            dp = copy.copy(nums)
            for j in range(i, len(nums)):
                if k==0:
                    if (nums[j] + dp[j - 1]) == 0:
                        return True
                    else:
                        dp[j] = nums[j] + dp[j - 1]
                else:
                    if (nums[j] + dp[j - 1]) % k == 0:
                        return True
                    else:
                        dp[j] = nums[j] + dp[j - 1]
        return False
```
在看了评论区的[zym_ustc的解法](https://discuss.leetcode.com/topic/80841/python-with-explanation-62ms-time-o-min-n-k-mostly/2)后，才明白这个问题必须明白一些trick才能降低复杂度。

```py
class Solution(object):
    def checkSubarraySum(self, nums, k):
        if k == 0:
            # if two continuous zeros in nums, return True
            # time O(n)
            for i in range(0, len(nums) - 1):
                if nums[i] == 0 and nums[i+1] == 0:
                    return True
            return False

        k = abs(k)
        if len(nums) >= k * 2:
            return True

        #if n >= 2k: return True
        #if n < 2k:  time O(n) is O(k) <-不明白这里为什么是O(k)···为何不是O(n)?

        sum = [0]
        for x in nums:
            sum.append((sum[-1] + x) % k)

        Dict = {}
        for i in range(0, len(sum)):
            if Dict.has_key(sum[i]):
                #若新的和 第一个 相同余数的间距大于1的话就是存在解
                if i - Dict[sum[i]] > 1:
                    return True
            else:
                # 仅在出现新的未见过的余数的时候才更新dic
                Dict[sum[i]] = i
        return False
```
这个解法就算没用鸽巢原理也是`O(n)`的复杂度：



但鸽巢原理的能直接使长数据的解法为`O(1)`：
若`nums`的长度大于`2k`的话，那么长度至少为`2k+1`的`sum`的每个数值对`k`取余数的的范围都会在`[0,k-1]`之间共`k`种。所以应用鸽巢原理知，出现同一种余数的`sum`元素至少有\\( \lceil\frac{2k+1}{k}\rceil=3 \\)个。而这仨`sum`的元素（我们假设为`a,b,c`）的`a`和`c`的间距肯定是大于等于2的，而且因为两者的值相等（即对`k`的余数相同），所以`c-a = n*k`。同理在`len(nums)`小于等于`2k`的时候也就是在`sum`中找相同的数值，且相同的数值的元素间距大于等于2即为所求。

上诉解法的更为简洁的形式如下：

```py
def checkSubarraySum(self, nums, k):
        r = 0
        seen = {0: -1}
        for i, num in enumerate(nums):
            # 不用处理 k为负数的情形
            r = (r + num) % k if k else r + num
            if r not in seen:
                seen[r] = i
            #必须大于1，只有隔了2位，才能保证nums对应元素个数大于等于2
            if i - seen[r] > 1:
                return True
        return False
```
