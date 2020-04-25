# 11. Container With Most Water

[Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## 分析

本题公式：`min(ai, bj) * (j - i) (j > i)`，对于所有的 ai，bj 求出此公式的最大值。

- 暴力法：双循环

```java
class Solution {
  public int maxArea(int[] height) {
    if (height == null || height.length == 0) {
      return 0;
    }
    int res = 0;
    for (int i = 0; i < height.length; i++) {
      for (int j = i; j >= 0; j--) {
        res = Math.max(res, Math.min(height[i], height[j]) * (i - j));
      }
    }
    return res;
  }
}
```

- 双指针法（一前一后）

```java
class Solution {
  public int maxArea(int[] height) {
    if (height == null || height.length == 0) {
      return 0;
    }
    int res = 0;
    int i = 0, j = height.length - 1;
    while (i < j) {
      res = Math.max(res, Math.min(height[j], height[i]) * (j - i));
      // 由于移动的过程中两者的间距是一直变小的，需要移动的是值较小的一端，决定了 min(ai, bj) 值的变化。
      if (height[j] > height[i]) {
        i++;
      } else {
        j--;
      }
    }
    return res;
  }
}
```

时间复杂度：O(N)
空间复杂度：O(1)

## 总结

对于数组或者字符串等涉及到窗口的类型的题目，双指针是一种常用的方法，往往能够达到线性时间的复杂度。
 
双指针可以是一头一尾开始遍历，也可以是同一起点，不断的扩大两个指针的窗口间隔的方式遍历。

常见题目：

- 判断链表是否有环
- [无重复字符的最长子串](https://leetcode.com/problems/longest-substring-without-repeating-characters/)