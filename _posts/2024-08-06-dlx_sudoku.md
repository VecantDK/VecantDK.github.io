---
layout:     post
title:      「leetcode」37. 解数独【舞蹈链算法】解题报告
subtitle:   LeetCode
date:       2024-08-06
author:     VecantDK
header-img: img/post-bg-coffee.jpeg
catalog: 	 true
tags:
    - OI
    - 题解
    - 学习笔记

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

# 声明

本文章中所有内容仅供学习交流使用，不用于其他任何目的。部分有关舞蹈链的内容内容摘自 [Dancing Links - OI Wiki](https://oi-wiki.org/search/dlx/)

---

来看这样一道题：

[37. 解数独 - Leetcode](https://leetcode.cn/problems/sudoku-solver/)

编写一个程序，通过填充空格来解决数独问题。

数独的解法需 **遵循如下规则**：

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1：**

![INPUT](https://vecantdk.github.io/img/Sudoku_1.png)

> **输入**：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
> **输出**：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
> **解释**：输入的数独如上图所示，唯一有效的解决方案如下所示：

![OUTPUT](https://vecantdk.github.io/img/Sudoku_2.png)

**提示：**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` 是一位数字或者 `'.'`
- 题目数据 **保证** 输入数独仅有一个解

---

# 回溯法

看到这道题目，你应该会首先想到回溯法。不过，这道题的数据大小是 $9\times 9=81$，这意味着你最多需要执行深度为 $81$ 的搜索。显然，这是会超时的。所以，我们需要进行剪枝。

- 要填写的数需要保证：要填写的格子的行、列、九宫格中都没有与之相等的数；这个数的出现次数 $<9$​；
- 每次都去填写状态数最小的格子，即这个格子所在的行、列上已经填写的格子数量最多；

凭借以上思路，我们容易写出回溯法的代码：

```cpp
class Solution {
    int bd[10][10], ans[10][10];
    bool row[10][10], col[10][10], squ[10][10];
    int row_cnt[10], col_cnt[10];
    int visnum[10];
    int get_id(int x, int y)
    {
        return (x / 3) * 3 + y / 3 + 1;
    }
    void dfs(int r, int c, int dep)
    {
        if (dep == 81)
        {
            for (int i = 0; i < 9; i++)
                for (int j = 0; j < 9; j++)
                    ans[i][j] = bd[i][j];
            return;
        }
        for (int k = 1; k <= 9; k++)
        {
            if (visnum[k] == 9) continue;
            if (row[r][k] || col[c][k] || squ[get_id(r, c)][k]) continue;
            bd[r][c] = k, visnum[k]++;
            row[r][k] = col[c][k] = squ[get_id(r, c)][k] = true;
            row_cnt[r]++, col_cnt[c]++;
            int tmpr = -1, nxt_r = -1, tmpc = -1, nxt_c = -1;
            for (int i = 0; i < 9; i++)
                if (row_cnt[i] > tmpr && row_cnt[i] < 9)
                    tmpr = row_cnt[i], nxt_r = i;
            for (int i = 0; i < 9; i++)
                if (col_cnt[i] > tmpc && col_cnt[i] < 9 && !bd[nxt_r][i])
                    tmpc = col_cnt[i], nxt_c = i;
            dfs(nxt_r, nxt_c, dep + 1);
            bd[r][c] = 0, visnum[k]--;
            row[r][k] = col[c][k] = squ[get_id(r, c)][k] = false;
            row_cnt[r]--, col_cnt[c]--;
        }
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        int cnt = 0;
        for (int i = 0; i < 9; i++)
            for (int j = 0; j < 9; j++)
                if (board[i][j] >= '1' && board[i][j] <= '9')
                {
                    bd[i][j] = (int)(board[i][j] ^ 48);
                    row[i][bd[i][j]] = col[j][bd[i][j]] = squ[get_id(i, j)][bd[i][j]] = true;
				    row_cnt[i]++,col_cnt[j]++;
				    cnt++;
                }
    	int tmpr = -1, nxt_r = -1, tmpc = -1, nxt_c = -1;
        for (int i = 0; i < 9; i++)
            if (row_cnt[i] > tmpr && row_cnt[i] < 9)
                tmpr = row_cnt[i], nxt_r = i;
        for (int i = 0; i < 9; i++)
            if (col_cnt[i] > tmpc && col_cnt[i] < 9 && !bd[nxt_r][i])
                tmpc = col_cnt[i], nxt_c = i;
        dfs(nxt_r, nxt_c, cnt);
        for (int i = 0; i < 9; i++)
            for (int j = 0; j < 9; j++)
                if (board[i][j] < '1' || board[i][j] > '9')
                    board[i][j] = (char)(ans[i][j] ^ 48);
    }
};
```

提交这段代码，它已经AC了。

![DFS_AC](https://vecantdk.github.io/img/Sudoku_3.png)

但是，这已经是最快了吗？

> 我们写代码不是为了AC，而是为了获得经验。
>
> ——忘记谁说的了

# X 算法

### 精确覆盖问题

**精确覆盖问题** 的定义：

给定一个 0-1 矩阵，你可以选择一些行，使得最终每列都恰好有一个 1。

例如这个矩阵：
$$
\begin{eqnarray}
\begin{pmatrix}
0 & 0 & 1 & 0 & 1 & 1 & 0 \\
1 & 0 & 0 & 1 & 0 & 0 & 1 \\
0 & 1 & 1 & 0 & 0 & 1 & 0 \\
1 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 & 0 & 1 \\
0 & 0 & 0 & 1 & 1 & 0 & 1 
\end{pmatrix}
\end{eqnarray}
$$

### X 算法

Donald E. Knuth 提出了 X 算法 (Algorithm X)。它的过程如下：

1. 选中第一行，打上红色标记。此时第一行有 $3$ 个 $1$。分别是第 $3$ 列，第 $5$ 列，第 $6$​ 列，将这三列全部打上蓝色标记。
   **注意：每一次不一定要选择第一行，这一点在下文会体现。**

$$
\begin{eqnarray}
\begin{pmatrix}
\textcolor{red}0 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}1 & \textcolor{red}0 \\
1 & 0 & \textcolor{blue}0 & 1 & \textcolor{blue}0 & \textcolor{blue}0 & 1 \\
0 & 1 & \textcolor{blue}1 & 0 & \textcolor{blue}0 & \textcolor{blue}1 & 0 \\
1 & 0 & \textcolor{blue}0 & 1 & \textcolor{blue}0 & \textcolor{blue}0 & 0 \\
0 & 1 & \textcolor{blue}0 & 0 & \textcolor{blue}0 & \textcolor{blue}0 & 1 \\
0 & 0 & \textcolor{blue}0 & 1 & \textcolor{blue}1 & \textcolor{blue}0 & 1 
\end{pmatrix}
\end{eqnarray}
$$

2. 选中所有带有蓝色标记的列，将含有 $1$​ 的行全部打上紫色标记。

$$
\begin{eqnarray}
\begin{pmatrix}
\textcolor{red}0 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}1 & \textcolor{red}0 \\
1 & 0 & \textcolor{blue}0 & 1 & \textcolor{blue}0 & \textcolor{blue}0 & 1 \\
\textcolor{purple}0 & \textcolor{purple}1 & \textcolor{blue}1 & \textcolor{purple}0 & \textcolor{blue}0 & \textcolor{blue}1 & \textcolor{purple}0 \\
1 & 0 & \textcolor{blue}0 & 1 & \textcolor{blue}0 & \textcolor{blue}0 & 0 \\
0 & 1 & \textcolor{blue}0 & 0 & \textcolor{blue}0 & \textcolor{blue}0 & 1 \\
\textcolor{purple}0 & \textcolor{purple}0 & \textcolor{blue}0 & \textcolor{purple}1 & \textcolor{blue}1 & \textcolor{blue}0 & \textcolor{purple}1 
\end{pmatrix}
\end{eqnarray}
$$

3. 选择所有被标记的**行**，将它们删除。

**这表示这一行已被选择，且这一行的所有 $1$ 所在的列不能有其他 $1$ 了。**

于是得到一个新的小 0-1 矩阵：
$$
\begin{eqnarray}
\begin{pmatrix}
1 & 0 & 1 & 1 \\
1 & 0 & 1 & 0 \\
0 & 1 & 0 & 1
\end{pmatrix}
\end{eqnarray}
$$

4. 选中此时的第 $1$ 行（原来的第 $2$ 行），打上红色标记。此时第一行有 $3$ 个 $1$。分别是 第 $1$ 列，第 $3$ 列，第 $4$ 列，将这三列全部打上蓝色标记。

$$
\begin{eqnarray}
\begin{pmatrix}
\textcolor{red}1 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}1 \\
\textcolor{blue}1 & 0 & \textcolor{blue}1 & \textcolor{blue}0 \\
\textcolor{blue}0 & 1 & \textcolor{blue}0 & \textcolor{blue}1
\end{pmatrix}
\end{eqnarray}
$$

5. 选中所有带有蓝色标记的列，将含有 $1$ 的行全部打上紫色标记。

$$
\begin{eqnarray}
\begin{pmatrix}
\textcolor{red}1 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}1 \\
\textcolor{blue}1 & \textcolor{purple}0 & \textcolor{blue}1 & \textcolor{blue}0 \\
\textcolor{blue}0 & \textcolor{purple}1 & \textcolor{blue}0 & \textcolor{blue}1
\end{pmatrix}
\end{eqnarray}
$$

6. 选择所有被标记的行，将它们删除。

$$
\begin{eqnarray}
\begin{pmatrix}
\end{pmatrix}
\end{eqnarray}
$$

这样就得到了一个空矩阵。但是上次删除的行 `1 0 1 1` 不是全 ![1](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7) 的，说明选择有误。

7. 回溯到步骤 4，考虑选择第二行（原来的第四行），打上红色标记。此时第一行有 $2$ 个 $1$。分别是 第 $1$ 列，第 $3$ 列，将这两列全部打上蓝色标记。

$$
\begin{eqnarray}
\begin{pmatrix}
\textcolor{blue}1 & 0 & \textcolor{blue}1 & 1 \\
\textcolor{red}1 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}0 \\
\textcolor{blue}0 & 1 & \textcolor{blue}0 & 1
\end{pmatrix}
\end{eqnarray}
$$

8. 选中所有带有蓝色标记的列，将含有 $1$ 的行全部打上紫色标记。

$$
\begin{eqnarray}
\begin{pmatrix}
\textcolor{blue}1 & \textcolor{purple}0 & \textcolor{blue}1 & \textcolor{purple}1 \\
\textcolor{red}1 & \textcolor{red}0 & \textcolor{red}1 & \textcolor{red}0 \\
\textcolor{blue}0 & 1 & \textcolor{blue}0 & 1
\end{pmatrix}
\end{eqnarray}
$$

9. 选择所有被标记的行，将它们删除。

于是得到一个这样的一个矩阵：
$$
\begin{eqnarray}
\begin{pmatrix}
1 & 1
\end{pmatrix}
\end{eqnarray}
$$

10. 选中此时的第 $1$ 行（原来的第 $5$ 行），将其全部删除，得到一个空矩阵：

$$
\begin{eqnarray}
\begin{pmatrix}
\end{pmatrix}
\end{eqnarray}
$$

11. 上一次删除时，删除的是全 $1$​ 的行，因此成功，算法结束。
    答案即为被删除的三行 $1,4,5$​​。

---

通过上述步骤，可将 X 算法的流程概括如下：

1. 从矩阵中选择一行；如果选择所有行都无解，输出无解信息，停机；
2. 根据定义，标记与其相关的行与列；
3. 删除所有标记的行与列，得到新矩阵；
4. 如果新矩阵为空，且选择的行全为 $1$，则求解结束，输出所有被删除的行；
   如果新矩阵为空，且选择的行不全为 $1$，则恢复与这行相关的行与列，跳转至步骤 1；
   如果新矩阵不为空，则跳转至步骤 1。

从上面可以看出，求解过程中有大量的「删除行」、「删除列」和「恢复行」、「恢复列」的操作。

于是 Donald E. Knuth 想到用双向十字链表来维护这些操作。而在双向十字链表上不断跳跃的过程被形象地比喻成「跳跃」，因此被用来优化 X 算法的双向十字链表也被称为「Dancing Links」。
