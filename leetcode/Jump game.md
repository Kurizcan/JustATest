# 55. 跳跃游戏

[跳跃游戏](https://leetcode.com/problems/jump-game/)

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

## 分析

采用类似的贪心算法，对于每一个可以到达的位置 xx，它是的 x+1, x+2, x+1,x+2,⋯,x+nums 这些连续的位置都可以到达。


自底向上
```go
func canJump(nums []int) bool {
  if len(nums) == 0 {
    return false
  }
  right := 0
  for i, v := range nums {
    if i <= right {
      right = max(right, i + v)
      if right >= len(nums) - 1 {
        return true
      }
    }
  }
  return false
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```

自底向下
```java
class Solution {
  public boolean canJump(int[] nums) {
    if (nums == null || nums.length == 0) {
      return false;
    }
    int cur = 0;
    for (int i = nums.length - 1; i >= 0; i--) {
      if (nums[i] + i >= cur) {
        cur = i;
      }
    }
    return cur == 0;
  }
}
```

时间复杂度：O(N)
空间复杂度：O(1)