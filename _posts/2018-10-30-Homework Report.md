---
layout:     post
title:      "Homework Report"
subtitle:   "哈尔滨工程大学ACM专选课解题报告下载链接"
date:       2018-10-30 00:00:00
author:     "Tivility"
header-img: "img/post_bg/acm.png"
tags:
    - Courseware for ACM Professional Electives
---


  
### 模拟考试题解代码:

- 1.博弈论

```cpp
#include <stdio.h>
#include <iostream>

using namespace std;

int main() {
    int t, n, k;
    cin >> t;
    while (t--) {
        cin >> n >> k;
        if (n % (k + 1))
            cout << "Alice" << endl;
        else 
            cout << "Bob" << endl;
    }
    
    return 0;
}
```

- 2.简单搜索

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
bool judge() {
    while (que.size()) que.pop();
    memset (used, 0, sizeof (used));
    que.push(st);
    used[st.x][st.y] = true;
    while (que.size()) {
        now = que.front(); que.pop();
        if (now.x == ed.x && now.y == ed.y)
            return true;
            //return now.deep < maxdeep;
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
    return false;
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
        if (judge()) cout << "yes" << endl;
        else cout << "no" << endl;
    }
    return 0;
}
```

- 3.二分图判定(dfs黑白染色)

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <iostream>
#include <algorithm>

using namespace std;
int t, n, m, u, v;
const int MAX = 2e6;
struct Edge {
    int to, nxt;
}edge[MAX << 1];
int head[MAX], edgecnt;
int color[MAX];
void init(int n = MAX) {
    memset (head, 0, sizeof (head[0]) * (n + 1));
    memset (color, -1, sizeof (color[0]) * (n + 1));
    edgecnt = 1;
    return ;
}
void addedge(int u, int v) {
    edge[edgecnt].to  = v;
    edge[edgecnt].nxt = head[u];
    head[u] = edgecnt++;
    return ;
}
bool dfs(int n, int col) {
    for (int i = head[n]; i; i = edge[i].nxt) {
        int nxt = edge[i].to;
        if (color[nxt] != -1) {
            if (color[nxt] != col)
                return false;
            else 
                continue;
        }
        else {
            color[nxt] = col;
            if (!dfs(nxt, col ^ 1))
                return false;
        }
    }
    return true;
}
bool judge() {
    for (int i = 1; i <= n; ++i)
        if (color[i] == -1 && (!dfs(i, 0)))
            return false;
    return true;
}
int main() {
    cin >> t;
    while (t--) {
        cin >> n >> m;
        init(n);
        for (int i = 0; i < m; ++i) {
            cin >> u >> v;
            addedge(u, v), addedge(v, u);
        }
        if (judge()) cout << "yes" << endl;

        
        if (dfs(1, 0)) cout << "yes" << endl;
        else cout << "no" << endl;
    }
    return 0;
}
```

- 4.spfa判负环

```cpp
// code from lajiyuan
#include <stdio.h>
#include <algorithm>
#include <string.h>
#include <queue>
#include <iostream>
#define IL inline
#define RI register int
#define N 100086
#define clearr(a) memset(a,0,sizeof a)
#define rk for(RI i=1;i<=n;i++)
using namespace std;
IL void read(int &x)
{
    int f=1;x=0;char s=getchar();
    while(s>'9'||s<'0'){if(s=='-')f=-1;s=getchar();}
    while(s<='9'&&s>='0'){x=x*10+s-'0';s=getchar();}
    x*=f;
}
int n,m,T;
struct code{int u,v,w;}edge[N];
bool vis[N];
int head[N],tot,dis[N],cnt[N];
IL void add(int x,int y,int z){edge[++tot].u=head[x];edge[tot].v=y;edge[tot].w=z;head[x]=tot;}
IL bool spfa(int now)
{
    rk vis[i]=false,dis[i]=2147483647,cnt[i]=false;
    queue<int>q;
    q.push(now);
    vis[now]=true;
    dis[now]=0;
    while(!q.empty())
    {
        int u=q.front();q.pop();vis[u]=false;
        if(cnt[u]>=n)return true;
        for(RI i=head[u];i;i=edge[i].u)
        {
            if(dis[edge[i].v]>dis[u]+edge[i].w)
            {
                dis[edge[i].v]=dis[u]+edge[i].w;
                if(!vis[edge[i].v])
                {
                    q.push(edge[i].v);
                    vis[edge[i].v]=true;
                    cnt[edge[i].v]++;
                    if(cnt[edge[i].v]>=n)return true;
                }
            }
        }
    }
    return false;
}
int vis2[N];
vector<int> vec[N];
void dfs(int x)
{
    vis2[x]=1;
    for(int i=0;i<vec[x].size();i++)
    {
        int to=vec[x][i];
        if(!vis2[to])
        {
            dfs(to);
        }
    }
    return ;
}
int main()
{
    read(T);
    while(T--)
    {
        read(n),read(m);
        tot=0;clearr(head);
        rk vis2[i]=0;
        rk vec[i].clear();
        for(RI i=1,u,v,w;i<=m;i++)
        {
            read(u),read(v),read(w);
            vec[u].push_back(v);
            vec[v].push_back(u);
            if(w<0)add(u,v,w);
            else add(u,v,w),add(v,u,w);
        }
        int flag=0;
        for(int i=1;i<=n;i++)
        {
            if(!vis2[i])
            {
                dfs(i);
                if(spfa(i))
                {
                    flag=1;
                    break;
                }
            }
        }
        if(flag==0) puts("no");
        else puts("yes");
    }
}
```

### 解题报告下载链接
  - [解题报告](https://github.com/Tivility/tivility.github.io/raw/master/pdf/ACM_lesson.doc)

<iframe src='https://view.officeapps.live.com/op/view.aspx?src=https://github.com/Tivility/tivility.github.io/raw/master/pdf/ACM_lesson.doc' width='100%' height='800%' frameborder='1'>
</ifream>

---



