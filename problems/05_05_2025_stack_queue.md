## C++ containers 
Leetcode 232 & 225 stack <=> queue

Implementing stack using queue or the other way around is straightforward. Just keep in mind that using stack to implement queue requires rwo stacks (one in one out) becuase one end of a stack is blocked. However, 
since a queue is not blocked, we don't need to use two queues to implement stack. We can get an element from the front and insert it back to the back to do the bookkepping. 

I read from [here](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.md#%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80) 
that a stack or a queue is not a container in C++ but a container adaptor. I asked AI to help me learn more on the concepts of `container` and  `container adaptor`

### What is a C++ container 
A C++ container is a class template that stores collections of objects. Containers provide standardized interfaces to access, insert, delete, and iterate over elements.
The Standard Template Library (STL) defines several containers, including:
- Sequence containers (ordered by insertion): `vector`, `deque`, `list`, `forward_list`, `array (C++11 and later)`
- Associative containers (ordered by key comparison): `set`, `map`, `multiset`, `multimap`
- Unordered associative containers (hash-based): `unordered_set`, `unordered_map`, `unordered_multiset`, `unordered_multimap`

### What is C++ container adpator
A container adaptor is a wrapper around a container that provides a restricted interface, modeling a specific data structure (like a stack or queue).

**Instead of exposing full container functionality (like random access), it provides only what's necessary for the abstraction.**

| Adaptor               | Description                         | Default Underlying Container |
|------------------------|-------------------------------------|-------------------------------|
| `std::stack`           | LIFO (Last-In-First-Out) stack      | `std::deque`                  |
| `std::queue`           | FIFO (First-In-First-Out) queue     | `std::deque`                  |
| `std::priority_queue`  | Max-heap priority queue             | `std::vector`                 |

### FILO (back insertion/removel)
Leetcode 1047 (Can use to create 开心消消乐 game)

Again problem is easy to solve, but there are multiple possible data structures that could be used like stack, string, vector, etc.. These data strcutures have one thing in common:: they all provide FILO
So I asked AI to help me go over all C++ STL data structures that would allow such operations

### STL Data Structures with FILO / Back Insertion Support

| Data Structure     | FILO Support | Back Insertion | Back Removal | Time Complexity (Insert/Remove) | Space Efficiency     | Notes                                                                 |
|--------------------|--------------|----------------|---------------|-------------------------------|----------------------|-----------------------------------------------------------------------|
| `std::stack<T>`    | ✅ Yes        | ✅ `.push()`    | ✅ `.pop()`    | O(1)                         | Efficient             | Adapter over `std::deque` (default) or `std::vector`                  |
| `std::vector<T>`   | ✅ Yes        | ✅ `.push_back()` | ✅ `.pop_back()` | O(1) amortized              | Efficient             | Allows random access; contiguous memory                               |
| `std::deque<T>`    | ✅ Yes        | ✅ `.push_back()` | ✅ `.pop_back()` | O(1)                         | Slightly more overhead| Supports front/back insert/remove                                     |
| `std::string`      | ✅ Yes (for `char`) | ✅ `.push_back()` | ✅ `.pop_back()` | O(1) amortized              | Efficient for strings | Specialized `vector<char>`                                            |

`std::string` is more efficient compared to `std::stack` (if implemented by `std::deque`) because it uses contiguous memory (stack is implented using deque which is esentially a linked list, string uses dynamic array). 

But keep in mind that **both** string and stack's data (not the object that stores the metaeta) is stored on the `heap`. Actually all data structure supporting FILO have the actual data stored on the heap. 





