---
title: leetcode30天算法计划
date: 2021-07-24 11:26:36
tags:
  - leetcode
categories:
  - algothrim
---

# day1 二分查找

## [704.二分查找](https://leetcode-cn.com/problems/binary-search/)

### 题目描述

给定一个  n  个元素有序的（升序）整型数组  nums 和一个目标值  target  ，写一个函数搜索  nums  中的 target，如果目标值存在返回下标，否则返回 -1。

### 示例

```bash

输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4

```

```bash

输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1

```

### answer

```js
var search = function (nums, target) {
  let star = 0;
  let end = nums.length - 1;
  let mid;
  while (star <= end) {
    mid = Math.floor((star + end) / 2);
    if (nums[mid] > target) {
      end = mid - 1;
    } else if (nums[mid] < target) {
      star = mid + 1;
    } else {
      return mid;
    }
  }
  return -1;
};
```

### total

## [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

### 题目描述

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用  bool isBadVersion(version)  接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

### 示例

```bash

输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false
调用 isBadVersion(5) -> true
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。

```

```bash

输入：n = 1, bad = 1
输出：1

```

### answer

```js
//todo become short
var solution = function (isBadVersion) {
  return function (n) {
    let star = 1;
    let end = n;
    if (isBadVersion(1)) {
      return 1;
    }
    while (star <= end) {
      let mid = Math.ceil((star + end) / 2);
      if (isBadVersion(mid) && !isBadVersion(mid - 1)) {
        return mid;
      } else {
        if (!isBadVersion(mid)) {
          star = mid;
        } else {
          end = mid;
        }
      }
    }
  };
};
```

### total

## [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

### 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

### 示例

```bash
输入: nums = [1,3,5,6], target = 5
输出: 2
```

```bash
输入: nums = [1,3,5,6], target = 2
输出: 1
```

```bash
输入: nums = [1,3,5,6], target = 7
输出: 4
```

```bash
输入: nums = [1,3,5,6], target = 0
输出: 0
```

```bash
输入: nums = [1], target = 0
输出: 0
```

### answer

```js
var searchInsert = function (nums, target) {
  let star = 0;
  let end = nums.length - 1;
  while (star <= end) {
    let mid = Math.floor((star + end) / 2);
    if (nums[mid] === target) {
      return mid;
    } else {
      if (nums[mid] > target) {
        end = mid - 1;
      } else {
        star = mid + 1;
      }
    }
  }
  return star;
};
```

### total

# day2 双指针

## [997.有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

### 题目描述

给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

### 示例

```bash
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

```bash
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

### answer

```js
var sortedSquares = function (nums) {
  let starIndex = 0;
  let endIndex = nums.length - 1;
  let temparr = [];
  while (starIndex < endIndex) {
    if (Math.abs(nums[starIndex]) < Math.abs(nums[endIndex])) {
      temparr.push(nums[endIndex]);
      endIndex--;
    } else {
      temparr.push(nums[starIndex]);
      starIndex++;
    }
  }
  temparr.push(nums[starIndex]);
  let rsarr = [];
  for (let i = temparr.length - 1; i >= 0; i--) {
    rsarr.push(temparr[i] * temparr[i]);
  }
  return rsarr;
};
```

### total

## 189.旋转数组

### 题目描述

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

进阶

- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题
- 你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？

### 示例

```bash
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

```bash
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释:
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

### answer1

```js
//这道题，其实比较好像，只是每个下标索引对应的值发生了改变
//所以，新数组对应下标索引便是题解,有趣的是解题时间蛮快的,超过了95%
var rotate = function (nums, k) {
  let rsNums = [];
  for (let i = 0, length = nums.length; i < nums.length; i++) {
    rsNums.push(nums[(i + k * length - k) % length]);
  }
  for (let i = 0; i < nums.length; i++) {
    nums[i] = rsNums[i];
  }
};
```

### answer2

```js
//进阶的要求是变量是可控的
//先想可行性，只要一个数组下标被记录了，那这个下标的空间就是可用的
//应该是可行的,有趣的是执行时间比第一个长，时间复杂度应该都是O(n)
//虽然看似有两个循环，可是这个循环只是解决了k被length整除的清除，整体上来看应该是O(n)

var rotate = function (nums, k) {
  let temp;
  let count = 0;
  let length = nums.length;
  let i = 0;
  let j = 0;
  while (count !== length) {
    temp = nums[i];
    while (i !== (k + j) % length) {
      nums[i] = nums[(i + k * length - k) % length];
      i = (i + k * length - k) % length;
      count++;
    }
    nums[i] = temp;
    count++;
    i++;
    j = i;
  }
};
```

### total

如果让我选择方案的话，我会选第一种，必要的空间换取代码的可阅读性是必要的，
且执行时间相差不大。

# day3 双指针

## [283.移动零](https://leetcode-cn.com/problems/move-zeroes/)

### 题目描述

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

### 示例

```bash
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

### 说明

1. 必须在原数组上操作，不能拷贝额外的数组
2. 尽量减少操作次数

### answer1

```js
//双指针思路
var moveZeroes = function (nums) {
  let low = 0;
  for (let fast = 0; fast < nums.length; fast++) {
    if (nums[fast] !== 0) {
      nums[low++] = nums[fast];
    }
  }
  while (low < nums.length) {
    nums[low++] = 0;
  }
};
```

### answer2

```js
//数学思维寻找映射
var moveZeroes = function (nums) {
  let count = 0;
  for (let i = 0; i < nums.length; i++) {
    nums[i] === 0 ? count++ : (nums[i - count] = nums[i]);
  }
  while (count > 0) {
    nums[nums.length - count--] = 0;
  }
};
```

### total

- 今天的题比较简单，启发主要是思考什么时候用双指针
- 一般情况这样考虑快指针只是为了遍历元素，慢指针起筛选作用
- 思考其他解，从整体上看，移动零其实只是数组下标映射发生改变
- 映射的关系，只跟当前元素之前计数的零相关
- 什么时候用三元表达式，当 if else 代码只有一行，大大增加可阅读性

## [167.两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

### 题目描述

给定一个已按照 升序排列   的整数数组  numbers ，请你从数组中找出两个数满足相加之和等于目标数  target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

### 示例

```bash
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

```bash
输入：numbers = [2,3,4], target = 6
输出：[1,3]
```

```bash
输入：numbers = [-1,0], target = -1
输出：[1,2]
```

### answer

```js
var twoSum = function (numbers, target) {
  let star = 0;
  let end = numbers.length - 1;
  while (star < end) {
    if (numbers[star] + numbers[end] > target) {
      end--;
    } else if (numbers[star] + numbers[end] < target) {
      star++;
    } else {
      break;
    }
  }
  return [star + 1, end + 1];
};
```

### total

- 这道题还是比较好想
- 从结果出发，看怎么逼近的，就很容易想到双指针了
- 还有一种办法是数学的划归思想
- 举例
- 输入：numbers = [2,7,11,15], target = 9
- 因为 target 为 numbers 内的两数之和
- 那 numbers 减去一个 a，target - 2a 求的解是不变的
- 所以咱们减去一半 numbers = [-2.5,2.5,6.5,11.5] 中的相反数下标

#day4 双指针

## [334.反转字符串](https://leetcode-cn.com/problems/reverse-string/)

### 题目描述

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

### 示例

```bash
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

```bash
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

### answer

```js
var reverseString = function (s) {
  let star = 0;
  let end = s.length - 1;
  while (star < end) {
    temp = s[star];
    s[star] = s[end];
    s[end] = temp;
    star++;
    end--;
  }
};
```

### total

## 557.反转字符串中的单词 III

### 题目描述

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

### 示例

```bash
输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```

### answer

```js
var reverseWords = function (s) {
  const arr = s.split(" ");
  const res = [];
  for (let i = 0; i < arr.length; i++) {
    res.push(arr[i].split("").reverse().join(""));
  }
  return res.join(" ");
};
```

### total

# day5 双指针

## [876.链表的中间节点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

### 题目描述

给定一个头结点为 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

### 示例

```bash
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```

```bash
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```

### answer

```js
var middleNode = function (head) {
  let rs = head;
  let count = 0;
  while (head.next !== null) {
    head = head.next;
    if (count % 2 === 0) {
      rs = rs.next;
    }
    count++;
  }
  return rs;
};
```

### total

## [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### 题目描述

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶

- 你能尝试使用一趟扫描实现吗

### 示例

```bash
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

```bash
输入：head = [1], n = 1
输出：[]
```

```bash
输入：head = [1,2], n = 1
输出：[1]
```

### answer

```js
var removeNthFromEnd = function (head, n) {
  let count = 0;
  let temp = head;
  let rs = head;
  while (head) {
    count++;
    head = head.next;
  }
  count = count - n - 1;
  while (count-- > 0) {
    temp = temp.next;
  }
  if (temp === rs) {
    return temp.next;
  } else {
    temp.next = temp.next.next;
  }
  return rs;
};
```

### total

# day6 滑动窗口

## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

### 题目描述

给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

### 示例

```bash
输入: s = "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

```bash
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

```bash
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

```bash
输入: s = ""
输出: 0
```

### answer

```js
var lengthOfLongestSubstring = function (s) {
  let rs = 0;
  let left = 0;
  for (let i = 0; i < s.length; i++) {
    let temp = s.charAt(i);
    while (s.substring(left, i).indexOf(temp) !== -1) {
      left++;
    }
    if (i - left + 1 > rs) {
      rs = i - left + 1;
    }
  }
  return rs;
};
```

### total

## [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

### 题目描述

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的 子串 。

### 示例

```bash
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```

```bash
输入: s1= "ab" s2 = "eidboaoo"
输出: False
```

### answer

```js
var checkInclusion = function (s1, s2) {
  let rs = "";
  for (let i = 0; i < s2.length; i++) {
    let currChar = s2.charAt(i);
    // console.log(currChar,rs);
    if (s1.indexOf(currChar) !== -1) {
      s1 = s1.replace(currChar, "");
      rs += currChar;
    } else {
      while (rs.length >= 1) {
        s1 += rs.charAt(0);
        rs = rs.substring(1, rs.length);
        if (s1.indexOf(currChar) !== -1) {
          s1 = s1.replace(currChar, "");
          rs += currChar;
          break;
        }
      }
    }
    if (s1 === "") {
      return true;
    }
  }
  return false;
};
```

### total
