---
layout:     post
title:      猪国杀 解题报告
subtitle:   SDOI2010
date:       2024-08-04
author:     VecantDK
header-img: img/post-bg-coffee.jpeg
catalog: 	 true
tags:
    - OI
    - 题解

---


<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

# 前情提要

<center>    <img style="border-radius: 0.3125em;    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"     src="https://vecantdk.github.io/img/Zhuguosha_01.png" width = "100%" alt=""/>    <br>    <div style="color:orange; border-bottom: 1px solid #d9d9d9;    display: inline-block;    color: #999;    padding: 2px;">      知乎玩家对 SDOI2010 的评价      </div> </center>

<center>    <img style="border-radius: 0.3125em;    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"     src="https://vecantdk.github.io/img/Zhuguosha_02.png" width = "100%" alt=""/>    <br>    <div style="color:orange; border-bottom: 1px solid #d9d9d9;    display: inline-block;    color: #999;    padding: 2px;">      摘自 OI Wiki - 比赛相关 - 出题      </div> </center> 

# 题面

### 游戏背景

《猪国杀》是一种多猪牌类回合制游戏，一共有 $3$ 种角色：主猪，忠猪，反猪。每局游戏主猪有且只有 $1$ 只，忠猪和反猪可以有多只，每只猪扮演 $1 $ 种角色。

### 游戏目的

主猪 / $\texttt{MP}$：自己存活的情况下消灭所有的反猪。  

忠猪 / $\texttt{ZP}$：不惜一切保护主猪，胜利条件与主猪相同。  

反猪 / $\texttt{FP}$：杀死主猪。

### 游戏过程

游戏开始时，每个玩家手里都会有 $4$ 张牌，且体力上限和初始体力都是 $4$ 。

开始游戏时，从主猪开始，按照逆时针方向（数据中就是按照编号从 $ 1 , 2, 3 \ldots n , 1 \ldots $ 的顺序）依次行动。

每个玩家自己的回合可以分为 2 个阶段：

* **摸牌阶段**：从牌堆顶部摸 $2$ 张牌，依次放到手牌的最右边； 
* **出牌阶段**：你可以使用任意张牌，每次使用牌的时候都使用最靠左的能够使用的牌。当然，要满足如下规则：
  1. 如果没有猪哥连弩，每个出牌阶段只能使用 $1$ 次「杀」来攻击；
  2. 任何牌被使用后被弃置（武器是装备上）；被弃置的牌以后都不能再用，即与游戏无关。

### 各种牌介绍

每张手牌用 $1$ 个字母表示，字母代表牌的种类。

#### 基本牌

- 『桃 / $\texttt{P}$』在自己的回合内，如果自己的体力值不等于体力上限，那么使用 $1$ 个桃可以为自己补充 $1$ 点体力，否则不能使用桃；桃只能对自己使用；在自己的回合外，如果自己的血变为 $0$ 或者更低，那么也可以使用。

- 『杀 / $\texttt{K}$』在自己的回合内，对攻击范围内除自己以外的 $1$ 名角色使用。如果没有被『闪』抵消，则造成 $1$ 点伤害。无论有无武器，杀的攻击范围都是 $1$。

- 『闪 / $\texttt{D}$』当你受到杀的攻击时，可以弃置 $1$ 张闪来抵消杀的效果。

#### 锦囊牌

- 『决斗 / $\texttt{F}$』出牌阶段，对除自己以外任意 $1$ 名角色使用，由目标角色先开始，自己和目标角色轮流弃置 $1$ 张杀，首先没有杀可弃的一方受到 $1$ 点伤害，另一方视为此伤害的来源。

- 『南猪入侵 / $\texttt{N}$』出牌阶段，对除你以外所有角色使用，按逆时针顺序从使用者下家开始依次结算，除非弃置 $1$ 张杀，否则受到 $1$ 点伤害。

- 『万箭齐发 / $\texttt{W}$』和南猪入侵类似，不过要弃置的不是杀而是闪。

- 『无懈可击 / $\texttt{J}$』在目标锦囊生效前抵消其效果。每次有 $1$ 张锦囊即将生效时，从使用这张锦囊的猪开始，按照逆时针顺序，依次得到使用无懈可击的机会；效果：用于决斗时，决斗无效并弃置；用于南猪入侵或万箭齐发时，当结算到某个角色时才能使用，当前角色不需弃置牌并且不会受到伤害（仅对 $1$ 个角色产生效果）；用于无懈可击时，成为目标的无懈可击被无效。

#### 装备牌

- 『猪哥连弩 / $\texttt{Z}$』武器，攻击范围 $1$ ，出牌阶段你可以使用任意张杀; 同一时刻最多只能装 $1$ 把武器；如果先前已经有了 $1$ 把武器，那么之后再装武器的话，会弃置以前的武器来装现在的武器。

### 特殊事件及概念解释

* **伤害来源**：杀、南猪入侵、万箭齐发的伤害来源均是使用该牌的猪，决斗的伤害来源如上；

* **距离**：两只猪的距离定义为沿着逆时针方向间隔的猪数 $+1$ 。即初始时 $1$ 和 $2$ 的距离为 $1$ ，但是 $2$ 和 $1$ 的距离就是 $n-1$ 。注意一个角色的死亡会导致一些猪距离的改变；

* **玩家死亡**：如果该玩家的体力降到 $0$ 或者更低，并且自己手中没有足够的桃使得自己的体力值回到 $1$ ，那么就死亡了，死亡后所有的牌（装备区，手牌区）被弃置；

* **奖励与惩罚**：反猪死亡时，最后一个伤害来源处（即使是反猪）立即摸 $3$ 张牌。忠猪死亡时，如果最后一个伤害来源是主猪，那么主猪所有装备牌、手牌被弃置。

注意：一旦达成胜利条件，游戏立刻结束，因此即使会摸 $3$ 张牌或者还有牌可以用也不用执行了。

现在，我们已经知道每只猪的角色、手牌，还有牌堆初始情况，并且假设每个角色会按照如下的行为准则进行游戏，你需要做的就是告诉小猪 iPig 最后的结果。

### 几种行为

* **献殷勤**：使用无懈可击挡下南猪入侵、万箭齐发、决斗；使用无懈可击抵消表敌意；  
* **表敌意**：对某个角色使用杀、决斗；使用无懈可击抵消献殷勤；  
* **跳忠**：即通过行动表示自己是忠猪。跳忠行动就是对主猪或对某只已经跳忠的猪献殷勤，或者对某只已经跳反的猪表敌意；  
* **跳反**：即通过行动表示自己是反猪。跳反行动就是对主猪或对某只已经跳忠的猪表敌意，或者对某只已经跳反的猪献殷勤。

**注意：忠猪不会跳反，反猪也不会跳忠；不管是忠猪还是反猪，能够跳必然跳**。

### 行动准则

#### 共性

* 每个角色如果手里有桃且生命值未满，那么必然吃掉；
* 有南猪入侵、万箭齐发、必然使用；有装备必然装上；
* 受到杀时，有闪必然弃置；
* 响应南猪入侵或者万箭齐发时候，有杀 / 闪必然弃置；
* 不会对未表明身份的猪献殷勤（**包括自己**）。

#### 特性

 - 主猪：
   + 主猪会认为「没有跳身份，且用南猪入侵 / 万箭齐发对自己造成伤害的猪」是**类**反猪（没伤害到不算，注意类反猪并没有表明身份），如果之后跳了，那么主猪会重新认识这只猪；  
   + 对于每种表敌意的方式，对逆时针方向能够执行到的第一只类反猪或者已跳反猪表；如果没有，那么就不表敌意；
   + 决斗时会不遗余力弃置杀；
   + 如果能对已经跳忠的猪或自己献殷勤，那么一定献；如果能够对已经跳反的猪表敌意，那么一定表。
 - 忠猪：
   + 对于每种表敌意的方式，对「逆时针方向能够执行到的第一只已经跳反的猪」表，如果没有，那么就不表敌意；
   + 决斗时，如果对方是主猪，那么不会弃置杀，否则，会不遗余力弃置杀；
   + 如果有机会对主猪或者已经跳忠的猪献殷勤，那么一定献。
 - 反猪：
   + 对于每种表敌意的方式，如果有机会则对主猪表，否则，对「逆时针方向能够执行到的第一只已经跳忠的猪」表，如果没有，那么就不表敌意；
   + 决斗时会不遗余力弃置杀；
   + 如果有机会对已经跳反的猪献殷勤，那么一定献。

限于 iPig 只会用 P++ 语言写 A + B，他请你用 Pigcal (Pascal)、P (C) 或 P++ (C++) 语言来帮他预测最后的结果。

---

# 思路

对于这~~又长又臭的~~题面，我们需要先仔细地阅读理解，列出有用信息。

## 1：变量/初始化/每只猪 `struct` 的定义

### 变量与常量

```cpp
const int N = 15, M = 2005;
enum PIG_TYPE {
    MP = 1, ZP, FP 
};
int n, m;
int top = 1;    //牌堆顶
char cards[M];  //牌堆
bool gameover;  //游戏是否结束
int fpcnt;      //反猪数量
```

我们使用一个枚举型来表示猪的种类。其值对应如下：

| 名称     | MP   | ZP   | FP   |
| -------- | ---- | ---- | ---- |
| **数值** | 1    | 2    | 3    |
| **解释** | 主猪 | 忠猪 | 反猪 |

### 每只猪的 `struct`

```cpp
struct Pig {
    int id, eid;  //身份, 暴露的身份
    bool like_v;  //是否为类反猪 
    int hp;	  //生命值 
    bool weapon;  //是否装备武器 
    int cnt;	  //牌数 
    char card[M]; //牌堆
    int pre, nxt; //前面的猪和后面的猪的位置 
    Pig() {	  //构造函数, 初始化牌堆, 以及设置初值
        for (int i = 1; i < M; i++)
            card[i] = 0;
        hp = 4;
    }
}pig[N];
```

其中

- 对于身份 `id`，其对应值为 `PIG_TYPE` 中的数；
- 若 `card[i]==NULL`，则这个位置没有牌。

## 2：一些简单的函数

### 打印猪的手牌

```cpp
int printcard(int x) { //打印第x只猪的手牌
    for (int i = 1; i <= pig[x].cnt; i++)
        if (pig[x].card[i])
            printf("%c ", pig[x].card[i]);
    puts("");
    return 0;
}
```

### 找牌

```cpp
bool findcard(int x, char card) {
    for (int i = 1; i <= pig[x].cnt; i++)
        if (pig[x].card[i] == card) {
            pig[x].card[i] = 0;
            return true;
        }
    return false;
}
```

若找到对应卡牌，该函数会返回 `true`，并弃置这张卡牌；否则返回 `false`。

### 摸牌

```cpp
void getcard(int x, int times = 1) {
    while (times--)
        pig[x].card[++pig[x].cnt] = cards[(top = std::min(top, m))++];
}
```

## 3：扣血与死亡判定

在定义猪的结构体中，我们通过 `nxt` 和 `pre`，即以循环链表的形式来实现环的运行。

```cpp
void hurt(int from, int to)
{
    pig[to].hp--;
    while (pig[to].hp < 1 && findcard(to, 'P'))
        pig[to].hp++;
    if (pig[to].hp <= 0) {
        pig[to].cnt = 0, pig[to].weapon = false; //清空物品
        switch (pig[to].id) {
            case MP: {
                gameover = true;
                return;
            } case ZP: { //若主猪杀死忠猪, 则清空主猪手牌 
                if (pig[from].id == MP)
                pig[from].cnt = 0, pig[from].weapon = false;
                break;
            } case FP: {
                if(!--fpcnt) {
                    gameover = true;
                    return;
                }
                for (int T = 1; T <= 3; T++)
                    getcard(from); //杀死反猪者摸牌 
                break;
            }
        }
        pig[pig[to].pre].nxt = pig[to].nxt, pig[pig[to].nxt].pre = pig[to].pre; //更新链表 
    }
}
```

## 4：猪之间的关系

该部分实现了四个函数。它们的功能如下：

- `bool isEnemy(int x, int y)`：判断 $x$ 和 $y$ 是否为敌人关系；
- `bool isFriend(int x, int y)`：判断 $x$ 和 $y$ 是否为朋友关系；
- `void getEnemy(int x, int y)`：$x$ 向 $y$「表敌意」；

- `void getFriend(int x, int y)`：$x$ 向 $y$「献殷勤」。

注意：前两个函数判断的是两只猪的真实身份（即 `id`），而后者所改变的是暴露的身份（即 `eid`）。

```cpp
bool isEnemy(int x, int y) {
    if (pig[x].id == MP && pig[y].like_v) return true;	//主猪与类反猪是敌人 
    else if (!pig[y].eid) return false;			//未显露身份, 不是敌人 
    //主猪、忠猪与反猪是敌人 
    else if (pig[x].id != FP && pig[y].eid == FP || pig[x].id == FP && pig[y].eid != FP)
        return true;
    return false;
}
bool isFriend(int x, int y) {
    if (!pig[y].eid) return false;
    //同类是朋友; 主猪与忠猪是朋友 
    else if (pig[x].id != FP && pig[y].eid != FP || pig[x].id == FP && pig[y].eid == FP)
        return true;
    return false; 
}
void getEnemy(int x, int y) {
    if (!pig[y].eid) return; //不应对未亮身份的猪操作
    if (pig[y].eid != FP) pig[x].like_v = true, pig[x].eid = FP;
    else pig[x].like_v = false, pig[x].eid = ZP;
}
void getFriend(int x, int y) { 
    if (!pig[y].eid) return;
    if (pig[y].eid != FP) pig[x].like_v = false, pig[x].eid = ZP;
    else pig[x].like_v = true, pig[x].eid = FP;
}
```

## 5：使用卡牌

### Ⅰ. 杀/闪

按题意模拟即可。

```cpp
void useK(int from, int to) {
    getEnemy(from, to);
    if (!findcard(to, 'D'))
        hurt(from, to);
}
```

### Ⅱ. 决斗

此时的「决斗」函数当且仅当未被「无懈可击」挡住、决斗生效时。

```cpp
void useF(int from, int to) {
    if (pig[from].id == MP && pig[to].id == ZP) { //忠猪不会对主猪弃置杀 
        hurt(from, to);
        return;
    }
    while (true) {
        if (!findcard(to, 'K'))   {hurt(from, to); return;}
        if (!findcard(from, 'K')) {hurt(to, from); return;}
    }
}
```

### Ⅲ. 无懈可击

理解题意，可得：

- 若朋友对自己发出锦囊牌，则使用「无懈可击」挡住，同时「献殷勤」；
- 若敌人使用了「无懈可击」，则使用「无懈可击」使敌人的「无懈可击」无效，同时「表敌意」。

```cpp
bool useJ(int from, int to, int last) {	//此处last表示第一次使用无懈可击, 而不是未表明身份 
    int now = from;
    while (true) {
        if (!last && isFriend(now, to) || last && isEnemy(now, last))
            if(findcard(now, 'J')) {
                if (!last && isFriend(now, to))  getFriend(now, to);
                if ( last && isEnemy (now, last)) getEnemy(now, last);
                if (!useJ(pig[now].nxt, to, now))
                    return true;	//若未被其他无懈可击抵消, 则该操作有效 
            } 
        if ((now = pig[now].nxt) == from)
            return false;	 
    }
}
```

### Ⅳ. 南猪入侵

要点：

- 若游戏结束，则应立即结束锦囊牌，不继续弃牌；
- 若忠猪误伤主猪，且忠猪未表明身份，则应视为类反猪。

```cpp
void useN(int from, int to) {
    while (true) {
        if (gameover) return; //一旦达成胜利条件, 游戏立刻结束
        if (!useJ(from, to, 0))
            if (!findcard(to, 'K')) { //不能响应则扣血
                if (pig[to].id == MP && !pig[from].eid)
                    pig[from].like_v = true;
                hurt(from, to); 
            }
        if ((to = pig[to].nxt) == from)
            return;
    }
}
```

### Ⅴ. 万箭齐发

同南猪入侵。

```cpp
void useW(int from, int to) {
    while (true) {
        if (gameover) return;
        if (!useJ(from, to, 0))
            if (!findcard(to, 'D')) {
                if (pig[to].id == MP && !pig[from].eid)
                    pig[from].like_v = true;
                hurt(from, to); 
            }
        if ((to = pig[to].nxt) == from)
            return;
    }
}
```

### 6. 游戏运行/输入输出

按题意模拟即可。

要点：

- 使用某些手牌后可能改变局面，所以使用手牌后要从头扫描手牌；
- 不论如何，一旦达成胜利条件, 游戏立刻结束；

```cpp
void rungame() {
while (true) {
    for (int i = 1; i <= n; i++) {
        if(gameover) return;
        if (pig[i].hp <= 0) continue;
        getcard(i, 2);
        bool flag = false; //是否出过杀
        for (int j = 1; j <= pig[i].cnt; j++) {
            if (gameover || !pig[i].card[j]) continue;
            switch (pig[i].card[j]) {
                case 'P': {
                    if (pig[i].hp < 4)
                        pig[i].card[j] = 0, pig[i].hp++;
                    break;
                } case 'K': {
                    if (!flag && isEnemy(i, pig[i].nxt)) {
                        pig[i].card[j] = 0;
                        useK(i, pig[i].nxt);
                        if (!pig[i].weapon) flag = true;
                        j = 0;
                    }
                    break;
                } case 'F': {
                    if (pig[i].id == FP) {
                        pig[i].card[j] = 0;
                        getEnemy(i, MP);
                        if (useJ(i, 1, 0)) break;
                        useF(i, 1);
                        j = 0;
                    } else {
                        int to = pig[i].nxt;
                        while (true) {
                            if (isEnemy(i, to)) {
                                pig[i].card[j] = 0;
                                getEnemy(i, to);
                                if (useJ(i, to, 0)) break;
                                useF(i, to);
                                j = 0;
                                break;
                            }
                            to = pig[to].nxt;
                            if (to == i) break;
                        }
                    }
                    break;
                } case 'N': {
                    pig[i].card[j] = 0;
                    useN(i, pig[i].nxt);
                    j = 0;
                    break;
                } case 'W': {
                    pig[i].card[j] = 0;
                    useW(i, pig[i].nxt);
                    j = 0;
                    break;
                } case 'Z': {
                    pig[i].card[j] = 0;
                    pig[i].weapon = true, flag = false;
                    j = 0;
                    break;
                }
            }
        }
    }
}}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        char ch;
        std::string name;
        std::cin >> name;
        switch (name[0]) {
            case 'M': pig[i].id = MP, pig[i].eid = MP; break;
            case 'Z': pig[i].id = ZP; break; 
            case 'F': pig[i].id = FP, fpcnt++; break;
        }
        for (pig[i].cnt = 1; pig[i].cnt <= 4; pig[i].cnt++)
            std::cin >> ch, pig[i].card[pig[i].cnt] = ch;
        pig[i].nxt = i + 1, pig[i].pre = i - 1;
    }
    pig[1].pre = n, pig[n].nxt = 1;
    for (int i = 1; i <= m; i++)
    std::cin >> cards[i];
    rungame();
    puts(pig[MP].hp <= 0 ? "FP" : "MP");
    for (int i = 1; i <= n; i++)
        pig[i].hp <= 0 ? puts("DEAD") : printcard(i);
    return 0;
}
```

# 总结

非常好大模拟，这使我的大脑旋转。
