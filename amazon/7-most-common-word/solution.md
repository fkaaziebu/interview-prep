# Solution
```python
class Solution:
    def preprocess(self, paragraph):
        words = []

        word = ''
        for char in paragraph:
            if char.isalpha() and char != ' ':
                word += char
            else:
                if len(word) != 0:
                    words.append(word)
                word = ''
        if len(word) != 0:
            words.append(word)

        return words

    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        words = self.preprocess(paragraph)

        banned_hash = { banned_word for banned_word in banned }
        paragraph_map = {}

        for word in words:
            word = word.lower()
            if word in banned_hash:
                continue
            paragraph_map[word] = 1 + paragraph_map.get(word, 0)

        max_count, word_with_max = 0, ''
        for word, count in paragraph_map.items():
            if max_count > count:
                continue
            max_count = count
            word_with_max = word

        return word_with_max
```
