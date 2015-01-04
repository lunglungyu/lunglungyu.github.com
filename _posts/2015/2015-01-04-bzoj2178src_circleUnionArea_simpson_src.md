---
layout: post
title: 2015-01-04-bzoj2178src_circleUnionArea_simpson_src
categories:
- bzoj
tags:
- geom
- circle
---

##2015-01-04-bzoj2178src_circleUnionArea_simpson_src
{% highlight cpp %} 
#include<cstdio>
#include<cmath>
#include<algorithm>
#include<cstring>
#define eps 1e-13
using namespace std;
int n,top,st,ed;
double xl[1001],xr[1001],ans;
bool del[1001];
struct cir{double x,y,r;}t[1001],sk[1001];
struct line{double l,r;}p[1001];
double dis(cir a,cir b){return sqrt((a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y));}
bool cmp1(cir a,cir b){return a.r<b.r;}
bool cmp2(cir a,cir b){return a.x-a.r<b.x-b.r;}//circleleft
bool cmp3(line a,line b){return a.l<b.l;}//start pt
void init(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++){scanf("%lf%lf%lf",&t[i].x,&t[i].y,&t[i].r);}
	sort(t+1,t+n+1,cmp1);
	for(int i=1;i<=n;i++)for(int j=i+1;j<=n;j++)if(dis(t[i],t[j])<=t[j].r-t[i].r){del[i]=1;break;}
	for(int i=1;i<=n;i++)if(!del[i])sk[++top]=t[i];n=top;
	sort(sk+1,sk+n+1,cmp2);
}
double geth(double x){
	int sz=0,i,j;double r,len=0,dis;
	for(i=st;i<=ed;i++){
		if(x<=xl[i]||x>=xr[i])continue;
		dis=sqrt(sk[i].r-(x-sk[i].x)*(x-sk[i].x));
		p[++sz].l=sk[i].y-dis;p[sz].r=sk[i].y+dis;
	}
	sort(p+1,p+sz+1,cmp3);
	for(i=1;i<=sz;i++){
		r=p[i].r;
		for(j=i+1;j<=sz;j++){
			if(p[j].l>r)break;
			if(r<p[j].r)r=p[j].r;
		}
		len+=r-p[i].l;i=j-1;
	}return len;
}
double cal(double l,double fl,double fmd,double fr){return (fl+fmd*4.0+fr)*l/6.0;}
double simpson(double l,double md,double r,double fl,double fmd,double fr,double s){
	double m1=(l+md)/2.0,m2=(r+md)/2.0;
	double f1=geth(m1),f2=geth(m2);
	double g1=cal(md-l,fl,f1,fmd),g2=cal(r-md,fmd,f2,fr);
	if(fabs(g1+g2-s)<eps)return g1+g2;
	return simpson(l,m1,md,fl,f1,fmd,g1)+simpson(md,m2,r,fmd,f2,fr,g2);
}
void work(){
	int i,j;double l,r,md,fl,fr,fmd;
	for(i=1;i<=n;i++){xl[i]=sk[i].x-sk[i].r;xr[i]=sk[i].x+sk[i].r;sk[i].r*=sk[i].r;}
	for(i=1;i<=n;i++){
		l=xl[i];r=xr[i];
		for(j=i+1;j<=n;j++){
			if(xl[j]>r)break;if(xr[j]>r)r=xr[j];
		}
		st=i;ed=j-1;i=j-1;
		md=(l+r)/2;
		fl=geth(l);fr=geth(r);fmd=geth(md);
		ans+=simpson(l,md,r,fl,fmd,fr,cal(r-l,fl,fmd,fr));
	}
}
int main(){
	init();work();printf("%.3lf\n",ans);
	return 0;
}
{% endhighlight %}


