> # –                             N皇后问题

>首先对于n皇后，他不能在同一行列，和对角线，那么我们需要**多个数组**进行标记 行列的使用情况
>
>==bool  row[N],col[N],dg[N],udg[N]==    col是列，dg是对角线，udg是反对角线
>
>

>判断代码
>
>>`if(!row[x]&&!col[y]&&!dg[y-x+n]&&!udg[x+y])`
>>
>>//dg相当于y=x+c  c=y-x 但是这里==y-x可能为负数所以加上n== ，udg相当于==y=-x+c  c=x+y==
>>
>><img src="C:/Typora(Markdown)/ChichengL/n%E7%9A%87%E5%90%8E.png" alt="n皇后" style="zoom:50%;" />

> 一个格子有两种情况，放皇后和不放皇后
>
> 那么我们可以直接分别递归这两种情况
>
> //不放皇后
>
> > ==dfs(x,y+1,u);==
>
> //放皇后
>
> 切记==放皇后==要满足之前的==判断条件==
>
> ### so
>
> 
>
> >```c++
> >if(!row[x]&&!col[y]&&!dg[y-x+n]&&!udg[x+y])
> >
> > {
> >
> >  g[x][y]='Q';
> >
> >  row[x]=col[y]=udg[x+y]=dg[n - x + y]=true;
> >
> >  dfs(x,y+1,u+1);
> >
> >  row[x]=col[y]=udg[x+y]=dg[n - x + y]=false;
> >
> >  g[x][y]='.';
> >
> > }
> >```

>最后 还需要判断如果到了最后一层了
>
>再判断是否放完，放完了就可以输出
>
>```c++
>if(y==n)y=0,x++;
>  if(x==n)
>  {
>    if(u==n)
>    {
>      for(int i=0;i < n;i ++) puts(g[i]);
>      puts("");
>    }
>    return ;
>  }
>```
>
>

> 最后最终代码奉上来
>
> > **g**存储的图  
> >
> > 1.==row就是判断该行是否有重复==  ==col就是判断该列是否有重复==比如x，y放了皇后==row  col就会改变==导致这一排无法再放皇后
>
> ```c++
> #include<iostream>
> using namespace std;
> 
> const int N=20;
> bool row[N],col[N],dg[N],udg[N];
> char g[N][N];
> int n;
> void dfs(int x,int y,int u)
> {
>   if(y==n)y=0,x++;
>   if(x==n)
>   {
>     if(u==n)
>     {
>       for(int i=0;i < n;i ++)puts(g[i]);
>       puts("");
>     }
>     return ;
>   }
>   //不放皇后
>   dfs(x,y+1,u);
> 
>   //放皇后
>   if(!row[x]&&!col[y]&&!dg[y-x+n]&&!udg[x+y])//dg相当于y=x+c  c=y-x 但是这里y-x可能为负数所以加上n udg相当于y=-x+c  c=x+y
>   {
>     g[x][y]='Q';
>     row[x]=col[y]=udg[x+y]=dg[n - x + y]=true;
>     dfs(x,y+1,u+1);
>     row[x]=col[y]=udg[x+y]=dg[n - x + y]=false;
>     g[x][y]='.';
>   }
> }
> int main()
> {
>   cin>>n;
>   for(int i=0;i<n;i++)
>     for(int j=0;j<n;j++)
>       g[i][j]='.';
>   dfs(0,0,0);
>   return 0;
> }
> ```
>
> 