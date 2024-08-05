---
layout:     post
title:      The solution report of the Legends of the Pig Kingdoms
subtitle:   SDOI2010
date:       2024-08-04
author:     VecantDK
header-img: img/post-bg-coffee.jpeg
catalog: 	 true
tags:
    - OI
    - 题解

---

<style>
pre {
    overflow-y: auto;
    max-height: 300px;
}
</style>

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
[Chinese version（猪国杀 解题报告）.](https://vecantdk.github.io/2024/08/04/zhuguosha/) 

# Previous synopsis

<center>    <img style="border-radius: 0.3125em;    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"     src="https://vecantdk.github.io/img/Zhuguosha_01.png" width = "100%" alt=""/>    <br>    <div style="color:orange; border-bottom: 1px solid #d9d9d9;    display: inline-block;    color: #999;    padding: 2px;">      The Zhihu users' comments on the SDOI2010      </div> </center>

> **What kind of the competition was the SDOI2010?**
>
> As title, is it a provincial team selection like the ACM?
>
> > SuperBrain0xb
> >
> > The most terrible provincial team selection in the history of China's OI...... Disgusting!
>
> > Anonymous users
> >
> > You can learn about the "the Legends of the Pig Kingdoms".

<center>    <img style="border-radius: 0.3125em;    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"     src="https://vecantdk.github.io/img/Zhuguosha_02.png" width = "100%" alt=""/>    <br>    <div style="color:orange; border-bottom: 1px solid #d9d9d9;    display: inline-block;    color: #999;    padding: 2px;">      Excerpt from OI Wiki - Contest Related - Make Problems      </div> </center> 

> Not only physics, but also the knowledge of other disciplines should not be involved too much in the OI problems. And if it does, it should be explained in detail. And the knowledge of other disciplines should not be used as a major obstacle to solving the problem.
>
> > A good problem, no matter how difficult it is, it should have its own thinking difficulty, which requires the contestant to think about and discover some properties.
> >
> > The code for a good problem can be long, but it mustn't be made longer by forcibly nesting or adding conditions. But it must look natural, and making people feel that the code of the problem should be so long.
> >
> > ——Wang Tianyi, *Study on the harm of deviance*
>
> Classic example: [SDOI2010] The Legends of the Pig Kingdoms, [training team mutual test 2015] future program · changed

Recently, I'm looking for some problems of the **big simulation**. The first thing that came to mind was ~~the notorious~~ the famous [the Legends of the Pig Kingdoms](https://www.luogu.com.cn/problem/P2482), so I tried it~~, although I can't even write the [structure](https://www.luogu.com.cn/problem/P9754).~~ After an afternoon ~~of eating shit~~, I finally made it, and posted this commemorative article.

**NOTE: The Chinese Zodiac in 2010 is Pig. So in that year's SDOI, there were many problems in which the protagonists were pigs. The name of this problem, "the Legends of the Pig Kingdoms (Zhuguosha, 猪国杀)", is adapted from a cardgame which is called "Legends of the Three Kingdoms (Sanguosha, 三国杀)".**

# Topic

## The description of the topic

### Game background

*The Legends of the Pig Kingdoms* is a turn-based game with a variety of pig-cards，There are three types of characters in the game: Monarch-Pig, Minister-Pig, Rebel-Pig. There is *only* $1$ Monarch-Pig per a game, and the Minister-Pig and Rebel-Pig can have multiple. Each pig plays only one role.

### The purpose of the game

Monarch-Pig / $\texttt{MP}$：Kill all the Rebel-Pigs while surviving itself. 

Minister-Pig / $\texttt{ZP}$：Protect the Monarch-Pig at all costs, and its victory conditions are the same as the Monarch-Pig。  

Rebel-Pig / $\texttt{FP}$：Kill the Monarch-Pig.

### Gameplay

At the start of the game, At the start of the game, each player will have $4$ cards in their hand. And both the maximum and initial health of them are $4$.

When the game starts, it starts with the Monarch-Pig and moves in a counterclockwise direction（The data is numbered from $ 1 , 2, 3 \ldots n , 1 \ldots $）. 

Each player's own turn can be divided into 2 phases:

* **The draw phase**：Draw $2$ cards from the top of the deck, and place them to the far right of your hand; 
* **The play phase**：You can play any hand. And each time you play a hand, you should use the leftmost hand that you can play. Of course, the following rules must be met:
  1. If you don't have a Pig Crossbow, you can only attack with $1$ "Strike" per a turn；
  2. Any cards will be discarded after they were used (and any weapons will be discarded after they were equipped) ; Any cards which have been discarded can't be used in the future, i.e. they have nothing to do with the game.

### The introduction of the various cards

Each hand is represented by $1$ letter, which represents the type of the hand.

#### The Basic hands

- "Peach / $\texttt{P}$" : In your turn, if your HP isn't equal to your upper HP, then you can use $1$ peach to replenish $1$  HP for yourself. Otherwise you can't use peaches; Peaches can only be used on yourself; Outside of your turn, you can also use it if your health becomes $0$ or less.

- "Strike / $\texttt{K}$" : In your turn, Cast it on $1$ character other than yourself within the attack range. It deals $1$ damage if it isn‘t negated by "Dodge". With or without a weapon, the attack range of the "Strike" is $1$.

- "Dodge / $\texttt{D}$" : When you are attacked by "Strike", you can discard $1$ "Dodge" to counteract the effect of "Strike".

#### The Strategic hands

- "Duel / $\texttt{F}$" : In the play phase, use it on any $1$ character other than yourself. Starting with the target character first, and take turns discarding $1$ Strike for yourself and the target character. And the first character that has no "Strike" to discard will receive $1$ damage, and the other side will be deemed to be the source of this damage.

- "Barbarianpig Invasion / $\texttt{N}$" : In the play phase, use it on all characters except you. The payments are made in counterclockwise order starting from the user's next hand. Unless $1$ "Strike" is discarded, it takes $1$ damage.

- "Arrow Barrage / $\texttt{W}$" : It's similar to the "Barbarianpig Invasion", but instead of "Strike", "Dodge" is discarded.

- "Cancel / $\texttt{J}$" : Negates the effect of the target Strategic hand before it takes effect. Whenever $1$ Strategic hands is about to take effect, Starting with the pig using this Strategic hands, in counterclockwise order, you will be given the opportunity to use "Cancel"; Effect: When it's used for "Duel", the "Duel" is invalidated and discarded; When it's used for "Barbarianpig Invasion" or "Arrow Barrage", it can only be used when a character resolves, and the current character doesn't need to discards card, and doesn't take damage (only affects $1$ character); When it's used for "Cancel", the targeted "Cancel" is invalidated.

#### The Armor hands

- "Pig Crossbow / $\texttt{Z}$" : Weapon. Its attack range is $1$. During the discard phase, you can use any "Strike"; You can only have a maximum of $1$ weapon loaded at a time; If you already had $1$ weapon before, then if you reload the weapon later, the previous weapon will be discarded to load the current weapon.

### 特殊事件及概念解释

* **Damage source**: The damage source of "Strike", "Barbarianpig Invasion", and "Arrow Barrage" is the pig that uses the card. And the damage source of "Duel" is as above.

* **Distance**: The distance between the two pigs is defined as the number of pigs spaced in a counterclockwise direction $+1$, i.e. the distance between $1$ and $2$ is $1$ initially, but the distance between $2$ and $1$ is $n-1$. Note that the death of a character causes the distance of some pigs to change.

* **Death of the Player**: If a player's HP drops to $0$ or lower, and he doesn't have enough "Peach" in his hand to return his HP to $1$, he will die. And all of his hands (equipments area, hands area) are discarded upon death.

* **Rewards and Punishments**: When the Rebel-Pig dies, The last source of damage (even if he's a Rebel-Pig) immediately draws $3$ cards. When a Minister-Pig dies, if the last source of damage is the Monarch-Pig, all equitment cards and hand cards of the Monarch-Pig are discarded.

Note: If the victory condition is met, the game ends immediately. So you don't have to play even if you draw $3$ cards or have cards to use.

Now tht we know each pig's character, hands, and initial deck. And assuming that each character will play according to the following code of conduct, all you need to do is tell pig iPig the final result.

### Some actions

* **Expressing friendly**: Use "Cancel" to block "Barbarianpig Invasion", "Arrow Barrage", and "Duel"; Use "Cancel" to counteract “Expressing hostility”.
* **Expressing hostility**: Use "Strike" or "Duel" on a character; Use "Cancel" to counteract "Expressing friendly".  
* **Showing friendly**: i.e. to show that you are a Minister-Pig through your actions. The Showing friendly action is Expressing friendly to the Monarch-Pig or to a pig that has Shown friendly. Or Express hostility to a pig that has Shown hostility.
* **Showing hostility**: i.e. to show that you are a Rebel-Pig through your actions. The Showing hostility action is Expressing hostility to the Monarch-Pig or to a pig that has Shown friendly. Or Express friendly to a pig that has Shown hostility. 

**NOTE: The Minister-Pigs won't Showing hostility, and the Rebel-Pigs won't Showing friendly; Whether you are a Minister-Pig or a Rebel-Pig, if you can show, you must show.**

### The code of conduct

#### Commonalities

* For each character, if he has a "Peach" in his hand and his HP isn't full, he will definitely eat it; 
* If you have "Barbarianpig Invasion" or "Arrow Barrage", you must use it; If there is an equipment, you must equip it; 
* When you are attacked by "Strike", if you have "Dodge", you must discard it; 
* When you respond to the "Barbarianpig Invasion" or "Arrow Barrage"， if you have "Strike" / "Dodge", you must discard it; 
* A pig won't Show friendly to an unidentified pig (including itself). 

#### Characteristics

 - Monarch-Pig: 
   + 主猪会认为「没有跳身份，且用南猪入侵 / 万箭齐发对自己造成伤害的猪」是**类**反猪（没伤害到不算，注意类反猪并没有表明身份），如果之后跳了，那么主猪会重新认识这只猪；  
   + 对于每种表敌意的方式，对逆时针方向能够执行到的第一只类反猪或者已跳反猪表；如果没有，那么就不表敌意；
   + 决斗时会不遗余力弃置杀；
   + 如果能对已经跳忠的猪或自己献殷勤，那么一定献；如果能够对已经跳反的猪表敌意，那么一定表。
 - Minister-Pigs:
   + 对于每种表敌意的方式，对「逆时针方向能够执行到的第一只已经跳反的猪」表，如果没有，那么就不表敌意；
   + 决斗时，如果对方是主猪，那么不会弃置杀，否则，会不遗余力弃置杀；
   + 如果有机会对主猪或者已经跳忠的猪献殷勤，那么一定献。
 - Rebel-Pigs:
   + 对于每种表敌意的方式，如果有机会则对主猪表，否则，对「逆时针方向能够执行到的第一只已经跳忠的猪」表，如果没有，那么就不表敌意；
   + 决斗时会不遗余力弃置杀；
   + 如果有机会对已经跳反的猪献殷勤，那么一定献。

限于 iPig 只会用 P++ 语言写 A + B，他请你用 Pigcal (Pascal)、P (C) 或 P++ (C++) 语言来帮他预测最后的结果。

## 输入格式

输入文件第一行包含两个正整数 $ n $ $ (2 \leqslant n \leqslant 10) $ 和 $m$ $ (m \leqslant 2000) $，分别代表玩家数和牌堆中牌的数量。数据保证牌的数量够用。

接下来 $n$ 行，每行 $5$ 个字符串，依次表示对第 $i$ 只猪的角色和初始 $4 $ 张手牌描述。编号为 $1$ 的肯定是主猪。

再接下来一行，一共 $m$ 个字符串，按照从牌堆顶部到牌堆底部的顺序描述每张牌。

**注意：所有的相邻的两个字符串都严格用 $1$ 个空格隔开，行尾没有多余空格**。

## 输出格式

输出数据第一行包含一个字符串代表游戏结果。如果是主猪胜利，那么输出 $\texttt{MP}$ ，否则输出 $\texttt{FP}$ 。数据保证游戏总会结束。

接下来 $n$ 行，第 $i$ 行是对第 $i$ 只猪的手牌描述（注意只需要输出手牌），按照手牌从左往右的顺序输出，相邻两张牌用 $1$ 个空格隔开，行末尾没有多余空格。如果这只猪已阵亡，那么只要输出 $\texttt{DEAD}$ 即可。

**注意：如果要输出手牌而没有手牌的话，那么只需输出 $1$ 个空行**。

**由于数据问题，若牌堆已空，按照每次抽牌抽到的都是最后一张。**

## 样例 #1

### 样例输入 #1

```
3 10
MP D D F F
ZP N N N D
FP J J J J
F F D D J J F F K D
```

### 样例输出 #1

```
FP
DEAD
DEAD
J J J J J J D
```

## 提示

### 样例解释

第一回合：
* 主猪没有目标可以表敌意；
* 接下来忠猪使用了 $3$ 张南猪入侵，主猪掉了 $3$ 点体力，并认为该角色为类反猪，$3$ 号角色尽管手里有无懈可击，但是因为自己未表明身份，所以同样不能对自己用，乖乖掉 $3$ 点体力；  

下一回合：
* 反猪无牌可出；
* 接下来主猪对着类反猪爆发，使用 $4$ 张决斗，忠猪死亡，结果主猪弃掉所有牌；
* 下来反猪摸到 $1$ 张杀直接杀死主猪获胜。

### 子任务

一共 $20$ 组测试数据，每个点 $5$ 分。

$10\%$ 的数据没有锦囊牌，另外 $20\%$ 的数据没有无懈可击。

---

# 思路

对于这~~又长又臭的~~题面，我们需要先仔细地阅读理解，列出有用信息。

## 1：变量/初始化/每只猪 `struct` 的定义

### 变量与常量

```cpp
const int N = 15, M = 2005;
enum PIG_TYPE
{
	UKE, MP, ZP, FP 
};
int n, m;
int top = 1;	//牌堆顶
char cards[M];	//牌堆
bool gameover;	//游戏是否结束
```

我们使用一个枚举型来表示猪的种类。其值对应如下：

| 名称     | UKE        | MP   | ZP   | FP   |
| -------- | ---------- | ---- | ---- | ---- |
| **数值** | 0          | 1    | 2    | 3    |
| **解释** | 未表明身份 | 主猪 | 忠猪 | 反猪 |

### 每只猪的 `struct`

```cpp
struct Pig
{
	int id,	eid;	//身份, 暴露的身份
	bool like_v;	//是否为类反猪 
	int hp;			//生命值 
	bool weapon;	//是否装备武器 
	int cnt;		//牌数 
	char card[M];	//牌堆
	int pre, nxt;	//前面的猪和后面的猪的位置 
	Pig()			//构造函数, 初始化牌堆, 以及设置初值
	{
		for (int i = 1; i < M; i++)
			card[j] = 0;
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
void printcard(int x)	//打印第x只猪的手牌
{
	for (int i = 1; i <= pig[x].cnt; i++)
		if (pig[x].card[i])
			printf("%c ", pig[x].card[i]);
	puts("");
}
```

### 找牌

```cpp
bool findcard(int x, char card)
{
	for (int i = 1; i <= pig[x].cnt; i++)
		if (pig[x].card[i] == card)
		{
			pig[x].card[i] = 0;
			return true;
		}
	return false;
}
```

若找到对应卡牌，该函数会返回 `true`，并弃置这张卡牌；否则返回 `false`。

### 摸牌

```cpp
void getcard(int x, int times = 1)
{
	while (times--)
	{
		top = min(top, m);
		pig[x].card[++pig[x].cnt] = cards[top++];
	}
}
```

**注意：在洛谷中，由于数据问题，若牌堆已空，按照每次抽牌抽到的都是最后一张。**

## 3：扣血与死亡判定

在定义猪的结构体中，我们通过 `nxt` 和 `pre`，即以循环链表的形式来实现环的运行。

```cpp
void hurt(int from, int to)
{
	pig[to].hp--;
	while (pig[to].hp < 1 && findcard(to, 'P'))
		pig[to].hp++;
	if (pig[to].hp <= 0)
	{
		pig[to].cnt = 0, pig[to].weapon = false;	//清空物品
		switch (pig[to].id)
		{
			case MP:
			{
				gameover = true;
				return;
			}
			case ZP:
			{	//若主猪杀死忠猪, 则清空主猪手牌 
				if (pig[from].id == MP)
					pig[from].cnt = 0, pig[from].weapon = false;
				break;
			}
			case FP:
			{
				if(!--fpcnt)
				{
					gameover = true;
					return;
				}
				//杀死反猪者摸牌	 
				for (int T = 1; T <= 3; T++)
					getcard(from);
				break;
			}
		}
		pig[pig[to].pre].nxt = pig[to].nxt, pig[pig[to].nxt].pre = pig[to].pre;	//更新链表 
	}
}
```

## 4：猪之间的关系

该部分实现了四个函数。它们的功能如下：

- `bool is_enemy(int x, int y)`：判断 $x$ 和 $y$ 是否为敌人关系；
- `bool is_friend(int x, int y)`：判断 $x$ 和 $y$ 是否为朋友关系；
- `void getenemy(int x, int y)`：$x$ 向 $y$「表敌意」；

- `void getfriend(int x, int y)`：$x$ 向 $y$​「献殷勤」。

注意：前两个函数判断的是两只猪的真实身份（即 `id`），而后者所改变的是暴露的身份（即 `eid`）。

```cpp
bool is_enemy(int x, int y)
{
	if (pig[x].id == MP && pig[y].like_v) return true;	//主猪与类反猪是敌人 
	else if (!pig[y].eid) return false;					//未显露身份, 不是敌人 
	//主猪、忠猪与反猪是敌人 
	else if (pig[x].id != FP && pig[y].eid == FP || pig[x].id == FP && pig[y].eid != FP)
		return true;
	return false;
}
bool is_friend(int x, int y)
{
	if (!pig[y].eid) return false;
	//同类是朋友; 主猪与忠猪是朋友 
	else if (pig[x].id != FP && pig[y].eid != FP || pig[x].id == FP && pig[y].eid == FP)
		return true;
	return false; 
}
void getenemy(int x, int y)
{
	//不应对未亮身份的猪操作
	if (!pig[y].eid) return;
	if (pig[y].eid != FP) pig[x].like_v = true, pig[x].eid = FP;
	else pig[x].like_v = false, pig[x].eid = ZP;
}
void getfriend(int x, int y)
{ 
	if (!pig[y].eid) return;
	if (pig[y].eid != FP) pig[x].like_v = false, pig[x].eid = ZP;
	else pig[x].like_v = true, pig[x].eid = FP;
}
```

## 5：使用卡牌

### Ⅰ. 杀/闪

按题意模拟即可。

```cpp
void useK(int from, int to)
{
	getenemy(from, to);
	if (!findcard(to, 'D'))
		hurt(from, to);
}
```

### Ⅱ. 决斗

此时的「决斗」函数当且仅当未被「无懈可击」挡住、决斗生效时。

```cpp
void useF(int from, int to)
{
	if (pig[from].id == MP && pig[to].id == ZP)	//忠猪不会对主猪弃置杀 
	{
		hurt(from, to);
		return;
	}
	while (true)
	{
		if (!findcard(to, 'K'))   {hurt(from, to); return;}
		if (!findcard(from, 'K')) {hurt(to, from); return;}
	}
}
```

### Ⅲ. 无懈可击

理解题意，可得：

- 若朋友对自己发出锦囊牌，则使用「无懈可击」挡住，同时「献殷勤」；
- 若敌人使用了「无懈可击」，则使用「无懈可击」使敌人的「无懈可击」无效，同时「表敌意」。

```CPP
bool useJ(int from, int to, int last)	//此处last表示第一次使用无懈可击, 而不是未表明身份 
{
	int now = from;
	while (true)
	{
		if (!last && is_friend(now, to) || last && is_enemy(now, last))
			if(findcard(now, 'J'))
			{
				if (!last && is_friend(now, to))  getfriend(now, to);
				if ( last && is_enemy (now, last)) getenemy(now, last);
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
void useN(int from, int to)
{
	while (true)
	{
		if (gameover) return;	//一旦达成胜利条件, 游戏立刻结束
		if (!useJ(from, to, 0))
			if (!findcard(to, 'K'))	//不能响应则扣血
			{
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
void useW(int from, int to)
{
	while (true)
	{
		if (gameover) return;
		if (!useJ(from, to, 0))
			if (!findcard(to, 'D'))
			{
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

所有功能都已实现，接下来只需按题意模拟即可。

要点：

- 使用某些手牌后可能改变局面，所以使用手牌后要从头扫描手牌；
- 不论如何，一旦达成胜利条件, 游戏立刻结束；

```cpp
void rungame()
{
while (true)
{
	for (int i = 1; i <= n; i++)
	{
		if(gameover) return;
		if (pig[i].hp <= 0) continue;
		getcard(i, 2);
		bool flag = false;	//是否出过杀
		for (int j = 1; j <= pig[i].cnt; j++)
		{
			if (gameover || !pig[i].card[j]) continue;
			switch (pig[i].card[j])
			{
				case 'P':
				{
					if (pig[i].hp < 4)
						pig[i].card[j] = 0, pig[i].hp++;
					break;
				}
				case 'K':
				{
					if (!flag && is_enemy(i, pig[i].nxt))
					{
						pig[i].card[j] = 0;
						useK(i, pig[i].nxt);
						if (!pig[i].weapon) flag = true;
						j = 0;
					}
					break;
				}
				case 'F':
				{
					if (pig[i].id == FP)
					{
						pig[i].card[j] = 0;
						getenemy(i, MP);
						if (useJ(i, 1, 0)) break;
						useF(i, 1);
						j = 0;
					}
					else
					{
						int to = pig[i].nxt;
						while (true)
						{
							if (is_enemy(i, to))
							{
								pig[i].card[j] = 0;
								getenemy(i, to);
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
				}
				case 'N':
				{
					pig[i].card[j] = 0;
					useN(i, pig[i].nxt);
					j = 0;
					break;
				}
				case 'W':
				{
					pig[i].card[j] = 0;
					useW(i, pig[i].nxt);
					j = 0;
					break;
				}
				case 'Z':
				{
					pig[i].card[j] = 0;
					pig[i].weapon = true, flag = false;
					j = 0;
					break;
				}
			}
		}
	}
}}
int main()
{
	scanf("%d%d", &n, &m);
	for (int i = 1; i <= n; i++)
	{
		std :: string name; char ch;
		std :: cin >> name;
		switch (name[0])
		{
			case 'M': pig[i].id = MP, pig[i].eid = MP; break;
			case 'Z': pig[i].id = ZP; break; 
			case 'F': pig[i].id = FP, fpcnt++; break;
		}
		for (pig[i].cnt = 1; pig[i].cnt <= 4; pig[i].cnt++)
			std :: cin >> ch,
			pig[i].card[pig[i].cnt] = ch;
		pig[i].nxt = i + 1, pig[i].pre = i - 1;
	}
	pig[1].pre = n, pig[n].nxt = 1;
	for (int i = 1; i <= m; i++)
	{
		char ch;
		std :: cin >> ch;
		cards[i] = ch; 
	}
	rungame();
	puts(pig[MP].hp <= 0 ? "FP" : "MP");
	for (int i = 1; i <= n; i++)
		if (pig[i].hp <= 0) puts("DEAD");
		else printcard(i);
	return 0;
}
```

# 完整代码

```cpp
#include <iostream>

#include <cstdio>

#include <string>


const int N = 15, M = 2005;
enum PIG_TYPE
{
	UKE, MP, ZP, FP 
};
struct Pig
{
	int id,	eid;
	bool like_v;
	int hp;
	bool weapon;
	int cnt;
	char card[M];
	int pre, nxt;
	Pig()
	{
		for (int i = 1; i < M; i++)
			card[i] = 0;
		hp = 4;
	}
}pig[N];
int n, m;
int top = 1;
char cards[M];
bool gameover;
int fpcnt;

void printcard(int);
bool findcard (int, char);
void getcard  (int, int);
void hurted   (int, int); 
bool is_enemy (int, int);
bool is_friend(int, int);
void getfriend(int, int);
void getenemy (int, int);
void useK	  (int, int);
void useF     (int, int);
bool useJ	  (int, int, int);
void useN	  (int, int);
void useW	  (int, int);
void rungame  ();

void printcard(int x)
{
	for (int i = 1; i <= pig[x].cnt; i++)
		if (pig[x].card[i])
			printf("%c ", pig[x].card[i]);
	puts("");
}
bool findcard(int x, char card)
{
	for (int i = 1; i <= pig[x].cnt; i++)
		if (pig[x].card[i] == card)
		{
			pig[x].card[i] = 0;
			return true;
		}
	return false;
}
void getcard(int x, int times = 1)
{
	while (times--)
	{
		top = std :: min(top, m);
		pig[x].card[++pig[x].cnt] = cards[top++];
	}
}
void hurt(int from, int to) 
{
	pig[to].hp--;
	while (pig[to].hp < 1 && findcard(to, 'P'))
		pig[to].hp++;
	if (pig[to].hp <= 0)
	{
		pig[to].cnt = 0, pig[to].weapon = false;
		switch (pig[to].id)
		{
			case MP:
			{
				gameover = true;
				return;
			}
			case ZP:
			{
				if (pig[from].id == MP)
					pig[from].cnt = 0, pig[from].weapon = false;
				break;
			}
			case FP:
			{
				if(!--fpcnt)
				{
					gameover = true;
					return;
				}	 
				getcard(from, 3);
				break;
			}
		}
		pig[pig[to].pre].nxt = pig[to].nxt, pig[pig[to].nxt].pre = pig[to].pre;
	}
}
bool is_enemy(int x, int y)
{
	if (pig[x].id == MP && pig[y].like_v) return true; 
	else if (!pig[y].eid) return false;
	else if (pig[x].id != FP && pig[y].eid == FP || pig[x].id == FP && pig[y].eid != FP)
		return true;
	return false;
}
bool is_friend(int x, int y)
{
	if (!pig[y].eid) return false;
	else if (pig[x].id != FP && pig[y].eid != FP || pig[x].id == FP && pig[y].eid == FP)
		return true;
	return false; 
}
void getenemy(int x, int y)
{
	if (!pig[y].eid) return;
	if (pig[y].eid != FP) pig[x].like_v = true, pig[x].eid = FP;
	else pig[x].like_v = false, pig[x].eid = ZP;
}
void getfriend(int x, int y)
{ 
	if (!pig[y].eid) return;
	if (pig[y].eid != FP) pig[x].like_v = false, pig[x].eid = ZP;
	else pig[x].like_v = true, pig[x].eid = FP;
}
void useK(int from, int to)
{
	getenemy(from, to);
	if (!findcard(to, 'D'))
		hurt(from, to);
}
void useF(int from, int to)
{
	if (pig[from].id == MP && pig[to].id == ZP)
	{
		hurt(from, to);
		return;
	}
	while (true)
	{
		if (!findcard(to, 'K'))   {hurt(from, to); return;}
		if (!findcard(from, 'K')) {hurt(to, from); return;}
	}
}
bool useJ(int from, int to, int last)
{
	int now = from;
	while (true)
	{
		if (!last && is_friend(now, to) || last && is_enemy(now, last))
			if(findcard(now, 'J'))
			{
				if (!last && is_friend(now, to))  getfriend(now, to);
				if ( last && is_enemy (now, last)) getenemy(now, last);
				if (!useJ(pig[now].nxt, to, now))
					return true;
			} 
		if ((now = pig[now].nxt) == from)
			return false;	 
	}
}
void useN(int from, int to)
{
	while (true)
	{
		if (gameover) return;
		if (!useJ(from, to, 0))
			if (!findcard(to, 'K'))
			{
				if (pig[to].id == MP && !pig[from].eid)
					pig[from].like_v = true;
				hurt(from, to); 
			}
		if ((to = pig[to].nxt) == from)
			return;
	}
}
void useW(int from, int to)
{
	while (true)
	{
		if (gameover) return;
		if (!useJ(from, to, 0))
			if (!findcard(to, 'D'))
			{
				if (pig[to].id == MP && !pig[from].eid)
					pig[from].like_v = true;
				hurt(from, to); 
			}
		if ((to = pig[to].nxt) == from)
			return;
	}
}
void rungame()
{
while (true)
{
	for (int i = 1; i <= n; i++)
	{
		if(gameover) return;
		if (pig[i].hp <= 0) continue;
		getcard(i, 2);
		bool flag = false;
		for (int j = 1; j <= pig[i].cnt; j++)
		{
			if (gameover || !pig[i].card[j]) continue;
			switch (pig[i].card[j])
			{
				case 'P':
				{
					if (pig[i].hp < 4)
						pig[i].card[j] = 0, pig[i].hp++;
					break;
				}
				case 'K':
				{
					if (!flag && is_enemy(i, pig[i].nxt))
					{
						pig[i].card[j] = 0;
						useK(i, pig[i].nxt);
						if (!pig[i].weapon) flag = true;
						j = 0;
					}
					break;
				}
				case 'F':
				{
					if (pig[i].id == FP)
					{
						pig[i].card[j] = 0;
						getenemy(i, MP);
						if (useJ(i, 1, 0)) break;
						useF(i, 1);
						j = 0;
					}
					else
					{
						int to = pig[i].nxt;
						while (true)
						{
							if (is_enemy(i, to))
							{
								pig[i].card[j] = 0;
								getenemy(i, to);
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
				}
				case 'N':
				{
					pig[i].card[j] = 0;
					useN(i, pig[i].nxt);
					j = 0;
					break;
				}
				case 'W':
				{
					pig[i].card[j] = 0;
					useW(i, pig[i].nxt);
					j = 0;
					break;
				}
				case 'Z':
				{
					pig[i].card[j] = 0;
					pig[i].weapon = true, flag = false;
					j = 0;
					break;
				}
			}
		}
	}
}}
int main()
{
	scanf("%d%d", &n, &m);
	for (int i = 1; i <= n; i++)
	{
		std :: string name; char ch;
		std :: cin >> name;
		switch (name[0])
		{
			case 'M': pig[i].id = MP, pig[i].eid = MP; break;
			case 'Z': pig[i].id = ZP; break; 
			case 'F': pig[i].id = FP, fpcnt++; break;
		}
		for (pig[i].cnt = 1; pig[i].cnt <= 4; pig[i].cnt++)
			std :: cin >> ch,
			pig[i].card[pig[i].cnt] = ch;
		pig[i].nxt = i + 1, pig[i].pre = i - 1;
	}
	pig[1].pre = n, pig[n].nxt = 1;
	for (int i = 1; i <= m; i++)
	{
		char ch;
		std :: cin >> ch;
		cards[i] = ch; 
	}
	rungame();
	puts(pig[MP].hp <= 0 ? "FP" : "MP");
	for (int i = 1; i <= n; i++)
		if (pig[i].hp <= 0) puts("DEAD");
		else printcard(i);
	return 0;
}
```

# 总结

非常好大模拟，这使我的大脑旋转。
