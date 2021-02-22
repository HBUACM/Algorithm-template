# 可持久化trie



例题，最大异或和acwing256

题目大意：给出一个原始数组，数组长度为N。给出若干查询

查询分为1.在数组末尾加上一个数2.在[l,r]之间取一个数k使得a[k]^a[k+1]^...^a[N]^x最大输出最大值

用差分思想，S<sub>i</sub>=a<sub>1</sub>>^a<sub>2</sub>^...^a<sub>i</sub>，则a[k]^a[k+1]^...^a[N]^x=S<sub>k-1</sub>^S<sub>N</sub>^x，S<sub>N</sub>^x为常数



思路：

将a[N]^x算出来，转换成二进制，从最高位开始查找。设当前位的二进制数是x，优先查找当前位是x^1的数，一直向低位查找。

可以构建出一颗字母只有'0'和'1'的trie。

每次在末尾加上数都对trie进行一次更新并保存历史修改，每次查询最大值只需在R的版本中贪心寻找直到找到第一课最后能在L之后的版本找到的数，即是解。

~~~c++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 600010, M = N * 25;

int n, m;
int s[N];
int tr[M][2], max_id[M];
int root[N], idx;//root为根节点记录的是版本号

void insert(int i, int k, int p, int q)//S的下标为i，当前处理到第k位,上一版本p,最新版本q
{
    if (k < 0)
    {
        max_id[q] = i;
        return;
    }
    int v = s[i] >> k & 1;
    if (p) tr[q][v ^ 1] = tr[p][v ^ 1];
    tr[q][v] = ++ idx;
    insert(i, k - 1, tr[p][v], tr[q][v]);
    max_id[q] = max(max_id[tr[q][0]], max_id[tr[q][1]]);
}

int query(int root, int C, int L)//root版本，C=Sn^x，查找版本大于L的**
{
    int p = root;
    for (int i = 23; i >= 0; i -- )
    {
        int v = C >> i & 1;
        if (max_id[tr[p][v ^ 1]] >= L) p = tr[p][v ^ 1];
        else p = tr[p][v];
    }

    return C ^ s[max_id[p]];
}

int main()
{
    scanf("%d%d", &n, &m);

    max_id[0] = -1;
    root[0] = ++ idx;
    insert(0, 23, 0, root[0]);

    for (int i = 1; i <= n; i ++ )
    {
        int x;
        scanf("%d", &x);
        s[i] = s[i - 1] ^ x;
        root[i] = ++ idx;
        insert(i, 23, root[i - 1], root[i]);
    }

    char op[2];
    int l, r, x;
    while (m -- )
    {
        scanf("%s", op);
        if (*op == 'A')
        {
            scanf("%d", &x);
            n ++ ;
            s[n] = s[n - 1] ^ x;
            root[n] = ++ idx;
            insert(n, 23, root[n - 1], root[n]);
        }
        else
        {
            scanf("%d%d%d", &l, &r, &x);
            printf("%d\n", query(root[r - 1], s[n] ^ x, l - 1));
        }
    }

    return 0;
}

作者：yxc
链接：https://www.acwing.com/activity/content/code/content/168518/
来源：AcWing
~~~

