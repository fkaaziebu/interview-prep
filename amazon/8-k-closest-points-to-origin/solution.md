# Solution
```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        def partition(l, r, pivot_idx):
            pivot = points[pivot_idx][0]**2 + points[pivot_idx][1]**2
            stored_pivot_idx = l

            points[pivot_idx], points[r] = points[r], points[pivot_idx]

            for i in range(l, r):
                if (points[i][0]**2 + points[i][1]**2) < pivot:
                    points[stored_pivot_idx], points[i] = points[i], points[stored_pivot_idx]
                    stored_pivot_idx += 1

            points[stored_pivot_idx], points[r] = points[r], points[stored_pivot_idx]
            return stored_pivot_idx

        def select(l, r, k):
            if l < r:
                pivot_idx = random.randint(l, r)
                pivot_idx = partition(l, r, pivot_idx)

                if pivot_idx == k:
                    return

                if pivot_idx < k:
                    select(pivot_idx + 1, r, k)
                else:
                    select(l, pivot_idx - 1, k)

        select(0, len(points) - 1, k)
        return points[:k]
```
