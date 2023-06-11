[toc]
###1.栈简介
>**栈**是一种**先进后出**的数据结构。
![](https://pic.imgdb.cn/item/64857f821ddac507cc92456a.jpg)
####1.1. 两个重要元素
>1.栈的**大小**
>2.**栈顶指针**
####1.2. 栈的基本操作
>- 查询**栈顶**元素        
>- 将数据放在**栈顶**    
>- 删除**栈顶**数据
####1.3. 注意
>注意下标是习惯将`1`作为`base`，还是习惯将`0`作为`base`，无论是栈的初始化，还是对于区间的计算，
> 因为`base`的不同都会被影响到。
####1.4. 例题 
#####栈1 AC代码 + 解释
>**题意**：输入入栈顺序，输出出栈顺序。<br/>
>**注意**：因为这里是`1`作为`base`所以数组起码开到`100001`。
```c++
#include <iostream>

const int MXN = 100005;
int n, top, stk[MXN];
char s[MXN];

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	for (int i = 1; i <= n; ++ i){
		std::cin >> s;
		if (s[1] == 'u'){			
			int x; std::cin >> x;
			stk[ ++ top] = x;
		} 
		else{
			if (s[0] == 'p') -- top;  
			else std::cout << stk[top] << std::endl;	
		}
	}
	return 0;
}
```

#####栈2 AC代码 + 解释
>**题意**：输入入栈顺序，输出出栈顺序。<br/>
>**注意**：
因为是用数组模拟，所以还可以新增一种操作`query`。
>`query k`：询问栈顶往下数第`k`个元素是多少。
>>- 通过代**特殊值法**就可以推出通项公式：$top-k+1$
>>- 递推：既然我们想知道`从栈顶往下数第k个元素是多少`，那么我们就得先知道第`k`个元素的**下标**。
从栈顶往下数：`第1个元素的位置是top`，`第2个元素的位置是top-1`...
**观察`top`和`k`之间的变量关系**则可以得出：`第k个元素的位置则是top-k+1`。
>>>>>- 带入值：假设`top=6`，我们想知道`从栈顶往下数第5个元素是多少`。
因为第`5`个元素在**下标**为`2`的位置，所以我们要使得`top-5 = 2`即推出：$top-k+1$
![](https://pic.imgdb.cn/item/648577531ddac507cc7fdbe2.jpg)
```c++
#include <iostream>

const int MXN = 100005;
int n, top, stk[MXN];
char s[MXN];

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	for (int i = 1; i <= n; ++ i){
		std::cin >> s;
		if (s[2] == 's'){				
			int x; std::cin >> x;
			stk[ ++ top] = x;
		} 
		else{
			if (s[0] == 'p') -- top;  
			else{
				int k; std::cin >> k;
				std::cout << stk[top - k + 1] << std::endl;
			}
		}
	}
	return 0;
}
``` 
#####出栈序列判断 AC代码 + 解释
>**题意**：输入出栈顺序，输出入栈顺序。<br/>
>**注意**：**一个出栈序列对应的操作序列是唯一的**。<br/>
>**题目分析**：因为默认的序列是有序的，记录栈顶元素，所以我们只需要从左往右遍历数组，模拟即可。
######无数组:
>&emsp;&emsp;如果`栈顶元素 < 出栈元素`，说明当前的出栈元素还没有入栈，输出`push`，
&emsp;&emsp;如果`栈顶元素 > 出栈元素`，说明当前的出栈元素已经入栈过了，输出`pop`。

```c++
#include <iostream>

const int MXN = 100005;

int n, top, stk[MXN];

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	for (int i = 1; i <= n; ++ i){
		int x; std::cin >> x;
		while (top < x){
			top ++ ;
			std::cout << "push " << top << std::endl;
		} 
		std::cout << "pop" << std::endl;
	}
}
```
######有数组：
>&ensp;&ensp;记录最后一个入栈的元素`l`。
&ensp;&ensp;如果出栈元素和当前栈顶元素不一样，从`l + 1`开始进行入栈，因为**序列是有序**的，所以`入栈元素 < 出栈元素`。
&ensp;&ensp;如果相等出栈即可，如果小于最后一个出栈的元素出栈即可。
```c++
#include <iostream>

const int MXN = 100005;

int n, top, l, stk[MXN];    // l: 记录到哪个数字为止，已经入栈了

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	
	for (int i = 1; i <= n; ++ i){
		int x; std::cin >> x;
		if (stk[top] != x){
			for (int j = l + 1; j <= x; ++ j){
				stk[ ++ top] = j;
				std::cout << "push " << j << std::endl;
			}
			l = x;
		}
		std::cout << "pop" << std::endl;
		-- top;
	}
}
```
#####括号序列 AC代码 + 解释
>**题意**：给定长度为`n`的字符串`s`，字符串由`(, ), [, ]`组成，问`s`是不是一个合法的括号序列。
>- 合法的括号序列定义：
>>>>- 空串是一个合法的括号序列
>>>>- 若`A`是一个合法的括号序列，则`[A]`,`(A)`也是合法的括号序列
>>>>>- 例：[()] 
>>>>- 若`A`, `B`是一个合法的括号序列，则`AB`也是合法的括号序列
>>>>>- 例：[] ()

>**题目分析**：利用**栈的性质**，栈中存储没有被比匹配的括号，如果遇到可匹配的括号弹出栈中括号，判断他们是否匹配即可。
```c++
#include <iostream>

const int MXN = 100010;

int n, top;
char s[MXN], stk[MXN];

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	bool flag = true; 
	for (int i = 1; i <= n && flag; ++ i){
		std::cin >> s[i];
		if (s[i] == '(' || s[i] == '[') stk[++ top] = s[i];
		else{
			if (!top) flag = false;
			if (stk[top] == '(' && s[i] == ')') top -- ;
			else if (stk[top] == '[' && s[i] == ']') top -- ;
			else flag = false;
		}
	}
	if (top) flag = false;
	flag ? std::cout << "Yes" : std::cout << "No";
	return 0;
}
```
#####字符串处理1 AC代码 + 解释
>**题意**：给定长度为`n`的字符串`s`，字符串只由`a...z`组成。
>&emsp;&emsp;&emsp;如果发现两个相同的字母并排在一起，就会把这两个字符都删除掉，
>&emsp;&emsp;&emsp;重复这个操作，知道没有相邻的相同字符。 <br/>
>**题目分析**：利用栈**后进先出**的性质，如果入栈元素和栈顶元素一样，将他们抵消掉。
>&emsp;&emsp;&emsp;&emsp;&emsp;如果入栈元素和栈顶元素不一样，就入栈。 
```c++
#include <iostream>

const int MXN = 100010;

int n, top;
char s[MXN], stk[MXN];

int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	std::cin >> n;
	for (int i = 1; i <= n; ++ i){
		std::cin >> s[i];
		if (!top || stk[top] != s[i]) stk[ ++ top] = s[i];
		else -- top;
		
	}
	for (int i = 1; i <= top; ++ i) std::cout << stk[i];
	return 0;
}
```
####小结
>**相邻匹配问题**和**括号配对**问题都可以用栈去做