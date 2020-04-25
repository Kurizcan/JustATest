# 56. Merge Intervals

[Merge Intervals - 合并区间](https://leetcode.com/problems/merge-intervals/)

```
给出一个区间的集合，请合并所有重叠的区间。

示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

## 分析

Java

```java
class Solution {
  public int[][] merge(int[][] intervals) {
    // 先按照首数字的大小排序
    Arrays.sort(intervals, (o1, o2) -> (o1[0] - o2[0]));
    ArrayList<int[]> res = new ArrayList<>();
    if (intervals.length <= 1) {
      // 特殊情况处理
      return intervals;
    }
    for (int i = 1; i < intervals.length; i++) {
      if (intervals[i][0] <= intervals[i - 1][1]) {
        // 合并区间
        int min = Math.min(intervals[i][0], intervals[i - 1][0]);
        int max = Math.max(intervals[i][1], intervals[i - 1][1]);
        intervals[i][0] = min;
        intervals[ i][1] = max;
      } else {
        // 无法合并，则将前一个区间的并入集合
        res.add(intervals[i - 1]);
      }
    }
    res.add(intervals[intervals.length - 1]);
    int length = res.size();
    int[][] output = new int[length][2];
    for (int i = 0; i < length; i++) {
      output[i] = res.get(i);
    }
    return output;
  }
}
```

Go

```go
func merge(intervals [][]int) [][]int {
  if len(intervals) <= 1 {
    return intervals
  }
  sort.Slice(intervals, func(i, j int) bool {
    return intervals[i][0] < intervals[j][0]
  })
  res := make([][]int, 0)
  for i := 1; i < len(intervals); i++ {
    if intervals[i][0] <= intervals[i - 1][1] {
      l := min(intervals[i][0], intervals[i - 1][0])
      r := max(intervals[i][1], intervals[i - 1][1])
      intervals[i][0], intervals[i][1] = l, r
    } else {
      res = append(res, intervals[i - 1])
    }
  }
  res = append(res, intervals[len(intervals) - 1])
  return res
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}

func min(a, b int) int {
  if a > b {
    return b
  }
  return a
}
```

时间复杂度：O(NlogN)，涉及到排序
空间复杂度：O(N)