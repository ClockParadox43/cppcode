### 843. n-皇后问题 
#### 题目链接
>   https://www.acwing.com/problem/content/845/
#### 题目分析
>  因为**一条线上的对角线上的通过公式计算出的值都相等**，所以**一条对角线上的值是通用的，** 所以可以将值作为下标, 只要 格子的 `dg[i]/udg[i]` 为 1 (被记录过了)
> **上对角线:** 行号减列号：`row - col`由于相减会导致负数，所以最终为`row - col + n`:
> &emsp;&emsp;&emsp;&emsp;&ensp;&ensp;<img src="https://pic.imgdb.cn/item/64a921491ddac507cce61044.jpg" width=30%>
> **下对角线:** 行加列：`row + col`
> &emsp;&emsp;&emsp;&emsp;&ensp;&ensp;<img src="https://pic.imgdb.cn/item/64a923541ddac507ccea04a6.jpg" width=30%>
>
> **剪枝:** 当一个方法存在矛盾, 直接停下, 不用在展开了，当前方案一定是不合法的, 就不用往下搜了。
> 
>**思路:** 枚举每一列, 要满足不同行不同列不同对角线
#### AC代码
```c++
#include <iostream>

const int MXN = 20;

int n;
char g[MXN][MXN];
bool col[MXN], dg[MXN], udg[MXN];

void dfs(int u)
{
	if (u == n) {
		for (int i = 0; i < n; ++ i) puts(g[i]);
		std::cout << std::endl;
		return;
	}
	for (int i = 0; i < n; ++ i) 
		if (!col[i] && !dg[i + u] && !udg[u - i + n]) {
			g[u][i] = 'Q';
			col[i] = dg[i + u] = udg[u - i + n] = true;
			dfs(u + 1);
			col[i] = dg[i + u] = udg[u - i + n] = false;
			g[u][i] = '.';
		}
}

int main()
{
	// std::ios::sync_with_stdio(false);
	// std::cin.tie(nullptr);
	std::cin >> n;
	for (int i = 0; i < n; ++ i) 
		for (int j = 0; j < n; ++ j) 
			g[i][j] = '.';
	dfs(0);
}
```