---
layout: post
title: codeforces wandering
categories:
- cf
tags:
- cf
---

##cf487c
{% highlight cpp %} 
http://codeforces.com/blog/entry/14832
find a permutation of [a1, a2, ..., an], such that its prefix product sequence is a permutation of [0, 1, ..., n - 1].
n=1,4 or prime
#include <cstdio>
int n,inv[100005];
int main(){
	scanf("%d",&n);
	if(n==1)return printf("YES\n1\n"),0;
	if(n==4)return printf("YES\n1 3 2 4\n"),0;
	for(int i=2;i*i<=n;i++) if(n%i==0)return printf("NO\n"),0;
	printf("YES\n1\n");
	for(int i=1;i<n;i++){
		int now=1LL*(i+1)*(inv[i]=i==1?1:1LL*(n-n/i)*inv[n%i]%n)%n;//*** compute
		printf("%d\n",now?now:n);
	}
	return 0;
}
{% endhighlight %}
##cf195c
expression parsing
{% highlight cpp %} 
#include<cstdio>
#include<cstring>
#define N 99
using namespace std;
char s[N],r[N],a[N],b[N],ans[N]={"Unhandled Exception"};
int n;
void check(){
	while(scanf("%*[^a-z]%[a-z]",s),s[1]!='a'){//catch
		if(s[1]=='r') check();//inner loop
		if(s[1]=='h'){
			scanf("%*[^A-Za-z]%[A-Za-z]",r);
		}
	}
	scanf("%*[^A-Za-z]%[A-Za-z]",a);//the first word
	scanf("%*[^\"]\"%[^\"]",b);
	if(!strcmp(a,r))strcpy(ans,b);
}
int main(){
	scanf("%d",&n);
	while(scanf("%*[^a-z]%[a-z]",s)!=EOF&&s[1]=='r')check();
	scanf("%*[^\n]\n");//swallow \")\n
	printf("%s\n",ans);
	return 0;
}
{% endhighlight %}
