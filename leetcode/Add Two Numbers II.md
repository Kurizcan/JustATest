# 445. Add Two Numbers II

给你两个非空链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

Example:
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

## 分析

可以使用数组存储链表元素，然后从后往前进行加法模拟，使用头插法建立新的链表。

或者也可以使用栈存储。

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
  nums1 := make([]int, 0)
  nums2 := make([]int, 0)
  
  for l1 != nil {
    nums1 = append(nums1, l1.Val)
    l1 = l1.Next
  }
  
  for l2 != nil {
    nums2 = append(nums2, l2.Val)
    l2 = l2.Next
  }
  
  i, j := len(nums1) - 1, len(nums2) - 1
  cnt := 0
  var cur *ListNode
  
  for i >= 0 || j >= 0 {
    sum := 0
    if i < 0 {
      sum = nums2[j] + cnt 
      j--
    } else if j < 0 {
      sum = nums1[i] + cnt
      i--
    } else {
      sum = nums1[i] + nums2[j] + cnt
      i--
      j--
    }
    val  := sum % 10
    node := &ListNode{val, nil}
    cnt = sum / 10
    if cur == nil {
      cur = node
    } else {
      node.Next = cur
      cur = node
    }
  }
  if cnt > 0 {
    node := &ListNode{cnt, cur}
    cur = node
  }
  return cur;
}
```

## 注意

go 的结构体注意：
```go
var cur *ListNode    // cur = nil
cur := &ListNode{}   // *cur = ListNode{} 不等于 nil，结构体中的字段取默认值 
```