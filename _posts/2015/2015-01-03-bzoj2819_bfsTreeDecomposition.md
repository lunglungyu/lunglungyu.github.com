---
layout: post
title: bzoj2819
categories:
- bzoj
tags:
- segtree
---

## bfs樹鏈剖分＋點權線段樹
{% highlight cpp %} 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define N 555555
#define M N*2
#define CLR(a,b) memset(a,b,sizeof(a))
#define lc(x) (x<<1)
#define rc(x) (lc(x)|1)
using namespace std;
int vv[N<<2];
struct edg{
	int v,nxt;
}E[M];
int hd[N],eid;
int a[N];
int did,fa[N],top[N],sz[N],son[N],dep[N],tid[N],tval[N];
int n,m;
void init(){
	CLR(hd,-1);eid=did=0;
}
void adde(int u,int v){
	E[eid].v=v;E[eid].nxt=hd[u];hd[u]=eid++;
}
void bfs(){
	static int q[N],qh=0,qt=0;
	int u,v,w;
	q[qt++]=1;fa[1]=0;son[0]=sz[0]=0;dep[0]=-1;
	while(qh<qt){
		u=q[qh++];
		dep[u]=dep[fa[u]]+1;sz[u]=1;son[u]=0;
		for(int i=hd[u];i!=-1;i=E[i].nxt){
			if((v=E[i].v)==fa[u])continue;
			fa[v]=u;q[qt++]=v;
		}
	}
	for(int i=qt-1;i>=0;--i){
		u=q[i];sz[fa[u]]+=sz[u];
		if(sz[son[fa[u]]]<sz[u])son[fa[u]]=u;
	}
	for(int i=0;i<qt;++i){
		int u=q[i];
		if(son[fa[u]]!=u)for(int j=u;j;j=son[j]){
			top[j]=u;
			tid[j]=++did;
			tval[tid[j]]=a[j];
		}
	}
	//for(int i=1;i<=n;++i)printf("i:%d fa:%d tp:%d sz:%d dp:%d tid:%d\n",i,fa[i],top[i],sz[i],dep[i],tid[i]);
}
void pu(int x){
	vv[x]=vv[lc(x)]^vv[rc(x)];
}
void bt(int x,int l,int r){
	if(l==r){vv[x]=tval[l];return;}
	int md=(l+r)>>1;
	bt(lc(x),l,md),bt(rc(x),md+1,r);
	pu(x);
}
void upd(int x,int l,int r,int p,int v){
	if(l==r){
		vv[x]=v;return;
	}
	int md=(l+r)>>1;
	if(p<=md)upd(lc(x),l,md,p,v);
	else upd(rc(x),md+1,r,p,v);
	pu(x);
}
int qry(int x,int l,int r,int L,int R){
	if(L<=l && r<=R)return vv[x];
	int md=(l+r)>>1;
	int rt=0;
	if(L<=md)rt=qry(lc(x),l,md,L,R);
	if(R>md)rt^=qry(rc(x),md+1,r,L,R);
	return rt;
}
int pathqry(int x,int y){
	int fx=top[x],fy=top[y],rt=0;
	while(fx!=fy){
		if(dep[fx]<dep[fy])swap(x,y),swap(fx,fy);
		rt^=qry(1,1,did,tid[fx],tid[x]);
		x=fa[fx],fx=top[x];
	}
	if(dep[x]>dep[y])swap(x,y);
	rt^=qry(1,1,did,tid[x],tid[y]);
	return rt;
}
int main(){
	scanf("%d",&n);
	init();
	for(int i=1;i<=n;++i)scanf("%d",&a[i]);
	for(int i=0;i<n-1;++i){
		int u,v,w;
		scanf("%d%d",&u,&v);
		adde(u,v),adde(v,u);
	}
	bfs();
	bt(1,1,did);
	scanf("%d",&m);
	char op[20];int u,v;
	for(int i=1;i<=m;++i){
		scanf("%s%d%d",op,&u,&v);
		if(op[0]=='C')upd(1,1,did,tid[u],v);
		else{
			int rt=pathqry(u,v);
			printf("%s\n",rt==0?"No":"Yes");
		}
	}
	return 0;
}

% endhighlight %}
