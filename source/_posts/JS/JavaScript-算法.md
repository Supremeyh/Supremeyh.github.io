---
title: JavaScript 算法
comments: true
date: 2019-03-24 13:26:28
categories: JS
tags: ['JS', 'Algorithm']
---
1. 斐波拉契数列
递归: 有边界条件，防止无限递归; 函数自身调用
```JavaScript
function Fibonacci(n) {
  if (n===0 || n===1)  return n
  return Fibonacci(n-1) + Fibonacci(n-2)
}
```

2. 阶乘
```JavaScript
function Factorial(n) {
  if (n===0 || n===1)  return 1
  Factorial(n-1) * n
}
```

3. 快速排序 Quicksort
快速排序的思想很简单，整个排序过程只需要三步：
（1）在数据集之中，选择一个元素作为"基准"（pivot）。
（2）所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
（3）对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。
```JavaScript
function quickSort(arr) {
  if (arr.length <= 1) { return arr }
  
  var pivotIndex = Math.floor(arr.length / 2)
  var pivot = arr.splice(pivotIndex, 1)[0]
  var left = [], right = []
  for (var i = 0; i < arr.length; i++) {
    if(arr[i] < pivot) {
      left.push(arr[i])
    } else {
      right.push(arr[i])
    }
  }

  return quickSort(left).concat(pivot, quickSort(right))
}
```

4. 二分法查找
二分法查找，也称折半查找，是一种在有序数组中查找特定元素的搜索算法。查找过程可以分为以下步骤：
（1）从有序数组的中间的元素开始搜索，如果该元素正好是目标元素（即要查找的元素），则搜索过程结束，否则进行下一步。
（2）如果目标元素大于或者小于中间元素，则在数组大于或小于中间元素的那一半区域查找，然后重复第一步的操作。
（3）如果某一步数组为空，则表示找不到目标元素。
```JavaScript
// 递归算法
// 二分查找法在算法家族大类中属于 分治法，分治法基本都可以用递归来实现的，二分查找法的递归JS实现如下：
function binarySearch(arr, key, low, high) {
  var low = low || 0, high = high || arr.length - 1
  if (low > high) { return - 1 }

  var mid = Math.floor((low + high) / 2)
  if (arr[mid] === key) {
    return mid
  } else if (key> arr[mid]) {
    return binarySearch(arr, key, mid+1, high)
  } else {
    return binarySearch(arr, key, low, mid-1)
  }
}

// 非递归算法
// 不过所有的递归都可以自行定义stack来解递归，所以二分查找法也可以不用递归实现，而且它的非递归实现甚至可以不用栈，因为二分的递归其实是尾递归，它不关心递归前的所有信息。
function binarySearch(arr, key) {
  var low = 0, high = arr.length - 1
  while(low <= high) {
    var mid = Math.floor((low + high) / 2)
    if (key === arr[mid]) {
      return mid
    } else if (key > arr[mid]) {
      low = mid + 1
    } else if (key < arr[mid]) {
      high = mid - 1
    } else {
      return -1
    }
  }
}
```

5. 重复某字符串多次
```js
// 利用 数组的 join 方法
function repeatStr(str, n) {
  return new Array(n+1).join(str)
}

repeatStr('Hi', 3) // HiHiHi
```

