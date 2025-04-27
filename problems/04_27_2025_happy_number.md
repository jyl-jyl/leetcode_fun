## Happy Number Solutions
Leetcode 202

> A happy number is a number defined by the following process:
>
> Starting with any positive integer, replace the number by the sum of the squares of its digits.
> Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
> Those numbers for which this process ends in 1 are happy.
> Return true if n is a happy number, and false if not.
>
> Example 1:
> 
> Input: n = 19
> Output: true  
> Explanation:   
> 12 + 92 = 82     
> 82 + 22 = 68   
> 62 + 82 = 100   
> 12 + 02 + 02 = 1

Key: if sum reaches 1, then happy, but if sum goes into cycle, then not happy

### Solution 1 - hash set
```cpp
int getSum(int n) {
    int sum = 0;
    while (n) {
        sum += (n % 10) * (n % 10);
        n /= 10;
    }
    return sum;
}
bool isHappy(int n) {
    unordered_set<int> set;
    while(1) {
        int sum = getSum(n);
        if (sum == 1) {
            return true;
        }
        // 如果这个sum曾经出现过，说明已经陷入了无限循环了，立刻return false
        if (set.find(sum) != set.end()) {
            return false;
        } else {
            set.insert(sum);
        }
        n = sum;
    }
}
```

### Solution 2 - two ptrs
Yes! I saw cycle so this could also be solved by the slow-fast pointer method as used in detecting cycles in linked lists
```cpp
#include <ranges>
#include <iostream>

constexpr int getNext(int n) {
    int sum = 0;
    while (n > 0) {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    return sum;
}

bool isHappy(int n) {
    int slow = n;
    int fast = getNext(n);

    while (fast != 1 && slow != fast) {
        slow = getNext(slow);
        fast = getNext(getNext(fast));
    }
    return fast == 1;
}
```
