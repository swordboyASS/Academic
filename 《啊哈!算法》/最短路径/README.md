#### :star: 1.Floyd-Warshall

```c++
/*
弗洛伊德和沃舍尔是2个人，他们独立发明了这个算法
*/
#include <iostream>
#include <stdio.h> 
using namespace std;
int main()
{
	int e[10][10],k,i,j,n,m,t1,t2,t3;
	int inf=99999999;//代表无穷大
	cin>>n>>m;
	for(i=1;i<=n;i++)
		for(j=1;j<=n;j++)
			if(i==j) e[i][j]=0;
			else e[i][j]=inf;
	for(i=1;i<=m;i++) //读入边  
	{
		cin>>t1>>t2>>t3;
		e[t1][t2]=t3; 
	 } 
	
	//Floyd-Wallshall算法核心语句--5行
	for(k=1;k<=n;k++)
		for(i=1;i<=n;i++)
			for(j=1;j<=n;j++)
				if(e[i][j]>e[i][k] + e[k][j])
					e[i][j]= e[i][k]+e[k][j]; 
	for(i=1;i<=n;i++)
	{
		for(j=1;j<=n;j++)
		{
			printf("%10d",e[i][j]);
		}
		cout<<"\n"; 
	}
	
	/*
	4 8
1 2 2
1 3 6
1 4 4
2 3 3
3 1 7
3 4 1
4 1 5
4 3 12
	*/
	return 0;
}
/*
弗洛伊德算法，可以处理带负权边的图，但是不能处理带负权回路的图，因为会造成，每一次的路径都越加越小
*/

```

#### :star: 2.Dijkstra-单源最短路径--某一个点到其余点的最短路径(邻接矩阵存储)
```c++
#include <iostream>
#include <stdio.h>
using namespace std;
int main()
{
	int e[10][10],dis[10],book[10]={0},i,j,k,n,m,t1,t2,t3,u,v,min_;
	int inf=99999999;
	book[1]=1;
	cin>>n>>m;//顶点数
//	scanf("%d%d",&n,&m);
	for(i=1;i<=n;i++)
        for(j=1;j<=n;j++)
            if(i==j) e[i][j]=0;
			else e[i][j]=inf;
        

    

	for(i=1;i<=m;i++)	//输入邻接矩阵
	{
		cin>>t1>>t2>>t3;
		e[t1][t2]=t3;
	 }
	for(i=1;i<=n;i++)	//初始化dis数组 
		dis[i]=e[1][i];
	
	//dijkstra算法核心语句
	for(i=1;i<=n-1;i++) //自身的顶点不参与运算 
	{
		//找到离1号顶点最近的点
		min=inf;
		for(j=1;j<=n;j++)
		{
			if(book[j]==0 && dis[j]<min)
			{
				min=dis[j];
				u=j;
			}
		 } 
		 book[u]=1; //已经参与过的点就是最终确定下来的点，不再作运算。 
		 for(v=1;v<=n;v++)
		 {
		 	if(e[u][v]<inf) //dis[v]代表1到v这个点的距离 
		 	{
		 		if(dis[v]>dis[u]+e[u][v])	//可以处理负权边 
		 		dis[v]=dis[u]+e[u][v];
			 }
		 }
	 } 
	
	for(i=1;i<=n;i++)
		cout<<dis[i]<<" ";
	return 0;
 }
 /*
 dijkstra算法不能处理带有负权边的情况，这是因为基于贪心的原则
 */
```

#### 3.bellman-ford算法初级版,可以处理负权边
```c++
#include <iostream>
using namespace std;
int main()
{
	int dis[10],i,k,n,m,u[10],v[10],w[10];
	int inf =99999999;
	cin>>n>>m;
	for(i=1;i<=m;i++) //使用邻接表存储 
		cin>>u[i]>>v[i]>>w[i];
	for(i=1;i<=n;i++)
		dis[i]=inf;
	dis[1]=0; //到自己的距离为0
	
	//bellman-ford算法核心语句,只需要外层循环n-1次 
	for(k=1;k<=n-1;k++)
	{
		int check=0; 
		for(i=1;i<=m;i++) //从不经过任何顶点，到经过1个，2个.... 
		{
			if(dis[v[i]]>dis[u[i]]+w[i])
			{
				dis[v[i]]=dis[u[i]]+w[i];
				check=1;
			}

		}
		if(check==0) break; //如果dis数组不再有变化，则已经更新完毕，不用再进行循环 
	}

	/*判断是否含有负权回路 
	int flag=0;
	for(k=1;k<=n-1;k++)
		if(dis[v[i]]>dis[u[i]]+w[i]) flag=1;
	if(flag==1) cout<<"此图含有负权回路";
	*/
	
			
	for(i=1;i<=n;i++)
		cout<<dis[i]<<" "; 
	return 0;
}
```

#### 4.bellman-ford算法进阶版,作了一些优化,使用邻接表存储
```c++
#include <iostream>
using namespace std;
int main()
{
	int i,j,k,n,m,u[8],v[8],w[8];
	int inf =99999999;
	//first要比n最大值大1，m要比m最大值大1 
	int first[6],next[8];
	int dis[6]={0},book[6]={0};//book标记数组
	int que[101]={0},head=1,tail=1; //定义一个队列并初始化 
	cin>>n>>m;

	for(i=1;i<=n;i++)
		dis[i]=inf;
	dis[1]=0; //到自己的距离为0
	
	for(i=1;i<=n;i++)	first[i]=-1;
	for(i=1;i<=m;i++)
	{
		cin>>u[i]>>v[i]>>w[i];
		//邻接表的关键 
		next[i]=first[u[i]]; 
		first[u[i]]=i; 
	}
	que[tail]=1; tail++;
	book[1]=1;//标记1号顶点入队 
	while(head<tail)//队列不为空时循环
	{
		k=first[que[head]]; //当前需要处理的队首顶点
		while(k!=-1)//扫描当前顶点的边
		{
			if(dis[v[k]]>dis[u[k]]+w[k])
			{
				dis[v[k]]=dis[u[k]]+w[k];//更新顶点1到v[k]的路程
				//这的book数组用来判断顶点v[k]是否在队列中
				//如果不是使用一个数组来标记的话，判断一个顶点是否在队列中每次都需要
				//从队列的head到tail扫描一遍，很浪费时间。
				if(book[v[k]]==0)
				{
					//入队操作
					que[tail]=v[k];
					tail++;
					book[v[k]]=1; 
				 } 
			}
			k=next[k];
		 } 
		 //出队操作 
		 book[que[head]]=0;
		 head++; 
	 } 
	 for(i=1;i<=n;i++)
	 	cout<<dis[i]<<" "; 
	 
	return 0;
}
/*
用链表来实现邻接表是最好的，不用提前预估大小
*/
```
