<!-- 
time: 2023/7/14
category: 技术
tag:
    - 算法
    - 滑动窗口
-->

# 滑动窗口思想

滑动窗口是一种基于双指针的技术，常用于处理数组或字符串的子串问题。它通过维护一个可以滑动的窗口来解决一些特定的问题。

## 基本思想

滑动窗口的基本思想是使用两个指针，通常称为左指针和右指针，来构建一个可调整大小的窗口。初始时，左右指针都指向数组或字符串的开始位置。然后，通过移动右指针来扩展窗口，同时根据题目要求调整窗口，直到满足特定的条件。一旦满足条件，我们可以更新结果，然后尝试缩小窗口，即移动左指针，继续寻找下一个满足条件的窗口。

基本步骤如下：

1. 定义左指针和右指针，并初始化为数组或字符串的开始位置。
2. 当满足特定条件时，移动右指针来扩大窗口。
3. 当不满足特定条件时，移动左指针来缩小窗口。
4. 在每次操作时，根据题目要求更新结果。
5. 重复2-4步骤，直到右指针到达数组或字符串的末尾。

## 例子： leetcode 3. 无重复字符的最长子串

```ts
/*
 * @lc app=leetcode id=3 lang=typescript
 *
 * [3] Longest Substring Without Repeating Characters
 */

// @lc code=start
function lengthOfLongestSubstring(s: string): number {
    // let max = 0;
    // const sarr = s.split('');
    // let temp = [] as string[];
    // for (let i = 0; i < sarr.length; i++) {
    //     if(temp.includes(sarr[i])) {
    //         max = Math.max(max, temp.length);
    //         temp = temp.slice(temp.indexOf(sarr[i]) + 1)
    //     }
    //     temp.push(sarr[i]);
    // }
    // return Math.max(max, temp.length);

    // better solution
    let max = 0;
    let window = new Set();
    let left = 0;
    let right = 0;
    while (right < s.length) {
        const char = s[right];
        while (window.has(char)) {
            window.delete(s[left]);
            left++;
        }
        window.add(char);
        max = Math.max(max, window.size);
        right++;
    }
    return max;
};
// @lc code=end
```


## 优势及适用场景

滑动窗口的优势在于它可以在较低的时间复杂度内解决一些问题。通过移动指针来改变窗口的大小，我们可以快速计算子串、子数组或满足特定条件的子序列等。滑动窗口一般适用于以下场景：

* 寻找满足特定条件的子串或子数组。
* 求最长或最短满足特定条件的子串或子数组。
* 寻找最小覆盖子串或最大无重复字符子串。
* 统计满足限制条件的子串或子数组的个数等。

## 时间复杂度与空间复杂度

滑动窗口通常可以将问题的时间复杂度优化为 O(n)，其中 n 是数组或字符串的长度。这是因为滑动窗口只需要遍历一次数组或字符串，并且每个元素最多被访问两次，即被左右指针各访问一次。

空间复杂度主要取决于额外使用的数据结构，通常是 O(k)，其中 k 是滑动窗口的大小。这是因为滑动窗口需要使用一个额外的数据结构来存储窗口中的元素或记录窗口的状态。