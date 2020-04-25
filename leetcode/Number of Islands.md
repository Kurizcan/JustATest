# 200. Number of Islands

[Number of Islands](https://leetcode.com/problems/number-of-islands/)

## 分析

- dfs 涂色法

```go 
func numIslands(grid [][]byte) int {
  if len(grid) == 0 {
    return 0
  }
  res := 0
  for i := 0; i < len(grid); i++ {
    for j := 0; j < len(grid[0]); j++ {
      // 遇到 1，DFS其相邻的 1，称之为陆地
      if grid[i][j] == '1' {
        res++
        dfs(grid, i, j)
      }
    }
  }
  return res
}

func dfs(grid [][]byte, i, j int) {
  if i < 0 || i >= len(grid) || j < 0 || j >= len(grid[0]) || grid[i][j] == '0' {
    return
  }
  grid[i][j] = '0'
  dfs(grid, i + 1, j)
  dfs(grid, i - 1, j)
  dfs(grid, i, j + 1)
  dfs(grid, i, j - 1)
}
```

- 并查集方式

初始化并查集，将`'1'`的位置初始化，然后进行结点的合并。每进行一次合并，统计中的`'1'`的个数减少一个。

Java 版本

```java
class Solution {
  class UnionFind {
    public int[] root;
    public int size;
    public int cnt; // 1 的个数
    public UnionFind(char[][] grid) {
      int rows = grid.length;
      int cols = grid[0].length;
      root = new int[rows * cols];
      for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
          if (grid[i][j] == '1') {
            root[i * cols + j] = i * cols + j;
            cnt++;
          }
        }
      }
    }
    
    public int findRoot(int i) {
      int n = i;
      while (root[n] != n) {
        n = root[n];
      }
      while (i != root[i]) {
        int tmp = root[i];
        root[i] = n;
        i = tmp;
      }
      return n;
    }
        
    public void union(int i, int j) {
      int ri = findRoot(i);
      int rj = findRoot(j);
      if (ri == rj) {
        return;
      }
      // 合并了之后，自然 1 的总数减少
      root[ri] = rj;
      cnt--;
    }
  }
  
  public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
      return 0;
    }
    int[][] dir = {{0, -1}, {0, 1}, {1, 0}, {-1, 0}};
    int rows = grid.length;
    int cols = grid[0].length;
    UnionFind uf = new UnionFind(grid);
    for (int i = 0; i < rows; i++) {
      for (int j = 0; j < cols; j++) {
        if (grid[i][j] == '0') {
          continue;
        }
        for (int[] d : dir) {
          int nx = i + d[0];
          int ny = j + d[1];
          if (nx >= 0 && nx < rows && ny >= 0 && ny < cols && grid[nx][ny] == '1') {
            uf.union(i * cols + j, nx * cols + ny);
          }
        }
      }
    }
    return uf.cnt;
  }
```
go 版本

```go
type Union struct {
  root []int
  cnt  int  // 记录 1 的数量     
}

func New(grid [][]byte) *Union {
  u := &Union{}
  row, col := len(grid),  len(grid[0])
  u.root = make([]int, row * col)
  for i := 0; i < row; i++ {
    for j := 0; j < col; j++ {
      if grid[i][j] == '1' {
        u.root[col * i + j] = col * i + j
        u.cnt++
      }
    }
  }
  return u
}

func (u *Union) FindRoot(i int) int {
  cur := i
  for u.root[cur] != cur {
    cur = u.root[cur]
  }
  // 路径压缩
  for u.root[i] != i {
    u.root[i], i = cur, u.root[i]
  }
  return cur
}
 

func (u *Union) Connect(i, j int) {
  rooti, rootj := u.FindRoot(i), u.FindRoot(j)
  if rooti == rootj {
    return
  }
  u.root[rooti] = rootj
  u.cnt--
}

func (u *Union) GetCnt() int {
  return u.cnt
}


func numIslands(grid [][]byte) int {
  if len(grid) == 0 {
    return 0
  }
  union := New(grid)
  row, col := len(grid), len(grid[0])
  dir := [4][2]int{{0, -1}, {0, 1}, {-1, 0}, {1, 0}}
  for i := 0; i < row; i++ {
    for j := 0; j < col; j++ {
      if grid[i][j] == '1' {
        for _, d := range dir {
          nx, ny := i + d[0], j + d[1]
          if nx < 0 || nx >= row || ny < 0 || ny >= col || grid[nx][ny] == '0' {
            continue
          }
          union.Connect(i * col + j, nx * col + ny)
        }
      }
    }
  }
  return union.GetCnt()
}
```