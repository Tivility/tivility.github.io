---
layout:     post
title:      "NE_of_CN_cpc_Spell_Boost_反思"
subtitle:   "2017ccpc东北四省赛I题反思"
date:       2018-05-21 00:00:00
author:     "Tivility"
header-img: "img/post_bg/auto.jpg"
tags:
    - Acm
---

# Problem I. Sell Boost

 Input file: | standard input
 |-|-|
 Output file: | standard output
 Time limit: | 6seconds
 Memory limit: | 256 megabytes

### Problem

 Shadowverse is a funny game. Oneday you are playing a round of this game.<br>
 You have n cards, each with two attributes $w_i$ and $x_i$. If you use card i, you will cost $w_i$ points of power and cause $x_i$ damage to the enemy. <br>
 Among them, there are two special types of cards: some cards are magic cards and some have "spell boost effect". Everytime you have used a magic card, for each unused "spell boost effect" card i: if the current cost of i (i.e. $w_i$) is positive, then $w_i$ will be reduced by 1. Note that some cards may be both magic cards and have spell boost effect. <br>
 Now you have W points of power, you need to calculate maximum total damage you can cause. <br>

### Input

 input is given from Standard Input in the following format: <br>
 n W <br>
 $w_1$, $x_1$, $is\usepackage{underscore}magic_1$, $is\usepackage{underscore}spell\usepackage{underscore}boost_1$ <br>
 $w_2$, $x_2$, $is\usepackage{underscore}magic_2$, $is\usepackage{underscore}spell\usepackage{underscore}boost_2$ <br>
 ...... <br>
 $w_n$, $x_n$, $is\usepackage{underscore}magic_n$, $is\usepackage{underscore}spell\usepackage{underscore}boost_n$ <br>

### Constraints

 $1 <= n <= 500$
 $0 <= W,\ w_i,\ x_i<=500$, and all of them are integers.
 $is\usepackage{underscore}magic_i$ means: If this card is magic card, the value is 1, otherwise the value is 0.
 $is\usepackage{underscore}spell\usepackage{underscore}boost_i$ means: If this card has spell boost effect, the value is 1, otherwise 0.

### Output

 one integer representing the maximum total daage.
 
### Examples

sandard imput | standard output
|-------------|-----------------|
3 3 <br> 3 3 1 1 <br> 2 3 1 1 <br> 1 3 1 1 | 9
4 3 <br> 3 3 1 1 <br> 3 4 1 0 <br> 1 3 0 1 <br> 1 0 1 0 | 7

## 题意
 - 有n张牌
 - 每个牌有四个值 m, b, w, x, 前两个是标记, 后两个是数值
 - w是花费, x是收益
 - 每出一张有m标记的牌, 在这张牌之后的含有b标记的牌的花费减少1
 - 求出在花费W之内的最大收益.


## 分析
 1. 显然, 如果最终答案一定要选定某些牌, 那么一定是优先选择带有magic标记的牌会更优.
 2. 当同时有magic标记的时候, 一定是优先选择不带boost标记的牌更优.
 3. 当同时带有magic和boost标记时, 按照花费从小到大选择不会比乱序选择更差.
 4. 所以我们按照以上顺序对输入排列, 排列之后简单01背包即可解决.


## 代码  

``` cpp
#include <stdio.h>
#include <algorithm>
using namespace std;

const int MAX = 555;
int n, w, tw, tx, tm, tb, cnt_m;
int dp[2][MAX][MAX];
pair < pair <int, int>, pair <int, int> > ary[MAX];

void getin() {
    scanf("%d%d", &n, &w);
    for (int i = 1; i <= n; ++i) {
        scanf("%d%d%d%d", &tw, &tx, &tm, &tb);
        if (tm) ++cnt_m;
        ary[i] = {
            {-tm, tb}, {tw, tx}
        };
    }
    sort (ary+1, ary+1+n);
    return ;
}
int judge() {
    int ans = -1;
    for (int i = 1; i <= n; ++i) {
        tm = -ary[i].first.first, tb = ary[i].first.second;
        tw = ary[i].second.first, tx = ary[i].second.second;
        for (int cnt = 0; cnt <= min(i, cnt_m); ++cnt) {
            for (int j = 0; j <= w; ++j) {
                dp[i&1][j][cnt] = dp[(i-1)&1][j][cnt];
                if (tm && !tb) { //m = 1, b = 0
                    if (!cnt) break;
                    if (j - tw >= 0)
                        dp[i&1][j][cnt] = max(dp[i&1][j][cnt],
                                dp[(i-1)&1][j-tw][cnt-1] + tx);
                }
                else if (tm && tb) { //m = 1, b = 1
                    if (!cnt) break;
                    if (j - max(0, (tw - cnt + 1)) >= 0)
                        dp[i&1][j][cnt] = max(dp[i&1][j][cnt],
                                dp[(i-1)&1][j-max(0, tw-cnt+1)][cnt-1] + tx);
                }
                else if (!tm && !tb) { //m = 0, b = 0
                    if (j - tw >= 0)
                        dp[i&1][j][cnt] = max(dp[i&1][j][cnt],
                                dp[(i-1)&1][j-tw][cnt] + tx);
                }
                else { //m = 0, b = 1;
                    if (j - max(0, (tw - cnt)) >= 0)
                        dp[i&1][j][cnt] = max(dp[i&1][j][cnt],
                                dp[(i-1)&1][j-max(0, (tw-cnt))][cnt] + tx);
                }
                ans = max(ans, dp[i&1][j][cnt]);
            }
        }
    }
    return ans;
}

int main() {
    getin();
    printf("%d\n", judge());
    return 0;
}

```

### ps.
在赛场上石乐志的统计了magic和boost能组合出4种类型的牌的个数, 然后在dp过程中利用个数确定位置, 进而确定类型. 时间复杂度确实更优, 然而忘记排序的时候, 不带magic的标记的两种牌的顺序. 这tm肯定就GG了. 赛后重新看代码发现问题. 简直智障. 

2018-05-21 智障留念.
