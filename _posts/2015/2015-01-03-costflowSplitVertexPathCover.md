---
layout: post
title: bzoj1927
categories:
- bzoj
tags:
- bzoj
- costflow
---

##拆點費用流,路徑覆蓋每點一且一次
{% highlight cpp %} 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define LL long long 
#define CLR(a,b) memset(a,b,sizeof(a))
#define N 111111
#define M 555555
#define inf 0x3f3f3f3f
using namespace std;
struct edg{
	int u,v,w,c,nxt;
}E[M];
int hd[N],eid;
int d[N],q[N],qh,qt,fe[N],vis[N];
LL mnct;
void init(){
	CLR(hd,-1);
	eid=0;
}
void adde(int u,int v,int w,int c,int r=0){
	E[eid].u=u;E[eid].v=v;E[eid].w=w;E[eid].c=c;E[eid].nxt=hd[u];hd[u]=eid++;
	E[eid].u=v;E[eid].v=u;E[eid].w=r;E[eid].c=-c;E[eid].nxt=hd[v];hd[v]=eid++;
}
bool spfa(int s,int t){
	CLR(d,inf);CLR(vis,0),CLR(fe,-1);qh=qt=0;d[s]=0;q[qt++]=s;
	int u,v;
	while(qh<qt){
		int u=q[qh++];vis[u]=0;
		for(int i=hd[u];i!=-1;i=E[i].nxt){
			if(E[i].w && d[v=E[i].v]>d[u]+E[i].c){
				fe[v]=i;d[v]=d[u]+E[i].c;
				if(!vis[v])vis[q[qt++]=v]=1;
			}
		}
	}
	return d[t]!=inf;
}
LL spfaflow(int s,int t,int nv){
	mnct=0;int v,x;LL rt=0;
	while(spfa(s,t)){
		x=t,v=inf;while(x!=s)v=min(v,E[fe[x]].w),x=E[fe[x]].u;rt+=v;mnct+=(LL)v*d[t];
		x=t;while(x!=s)E[fe[x]].w-=v,E[fe[x]^1].w+=v,x=E[fe[x]].u;
	}return rt;
}
int a[N],n,m;
int main(){
	//init();adde(0,1,30,1),adde(1,2,30,2);LL rt=spfaflow(0,2,3);printf("mnct:%lld rt:%lld\n",mnct,rt);
	init();
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;++i)scanf("%d",a+i);
	int ss=0,tt=2*n+1;
	for(int i=1;i<=n;++i){
		adde(ss,i+n,1,a[i]);
		adde(ss,i,1,0);
		adde(i+n,tt,1,0);
	}
	int u,v,w;
	for(int i=1;i<=m;++i){
		scanf("%d%d%d",&u,&v,&w);
		if(u>v)swap(u,v);
		adde(u,v+n,1,w);
	}
	LL ret=spfaflow(ss,tt,tt+1);
	//printf("rt:%lld\n",rt);
	printf("%lld\n",mnct);
	return 0;
}
{% endhighlight %}
