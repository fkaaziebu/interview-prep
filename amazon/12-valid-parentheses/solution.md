# Solution
```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 != 0 or s[0] in [')', '}', ']']:
            return False

        stack = []
        close_to_open = {
            ')': '(',
            '}': '{',
            ']': '['
        }

        for c in s:
            if c in ['(', '{', '[']:
                stack.append(c)
            else:
                if not stack:
                    return False

                opening = stack.pop()
                if opening != close_to_open[c]:
                    return False

        return len(stack) == 0
```
