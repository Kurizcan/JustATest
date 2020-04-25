# 238. Product of Array Except Self

[Product of Array Except Self - 除自身以外数组的乘积](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3300/)

```
示例

输入: [1,2,3,4]
输出: [24,12,8,6]
```

> 尽量不使用除法，常数空间复杂度

## 分析

- left[i]： i 左边的数字之积，不包括第 i 个数字
- right[i]: i 右边的数字之积，不包含第 i 个数字

```java
class Solution {
  public int[] productExceptSelf(int[] nums) {
    if (nums == null || nums.length == 0) {
      return nums;
    }
    int[] res = new int[nums.length];
    int[] left = new int[nums.length];
    int[] right = new int[nums.length];
    left[0] = 1;
    for (int i = 1; i < nums.length; i++) {
      left[i] = left[i - 1] * nums[i - 1];
    }
    
    right[nums.length - 1] = 1;
    for (int j = nums.length - 2; j >= 0; j--) {
      right[j] = right[j + 1] * nums[j + 1];
    }
    
    for (int i = 0; i < nums.length; i++) {
      res[i] = left[i] * right[i];
    }
    return res;
  }
}
```

进一步优化空间：

```java
class Solution {
  public int[] productExceptSelf(int[] nums) {
    if (nums == null || nums.length == 0) {
      return nums;
    }
    int[] res = new int[nums.length];
    // 先计算 left[i]，即左边包含第 i 位的乘积
    res[0] = 1;
    for (int i = 1; i < nums.length; i++) {
      res[i] = res[i - 1] * nums[i - 1];
    }
    
    // 从后遍历，进行右边部分的乘积计算
    int r = 1;
    for (int j = nums.length - 1; j >= 0; j--) {
      res[j] = r * res[j];
      r *= nums[j];
    }
    return res;
  }
}
```

> 考查的是数组