# Making a large island
## https://leetcode.com/problems/making-a-large-island

You are given an n x n binary matrix grid. You are allowed to change at most one 0 to be 1.

Return the size of the largest island in grid after applying this operation.

An island is a 4-directionally connected group of 1s.

 
```
Example 1:

Input: grid = [[1,0],[0,1]]
Output: 3
Explanation: Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

Example 2:

Input: grid = [[1,1],[1,0]]
Output: 4
Explanation: Change the 0 to 1 and make the island bigger, only one island with area = 4.

Example 3:

Input: grid = [[1,1],[1,1]]
Output: 4
Explanation: Can't change any 0 to 1, only one island with area = 4.
``` 

# Constraints:
```
1. n == grid.length
2. n == grid[i].length
3. 1 <= n <= 500
4. grid[i][j] is either 0 or 1.
```

## Implementation : DFS (Island ID)
```java
class Solution {
    public int largestIsland(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;
        
        int maxIslandSize = 0;
        int islandId = 2;
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 1) {
                    int size = dfs(grid, i, j, islandId);
                    map.put(islandId++, size);
                    maxIslandSize = Math.max(maxIslandSize, size);
                }
            }
        }
        int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 0) {
                    Set<Integer> set = new HashSet<>();
                    for(int[] direction : directions) {
                        int x = i + direction[0];
                        int y = j + direction[1];
                        if(x >= 0 && x < grid.length && y >= 0 && y < grid[0].length &&
                          grid[x][y] != 0) {
                            set.add(grid[x][y]);
                        }
                    }
                    int sum = 1;
                    for(int id : set) {
                        sum += map.get(id);
                    }
                    maxIslandSize = Math.max(maxIslandSize, sum);
                }
            }
        }
        return maxIslandSize;
        
    }
    
    private int dfs(int[][] grid, int row, int col, int islandId) {
        if(row < 0 || row >= grid.length || col < 0 || col >= grid[0].length || grid[row][col] != 1)
            return 0;
        
        grid[row][col] = islandId;
        
        return 1 + dfs(grid, row, col + 1, islandId)
                 + dfs(grid, row, col - 1, islandId)
                 + dfs(grid, row - 1, col, islandId)
                 + dfs(grid, row + 1, col, islandId);
    }
}
```

# References :
https://www.youtube.com/watch?v=_426VVOB8Vo
