# Solution
---
## Overview
This problem is more of sorting but not just sorting, it is sorting with a twist. You would have to develop your own sort
on top of the built-in sort function depending on the language of choice you are using.

From my findings, there are two ways to specify the sort order.
1. Comparatar
2. Sorting Key
---
## Approach 1: Comparatar
A comparatar is just is a function object that helps the sorting functions to determine the orders among a collection of
elements.

Example of a comparatar in Python:
```python
def compare(a, b):
    if a < b:
        return -1
    elif a == b:
        return 0
    else:
        return 1
```

Now, what we need to do is to define our own proper comparator according to the description of the problem.
We can translate the problem into the following rules:

1. The letter-logs should be prioritized above all digit-logs.

2. Among the letter-logs, we should further sort them firstly based on their contents, and then on their identifiers
if the contents are identical.

3. Among the digit-logs, they should remain in the same order as they are in the collection.

Now let's implement the comparatar solution based on the rules above.

```python
from typing import List
import functools

class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        def compare(log1: str, log2: str) -> int:
            # Split the logs into two parts: identifier and content.
            id1, rest1 = log1.split(" ", 1)
            id2, rest2 = log2.split(" ", 1)

            isDigit1, isDigit2 = rest1[0].isdigit(), rest2[0].isdigit()

            # Case 1: Both logs are letter-logs.
            if not isDigit1 and not isDigit2:
                # First, compare the contents.
                if rest1 != rest2:
                    return 1 if rest1 > rest2 else -1
                # Logs have identical contents, compare the identifiers.
                return 1 if id1 > id2 else -1

            # Case 2: one of the logs is a digit-log.
            if not isDigit1 and isDigit2:
                # The letter-log comes before the digit-logs.
                return -1
            elif isDigit1 and not isDigit2:
                # The digit-log comes after the letter-logs.
                return 1
            else:
                # Case 3: Both logs are digit-logs.
                # Keep them in the same order as they are in the input.
                return 0

        return sorted(logs, key=functools.cmp_to_key(compare))
```

### Complexity Analysis
- Time Complexity: O(M⋅N⋅logN)
  - Complexity of sorting log array is O(N⋅logN)
  - Complexity of comparing two logs is O(M) where M is the maximum length of a single log.

- Space Complexity: O(M⋅logN)
  - For each invocation of the compare function, we would need up to O(M) space to store the split logs.
  - We would need O(logN) space for the recursion stack when sorting the logs (based on quicksort).


## Approach 2: Sorting Key
Rather than defining pairwise relationships among all elements in a collection, the order of the elements
can also be defined with sorting keys.

Example is the use of a tuple
```python
(key_1, key_2, key_3)
```
If two elements have the same value on key_1, the comparison will carry on for the following keys,
i.e. key_2 ... key_n.

### Algorithm
We can define the sorting key for each log as a tuple, using the rules. We would be having three keys based on the rules

1. `key_1`: this key serves as a indicator for the type of logs. For the letter-logs, we could assign its
key_1 with 0, and for the digit-logs, we assign its key_1 with 1. Thanks to the assigned values, the letter-logs
would take proirity over the digit-logs.

2. `key_2`: for this key, we use the content of the letter-logs as its value, so that among the letter-logs, they would
be further ordered based on their content, as required in the Rule (2).

3. `key_3`: similarly with the key_2, this key serves to further order the letter-logs. We will use the identifier of the
letter-logs as its value, so that for the letter-logs with the same content, we could further sort the logs based on its
identifier, as required in the Rule (2).

```python
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:

        def get_key(log):
            _id, rest = log.split(" ", maxsplit=1)
            return (0, rest, _id) if rest[0].isalpha() else (1, None, None)

        return sorted(logs, key=get_key)
```

> Finally, thanks to the stability of sorting algorithms, the elements with the same key value would remain the same
> order as in the original input.

### Complexity Analysis
- Time Complexity: O(M⋅N⋅logN)
  - Complexity of sorting log array is O(N⋅logN)
  - Complexity of generating the key for each log is O(M) where M is the maximum length of a single log.

- Space Complexity: O(M⋅N)
  - We would need O(M) space to store the key for each log.
  - We would need O(N) space to store the sorted logs (Timsort).
