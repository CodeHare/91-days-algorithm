# 69. x 的平方根

https://leetcode-cn.com/problems/sqrtx

## 题目描述

```
实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

示例 1:

输入: 4
输出: 2
示例 2:

输入: 8
输出: 2
说明: 8 的平方根是 2.82842...,
  由于返回类型是整数，小数部分将被舍去

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sqrtx
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 思路

这个问题分两种情况：

1. x 有整数平方根
2. x 没有整数平方根

第 1 种就是最基本的**二分法查找目标值**，而第 2 种可以转化成**寻找最右边的满足条件的值**，在这个问题里，这个条件就是 target 的平方小于 x (因为题目要求结果只保留整数部分)。

-   首先定义搜索区间为 [l, r]，注意左右都是闭区间。
-   在循环过程中，如果碰到 m 平方等于 x 就可以提前返回了。
-   如果 m 平方小于 x，收缩左边界，如果 m 平方大于 x，收缩右边界。
-   最后搜索区间会被缩小到只剩一个数字 n，如果 n 不是 x 的整数平方根，那么还剩两种情况：
    1. 如果 $n^2 > x$，那么 n-1 就是我们想要的结果，而由于 n 平方大于 x 时我们会收缩右边界，此时右指针会左移，刚好指向 n-1，同时结束了循环，最后我们返回右指针 r 即可。
    2. 如果 $n^2 < x$，那么 n 就是我们想要的结果，由于 n 平方小于 x 时左边界会收缩，此时左指针右移，右指针不动，依然指向 n，循环结束，最后我们还是返回右指针 r。
-   所以循环结束后我们直接返回右指针 r 即可。

需要特别注意一下的是 0 和 1 这两个数字，不过上面的算法对这两个数字也是有效的。

## 复杂度

-   时间复杂度：$O(logx)$
-   空间复杂度：$O(1)$

## 代码

Python Code

```py
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        l, r, m = 0, x,  0
        while l <= r:
            m = l + (r - l) // 2
            if m ** 2 == x: return m
            elif m ** 2 > x : r = m - 1
            else: l = m + 1
        return r
```