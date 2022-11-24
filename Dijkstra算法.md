> # Dijkstra算法

> ## 1.朴素Dijkstra   ==n是点数,m是边数==
>
> >适用于==稠密图==，也就是边相对点较多的图   寻找最短路***==（权重皆为正）==***
> >
> >时间复杂度为==O(n^n)==
> >
> >每次遍历一边所有元素所有找到==t的时间复杂度为O(n^n)==
> >
> >然后在更新边的时候时间复杂度为==O(m)==
> >
> >所以总体长度为==O(n^n)==

>首先核心伪代码
>
>```c++
>dist[x]=0x3f3f3f3f;
>dist[1]=0;
>for(1:n)
>{
> t  //最靠近原点且不在标记中的点
> s <- t  // 将t加入其中
> //用t更新其它点的距离
>}
>```
>
>图片奉上
>
><img src="C:/Typora(Markdown)/%E6%9C%B4%E7%B4%A0Dijkstra.png" alt="朴素Dijkstra" style="zoom: 80%;" />
>
>so                  ==我们可以弄到核心代码了==
>
>```c++
>int dijkstra()
>{
>  memset(dist, 0x3f,sizeof dist);
>  dist[1] = 0;
>
>  for(int i = 0; i < n; i++)
>  {
>    int t = -1;
>    for(int j = 1 ; j <= n; j ++ )
>    {
>      if(!st[j]&&(t == -1||dist[t]>dist[j]))//遍历 dist 数组，找到没有确定最短路径的节点中距离源点最近的点t
>      {
>        t = j;
>      }
>    }
>    st[t] = true;
>    for(int j = h[t];j != -1 ;j = ne[j] )//遍历 t 所有可以到达的节点 i
>    {
>      int x = e[j];
>      dist[x] = min(dist[x], dist[t] + w[j]);
>    }
>  }
>  if(dist[n] == 0x3f3f3f3f)return -1;
>  else return dist[n];
>}
>```
>
>最后判断那里是看是否是无穷大如果是那么就不能到那点
>
>当然核心代码可以用==邻接矩阵==做,因为点少可以直接开二维数组
>
>==但是==后面的堆优化的Dijkstra是邻接表为了方便记忆咱就用==邻接表==来做
>
>最后核心代码奉上
>
>```c++
>#include<iostream>
>#include<cstring>
>#include<algorithm>
>
>using namespace std;
>
>const int N=1e5+10;
>int h[N],e[N],w[N],ne[N],idx;
>int dist[N];
>int n,m;
>bool st[N];
>void add(int a,int b,int c)
>{
>  e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
>}
>
>int dijkstra()
>{
>  memset(dist, 0x3f,sizeof dist);
>  dist[1] = 0;
>
>  for(int i = 0; i < n; i++)
>  {
>    int t = -1;
>    for(int j = 1 ; j <= n; j ++ )
>    {
>      if(!st[j]&&(t == -1||dist[t]>dist[j]))//遍历 dist 数组，找到没有确定最短路径的节点中距离源点最近的点t
>      {
>        t = j;
>      }
>    }
>    st[t] = true;
>    for(int j = h[t];j != -1 ;j = ne[j] )//遍历 t 所有可以到达的节点 i
>    {
>      int x = e[j];
>      dist[x] = min(dist[x], dist[t] + w[j]);
>    }
>  }
>  if(dist[n] == 0x3f3f3f3f)return -1;
>  else return dist[n];
>}
>
>int main()
>{
>  cin >> n >> m;
>
>  memset(h, -1, sizeof h);
>
>  for(int i = 0; i < m; i ++ )
>  {
>    int a, b, c;
>    cin >> a >> b >> c;
>    add(a,b,c);
>  }
>  int t = dijkstra();
>  cout << t;
>  return 0;
>}
>
>
>```
>
>==int h[N],e[N],w[N],ne[N],idx;==这个是典型的邻接表 w代表权重
>
>第一重for循环的是==为了找到距离1号点最近且没有被使用过的点t==

> ## 2.堆优化的Dijkstra算法
>
> > ### 适用于稀疏图也就是点和边差不多的图
> >
> > 时间复杂度为==O(n+mlogn)==     m是边数，n是点数
> >
> > 当点数较多的时候，==朴素Dijkstra==的时间复杂度很高,因此我们可以换一种方法进行
>
> 堆优化的核心和朴素差不了多少但是仍然有差距
>
> 他的重点是逐渐更新==找到未使用且距离1号点尽可能近的点==
>
> 所以可以用堆优化==每次只取出来距离1号点最近的点==
>
> 因此要创建一个小根堆每次取出==顶点==即可
>
> ==核心代码如下==
>
> ```c++
> priority_queue<PII,vector<PII>,greater<PII>>q;
> while(!q.empty())
> {
> 	auto t = q.top();
> 	q.pop();
> 	int ver = t.second,distance = t.first;//因为pair的排序是根据first排的要把距离放在第一位
> 	//而编号放在第二位
> 	if(st[ver])continue;//该点使用过就换下一个
> 	st[ver] = true;
> 	for(int i = h[ver]; i != -1; i ++ )
> 	{
> 		int j = e[i];
> 		if(dist[j] > dist[ver]+w[i])//或者distance 可用为wdist[ver]替换也行
> 		{
> 			dist[j] = dist[ver] + w[i];
> 			q.push({dist[j],j});//距离变小则进入堆中，以便找到以该节点到目标节点最小距离
> 		}
> 	}
> }
> ```
>
> 使用小根堆的好处就是优化了找到t的时间==从O(n^n)变为了O(1)==
>
> 只不过更新边长的时间复杂度==从O(m)变为了O(m*logn)==
>
> <img src="C:/Typora(Markdown)/%E5%A0%86%E4%BC%98%E5%8C%96Dijkstra.png" alt="堆优化Dijkstra" style="zoom:50%;" />
>
> 最后核心代码奉上
>
> ```c++
> #include<iostream>
> #include<cstring>
> #include<queue>
> 
> using namespace std;
> typedef pair<int,int> PII;
> const int N=1e6+10;
> int n, m;
> int h[N], w[N], e[N], ne[N], idx;//w是权重
> int dist[N];
> bool st[N];
> 
> void add(int a,int b, int c)
> { 
>   e[idx] = b, ne[idx] = h[a], w[idx]=c, h[a] = idx ++;//利用邻接表存储n和m都较多
> }
> int dijkstra()
> {
>   memset(dist, 0x3f, sizeof dist);
>   dist[1] = 0;
> 
>   priority_queue<PII,vector<PII>,greater<PII>>heap;//创建小根堆
>   heap.push({0,1});//第一个是距离，第二个是编号
>   //因为pair是以第一个为关键进行比较的要找到距离最近的那个那么就要将距离放在第二位
>   while(heap.size())
>   {
>     auto t = heap.top();
>     heap.pop();
> 
>     int ver = t.second ,distance = t.first;
>     if(st[ver]) continue;//如果这个出现过 
>     st[ver] = true;//该点未出现过就加入
>     for(int i = h[ver]; i != -1; i = ne[i])
>     {
>       int j = e[i];
>       if(dist[j] > distance + w[i])
>       {
>         dist[j] = distance + w[i];
>         heap.push({dist[j],j});
>       }
>     }
>   }
>   if(dist[n] == 0x3f3f3f3f)return -1;
>   return dist[n];
> }
> int main()
> {
>   cin >> n >> m;
>   
>   memset(h, -1, sizeof h);
> 
>   while(m--)
>   {
>     int a, b, c;
>     cin >> a >> b >>c;
>     add(a,b,c);
>   }
>   int t = dijkstra();
>   cout << t;
>   return 0;
> }
> ```
>
> 

