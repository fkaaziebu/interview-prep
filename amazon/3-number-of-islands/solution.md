# Solution
```python
import collections
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        rows, cols = len(grid), len(grid[0])
        marked_lands = set()

        def bfs(r, c):
            queue = collections.deque()
            queue.append([r, c])
            marked_lands.add((r, c))
            while queue:
                row, col = queue.popleft()

                for r, c in [[0, 1], [1, 0], [0, -1], [-1, 0]]:
                    row_val = row + r
                    col_val = col + c
                    if (row_val >= 0 and row_val < rows) and (col_val >= 0 and col_val <cols) and grid[row][col] == '1' and (row_val, col_val) not in marked_lands:
                        queue.append([row_val, col_val])
                        marked_lands.add((row_val, col_val))

        res = 0
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1' and (r, c) not in marked_lands:
                    bfs(r, c)
                    res += 1

        return res
```
