## Linked List and Recursion
Leetcode 203
> Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

Yes this is probably the easiest Linked List problem on Leetcode

But the **recursive** approach got me interested, so I started to probe my AI to learn more about it. 

- A linked list is `One node + a smaller linked list` - a recursive structure
- A linked list is a chain — each node is only concerned with itself and its next

Solution
```
ListNode* removeElements(ListNode* head, int val) {
    if (head == nullptr) {
        return nullptr;
    }

    // 递归处理
    if (head->val == val) {
        ListNode* newHead = removeElements(head->next, val);
        delete head;
        return newHead;
    } else {
        head->next = removeElements(head->next, val);
        return head;
    }
}
```

This is a process of constructing the new list from backwards (exmaple: `1 → 3 → 3 → 2 → 3 → 4`)
```
removeElements(1)
    removeElements(3)
        removeElements(3)
            removeElements(2)
                removeElements(3)
                    removeElements(4)
                        removeElements(nullptr)  --> returns nullptr
                    (head->val == 3) --> delete node, return 4
                (head->val == 2) --> head->next = 4; return head
            (head->val == 3) --> delete node, return 2
        (head->val == 3) --> delete node, return 2
    (head->val == 1) --> head->next = 2; return head

```
- when the node needs to be removed, we delete the node and reutrn the previous node - we skip! 
- when the node needs to be kept, we assign the next ptr to what's returned and return the head. 

The recursive approach could be a one-liner! (Not super helpful HEHE)
```
ListNode* removeElements(ListNode* head, int val) {
        return head == nullptr ? nullptr : (head->val == val ? removeElements(head->next, val) : (head->next = removeElements(head->next, val), head));
    }
```
