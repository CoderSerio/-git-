### 归并排序-merge sort ###

归并排序的主要思想是**分治**，将一个数组不断二分，递归，然后不断组合

稍微画一下图解

![归并图解](./picture\归并图解.png)

额。确实之前有点高估我的艺术细胞了。。。

归并排序的效率应该算是达到了巅峰：**时间复杂度**为O(nlogn)

代码附上

```c++
#include <iostream>
using namespace std;
int a[101010],b[101010];
void merge(int a[],int left,int right)
{
    if(left >= right)
    {
        return ;
    }
    int mid=(left+right)/2;
    merge(a,left,mid);merge(a,mid+1,right);
    int l=left,r=mid+1,x=0,i;
    while(l<=mid && r<=right)
    {
        if(a[l]<=a[r])
        {
            b[x++]=a[l++];
        }
        else
        {
            b[x++]=a[r++];
        }
    }
    while(l<=mid)
    {
        b[x++]=a[l++];
    }
    while(r<=right)
    {
        b[x++]=a[r++];
    }
    for(l=left,r=0;l<=right;l++,r++)
    {
        a[l]=b[r];
    }
}
int main()
{
    int n,i;
    cin >> n;
    for(i=0;i<n;i++)
    {
        cin >> a[i];
    }
    merge(a,0,n-1);
    for(i=0;i<n;i++)
    {
        cout << a[i] << ' ';
    }
    return 0;
}
```

-------------------------------------------------------------------------------------------------------------------------------