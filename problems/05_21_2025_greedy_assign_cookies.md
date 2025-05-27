## Greedy and the Two-pointer apporach
Leetcode 455. Assign Cookies
> Assume you are an awesome parent and want to give your children some cookies.
> But, you should give each child at most one cookie.
>
> Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will
> be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i,
> and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

胃口：g，g_ptr
饼干：s，s_ptr
1. 大胃口先挑大饼干：g快s慢
```cpp
int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(), g.end());
    sort(s.begin(), s.end());
    int index = s.size() - 1; // 饼干数组的下标
    int result = 0;
    for (int i = g.size() - 1; i >= 0; i--) { // 遍历胃口
        if (index >= 0 && s[index] >= g[i]) { // 遍历饼干
            result++;
            index--;
        }
    }
    return result;
}
```
3. 小饼干先喂小胃口：s快g慢
```cpp
public:
int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(),g.end());
    sort(s.begin(),s.end());
    int index = 0;
    for(int i = 0; i < s.size(); i++) { // 饼干
        if(index < g.size() && g[index] <= s[i]){ // 胃口
            index++;
        }
    }
    return index;
}
```




 
