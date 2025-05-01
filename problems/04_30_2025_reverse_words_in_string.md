## A great problem to reivist regularly (My favourate Leetcode problem so far!)
Leetcode 151 Reverse Words in String

> Given an input string s, reverse the order of the words.
> A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
> Return a string of the words in reverse order concatenated by a single space.
> Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.


> Example 1:  
> Input: s = "the sky is blue"
> Output: "blue is sky the"
>
> Example 2:  
> Input: s = "  hello world  "
> Output: "world hello"  
> Explanation: Your reversed string should not contain leading or trailing spaces.
>
>  Example 3:  
> Input: s = "a good   example"
> Output: "example good a"  
> Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.

Key insights (EVERTHING TAKES O(1) SPACE COMPLEXITY)
1. Use the two ptr technique to remove extra leading and trailing whitespaces in a string ([Leetcode 27](https://github.com/jyl-jyl/leetcode_fun/blob/main/problems/04_26_2025_linked_list_two_ptr.md))
    - fast ptr: pointing to the next char needs to be processed
    - slow ptr: pointing to the next char availble to move another char into
    - need special handling when adding a space in-between words
2. Use in-place reverse to reverse the string
3. Use in-place reverse to reverse each individual word in the string

Solution
```cpp
void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
    for (int i = start, j = end; i < j; i++, j--) {
        swap(s[i], s[j]);
    }
}

void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
    int slow = 0;   //整体思想参考https://programmercarl.com/0027.移除元素.html
    for (int i = 0; i < s.size(); ++i) { //
        if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
            if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
            while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                s[slow++] = s[i++];
            }
        }
    }
    s.resize(slow); //slow的大小即为去除多余空格后的大小。
}

string reverseWords(string s) {
    removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
    reverse(s, 0, s.size() - 1);
    int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
    for (int i = 0; i <= s.size(); ++i) {
        if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
            reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
            start = i + 1; //更新下一个单词的开始下标start
        }
    }
    return s;
}
```
