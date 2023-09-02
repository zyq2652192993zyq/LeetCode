> # LCP 19.秋叶收藏集

Tags: `Medium` `Dynamic Programming`

Links: https://leetcode-cn.com/problems/UlBDOe/

-----

小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集 `leaves`， 字符串 `leaves` 仅包含小写字符 `r` 和 `y`， 其中字符 `r` 表示一片红叶，字符 `y` 表示一片黄叶。
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于 1。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。

**示例 1：**

```
输入：leaves = "rrryyyrryyyrr"

输出：2

解释：调整两次，将中间的两片红叶替换成黄叶，得到 "rrryyyyyyyyrr"
```

**示例 2：**

```
输入：leaves = "ryr"

输出：0

解释：已符合要求，不需要额外操作
```

**提示：**

```
3 <= leaves.length <= 10^5
leaves 中只包含字符 'r' 和字符 'y'
```

------

解法一：参考了官方题解：https://leetcode-cn.com/problems/UlBDOe/solution/qiu-xie-shou-cang-ji-by-leetcode-solution/



```c++
class Solution {
public:
    int minimumOperations(string leaves) {
    	std::ios_base::sync_with_stdio(false);
    	cin.tie(NULL);
    	cout.tie(NULL);

    	int n = leaves.size();
    	vector<vector<int>> d(n, vector<int>(3));
    	d[0][0] = (leaves[0] == 'y');
    	d[0][1] = d[0][2] = d[1][2] = n + 1;

    	for (int i = 1; i < n; ++i) {
    		int isRed = (leaves[i] == 'r');
    		int isYellow = 1 - isRed;
    		d[i][0] = d[i - 1][0] + isYellow;
    		d[i][1] = min(d[i - 1][0], d[i - 1][1]) + isRed;
    		if (i >= 2) d[i][2] = min(d[i - 1][1], d[i - 1][2]) + isYellow;
    	}

    	return d[n - 1][2];
    }
};
```

