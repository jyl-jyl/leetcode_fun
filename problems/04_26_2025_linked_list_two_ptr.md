## Two Pointer 
Leetcode 19 
> Given the head of a linked list, remove the nth node from the end of the list and return its head.

- fast pointer: move n+1 step first
- slow pointer: move after fast, so that when fast is nullptr, the slow pointer will point to the node before the node that needs to be removed

Leetcode 27
> Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

- fast pointer: iterate through each element in the array
- slow pointer: only move when element value is not integer val, so that it will always point to the next available position for placing the non-val element.

Some other interesting cases to use the two pointer technique
- **Cycle Detection in Linked Lists**:  
  The two-pointer technique is famously used for detecting cycles in linked lists (Floyd’s Tortoise and Hare algorithm). The idea is to have two pointers moving at different speeds (fast pointer moves two steps at a time, slow moves one step) — if they meet, it indicates a cycle.

- **Merging Sorted Arrays**:  
  If you’re merging two sorted arrays (or linked lists), the two-pointer technique is often used to compare and merge elements efficiently without the need for extra space.

- **Palindrome Check**:  
  For both arrays and linked lists, you can use two pointers starting from the ends and moving toward the center to check if the structure is a palindrome.

- **Partitioning (Dutch National Flag Problem)**:  
  A common application is partitioning an array into three sections (e.g., separating smaller, equal, and larger elements around a pivot). You use two pointers to manage this partitioning efficiently.

Efficiency
- **Time Complexity**:  
  Most problems using two pointers have a time complexity of **O(n)** because each pointer typically moves once through the structure, either scanning or modifying elements.
  
- **Space Complexity**:  
  The space complexity is **O(1)** for in-place operations, making the technique highly efficient for problems that require no extra space (other than the pointers).
