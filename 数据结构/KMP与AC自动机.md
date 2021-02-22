# KMP与AC自动机



核心思想都是给串的每一个位置存一个next值

每一个点的next指向 最长的 与**该点的一个后缀**匹配的**前缀**的<u>最后一位</u>



## KMP

用于一个长串与一个子串的匹配

*注：字符串从1开始读入而非0

~~~c++
next[0]=next[1]=0;

for(int i=2;i<=m;i++)
{
    int j=next[i-1];
    while(j&&p[i]!=p[j+1])j=next[j];
    if(s[i]==p[j+1])j++;
    next[i]=j;
}
~~~





## AC自动机

用于一个长串与一堆子串的匹配

一堆子串用trie来存，用BFS遍历trie这棵树来进行KMP

tire树

~~~c++
/*
struct Node{
	int son[26];
}tr[N*S];
*/

int tr[N*S][26],cnt[N*S],idx;//N为单词数量，S为单词长度
char str[S];

void insert()//插入一个str单词
{
    int p=0;
    for(int i=0;str[i];i++)
    {
        int t=str[i]-'a';
        if(!tr[p][t])tr[p][t]=++idx;
        p=tr[p][t];
    }
    cnt[p]++;//以p结尾的单词数量++
}

~~~



AC自动机

~~~c++
int q[N*S],ne[N*S];

void build()//bfs构建AC自动机
{
    int hh=0,tt=-1;
    for(int i=0;i<26;i++)
        if(tr[0][i])q[++tt]=tr[0][i];
    
    while(hh<=tt)
    {
        int t=q[hh++];
        for(int i=0;i<26;i++)
        {
            int c=tr[t][i];
            int j=ne[t];
            while(j&&!tr[j][i])j=ne[j];
            if(tr[j][i])j=tr[j][i];
            ne[c]=j;
            q[++tt]=c;
        }
    }
}

//利用AC自动机匹配串
	int res=0;//出现了子串的次数
	for(int i=0,j=0;str[i];i++)
    {
        int t=str[i]-'a';
        while(j&&!tr[j][t])j=ne[j];
        if(tr[j][t])j=tr[j][t];
        
        int p=j;
        while(p)
        {
            res +=cnt[p];
            cnt[p]=0;
            p=ne[p];
        }
    }

~~~



