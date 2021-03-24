# CPO
Hash-map (collision resolution: open addressing)
-- title
Computer Process Organization
lab1 - data structure

-– group name and list of group members
spicy girl in this beach
Wu Kuncheng & Xu Zihao

--  laboratory work number
5

--  variant description
Hash-map (collision resolution: open addressing)

-- contribution
* contribution summary for each group member (should be checkable by git log and git blame);
    - Wu Kuncheng Completed the immutable and immutable_test parts of coding. Besides, She also did the README and the test of python. She inserts the figures to the repository.
    - Xu Zihao Completed the mutable and mutable_test parts of coding. He also did the open addressing
  
-- explanation 
hash map(open address)
1. we use the list to storage MapEntry(Node) which have k, v;
2. eg: [{k, v}, {k,v}] ---the position of the node storage by the hashvalue
3. we compute the hash use key % size rehash use the key + 1 % size it's mean we use the open address--linear probing
4. When a conflict occurs, we use _rehash to get a new hash_value. util it find a position. if not mean that the table is ful 
```
  def hash(self, key):
        return key % self.size
  def _rehash(self, old_hash):
        return (old_hash + 1) % self.size
```
-- work demonstration 
first, we chioce the data structure to storage the map value,  Since map entry has key and value, so we should design a node to storage the key and value. 
```
    def __init__(self, key, value):
        self.key = key
        self.value = value

    def __repr__(self):
        return str("{" + str(self.key) + ": " + str(self.value) + "}")
```
since map can get(key) in $ O(1) $ so So he must meet random storage. So we use list to store node.
```
  def __init__(self, size=11, **kwds):
        self.size = size
        self._len = 0
        self.kvEntry = [self._empty] * size
        self._keyset = [] * size
        self.index = 0
```
* work demonstration (how to use developed software, how to test it), should be repeatable by an instructor by given command-line
  examples;

    The pictures inserted show the outcome of every step of test.


-- conclusion
* Mutable objects are nice because you can make changes in-place, without allocating a new object. 
But be careful—whenever you make an in-place change to an object, all references to that object will now reflect the change.    

* Imutable objects are more easy to implement the thread safety. because it will not be change.but  However, whenever you do need a modified object of that new type you must suffer the overhead of a new object creation, as well as potentially causing more frequent garbage collections. 

* in our implementation:

    1.there are something to improve, the map is fix size, we can make the capcity more large when the capcity is not enough. 
    ``` 
    def __init__(self):
        super().__init__(self.MIN_SIZE)

    def put(self, key, value):
        rv = super().put(key, value)
        # increase size of dict * 2 if filled >= 2/3 size (like python dict)
        if len(self) >= (self.size * 2) / 3:
            self.__resize()

    def __resize(self):
        self.size *= 2  # this will be the new size
        self._len = 0
        self.kvEntry = [self._empty] * self.size
        for entry in self.kvEntry:
            if entry is not self._empty and entry is not self._deleted:
                self.put(key, value)
    ```
    but we don't have enough time to test this.
    2.the linear probing so make the data is unevenly distributed in the list, we can use We can use square probing or other methods to try to avoid this problem
    3.the immutable and the mutable compare
    ```
    #mutable
     def test_put(self):
        table = HashTableMutable()
        temp = table.put(1, 3)
        print(table) ### output {1:3} 
        print(temp) ### output None  we can see that , the modify is on the original map
        self.assertEqual(table.get(1), 3)
    #immutable 
    def test_put(self):
        table = HashTableImmutable()
        temp = table.put(5, 111)
        print(table) ###  output: {}
        print(temp) ### output:{5,111}  we can see that it is not change the original map return a new one
        self.assertEqual(temp.get(5), 111)
    ```
