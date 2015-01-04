---
layout: post
title: bzoj3786,splay+DFS序移動子樹+子樹修改
categories:
- bzoj
tags:
- splay
- subtree
---

##bzoj3786,splay+DFS序移動子樹+子樹修改
{% highlight cpp %} 
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
#define LL long long
#define abs(x) (x>0?x:-x)
#define N 222222
#define lc(x) x->ch[0]
#define rc(x) x->ch[1]
#define fa(x) x->f
#define inf 0x3f3f3f3f
using namespace std;
struct node{
	int pos,neg,sg;
	LL sm,v,dv;
	node *ch[2],*f;
}T[N],*nil,*rt,*pt[N],*lst;
int nid;
node* nn(node *&x,LL v,int sgn){
	x=&T[nid++];
	x->v=x->sm=(LL)sgn*v;
	x->sg=sgn;
	x->pos=x->neg=0;
	if(sgn==1)x->pos=1;else if(sgn==-1)x->neg=1;
	x->dv=0;
	lc(x)=rc(x)=fa(x)=nil;
	return x;
}
inline void pu(node *x){
	x->sm=x->v+lc(x)->sm+rc(x)->sm;
	x->pos=(x->sg>0)+lc(x)->pos+rc(x)->pos;
	x->neg=(x->sg<0)+lc(x)->neg+rc(x)->neg;
}
inline void fix(node *x,LL dv){
	if(x==nil || !dv)return;
	x->sm+=dv*(x->pos-x->neg);
	x->v+=(dv*x->sg);
	x->dv+=dv;
}
inline void pd(node *x){
	if(x->dv)fix(lc(x),x->dv),fix(rc(x),x->dv),x->dv=0;
}
inline void rot(node *x){
	node *y=fa(x),*z=fa(y);bool d=(y->ch[1]==x);
	y->ch[d]=x->ch[!d];if(y->ch[d])fa(y->ch[d])=y;
	fa(x)=z;if(z!=nil)z->ch[z->ch[1]==y]=x;
	x->ch[!d]=y;fa(y)=x;
	pu(y);
}
inline void pass(node *x){
	if(x==nil)return;
	pass(fa(x));
	pd(x);
}
inline void spy(node *x,node *gl){
	pass(x);
	while(fa(x)!=gl){
		node *y=fa(x),*z=fa(y);
		if(z==gl)rot(x);
		else{
			rot(((y->ch[1]==x)^(z->ch[1]==y))?x:y);
			rot(x);
		}
	}
	pu(x);
	if(gl==nil)rt=x;
}
node* gtmn(node *x){
	while(lc(x)!=nil)x=lc(x);return x;
}
node* gtmx(node *x){
	while(rc(x)!=nil)x=rc(x);return x;
}
inline void gtrange(node *x,node *y){
	spy(x,nil);
	node *tmp1=gtmx(lc(rt));
	spy(y,nil);
	node *tmp2=gtmn(rc(rt));
	spy(tmp1,nil);
	spy(tmp2,rt);
}
struct edg{
	int u,v,nx;
}E[N*2];
int hd[N],eid;
inline void adde(int u,int v){
	E[eid].v=v;E[eid].nx=hd[u];hd[u]=eid++;
}
int did,blg[N],tin[N],tout[N];
int n,m,q,a[N];
inline void init(){
	nn(nil,0,0);fa(nil)=lc(nil)=rc(nil)=nil;
	nn(rt,-inf,0);
	nn(rc(rt),-inf,0);
	fa(rc(rt))=rt;pu(rt);
	memset(hd,-1,sizeof(hd));eid=0;
	did=0;
	pt[0]=rt;
	lst=rc(rt);
}
inline void dfs(int u){
	blg[++did]=u;
	tin[u]=did;
	for(int i=hd[u];i!=-1;i=E[i].nx){
		int v=E[i].v;
		dfs(v);
	}
	blg[++did]=-u;
	tout[u]=did;
}
inline void bt(node *&x,int l,int r){
	if(l>r)return;
	int md=(l+r)>>1;
	//printf("bt :%d\n",blg[md]);
	pt[md]=nn(x,a[abs(blg[md])],blg[md]>0?1:-1);
	bt(lc(x),l,md-1);
	fa(lc(x))=x;
	bt(rc(x),md+1,r);
	fa(rc(x))=x;
	pu(x);
}
inline void dbgt(node *x){
	if(x==nil)return;
	dbgt(lc(x));
	printf("x:%ld lc:%ld rc:%ld v:%lld sm:%lld sg:%d pos:%d neg:%d\n",x-T,lc(x)-T,rc(x)-T,x->v,x->sm,x->sg,x->pos,x->neg);
	dbgt(rc(x));
}
inline void ptree(){
	dbgt(rt);
	puts("-----");
}
int main(){
	scanf("%d",&n);
	init();
	int u,v,w;
	for(int i=2;i<=n;++i){
		scanf("%d",&u);
		adde(u,i);
	}
	for(int i=1;i<=n;++i)scanf("%d",&a[i]);
	dfs(1);
	//for(int i=1;i<=did;++i)printf(" %d",blg[i]);puts("");
	bt(lc(rc(rt)),1,did);
	fa(lc(rc(rt)))=rc(rt);pu(rc(rt));pu(rt);
	pt[did+1]=lst;
	//ptree();
	//for(int i=1;i<=3;++i)spy(pt[i],nil),ptree();
	scanf("%d",&m);
	for(int i=1;i<=m;++i){
		char op[20];int u,v,w;
		scanf("%s%d",op,&u);
		if(op[0]=='Q'){
			spy(pt[0],nil);node *rtin=gtmn(rc(rt));
			gtrange(rtin,pt[tin[u]]);
			//gtrange(pt[tin[1]],pt[tin[u]]);
			printf("%lld\n",lc(rc(rt))->sm);
		}else{
			scanf("%d",&v);
			if(op[0]=='F'){
				gtrange(pt[tin[u]],pt[tout[u]]);
				fix(lc(rc(rt)),v);
			}else{
				gtrange(pt[tin[u]],pt[tout[u]]);
				node *tmp=lc(rc(rt));//this range
				lc(rc(rt))=nil;pu(rc(rt));pu(rt);
				spy(pt[tin[v]],nil);
				spy(gtmn(rc(rt)),rt);//this subtreestart
				lc(rc(rt))=tmp;tmp->f=rc(rt);
				pu(rc(rt));pu(rt);
			}
		}
	}
	return 0;
}
{% endhighlight %}

