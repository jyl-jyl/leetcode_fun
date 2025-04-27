## Cycles in Linked List
Leetcode 142
> Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.
> There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

I need to complain! This is the type of problem that's beyond intuition. How can you expect someone to work out all the formulus within the limited time of an interview??

So I need to figure out the intuition behind it. 

Here is the solution
```cpp
ListNode *detectCycle(ListNode *head) {
    auto fast = head;
    auto slow = head;

    while(fast && fast->next)
    {
        slow = slow->next;
        fast = fast->next->next;

        if(slow == fast)
        {
            auto index1 = fast;
            auto index2 = head;
            while(index1 != index2)
            {
                index1 = index1->next;
                index2 = index2->next;
            }
            return index2;
        }
    }
    return nullptr;
}
```

I had a hard time understanding why the following lines would work
```cpp
while(index1 != index2)
  {
      index1 = index1->next;
      index2 = index2->next;
  }
  return index2;
```

and here's my two cents:

The cyclic linked list can be partitioned into three segments:
- Segment_1: `node 0       -------- cycle entry`
- Segment_2: `cycle entry  -------- meeting node`
- Segment_3: `meeting node -------- cycle entry`

Distance travaled by the slow ptr by the time they meet:

`distance_slow` = `Segment_1` + `Segment_2`

Distance traveled by the fast ptr by the time they meet:

`distance_fast` = `Segment_1` + `Segment_2` + n times of `cycle`

We know that the `distance_fast` is two times of `distance_slow`

So `Segment_1` + `Segment_2` must be able to be counted by integer numbers of cycle, meaning 

`Segment_1` + `Segment_2` = n times of `cycle`

We also know that 

`Segment_3` + `Segment_2` = `cycle`

So

`Segment_1` - `Segment_3` = (n-1) times of `cycle`

So

`Segment_1` is either the same of `Segment_3`, or is `Segment_3` + some numbers of `cycle`


So 

By the time the ptr traveled from the head of the linked list to the end of `Segment_1` - the entry of the cycle, the other ptr must have traveled to the same point, either from the meeting point directly, or after a couple of iterations of the cycle itself.

And sorry, I wasn't able to find a good way to visualize this. I guess I'll defer that to someone in design (AI also didn't do a good job on this, so bummer!!)



