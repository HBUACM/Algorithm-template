# tarjan

## LCA

线性时间复杂度求最近公共祖先

~~~c++

~~~





## 连通分量

找连通分量→缩点→建边（去重边）

## SCC

求有向图的强连通分量(SCC)，时间复杂度O(n+m)

~~~c++
void tarjan(int u)
{
    dfn[u]=low[u]=++timestamp;
    stk[++top]=u,in_stk[u]=true;
    
    for(int i=head[u];i;i=Next[i])
    {
        int j=edge[i];
        if(!dfn[j])
        {
            tarjan(j);
            low[u]=min(low[u],low[j]);
        }
        else if(in_stk[j])
            low[u]=min(low[u],dfn[j]);
    }
    
    if(dfn[u]==low[u])
    {
        int y;
        ++scc_cnt;
        do{
            y=stk[top--];
            in_stk[y]=false;
            id[y]=scc_cnt;
        }while(y!=u);
    }
}
~~~

tarjan求出的**强连通分量递减序就是<u>拓扑序</u> **

将有向图一个图变为一个强连通分量需要至少新建max{p,q}条边，其中p、q分别是缩点后入度为0的点和出度为0的点的数量



## DCC

双连通分量分为 <u>边双连通分量 e-DCC</u>和<u>点双连通分量 v-DCC</u>。两种双连通分量无联系关系

### e-DCC

~~~c++
void tarjan(int u,int from)
{
    dfn[u]=low[u]=++timestamp;
    stk[++top]=u;
    
    for(int i=head[u];i;i=Next[i])
    {
        int j=edge[i];
        if(!dfn[j])
        {
            tarjan(j,i);
            low[u]=min(low[u],low[j]);
            if(dfn[u]<low[j])
                is_bridge[i]=is_bridge[i^1]=true;
        }
        else if(i!=(from^1))
            low[u]=min(low[u],dfn[j]);
    }
    
    if(dfn[u]==dfn[j])
    {
        ++dcc_cnt;
        do{
            y=stk[top--];
            id[y]=dcc_cnt;
        }while(y!=u);
    }
}
~~~

将无向图一个图变为一个边双连通分量需要至少新建(cnt+1)/2条边，其中cnt为缩点后度为0的点的数量



### v-DCC

求连通集中删除一个点（割点）能分成的最多连通集数量

~~~c++
void tarjan(int u)
{
    dfn[u]=low[u]=++timestamp;
    
    int cnt=0;
    for(int i=head[u];i;i=Next[i])
    {
        int j=e[i];
        if(!dfn[j])
        {
            tarjan(j);
            low[u]=min(low[u],low[j]);
            if(low[j]>=dfn[u])cnt++;
        }
        else
            low[u]=min(low[u],dfn[j]);
    }
    if(u!=root&&cnt)cnt++;
    ans=max(ans,cnt);
}
~~~



求v-DCC

~~~c++
void tarjan(int u)
{
    dfn[u]=low[u]=++timestamp;
    stk[++top]=u;
    
    if(u==root&&!head[u])
    {
        dcc_cnt++;
        dcc[dcc_cnt].push_back(u);
        return;
    }
    
    for(int i=head[u];i;i=Next[i])
    {
        int j=e[i];
        if(!dfn[j])
        {
            tarjan(j);
            low[u]=min(low[u],low[j]);
            if(dfn[u]<=low[j])
            {
                cnt++;
                if(u!=root||cnt>1)cut[u]=true;
                ++dcc_cnt;
                int y;
                do{
                    y=stk[top--];
                    dcc[dcc_cnt].push_back(y);
                }while(y!=j);
                dcc[dcc_cnt].push_back(u);
            }
        }
        else low[u]=min(low[u],low[j]);
    }
}
~~~

