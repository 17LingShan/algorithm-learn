# BFS 与 DFS

## DFS(深度优先搜索,Deep First Search)

深度优先搜索的步骤分为：

1. 向下递归。
2. 向上回溯。

深度优先就算一条路走到底,直达目标。

## 例题一

以 acwing<a href="https://www.acwing.com/problem/content/844/">842.排列数字</a>为例。

```c++
/*
分析：
本题是将1~n中所有的数全排列。
我们用got来存储目前是否走过这个数。path来存储数的路径。
*/


#include <bits/stdc++.h>

using namespace std;

const int N = 8;

int n, path[N];
bool got[N];

void dfs(int u)
{
    // 当走到最后一层时，输出path中的数。
    if (u == n)
    {
        for (int i = 0; i < n; ++ i) printf("%d ",  path[i]);
        cout << endl;
    }

    // 循环1~n的数。
    for (int i = 1; i <= n; ++ i)
    {
        // 找出没有走过的数，将其标记在got中，并进入下一层
        if (!got[i])
        {
            path[u] = i;
            got[i] = true;
            dfs(u + 1);	//进入下一层
            got[i] = false; //还原现场
        }
    }
}

int main(void)
{
    cin >> n;

    dfs(0);

    return 0;
}
```

## 例题二

以acwing<a href="https://www.acwing.com/problem/content/845/">843.n-皇后问题</a>为例。

```c++
/*
本题我们将标记三个地方：行col、对角线dg、反对角线udg。
对于dg与udg，加上偏移量使得其范围永远大于零
*/

#include <bits/stdc++.h>

using namespace std;

const int N = 20;

int n;
char q[N][N];
bool col[N], dg[N], udg[N];

void dfs(int u)
{
    if (u == n) 
    {
        for (int i = 0; i < n; ++ i) printf("%s\n", q[i]);
        cout << endl;
        return;
    }
    
    for (int i = 0; i < n; ++ i)
    {
        // 同列、对角线反对角线都没有放置
        if (!col[i] && !dg[u + i] && !udg[n - u + i])
        {
            q[u][i] = 'Q';
            col[i] = dg[u + i] = udg[n - u + i] = true;
            dfs(u + 1);
            q[u][i] = '.';
            col[i] = dg[u + i] = udg[n - u + i] = false; //恢复现场
        }
    }
}

int main(void)
{
    cin >> n;
    
    for (int i = 0; i < n; ++ i)
        for (int j = 0; j < n; ++ j)
            q[i][j] = '.';
    
    dfs(0);
    return 0;
}
```



