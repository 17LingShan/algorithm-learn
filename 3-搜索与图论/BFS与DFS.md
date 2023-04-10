# BFS 与 DFS

## DFS（深度优先搜索，Deep First Search）

深度优先搜索的步骤分为：

1. 向下递归。
2. 向上回溯。

深度优先就算一条路走到底,直达目标。

### 例题一

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

### 例题二

以 acwing<a href="https://www.acwing.com/problem/content/845/">843.n-皇后问题</a>为例。

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

## BFS（广度优先搜索，Breadth First Search）

BFS 与 DFS 不同的地方在于，DFS 是将一条路走到底，而 BFS 则是每到一个路口，便将可以走的路口先记录下来，然后再前进。

### 例题三

以 acwing <a href="https://www.acwing.com/problem/content/846/">844.走迷宫</a>为例。

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 110;

int grid[N][N], d[N][N];    //grid代表数组可走与不可走的路的标记，d代表可走路与起点的距离
int n, m;
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1}; //代表左下右上四个方向的向量

pair<int, int> q[N * N];    //一个队列，最长为110 * 110，保存的是要搜索的坐标

int bfs(void)
{
    int head = 0, tail = 0;    //head队头，tail队尾
    q[0] = {0, 0}; //将起点(0,0)加入

    memset(d, -1, sizeof d); // 初始化所有点距离起点为-1。方便分辨是否走过该点
    d[0][0] = 0; //起点的距离为0

    // 队列为空时才结束搜索
    while (head <= tail)
    {
        //取出队头后将队头向前移动一位
        auto current = q[head ++];

        for (int i = 0; i < 4; ++ i)
        {
            //当前要偏移的点
            int x = current.first + dx[i], y = current.second + dy[i];

            // 限定要搜索的是在数组范围内，并且是可走路0，并且是没有走过的路-1
            if (x >= 0 && x < n && y >= 0 && y < m && grid[x][y] == 0 && d[x][y] == -1)
            {
                // 偏移点的距离为目前的点的距离+1
                d[x][y] = d[current.first][current.second] + 1;
                q[++ tail] = {x, y};    //将偏移点的位置加入队列中，待搜索
            }
        }
    }

	//返回终点对于起点的距离
    return d[n - 1][m - 1];
}

int main(void)
{
    cin >> n >> m;

    for (int i = 0; i < n; ++ i)
        for (int j = 0; j < m; ++ j)
         cin >> grid[i][j];

    cout << bfs();

    return 0;
}


```
