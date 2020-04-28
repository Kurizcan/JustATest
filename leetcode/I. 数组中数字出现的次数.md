# 面试题56 - I. 数组中数字出现的次数

[I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
```
示例 1：

输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
示例 2：

输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]
```

## 分析

在一个整型数组如果只有一个数字没有出现1次，我们只需要对这一组数字进行异或，最终结果就是出现次数为1次的数字。

扩展到如果有两个出现次数唯一的情况，那么异或的结果就是这两个数字异或的结果。

如果可以对数组进行分组，共分成两组：

- 两个只出现一次的数字在不同的组中；
- 相同的数字会被分到相同的组中。

```java
class Solution {
  public int[] singleNumbers(int[] nums) {
    if (nums == null || nums.length == 0) {
      return new int[0];
    }
    int res = 0;
    for (int i = 0; i < nums.length; i++) {
      res ^= nums[i];
    }
    // 对应 a ^ b，结果中位数为 1 的位置代表 a,b 在该位置值不相同
    int div = 1;
    while ((div & res) == 0) {
      div = div << 1;
    }
    int a = 0, b = 0;
    for (int i = 0; i < nums.length; i++) {
      if ((nums[i] & div) == 0) {
        a ^= nums[i];
      } else {
        b ^= nums[i];
      }
    }
    return new int[]{a, b};
  }
}
```

- 时间复杂度：O(N)
- 空间复杂度：O(1)