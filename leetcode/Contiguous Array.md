# 525. Contiguous Array

[Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## 分析

```java
class Solution {
  public int findMaxLength(int[] nums) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int cnt = 0;
    int max = 0;
    map.put(cnt, 0);
    for (int i = 1; i <= nums.length; i++) {
      cnt += nums[i - 1] == 0? -1 : 1;
      if (map.containsKey(cnt)) {
        max = Math.max(max, i - map.get(cnt));
      } else {
        map.put(cnt, i);
      }
    }
    return max;
  }
}
```


时间复杂度：O(N)
空间复杂度：O(N)