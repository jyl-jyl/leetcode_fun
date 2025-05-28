## An AHA moment for leetcode 738. Monotone Increasing Digits
> An integer has monotone increasing digits if and only if each pair of adjacent digits x and y satisfy x <= y.
>
> Given an integer n, return the largest number that is less than or equal to n with monotone increasing digits.

1. Interate reversely
2. As long as we subtract prev by 1, we need to turn all digits after that to 9

```cpp
int monotoneIncreasingDigits(int n) {
    string num = to_string(n);
    int mark = num.size();

    for (int i = num.size() - 1; i > 0; --i) {
        if (num[i] < num[i - 1]) {
            num[i - 1]--;
            mark = i; 
        }
    }

    for (int i = mark; i < num.size(); ++i) {
        num[i] = '9';
    }
    return stoi(num);
}
```


 
