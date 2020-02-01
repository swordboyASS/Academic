#### :star: 1.Floyd-Warshall

```c++
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
```
