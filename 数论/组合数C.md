# 求组合数



组合数的公式：
$$
C_n^k=C_{n-1}^k+C_{n-1}^{k-1}
$$




* 递推法求组合数$C_n^k$

~~~c++
int c[N][N];

void init()
{
    for(int i=0;i<n;i++)
        for(int j=0;j<=i&&j<k;j++)
            if(!j)c[i][j]=1;
    		else c[i][j]=c[i-1][j-1]+c[i-1][j];
}
~~~



* 逆元法求组合数$C^k_n$和排列数$A_n^k$。配合快速幂使用，模数必须是质数

~~~c++
int fact[N],infact[N];//阶乘和它的倒数(这里可以用逆元)

int fpow(int a,int b,int c);//快速幂略

int C(int n,int k)
{
    if(n<k)return 0;
    return (LL)fact[n]*infact[n-k]*infact[k]%mod;
}

int A(int n,int k)
{
    if(n<k)return 0;
    return (LL)fact[n]*infact[n-k]%mod;
}

void init()
{
    fact[0]=infact[0]=1;
    for(int i=1;i<N;i++)
    {
        fact[i]=(LL)fact[i-1]*i%mod;
        infact[i]=(LL)infact[i-1]*fpow(i,mod-2,mod)%mod;//费马小定理
        //infact[i]=fpow(fact[i],mod-2);
    }
}
~~~



* Lucas定理求组合数$C_n^k\mod p$（p为质数）

~~~c++
int Lucas(int n,int k)
{
    if(n<p&&k<p)return C(n,k);
    return (LL)Lucas(n/p,k/p)*C(n%p,k%p)%p;
}
~~~



* Catalan数（迷惑）

一个迷惑的数列，大致表示为：有两种操作，分别记作01，任意时刻0操作的次数不能超过1操作。满足该条件下一共操作n次有多少种方法

从第零项开始，卡特兰数的前几项为：

$1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786, 208012, 742900, 2674440, 9694845, 35357670,\\ 129644790, 477638700, 1767263190, 6564120420, 24466267020, 91482563640, 343059613650,\\ 1289904147324, 4861946401452, ...$

递归定义：
$$
f_n=f_0f_{n-1}+f_1f_{n-2}+...+f_{n-1}f_0，其中n≥2
$$
递推关系：
$$
f_n=\frac{4n-2}{n+1}f_{n-1}
$$
通项公式：
$$
f_n=\frac{1}{n+1}C_{2n}^n=C_{2n}^n-C_{2n}^{n-1}
$$
