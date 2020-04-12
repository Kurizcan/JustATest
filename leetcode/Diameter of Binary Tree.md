# 543. Diameter of Binary Tree

[Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

求解二叉树中的最长路径

          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

> Note: The length of path between two nodes is represented by the number of edges between them.

## 分析

类似求解二叉树的高度，可以在遍历的过程中，对于某个结点，先计算左子树的高度，再计算右子树的高度，对于以这个结点为根的子树中，它的最长路径就是左子树的高度加上右子树的高度。

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func diameterOfBinaryTree(root *TreeNode) int {
  res := 0
  dfs(root, &res)
  return res
}

func dfs(root *TreeNode, res *int) int {
  if (root == nil) {
    return 0
  }
  left  := dfs(root.Left, res)
  right := dfs(root.Right, res)
  *res = max(left + right, *res)
  return max(left, right) + 1
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```

时间复杂度：O(N)
空间复杂度：O(N)

> N 为二叉树结点数

使用 go 等有指针的语言，可以不使用全局变量去记录比较遍历过程中的每个结点的最大值，而是将某个变量的引用作为方法参数传递，直接操作引用即可完成记录，类似于 `dfs(root *TreeNode, res *int)`。