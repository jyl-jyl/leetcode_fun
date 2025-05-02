## Decomposing a Complex Transformation into Simpler and Invertible Operations
卡码网55 右旋字符串
> 字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。 
>
> 例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。

Brute force
1. Create a new string to store the last n chars
2. shift all chars in the original string (starting from len-n) to right by n
3. assign the new string to the first n char

Key Insights
- The transformation/rotation we desire could be achieved by a series of reverse operations
```
int main()
{
    int n;
    string s;
    cin >> n;
    cin >> s;

    int len = s.size();

    // Method 1
    reverse(s.begin(), s.end());
    reverse(s.begin(), s.begin()+n);
    reverse(s.begin()+n, s.end());

    // Method 2
    reverse(s.begin(), s.begin()+len-n);
    reverse(s.begin()+len-n, s.end());
    reverse(s.begin(), s.end());

    cout << s << endl;
}
```
> Reversible operations can be composed to simulate non-trivial transformations.

**Similar Algorithms / Ideas**  
This is reminiscent of other elegant transformations:
- Matrix transposition and flip to rotate a matrix. To rotate an N x N matrix 90° clockwise:
  - original
  ```
  1 2 3
  4 5 6
  7 8 9
  ```
  - Transpose
  ```
  1 4 7
  2 5 8
  3 6 9
  ```
  - Reverse rows
  ```
  7 4 1
  8 5 2
  9 6 3
  ```

