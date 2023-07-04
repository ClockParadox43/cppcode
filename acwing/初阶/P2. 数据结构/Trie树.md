[toc]
### 835. Trie字符串统计
#### 题目链接
> https://www.acwing.com/problem/content/837/
#### 简介
> <img src="https://pic.imgdb.cn/item/64a433bb1ddac507ccce1e73.jpg" width=40%>
#### AC代码
```c++
#include <iostream>

const int MXN = 100010;

// 因为0要标记不存在，所以idx的0不使用
int T, idx, cnt[MXN], son[MXN][26];
char str[MXN];

void insert(char *s)
{
	int p = 0;
	for (int i = 0; s[i]; ++ i) {
		int u = s[i] - 'a';
		if (!son[p][u]) son[p][u] = ++ idx;	
		p = son[p][u];				// 如果存在的话就顺着之前的idx往下爬
	}
	cnt[p] ++ ;
}

int query(char *s) 
{
	int p = 0;
	for (int i = 0; str[i]; ++ i) {
		int u = s[i] - 'a';
		if (!son[p][u]) return 0;
		p = son[p][u];
	}
	return cnt[p];
}

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> T;
	while (T -- ) {
		char op[2]; std::cin >> op >> str;
		if (op[0] == 'I') insert(str);
		else std::cout << query(str) << std::endl;
	}
}
```
### 143. 最大异或对
#### 题目链接
> https://www.acwing.com/problem/content/145/
#### 题意
> 在给定的$N$个整数$A_1，A_2……A_N$中选出两个进行$xor$（异或）运算，得到的结果最大是多少？
#### 简介
> 异或涉及到位运算，因为位运算是**独立**的，所以可以将数字分解成二进制插入到`Trie树`上，然后一位一位判断，
> 因为是`Trie树`所以查找是哪个数字也很方便。 
#### AC代码
```c++
#include <iostream>

const int MXN = 100010, MXM = 3000010; 

int n, idx, a[MXN], son[MXM][2];

void insert(int x) 
{	
	int p = 0;
	for (int i = 30; ~ i; -- i) {
		int &s = son[p][x >> i & 1];	// 判断是走1还是走0路径
		if (!s) s = ++ idx;
		p = s;
	}
}

int query(int x)
{
	int res = 0, p = 0;
	for (int i = 30; ~ i; -- i) {
		int u = x >> i & 1;
		if (son[p][!u]) {	        // !u: 0就要取1, 1就要取0 (前提是存在) 
			res |= 1 << i;
			p = son[p][!u];
		} else p = son[p][u];
	}
	return res;
}

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	for (int i = 0; i < n; ++ i) 
		std::cin >> a[i], insert(a[i]);
	int ans = 0;
	for (int i = 0; i < n; ++ i) 
		ans = std::max(ans, query(a[i]));   // 对a[i]配对
	std::cout << ans << std::endl;
}
```