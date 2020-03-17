# 2020年2月27日

## 1001



```c++
#include <iostream>
#include <map>
#include <set>
#include <string>
#include <string.h>
#include <cstring>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <queue>
#include <vector>
#include <cctype>
#include <list>
#define REP(i,n) for(int i=0;i<(n);i++)
using namespace std;
typedef long long ll;

int main(){
    int n;
    scanf("%d",&n);
    int step=0;
    while(n!=1){
        if(n&1){
            n=(n*3+1)/2;
        }else{
            n/=2;
        }
        step++;
    }
    printf("%d",step);
	return 0;
}

```

