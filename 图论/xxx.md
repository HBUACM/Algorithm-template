# ACM训练营

## 算法模板

### ~~找答案~~，用二分！

在满足单调性的一个区间里，可以用二分来找到答案。~~自己理解一下这句话的意思~~

~~~c++
int find()
{
    int l=0,r=N;
    while(r>l)
    {
        int mid=l+r>>1;
        /*
        if(check(mid))r=mid;
        else l=mid+1;
        *///求最小值
            
        if(check(mid))l=mid;
        else r=mid-1;
        //求最大值
    }
    return r;
}
~~~



### 图论

#### 邻接表(邻接矩阵略)

​		算法书上是边从1开始编号，int类型默认值为0，不需要初始化head数组。

```c++
// head[x] = m 表示点 x 的邻接表的表头是编号为 m 的边
// ver[m] 表示编号为 m 的边的终点
// edge[m] 表示编号为 m 的边的权值
int head[N], ver[M], edge[M], Next[M], tot;

// 加入有向边 (x, y)，权值为 z
void add(int x, int y, int z) {
    // 真实数据
    ver[++tot] = y, edge[tot] = z;
    // 在表头 x 处插入
    Next[tot] = head[x], head[x] = tot;
}

// 访问从 x 出发的所有边
for (int i = head[x]; i; i = Next[i]) {
    int y = ver[i], z = edge[i];
    // 找到了一条有向边 (x, y)，权值为 z
}
```

或（h数组需要初始化为-1，可以处理存在编号为0的节点的情况）

~~~c++
int h[N],e[M],w[M],ne[M],idx;

void add(int a,int b,int c)
{
    e[idx]=b,w[idx]=c,ne[idx]=h[a],h[a]=idx++;
}

for(int i=h[a];~i;i=ne[i])//~i等同于i!=-1
    int j=e[i];
~~~



#### 最短路

​		可以先作一个虚拟源点连向图中所有的节点，虚拟源点到各个节点的距离根据题目而定（通常为0或+∞）。

* #### Floyd

* #### Dijkstra

	​		单源最短路算法。**朴素算法时间复杂度为O(n<sup>2</sup>)，优先队列优化算法时间复杂度为O(m log n)**，适用于稠密图即边数越多越适合使用Dijkstra。<u>**不能用于存在负权边的图中**</u>。

	
	
	朴素版
	
	~~~c++
	void dijkstra()
	{
	    memset(dist,0x3f,sizeof dist);
	    memset(st,0x3f,sizeof st);
	    dist[0]=0;
	    
	    for(int i=0;i<=n;i++)
	    {
	        int t=-1;
	        for(int j=0;j<=n;j++)
	            if(!st[j]&&(t==-1||dist[j]<dist[t]))t=j;
	        
	        st[t]=true;
	        for(int j=0;j<=n;j++)
	            dist[j]=min(dist[j],dist[t]+v[t][j]);
	    }
	}
	~~~
	
	
	
	优先队列优化版

```c++
struct Node{
    int pos,val;
    bool operator >(const Node &tmp)const{
        return val>tmp.val;
    }
};

int dist[N];
bool v[N];

int s;//源点
void dijkstra()
{
    priority_queue<Node,vector<Node>,greater<Node>>pq;
    memset(dist,0x3f,sizeof dist);
    memset(v,0,sizeof v);
    d[s]=0;
    pq.push((Node){s,0});
    while(pq.size())
    {
        Node now=pq.top();
        pq.pop();
        int x= now.pos;
        if(v[x])continue;
        v[x]=1;
        for(int i=head[x];i;i=Next[i])
        {
            int y=ver[i],z=edge[i];
            if(v[y])continue;
            if(dist[y]>dist[x]+z)
            {
                dist[y]=dist[x]+z;
                pq.push((Node){y,dist[y]});
            }
        }
    }
}
```



* #### SPFA

	​		单源最短路径算法。**时间复杂度为O(km)最坏情况为O(nm)**，适用于稀疏图。<u>**适用于存在负权边的图中。**该算法虽然好用但极有可能被大部分题目给卡掉，需要注意。</u>

  ```c++
  int dist[N],q[N];
  bool v[N];
  
  int s;
  void spfa()
  {
      memset(dist,0x3f,sizeof dist);
      dist[s]=0;
      
      int hh=0,tt=1;
      q[0]=s,v[s]=1;
      while(hh!=tt)
      {
          int x=q[hh++];
          if(hh==N)hh=0;
          v[x]=0;
          
          for(int i=head[x];i;i=Next[i])
          {
              int y=ver[i],z=edge[i];
              if(dist[y]>dist[x]+z)
              {
                  dist[y]=dist[x]+z;
                  if(!V[y])
                  {
                      q[tt++]=y;
                      if(tt==N)tt=0;
                      v[y]=true;
                  }
              }
          }
      }
  }
  ```
  
  SPFA求负环
  
  将队列换成栈可以很大程度上防止TLE，但一般的SPFA问题不要用栈 ~~DFS≈栈 BFS≈队列~~
  
  ``` c++
  int dist[N],cnt[N];
  bool v[N];
  
  int s;
  bool check()
  {
      memset(v,0,sizeof v);
      memset(cnt,0,sizeof cnt);
      
      int hh=0,tt=0;
      for(int i=0;i<N;i++)
      {
          q[tt++]=i;
          v[i]=true;
      }
      int count=0;
      while(hh!=tt)
      {
          int x=q[--tt];
          v[x]=false;
          
          for(int i=head[x];i;i=Next[i])
          {
              int y=ver[i],z=edge[i];
              if(dist[y]>dist[x]+z)
              {
                  dist[y]=dist[x]+z;
                  cnt[y]=cnt[x]+1;
                  if(cnt[y]>=n)return true;
                  if(!v[y])
                  {
                      q[tt++]=y;
                      v[y]=true;
                  }
              }
          }
      }
      
      return false;
  }
  ```
  
  




#### 最小生成树

* #### Prim

  ​		类似于Dijkstra算法，但Prim中选取的边不用加上邻点到源点的距离。**朴素算法时间复杂度为O(n<sup>2</sup>)，优先队列优化算法时间复杂度为O(m log n)。**

  

  朴素算法

  ```c++
  int w[N][N];
  int dist[N];
  bool v[N];
  
  int prim()
  {
      int res=0;
      memset(dist,0x3f,sizeof dist);
      dist[1]=0;
      
      for(int i=0;i<n;i++)
      {
          int t=-1;
          for(int j=1;j<=n;j++)
          	if(!st[j]&&(t==-1||dist[t]>dist[j]))t=j;
          
          res+=dist[t];
          v[t]=1;
          for(int j=1;j<=n;j++)dist[j]=min(dist[j],w[t][j]);
      }
      return res;
  }
  ```

  





* #### Kruskal

  ​		将边从小到大依次判断（先排序），若加入该边不构成环则选取。用并查集来判断是否构成环。**时间复杂度为O(m log m)。**Kruskal局部最优解可太最优了。

  ```c++
  struct Edge{
      int a,b,w;
      bool operator < (const Edge &tmp)const{
          return w<tmp.w;
      }
  }e[M];
  int p[N];
  
  int find(int x)
  {
      if(p[x]!=x)p[x]=find(p[x]);
      return p[x];
  }
  
  int Kruskal()
  {
      sort(e,e+m);
      int res=0;
      for(int i=0;i<m;i++)
      {
          int a=find(e[i].a),b=find(e[i].b),w=e[i].w;
          if(a!=b)p[a]=b,res+=w;
      }
      return res;
  }
  ```

  