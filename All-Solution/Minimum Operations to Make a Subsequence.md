> # Minimum Operations to Make a Subsequence

Tags: `Hard` `Array` `Binary Search` `Greedy`

Links: https://leetcode.com/problems/minimum-operations-to-make-a-subsequence/

-----

You are given an array `target` that consists of **distinct** integers and another integer array `arr` that **can** have duplicates.

In one operation, you can insert any integer at any position in `arr`. For example, if `arr = [1,4,1,2]`, you can add `3` in the middle and make it `[1,4,3,1,2]`. Note that you can insert the integer at the very beginning or end of the array.

Return *the **minimum** number of operations needed to make* `target` *a **subsequence** of* `arr`*.*

A **subsequence** of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example, `[2,7,4]` is a subsequence of `[4,2,3,7,2,1,4]` (the underlined elements), while `[2,4,2]` is not. 

**Example 1:**

```
Input: target = [5,1,3], arr = [9,4,2,3,4]
Output: 2
Explanation: You can add 5 and 1 in such a way that makes arr = [5,9,4,1,2,3,4], then target will be a subsequence of arr.
```

**Example 2:**

```
Input: target = [6,4,8,1,3,2], arr = [4,7,6,2,3,8,6,1]
Output: 3
```

**Constraints:**

- `1 <= target.length, arr.length <= 105`
- `1 <= target[i], arr[i] <= 109`
- `target` contains no duplicates.

------

```c++
class Solution {
public:
    int minOperations(vector<int>& target, vector<int>& arr) {
    	std::ios_base::sync_with_stdio(false);
    	cin.tie(NULL);
    	cout.tie(NULL);

    	unordered_map<int, int> posMap;
    	for (int i = 0; i < target.size(); ++i) {
    		posMap[target[i]] = i;
    	}

		int n = arr.size();
		for (int i = 0; i < n; ++i) {
			if (posMap.find(arr[i]) != posMap.end()) {
				arr[i] = posMap[arr[i]];
			}
			else arr[i] = -1;
		}

    	vector<int> d(n + 1);
        d[0] = -1;
        int tmp = -1;
        for (int i = 0; i < n; ++i) {
        	if (arr[i] > -1) {
        		tmp = i;
        		d[1] = arr[i];
        		break;
        	}
        }

    	int len = tmp < 0 ? 0 : 1;
        if (len > 0) {
            for (int i = tmp + 1; i < n; ++i) {
                int left = 0, right = len + 1;
                while (left < right) {
                    int mid = left + ((right - left) >> 1);
                    if (d[mid] < arr[i]) left = mid + 1;
                    else right = mid;
                }

                if (left == len + 1) d[++len] = arr[i];
                else d[left] = arr[i];
            }
        }

    	return target.size() - len;
    }
};
```

本题转化为寻找两个序列的最长公共子序列，但是如果采用动态规划的方法，则需要$O(n^2)$的时间复杂度，恰好`target`数组中的数字是不重复的，于是可以对`arr`中的数字进行标记，如果`arr`中的数字在`target`中存在，就用其对应的下标替换，否则标记为`-1`，这样问题转化为寻找替换完后`arr`中的最长上升子序列，时间复杂度为$O(n\log{n})$。





















