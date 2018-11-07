---
layout:     post
title:      "contest of ACM Lesson(part1)"
subtitle:   "哈尔滨工程大学ACM专选课期末考试题解"
date:       2018-11-07 22:00:00
author:     "Tivility"
header-img: "img/post_bg/acm.png"
tags:
    - Courseware for ACM Professional Electives
---


  
  
- 1.高精度: 计算2的1024次幂.  
  理论来说只需重载加号即可.
  ``for (int i = 0; i < n; ++i) ans = ans + ans;``

```cpp
// author: Tivility, 2016201621, hrbeu
// email : tivility@gmail.com
// 2018.09.25
// this code define +, *, fastpower.

// g++ -std=c++11 code.cpp

#include <stdio.h>
#include <string.h>
#include <iostream>

using namespace std;

const int MAX = 500;

class Biginterger {
    // considering all the answer is above zero
    // do not pay attention on the negative number
public:
    char ary[MAX]; 
    int len;
    Biginterger() {}
    void init(int a = 0) {
        memset (ary, 0, sizeof (ary));
        for (len = 0; a; ++len) {
            ary[len] = a % 10;
            a /= 10;
        }
        if (len) 
            len--;
        return ;
    }
    void show() const{
        for (int i = len; i >= 0; --i) 
            cout << (int)ary[i];
        return ;
    }
    void adjust() {
        while (ary[len] == 0 && len) 
            len--;
        return ;
    }
    void m10() {
        memcpy(ary+1, ary, sizeof(ary[0])*(++len));
        ary[0] = 0;
        adjust();
        return ;
    }
    Biginterger operator + (const Biginterger &b) const{
        Biginterger ret; ret.init();
        ret.len = (len > b.len ? len : b.len);
        bool add = false;
        for (int i = 0; i <= ret.len; ++i) {
            ret.ary[i] = ary[i] + b.ary[i] + (int)add;
            add = bool(ret.ary[i] / 10);
            ret.ary[i] %= 10;
        }
        if (add)
            ret.ary[++ret.len] = 1;
        ret.adjust();
        return ret;
    }
    Biginterger operator * (const char &b) const{
        // 0 <= b <= 9
        Biginterger ret; ret.init();
        ret.len = len;
        int add = 0;
        for (int i = 0; i <= ret.len; ++i) {
            ret.ary[i] = ary[i] * (int)b + add;
            add = ret.ary[i] / 10;
            ret.ary[i] %= 10;
        }
        if (add)
            ret.ary[++ret.len] = add;
        ret.adjust();
        return ret;
    }
    Biginterger operator * (const Biginterger &b) const {
        Biginterger ret; ret.init();
        for (int i = len; i >= 0; --i) {
            ret.m10();
            ret = (ret + b * ary[i]);
        }
        ret.adjust();
        return ret;
    }
};
Biginterger fastpow(Biginterger a, int p) {
    Biginterger ret; ret.init(1);
    for ( ; p; p >>= 1) {
        if (p & 1)
            ret = ret * a;
        a = a * a;
    }
    return ret;
}
int main() {
    Biginterger a;
    a.init(2);
    int T;
    cin >> T;
    while (T--) {
        int p;
        cin >> p;
        fastpow(a, p).show();
        cout << endl;
    }
    return 0;
}

```
  
- 2.逆序输出.  
  
```cpp
#include <stdio.h>
#include <string.h>

const int MAX = 111;
char s[MAX];

int main() {
    int t, n;
    scanf ("%d", &t);
    while (t--) {
        scanf ("%d", &n);
        scanf ("%s", s);
        for (int i = n - 1; i >= 0; --i)
            printf ("%c", s[i]);
        puts("");
    }
    return 0;
}
```  
  
- 3.判断是否为回文串: 两个指针, 对称遍历判断即可.  
  
```cpp
#include <stdio.h>

const int MAX = 111;
char s[MAX];
int t, n, k;

bool judge() {
    for (int i = 0; i < n; ++i)
        if (s[i] != s[n - 1 - i])
            return false;
    return true;
}
int main() {
    scanf ("%d", &t);
    while (t--) {
        scanf ("%d", &n);
        scanf ("%s", s);
        if (judge()) puts("yes");
        else puts("no");
    }
    return 0;
}
```

- 4.完全树的先序遍历:  
     先序遍历的顺序为根左右  
     深度优先搜索递归即可.  

```cpp
#include <stdio.h>
int t, n;
void dfs(int now) {
    printf ("%d ", now);
    if (2 * now <= n)
        dfs(2 * now);
    if (2 * now + 1 <= n)
        dfs(2 * now + 1);
    return ;
}
int main() {
    scanf ("%d", &t);
    while (t--) {
        scanf ("%d", &n);
        dfs(1);
        puts("");
    }
    return 0;
}
```
---



