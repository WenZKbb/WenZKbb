```
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;
const int N=1e5+5,LOGN=18;
struct Edge{
    int to,nxt;
}edge[N*2];
int dep[N],head[N],s[N],sum,len,n,u,v,val[N],cnt=0,m,st,ed,f[N][25];
void add(int from,int too){
    edge[++cnt].to=too;
    edge[cnt].nxt=head[from];
    head[from]=cnt;
}
void dfs(int node){
    dep[node]=dep[f[node][0]]+1;
    s[node]=s[f[node][0]]+val[node];
    for(int i=head[node];i!=0;i=edge[i].nxt){
        int nd=edge[i].to;
        if(nd!=f[node][0]){
            f[nd][0]=node;
            dfs(nd);
        }
    }
}
int LCA(int x,int y){
    if(dep[x]>dep[y])swap(x,y);
    for(int i=LOGN;i>=0;i--){
        if(dep[f[y][i]]>=dep[x])
            y=f[y][i];
    }
    if(x==y)return x;
    for(int i=LOGN;i>=0;i--){
        if(f[x][i]!=f[y][i]){
            x=f[x][i];
            y=f[y][i];
        }
    }
    return f[x][0];
}
int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++){
        char c;
        cin>>c;
        if(c=='G')val[i]=0;
        else val[i]=1;
    }
    for(int i=1;i<n;i++){
        scanf("%d%d",&u,&v);
        add(u,v);
        add(v,u);
    }
    dfs(1);
    for(int i=1;i<=LOGN;i++)
        for(int j=1;j<=n;j++)
            f[j][i]=f[f[j][i-1]][i-1];
    for(int i=1;i<=m;i++){
        char c;
        scanf("%d%d",&st,&ed);
        cin>>c;
        int lca=LCA(st,ed);
        len=dep[st]+dep[ed]-2*dep[f[lca][0]]-1;
        sum=s[st]+s[ed]-2*s[f[lca][0]]-val[lca];
        if(c=='G'&&sum==len)printf("0");
        else if(c=='H'&&sum==0)printf("0");
        else printf("1");
    }
	return 0;
}
```
<!---
WenZKbb/WenZKbb is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
