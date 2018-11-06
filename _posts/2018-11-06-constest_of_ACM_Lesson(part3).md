---
layout:     post
title:      "contest of ACM Lesson(part3)"
subtitle:   "哈尔滨工程大学ACM专选课期末考试题解"
date:       2018-11-06 22:00:00
author:     "Tivility"
header-img: "img/post_bg/acm.png"
tags:
    - Courseware for ACM Professional Electives
---


  

- 1. 已经出现了三次的博弈论

```cpp
#include <iostream>

using namespace std;

int main() {
    int t, n, k;
    cin >> t;
    while (t--) {
        cin >> n >> k;
        if (n % (k + 1))
            cout << "First" << endl;
        else 
            cout << "Second" << endl;
    }
    
    return 0;
}
```

- 2. BFS搜索, 存下来每个点的到达时间.

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;
int t, n, m;
struct Node{
    int x, y, deep;
}st, ed, now, nxt;
const int MAX = 333;
queue <Node> que;
char mp[MAX][MAX];
bool used[MAX][MAX];
const int dir[4][2] = {1, 0, 0, 1, 0, -1, -1, 0};
int judge() {
    while (que.size()) que.pop();
    memset (used, 0, sizeof (used));
    que.push(st);
    used[st.x][st.y] = true;
    while (que.size()) {
        now = que.front(); que.pop();
        if (now.x == ed.x && now.y == ed.y)
            return now.deep;
        for (int i = 0; i < 4; ++i) {
            nxt.x = now.x + dir[i][0], nxt.y = now.y + dir[i][1];
            nxt.deep = now.deep + 1;
            if (nxt.x < 0 || nxt.y < 0 || nxt.x >= n || nxt.y >= m)
                continue;
            if (used[nxt.x][nxt.y])
                continue;
            if (mp[nxt.x][nxt.y] == '#')
                continue;
            used[nxt.x][nxt.y] = true, que.push(nxt);
        }
    }
    return -1;
}
int main() {
    cin >> t;
    while (t--) {
        cin >> n >> m;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                cin >> mp[i][j];
                if (mp[i][j] == 'S')
                    st.x = i, st.y = j, st.deep = 0;
                else if (mp[i][j] == 'T')
                    ed.x = i, ed.y = j;
            }
        }
        cout << judge() << endl;
    }
    return 0;
}
```

- 3. DP / 记忆化搜索  
    原题是(LeetCode 198 House Robber)[https://leetcode.com/problems/house-robber/]
    转移方程: $dp[i] = max(dp[i - 2], dp[i - 3]) + a[i]$  

```cpp
#include <iostream>
#include <vector>

using namespace std;
class Solution {
public:
    vector <int> ary;
    vector <int> cnt;
    int dfs(int now) {
        if (cnt[now] != -1) return cnt[now];
        cnt[now] = ary[now];
        if (now + 2 < ary.size()) cnt[now] = max(cnt[now], dfs(now + 2) + ary[now]);
        if (now + 3 < ary.size()) cnt[now] = max(cnt[now], dfs(now + 3) + ary[now]);
        if (now + 1 < ary.size()) cnt[now] = max(cnt[now], dfs(now + 1));
        return cnt[now];
    }
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        for (int i = 0; i < nums.size(); ++i) 
            ary.push_back(nums[i]), cnt.push_back(-1);
        return dfs(0);
    }
};
int main() {
    int t, n, k;
    cin >> t;
    while (t--) {
        cin >> n;
        vector <int> nums;
        for (int i = 0; i < n; ++i) {
            int tmp;
            cin >> tmp;
            nums.push_back(tmp);
        }
        Solution S;
        cout << S.rob(nums) << endl;
    }
    return 0;
}
```

- 4. 容斥原理
    三个数各自的倍数
    任意两个数字的积的倍数
    三个数字积的倍数
    做容斥

```cpp
#include <iostream>
using namespace std;
int a, b, c, n, t;
int main() {
    cin >> t;
    while (t--) {
        cin >> a >> b >> c >> n;
        cout << n / a + n / b + n / c 
                 - n / (a * b) - n / (a * c) - n / (b * c)
                  + n / (a * b * c) << endl;
    }
    return 0;
}
```
---



