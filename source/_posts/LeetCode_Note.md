---
title: LeetCode Note
date: 2018-07-30 15:16:14
tags:
- python
- LeetCode
categories: code
---
LeetCode的刷题笔记
<!-- more -->

刷题，若干笔记
要点：永远考虑时间复杂度与存储空间需求

# Array and String

## Introduction to Array

### Find Pivot Index
Given an array of integers nums, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.


```python
def pivotIndex( nums):
        """
        :type nums: List[int]
        :rtype: int
        历遍list找pivot
        """
        sum_nums=sum(nums)
        left_sum=0
        for i,num in enumerate(nums):
            if left_sum == (sum_nums - left_sum - num):
                return i
            left_sum+=num
        return -1
```

### Largest Number At Least Twice of Others
In a given integer array nums, there is always exactly one largest element.

Find whether the largest element in the array is at least twice as much as every other number in the array.

If it is, return the index of the largest element, otherwise return -1.


```python
def dominantIndex( nums):
        """
        :type nums: List[int]
        :rtype: int
        对比sorted list最大和第二大元素，时间复杂度依据sort算法而定
        """
        if len(nums)==1:
            return 0
        indexes = range(len(nums))
        nums, indexes = zip(*sorted(zip(nums, indexes),reverse=True))
        
        return indexes[0] if nums[0]>=2*nums[1] else -1
```

### Plus One
Given a non-empty array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.


```python
def plusOne(self, digits):
        """
        :type digits: List[int]
        :rtype: List[int]
        从list尾部开始加1，如果大于十进一位
        """
        add_one_index=0
        digits=digits[::-1]
        while add_one_index<len(digits) and digits[add_one_index]==9:
            digits[add_one_index]=0
            add_one_index+=1
    
        if add_one_index==len(digits):
            digits.append(0)
        digits[add_one_index]+=1
        return digits[::-1]
```

### Diagonal Traverse
Given a matrix of M x N elements (M rows, N columns), return all elements of the matrix in diagonal order as shown in the below image.


```python
def findDiagonalOrder( matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        根据方向选择位置变化
        """
        temp_list=[]
        if not matrix:
            return temp_list
        r,c=0,0
        m,n=len(matrix),len(matrix[0])
        upper_right=True
        while len(temp_list)<m*n:
            temp_list.append(matrix[r][c])
            if upper_right:
                if r==0 or c==n-1:
                    upper_right=False
                    if c!=n-1:
                        c+=1
                    else:
                        r+=1                     
                else:
                    r-=1
                    c+=1                
            else:
                if r==m-1 or c==0:
                    upper_right=True
                    if r!=m-1:
                        r+=1
                    else:
                        c+=1
                else:
                    r+=1
                    c-=1
        return temp_list
```

### Spiral Matrix
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.


```python
def spiralOrder( matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        不断将最外圈元素依次加入list。另一种更python的做法：向左旋转matrix，将第一排元素加入list
        """
        if not matrix:
            return matrix
        ans = []
        m, n = len(matrix), len(matrix[0])
        u, d, l, r = 0, m - 1, 0, n - 1
        while l < r and u < d:
            ans.extend([matrix[u][j] for j in range(l, r)])
            ans.extend([matrix[i][r] for i in range(u, d)])
            ans.extend([matrix[d][j] for j in range(r, l, -1)])
            ans.extend([matrix[i][l] for i in range(d, u, -1)])
            u, d, l, r = u + 1, d - 1, l + 1, r - 1
        if l == r:
            ans.extend([matrix[i][r] for i in range(u, d + 1)])
        elif u == d:
            ans.extend([matrix[u][j] for j in range(l, r + 1)])
        return ans
```

### Pascal's Triangle
Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.
![Pascal's Triangle](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)


```python
def generate( numRows):
        """
        :type numRows: int
        :rtype: List[List[int]]
        迭代计算，多次计算将较小数缓存可以提高运行速度。
        """
        if numRows<1:
            return []
        elif numRows==1:
            return [[1]]
        elif numRows==2:
            return [[1],[1,1]]
        else:
            matrix=self.generate(numRows-1)
            temp_list=[1]
            for i in range(len(matrix[-1])-1):
                temp_list.append(matrix[-1][i]+matrix[-1][i+1])
            temp_list.append(1)
            matrix.append(temp_list)
            return matrix
```
