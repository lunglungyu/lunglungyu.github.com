---
layout: post
title: codechef_cannot_understand
categories: 
- codechef
tags:
- codechef
---

##CODECHEF G3  A Coin Game 
{% highlight cpp %} 
#include<stdio.h>
int t, n, i, val;
int a[10001], nimsm, b[10001];
int main() {
	scanf("%d", &t);
	while(t--) {
		scanf("%d", &n);
		nimsm = 0;
		a[0] = 0;
		for(i = 1; i <= n; i++) {
			scanf("%d", &a[i]);
			b[i] = a[i] - a[i - 1] - 1;
			if((n - i + 1) & 1){
				//printf("tg:%d %d\n",i-1,i);
				nimsm ^= b[i];
			}
		}
		if(!nimsm) printf("Johnny wins\n");//mary first
		else {
			printf("Mary wins\n");
			for(i = 1; i <= n; i++) {
				if(n - i + 1 & 1) {
					if((nimsm ^ b[i]) < b[i]) {
						printf("Move %d to %d\n", a[i], a[i] - b[i] + (nimsm ^ b[i]));//then xorsm - by that ammount
						break;
					}
				}
				//b[i]+b[i+1]= a[i]-a[i-1]-1+a[i+1]-a[i]-1 = a[i+1]-a[i-1]-2;
				else if((nimsm ^ b[i + 1]) > b[i + 1] && b[i] + b[i + 1]+1 > (nimsm ^ b[i + 1])) {//can become 0 here
					printf("Move %d to %d\n", a[i], a[i] - ((nimsm ^ b[i + 1]) - b[i + 1]));
					break;
				}
			}
		}
		printf("\n");
	}
} 
{% endhighlight %}

