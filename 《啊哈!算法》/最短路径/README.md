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
