# Solution
```python
class Node:
    def __init__(self, key=-1, prev=None, next=None):
        self.key = key
        self.prev = prev
        self.next = next

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.dummy = Node()
        self.recent_node = self.dummy
        self.map = {}

    def get(self, key: int) -> int:
        if key not in self.map:
            return -1
        res, node = self.map[key]
        self.remove_node(node)
        self.add_node(node)

        return res

    def put(self, key: int, value: int) -> None:
        if key in self.map:
            res, node = self.map[key]
            self.remove_node(node)
            self.add_node(node)
            self.map[key] = [value, node]
            return

        new_node = Node(key=key)
        if self.capacity == 0:
            node = self.dummy.next
            self.remove_node(node)
            del self.map[node.key]
            self.capacity += 1

        self.add_node(new_node)
        self.map[key] = [value, new_node]
        self.capacity -= 1


    def add_node(self, node):
        self.recent_node.next = node
        node.prev = self.recent_node
        self.recent_node = node

    def remove_node(self, node):
        prev, next = node.prev, node.next
        prev.next = next
        if next:
            next.prev = prev
        else:
            self.recent_node = node.prev
        node.prev = node.next = None



# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
