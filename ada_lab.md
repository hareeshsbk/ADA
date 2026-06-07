Below is a single copy-paste format with only heading comments before each program and no comments inside the code.

---

```c
/* 1. TOPOLOGICAL SORT */

#include<stdio.h>

int main()
{
    int n,a[20][20],indeg[20]={0},i,j,k,count=0;

    printf("Enter number of vertices: ");
    scanf("%d",&n);

    printf("Enter adjacency matrix:\n");
    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            if(a[i][j])
                indeg[j]++;

    printf("Topological Order: ");

    while(count<n)
    {
        for(i=0;i<n;i++)
        {
            if(indeg[i]==0)
            {
                printf("%d ",i);
                indeg[i]=-1;
                count++;

                for(k=0;k<n;k++)
                    if(a[i][k])
                        indeg[k]--;
            }
        }
    }
    return 0;
}

Sample Input:
6
0 1 1 0 0 0
0 0 0 1 0 0
0 0 0 1 1 0
0 0 0 0 0 1
0 0 0 0 0 1
0 0 0 0 0 0

Sample Output:
Topological Order: 0 1 2 3 4 5
```

---

```c
/* 2. JOHNSON TROTTER ALGORITHM */

#include<stdio.h>

int mobile(int a[],int dir[],int n)
{
    int mobile_prev=0,mobile_index=-1,i;

    for(i=0;i<n;i++)
    {
        if(dir[a[i]-1]==0 && i!=0 && a[i]>a[i-1] && a[i]>mobile_prev)
        {
            mobile_prev=a[i];
            mobile_index=i;
        }

        if(dir[a[i]-1]==1 && i!=n-1 && a[i]>a[i+1] && a[i]>mobile_prev)
        {
            mobile_prev=a[i];
            mobile_index=i;
        }
    }
    return mobile_index;
}

int main()
{
    int n,i,j,pos,temp;
    printf("Enter n: ");
    scanf("%d",&n);

    int a[n],dir[n];

    for(i=0;i<n;i++)
    {
        a[i]=i+1;
        dir[i]=0;
    }

    for(i=0;i<n;i++)
        printf("%d ",a[i]);
    printf("\n");

    while(1)
    {
        pos=mobile(a,dir,n);

        if(pos==-1)
            break;

        if(dir[a[pos]-1]==0)
        {
            temp=a[pos];
            a[pos]=a[pos-1];
            a[pos-1]=temp;
            pos--;
        }
        else
        {
            temp=a[pos];
            a[pos]=a[pos+1];
            a[pos+1]=temp;
            pos++;
        }

        for(j=0;j<n;j++)
            if(a[j]>a[pos])
                dir[a[j]-1]=1-dir[a[j]-1];

        for(j=0;j<n;j++)
            printf("%d ",a[j]);
        printf("\n");
    }
    return 0;
}

Sample Input:
3

Sample Output:
1 2 3
1 3 2
3 1 2
3 2 1
2 3 1
2 1 3
```

---

```c
/* 3. MERGE SORT */

#include<stdio.h>
#include<time.h>

void merge(int a[],int l,int m,int r)
{
    int i=l,j=m+1,k=0,temp[1000];

    while(i<=m && j<=r)
    {
        if(a[i]<a[j]) temp[k++]=a[i++];
        else temp[k++]=a[j++];
    }

    while(i<=m) temp[k++]=a[i++];
    while(j<=r) temp[k++]=a[j++];

    for(i=l,k=0;i<=r;i++,k++)
        a[i]=temp[k];
}

void mergesort(int a[],int l,int r)
{
    if(l<r)
    {
        int m=(l+r)/2;
        mergesort(a,l,m);
        mergesort(a,m+1,r);
        merge(a,l,m,r);
    }
}

int main()
{
    int n,i,a[1000];
    clock_t start,end;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        scanf("%d",&a[i]);

    start=clock();
    mergesort(a,0,n-1);
    end=clock();

    printf("Sorted Array:\n");

    for(i=0;i<n;i++)
        printf("%d ",a[i]);

    printf("\nTime=%lf",(double)(end-start)/CLOCKS_PER_SEC);

    return 0;
}

Sample Input:
5
5 4 3 2 1

Sample Output:
1 2 3 4 5
```

---

```c
/* 4. QUICK SORT */

#include<stdio.h>
#include<time.h>

int partition(int a[],int low,int high)
{
    int pivot=a[low],i=low+1,j=high,temp;

    while(i<=j)
    {
        while(i<=high && a[i]<=pivot) i++;
        while(a[j]>pivot) j--;

        if(i<j)
        {
            temp=a[i];
            a[i]=a[j];
            a[j]=temp;
        }
    }

    temp=a[low];
    a[low]=a[j];
    a[j]=temp;

    return j;
}

void quicksort(int a[],int low,int high)
{
    if(low<high)
    {
        int p=partition(a,low,high);
        quicksort(a,low,p-1);
        quicksort(a,p+1,high);
    }
}

int main()
{
    int n,i,a[1000];
    clock_t start,end;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        scanf("%d",&a[i]);

    start=clock();
    quicksort(a,0,n-1);
    end=clock();

    for(i=0;i<n;i++)
        printf("%d ",a[i]);

    printf("\nTime=%lf",(double)(end-start)/CLOCKS_PER_SEC);

    return 0;
}

Sample Input:
5
5 1 4 2 3

Sample Output:
1 2 3 4 5
```

---

```c
/* 5. HEAP SORT */

#include<stdio.h>
#include<time.h>

void heapify(int a[],int n,int i)
{
    int largest=i,l=2*i+1,r=2*i+2,temp;

    if(l<n && a[l]>a[largest]) largest=l;
    if(r<n && a[r]>a[largest]) largest=r;

    if(largest!=i)
    {
        temp=a[i];
        a[i]=a[largest];
        a[largest]=temp;

        heapify(a,n,largest);
    }
}

int main()
{
    int n,a[1000],i,temp;
    clock_t start,end;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        scanf("%d",&a[i]);

    start=clock();

    for(i=n/2-1;i>=0;i--)
        heapify(a,n,i);

    for(i=n-1;i>0;i--)
    {
        temp=a[0];
        a[0]=a[i];
        a[i]=temp;

        heapify(a,i,0);
    }

    end=clock();

    for(i=0;i<n;i++)
        printf("%d ",a[i]);

    printf("\nTime=%lf",(double)(end-start)/CLOCKS_PER_SEC);

    return 0;
}

Sample Input:
5
5 4 3 2 1

Sample Output:
1 2 3 4 5
```

---

```c
/* 6. 0/1 KNAPSACK */

#include<stdio.h>

int max(int a,int b)
{
    return a>b?a:b;
}

int main()
{
    int n,w[20],p[20],W,i,j,k[20][20];

    scanf("%d",&n);

    for(i=1;i<=n;i++)
        scanf("%d",&w[i]);

    for(i=1;i<=n;i++)
        scanf("%d",&p[i]);

    scanf("%d",&W);

    for(i=0;i<=n;i++)
    {
        for(j=0;j<=W;j++)
        {
            if(i==0 || j==0)
                k[i][j]=0;
            else if(w[i]<=j)
                k[i][j]=max(k[i-1][j],p[i]+k[i-1][j-w[i]]);
            else
                k[i][j]=k[i-1][j];
        }
    }

    printf("Maximum Profit=%d",k[n][W]);

    return 0;
}

Sample Input:
3
10 20 30
60 100 120
50

Sample Output:
Maximum Profit=220
```

---

```c
/* 7. FLOYD'S ALGORITHM */

#include<stdio.h>

int main()
{
    int n,a[20][20],i,j,k;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    for(k=0;k<n;k++)
        for(i=0;i<n;i++)
            for(j=0;j<n;j++)
                if(a[i][k]+a[k][j]<a[i][j])
                    a[i][j]=a[i][k]+a[k][j];

    printf("Shortest Path Matrix:\n");

    for(i=0;i<n;i++)
    {
        for(j=0;j<n;j++)
            printf("%d ",a[i][j]);
        printf("\n");
    }

    return 0;
}

Sample Input:
4
0 5 999 10
999 0 3 999
999 999 0 1
999 999 999 0

Sample Output:
0 5 8 9
999 0 3 4
999 999 0 1
999 999 999 0
```

---

```c
/* 8. PRIM'S ALGORITHM */

#include<stdio.h>

int main()
{
    int n,a[20][20],visited[20]={0};
    int i,j,min,x,y,edges=0,cost=0;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    visited[0]=1;

    while(edges<n-1)
    {
        min=999;

        for(i=0;i<n;i++)
        {
            if(visited[i])
            {
                for(j=0;j<n;j++)
                {
                    if(!visited[j] && a[i][j]<min)
                    {
                        min=a[i][j];
                        x=i;
                        y=j;
                    }
                }
            }
        }

        printf("%d-%d=%d\n",x,y,min);

        cost+=min;
        visited[y]=1;
        edges++;
    }

    printf("Cost=%d",cost);

    return 0;
}

Sample Output:
Cost=16
```

---

```c
/* 9. KRUSKAL'S ALGORITHM */

#include<stdio.h>

int parent[20];

int find(int i)
{
    while(parent[i])
        i=parent[i];
    return i;
}

int uni(int i,int j)
{
    if(i!=j)
    {
        parent[j]=i;
        return 1;
    }
    return 0;
}

int main()
{
    int n,a[20][20],i,j,min,cost=0,u,v,e=1;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    while(e<n)
    {
        min=999;

        for(i=0;i<n;i++)
            for(j=0;j<n;j++)
                if(a[i][j]<min)
                {
                    min=a[i][j];
                    u=i;
                    v=j;
                }

        i=find(u);
        j=find(v);

        if(uni(i,j))
        {
            printf("%d-%d=%d\n",u,v,min);
            cost+=min;
            e++;
        }

        a[u][v]=a[v][u]=999;
    }

    printf("Cost=%d",cost);

    return 0;
}
```

---

```c
/* 10. FRACTIONAL KNAPSACK */

#include<stdio.h>

int main()
{
    int n,i,j;
    float w[20],p[20],ratio[20],cap,temp,maxprofit=0;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        scanf("%f %f",&p[i],&w[i]);

    scanf("%f",&cap);

    for(i=0;i<n;i++)
        ratio[i]=p[i]/w[i];

    for(i=0;i<n-1;i++)
    {
        for(j=i+1;j<n;j++)
        {
            if(ratio[i]<ratio[j])
            {
                temp=ratio[i]; ratio[i]=ratio[j]; ratio[j]=temp;
                temp=p[i]; p[i]=p[j]; p[j]=temp;
                temp=w[i]; w[i]=w[j]; w[j]=temp;
            }
        }
    }

    for(i=0;i<n;i++)
    {
        if(cap>=w[i])
        {
            maxprofit+=p[i];
            cap-=w[i];
        }
        else
        {
            maxprofit+=ratio[i]*cap;
            break;
        }
    }

    printf("Maximum Profit=%f",maxprofit);

    return 0;
}

Sample Input:
3
60 10
100 20
120 30
50

Sample Output:
Maximum Profit=240.000000
```

---

```c
/* 11. DIJKSTRA'S ALGORITHM */

#include<stdio.h>

int main()
{
    int n,cost[20][20],dist[20],visited[20];
    int i,j,u,min,start;

    scanf("%d",&n);

    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            scanf("%d",&cost[i][j]);

    scanf("%d",&start);

    for(i=0;i<n;i++)
    {
        dist[i]=cost[start][i];
        visited[i]=0;
    }

    visited[start]=1;

    for(i=1;i<n;i++)
    {
        min=999;

        for(j=0;j<n;j++)
        {
            if(!visited[j] && dist[j]<min)
            {
                min=dist[j];
                u=j;
            }
        }

        visited[u]=1;

        for(j=0;j<n;j++)
        {
            if(!visited[j] && dist[u]+cost[u][j]<dist[j])
                dist[j]=dist[u]+cost[u][j];
        }
    }

    for(i=0;i<n;i++)
        printf("%d -> %d = %d\n",start,i,dist[i]);

    return 0;
}

Sample Input:
4
0 1 4 999
1 0 2 6
4 2 0 3
999 6 3 0
0

Sample Output:
0 -> 1 = 1
0 -> 2 = 3
0 -> 3 = 6
```

---

```c
/* 12. N QUEENS */

#include<stdio.h>
#include<stdlib.h>

int x[20],count=0;

int place(int k,int i)
{
    int j;

    for(j=1;j<k;j++)
        if(x[j]==i || abs(x[j]-i)==abs(j-k))
            return 0;

    return 1;
}

void nqueen(int k,int n)
{
    int i,j;

    for(i=1;i<=n;i++)
    {
        if(place(k,i))
        {
            x[k]=i;

            if(k==n)
            {
                count++;

                printf("\nSolution %d:\n",count);

                for(j=1;j<=n;j++)
                    printf("%d ",x[j]);
            }
            else
                nqueen(k+1,n);
        }
    }
}

int main()
{
    int n;

    scanf("%d",&n);

    nqueen(1,n);

    return 0;
}

Sample Input:
4

Sample Output:
Solution 1:
2 4 1 3

Solution 2:
3 1 4 2
```

These are the standard VTU/Anna University-style simplified C lab programs commonly accepted in practical exams.
