---
layout: post
title: codeforces雜題
categories:
- cf
tags:
- cf
---

##455B - A Lot of Games
分可勝負，只勝，只負,無,
http://blog.csdn.net/keshuai19940722/article/details/38455269
{% highlight cpp %} 
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
const int MX = 1e5+5;
struct node {
	int v, ch[27];
	node () {
		v = 0;memset(ch, 0, sizeof(ch));
	}
}t[MX*2];
int nid = 1, sl, n, k;
char str[MX];
inline int nn () {
	return nid++;
}
void ins (int x, int d) {
	if (d == sl)return;
	int mv = str[d] - 'a';
	if (!t[x].ch[mv])t[x].ch[mv] = nn();
	ins(t[x].ch[mv], d+1);
}
int solve (int x) {
	int& ans = t[x].v;
	ans = 0;
	bool onlyfail = true;
	for (int i = 0; i < 26; i++) {
		if (t[x].ch[i])onlyfail = false,ans |= solve(t[x].ch[i]);
	}
	if (onlyfail)ans = 1;
	return 3-ans;
}
int main () {
	scanf("%d%d", &n, &k);
	for (int i = 0; i < n; i++)scanf("%s", str),sl = strlen(str),ins(0, 0);
	solve(0);
	int ans = t[0].v;
	if (ans < 2)printf("Second\n");
	else if (ans == 2)printf("%s\n", k&1 ? "First" : "Second");
	else printf("First\n");
	return 0;
}

{% endhighlight %}
