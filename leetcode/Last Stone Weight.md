# 1046. Last Stone Weight

[Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)

给定一组整数集合，每次取出两个最大的数做差后销毁，如果差不为 0，则将该差值的绝对值加入集合中，直到集合中只剩下一个元素或者没有元素。返回最后剩下的元素。

## 分析

考虑使用堆的数据结构进行模拟，每次从堆顶取出两个元素进行做差，模拟题目操作过程。

```java
class Solution {
  public int lastStoneWeight(int[] stones) {
    if (stones == null || stones.length == 0) {
      return 0;
    }
    PriorityQueue<Integer> heap = new PriorityQueue<>((o1, o2) -> (o2 - o1));
    for (int num : stones) {
      heap.add(num);
    }
    while (heap.size() > 1) {
      int one = heap.poll();
      int two = heap.poll();
      if (one == two) {
        continue;
      }
      heap.add(one - two);
    }
    return heap.size() == 0? 0 : heap.poll();
  }
}
```


时间复杂度：O(NlogN)
空间复杂度：O(N)

> N 集合元素个数

## 其他做法

- [桶排序 + 双指针](https://leetcode.com/problems/last-stone-weight/discuss/360047/Super-Simple-O(N)-Java-Solution-using-bucket-sort-and-two-pointers)