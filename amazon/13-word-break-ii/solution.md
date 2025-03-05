# Solution
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        word_dict = defaultdict(lambda:0)
        memo = {}

        for a in wordDict:
            word_dict[a]=1

        def word(s):
            # Return sentence if sentence already formed with word
            if s in memo:
                return memo[s]

            ans=[]
            # Populate ans if s in word_dict
            if word_dict[s] == 1:
                ans=[s]

            # Go through the other words
            for i in range(1,len(s)):
                if word_dict[s[:i]] == 1:
                    res = word(s[i:])
                    for x in res:
                        ans.append(s[:i] + " " + x)

            memo[s] = ans

            return ans

        return word(s)
```
