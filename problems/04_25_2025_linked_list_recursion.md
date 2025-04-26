## Linked List and Recursion
Leetcode 203
> Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

Yes this is probably the easiest Linked List problem on Leetcode

But the **recursive** approach got me interested, so I started to probe my AI to learn more about it. 

- A linked list is `One node + a smaller linked list` - a recursive structure
- A linked list is a chain — each node is only concerned with itself and its next

Solution
```c++
ListNode* removeElements(ListNode* head, int val) {
    if (head == nullptr) {
        return nullptr;
    }

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
```c++
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

## Follow-up - Use Recursion to reverse a linked list
Leetcode 206

I found this amazing because I don't have to picture the complete linked list in my head 

I only need to worry about two nodes: `pre` and `cur`, then let the recursion does its job

That also means, no matter which method you wrote down in the end, linked list problem is always easier if you only think about how relavent nodes should interact with each other (the `recursiveness` nature of its structure)

```c++
ListNode* reverse(ListNode* pre, ListNode* cur)
{
	if(!cur) pre;
	ListNode* tmp = cur->next;
	cur->next = pre;
	return (cur, tmp);
}

ListNode* reverseList(ListNode* head)
{
	return reverse(nullptr, head);
}
```

## Linked List C++ 20 Impl with Iterator Support
Leetcode 707
At the end of the day, I'm a C++ developer, I need to do this ...
```c++
#include <memory>
#include <concept>
#include <iostream>
#include <iterator>

template<typename T>
requires std::is_copy_constructible_v<T>
class MyLinkedList
{
public:
	struct LinkedNode
	{
		T val;
		std::unique_ptr<LinkedNode> next;
		LinkedNode(const T& value)
		: val(value),
		  next(nullptr)
		{}
	};

	MyLinkedList()
	: dummy_(std::make_unique<LinkedNode>(T{})),
	  size_(0)
	{}

	[[nodiscard]] int size() const 
	{
		return size_;
	}

	[[nodiscard]] T get(int index) const
	{
		if(index < 0 || index >= size_)
		{
			throw std::out_of_range("Index out of range");
		}
		LinkedNode* cur = dummy_->next.get();
		while(index--)
		{
			cur = cur->next.get();
		}
		return cur->val;
	}

	void addAtHead(const T& val)
	{
		addAtIndex(0, val);
	}

	void addAtTail(const T& val)
	{
		addAtIndex(size_, val);
	}

	void addAtIndex(int index, const T& val)
	{
		if(index < 0 || index > size_) return;

		LinkedNode* prev = dummy_.get();
		for (int i = 0; i < index; ++i)
		{
			prev = prev->next.get();
		}
		auto new_node = std::make_unique<LinkedNode>(val);
		new_node->next = std::move(prev->next);
		prev->next = std::move(new_node);

		if(index == size_)
		{
			tail_ = prev->next.get();
		}
		++size_;
	}

	void deleteAtIndex(int index)
	{
		if(index < 0 || index >= size_) return;
		LinkedNode* prev = dummy_.get();
		for (int i = 0; i < index; ++i)
		{
			prev = prev->next.get();
		}
		auto to_delete = std::move(prev->next);
		prev->next = std::move(to_delete->next);
		if(index == size_-1)
		{
			tail_ = prev;
		}
		size_--;
	}

	class Iterator 
	{
	public:
		using iterator_category = std::forward_iterator_tag;
		using value_type = T;
		using reference = T&
		using pointer = T*;

		Iterator(LinkedNode* node) 
		: node_(node)
		{}

		T& operator*() const
		{
			return node->val;
		}

		Iterator& operator++()
		{
			node_ = node_->next.get();
			return *this;
		}

		bool operator!=(const Iterator& other) const
		{
			return node_ != other.node_;
		}
	private:
		LinkedNode* node_;
	}

	Iterator begin() const
	{
		return Iterator(dummy_->next.get())
	}

	Iterator end() const 
	{
		return Iterator(nullptr);
	}

private:
	std::unique_ptr<LinkedNode> dummy_;
	LinkedNode* tail_;
	int size_;
 
};
```

