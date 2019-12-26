#### :star: 1.UVa455
```c++
#include <iostream>
#include<string.h>
using namespace std;

int main()
{
    string s="";
    while(cin>>s)
    {

        int len = s.length();
        for(int i = 1;i<=len;i++) //i代表周期，++i表示
        {
            int flag=1;//代表是否具有周期
            for(int j = i;j<len;j++)
                if(s[j]!=s[j%i]) {flag=0; break;}
            if(flag) { cout<<i;   break;} //如果有周期则输出该周期，这个周期就是最小周期。
//            if(flag) { cout<<i;  }
        }
    }
    return 0;
}

/*
1. 扩展：找到最大周期，找到第二大周期
2. 理解：关键点   if(s[j]!=s[j%i]) {flag=0; break;}  j%i，达到从第一个数开始比较的目的。
*/

```



