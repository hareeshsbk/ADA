Here are **simple C programs** (lab-exam style) for each algorithm.

---

# 1. Topological Ordering of a Directed Graph

```c
#include <stdio.h>

int main() {
    int n, a[10][10], indegree[10] = {0};
    int i, j, count = 0;

    printf("Enter number of vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for(i=0;i<n;i++)
        for(j=0;j<n;j++) {
            scanf("%d",&a[i][j]);
            if(a[i][j])
                indegree[j]++;
        }

    printf("Topological Order: ");

    while(count < n) {
        for(i=0;i<n;i++) {
            if(indegree[i] == 0) {
                printf("%d ", i);

                indegree[i] = -1;
                count++;

                for(j=0;j<n;j++)
                    if(a[i][j])
                        indegree[j]--;
            }
        }
    }
    return 0;
}
```

---

# 2. Johnson-Trotter Algorithm (Permutations)

```c
#include <stdio.h>

void print(int p[], int n) {
    for(int i=0;i<n;i++)
        printf("%d ", p[i]);
    printf("\n");
}

int main() {
    int n;
    printf("Enter n: ");
    scanf("%d",&n);

    int p[10], dir[10];

    for(int i=0;i<n;i++) {
        p[i]=i+1;
        dir[i]=-1;
    }

    print(p,n);

    while(1) {
        int mobile=0,pos=-1;

        for(int i=0;i<n;i++) {
            int next=i+dir[i];

            if(next>=0 && next<n && p[i]>p[next]) {
                if(p[i]>mobile) {
                    mobile=p[i];
                    pos=i;
                }
            }
        }

        if(mobile==0) break;

        int swapPos=pos+dir[pos];

        int temp=p[pos];
        p[pos]=p[swapPos];
        p[swapPos]=temp;

        temp=dir[pos];
        dir[pos]=dir[swapPos];
        dir[swapPos]=temp;

        for(int i=0;i<n;i++)
            if(p[i]>mobile)
                dir[i]=-dir[i];

        print(p,n);
    }
}
```

---

# 3. Merge Sort with Time Calculation

```c
#include <stdio.h>
#include <time.h>

void merge(int a[], int l, int m, int r) {
    int i=l,j=m+1,k=0,temp[1000];

    while(i<=m && j<=r)
        temp[k++] = (a[i]<a[j]) ? a[i++] : a[j++];

    while(i<=m) temp[k++]=a[i++];
    while(j<=r) temp[k++]=a[j++];

    for(i=l,k=0;i<=r;i++,k++)
        a[i]=temp[k];
}

void mergesort(int a[], int l, int r) {
    if(l<r) {
        int m=(l+r)/2;
        mergesort(a,l,m);
        mergesort(a,m+1,r);
        merge(a,l,m,r);
    }
}

int main() {
    int n,a[1000];
    clock_t start,end;

    printf("Enter n: ");
    scanf("%d",&n);

    for(int i=0;i<n;i++)
        scanf("%d",&a[i]);

    start=clock();
    mergesort(a,0,n-1);
    end=clock();

    printf("Sorted array:\n");
    for(int i=0;i<n;i++)
        printf("%d ",a[i]);

    printf("\nTime = %lf seconds",
           (double)(end-start)/CLOCKS_PER_SEC);
}
```

---

# 4. Quick Sort with Time Calculation

```c
#include <stdio.h>
#include <time.h>

int partition(int a[], int low, int high) {
    int pivot=a[high];
    int i=low-1;

    for(int j=low;j<high;j++) {
        if(a[j] < pivot) {
            i++;
            int t=a[i];
            a[i]=a[j];
            a[j]=t;
        }
    }

    int t=a[i+1];
    a[i+1]=a[high];
    a[high]=t;

    return i+1;
}

void quicksort(int a[], int low, int high) {
    if(low<high) {
        int p=partition(a,low,high);
        quicksort(a,low,p-1);
        quicksort(a,p+1,high);
    }
}

int main() {
    int n,a[1000];
    clock_t start,end;

    scanf("%d",&n);

    for(int i=0;i<n;i++)
        scanf("%d",&a[i]);

    start=clock();
    quicksort(a,0,n-1);
    end=clock();

    printf("Sorted Array:\n");
    for(int i=0;i<n;i++)
        printf("%d ",a[i]);

    printf("\nTime=%lf sec",
           (double)(end-start)/CLOCKS_PER_SEC);
}
```

---

# 5. Heap Sort

```c
#include <stdio.h>
#include <time.h>

void heapify(int a[], int n, int i) {
    int largest=i;
    int l=2*i+1;
    int r=2*i+2;

    if(l<n && a[l]>a[largest])
        largest=l;

    if(r<n && a[r]>a[largest])
        largest=r;

    if(largest!=i) {
        int t=a[i];
        a[i]=a[largest];
        a[largest]=t;

        heapify(a,n,largest);
    }
}

void heapsort(int a[], int n) {
    for(int i=n/2-1;i>=0;i--)
        heapify(a,n,i);

    for(int i=n-1;i>0;i--) {
        int t=a[0];
        a[0]=a[i];
        a[i]=t;

        heapify(a,i,0);
    }
}

int main() {
    int n,a[1000];
    clock_t start,end;

    scanf("%d",&n);

    for(int i=0;i<n;i++)
        scanf("%d",&a[i]);

    start=clock();
    heapsort(a,n);
    end=clock();

    printf("Sorted Array:\n");
    for(int i=0;i<n;i++)
        printf("%d ",a[i]);

    printf("\nTime=%lf sec",
           (double)(end-start)/CLOCKS_PER_SEC);
}
```

---

# 6. 0/1 Knapsack (Dynamic Programming)

```c
#include <stdio.h>

int max(int a,int b){
    return (a>b)?a:b;
}

int main() {
    int n,W;
    int wt[20],val[20],dp[20][20];

    printf("Enter number of items: ");
    scanf("%d",&n);

    printf("Enter weights:\n");
    for(int i=1;i<=n;i++)
        scanf("%d",&wt[i]);

    printf("Enter values:\n");
    for(int i=1;i<=n;i++)
        scanf("%d",&val[i]);

    printf("Enter capacity: ");
    scanf("%d",&W);

    for(int i=0;i<=n;i++) {
        for(int w=0;w<=W;w++) {
            if(i==0 || w==0)
                dp[i][w]=0;
            else if(wt[i]<=w)
                dp[i][w]=max(val[i]+dp[i-1][w-wt[i]],
                             dp[i-1][w]);
            else
                dp[i][w]=dp[i-1][w];
        }
    }

    printf("Maximum Profit = %d",dp[n][W]);
}
```

---

# 7. Floyd's Algorithm

```c
#include <stdio.h>

int main() {
    int n,a[10][10];

    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    for(int k=0;k<n;k++)
        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
                if(a[i][k]+a[k][j] < a[i][j])
                    a[i][j]=a[i][k]+a[k][j];

    printf("Shortest Path Matrix:\n");

    for(int i=0;i<n;i++) {
        for(int j=0;j<n;j++)
            printf("%d ",a[i][j]);
        printf("\n");
    }
}
```

---

# 8. Prim's Algorithm

```c
#include <stdio.h>

int main() {
    int n,a[10][10];
    int selected[10]={0};
    int edges=0,mincost=0;

    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    selected[0]=1;

    while(edges<n-1) {
        int min=999,x=0,y=0;

        for(int i=0;i<n;i++) {
            if(selected[i]) {
                for(int j=0;j<n;j++) {
                    if(!selected[j] && a[i][j]) {
                        if(a[i][j]<min) {
                            min=a[i][j];
                            x=i;
                            y=j;
                        }
                    }
                }
            }
        }

        printf("%d -> %d = %d\n",x,y,min);

        mincost+=min;
        selected[y]=1;
        edges++;
    }

    printf("Minimum Cost=%d",mincost);
}
```

---

# 9. Kruskal's Algorithm

```c
#include <stdio.h>

int parent[20];

int find(int i){
    while(parent[i])
        i=parent[i];
    return i;
}

int uni(int i,int j){
    if(i!=j){
        parent[j]=i;
        return 1;
    }
    return 0;
}

int main() {
    int n,a[20][20];
    int min,mincost=0;

    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            scanf("%d",&a[i][j]);

    int edges=0;

    while(edges<n-1) {
        min=999;

        int u,v,x,y;

        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
                if(a[i][j]<min && a[i][j]!=0) {
                    min=a[i][j];
                    u=x=i;
                    v=y=j;
                }

        u=find(u);
        v=find(v);

        if(uni(u,v)) {
            printf("%d -> %d = %d\n",x,y,min);
            mincost+=min;
            edges++;
        }

        a[x][y]=a[y][x]=999;
    }

    printf("Minimum Cost=%d",mincost);
}
```

---

# 10. Fractional Knapsack

```c
#include <stdio.h>

int main() {
    int n,cap;

    float profit[20],weight[20],ratio[20];

    scanf("%d",&n);

    for(int i=0;i<n;i++) {
        scanf("%f %f",&profit[i],&weight[i]);
        ratio[i]=profit[i]/weight[i];
    }

    scanf("%d",&cap);

    for(int i=0;i<n-1;i++)
        for(int j=i+1;j<n;j++)
            if(ratio[i]<ratio[j]) {
                float t;
                t=ratio[i]; ratio[i]=ratio[j]; ratio[j]=t;
                t=profit[i]; profit[i]=profit[j]; profit[j]=t;
                t=weight[i]; weight[i]=weight[j]; weight[j]=t;
            }

    float total=0;

    for(int i=0;i<n;i++) {
        if(cap>=weight[i]) {
            total+=profit[i];
            cap-=weight[i];
        }
        else {
            total+=profit[i]*(cap/weight[i]);
            break;
        }
    }

    printf("Maximum Profit = %.2f",total);
}
```

---

# 11. Dijkstra's Algorithm

```c
#include <stdio.h>

int main() {
    int n,cost[10][10],dist[10];
    int visited[10]={0},source;

    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            scanf("%d",&cost[i][j]);

    scanf("%d",&source);

    for(int i=0;i<n;i++)
        dist[i]=cost[source][i];

    visited[source]=1;

    for(int count=1;count<n;count++) {
        int min=999,next=-1;

        for(int i=0;i<n;i++)
            if(!visited[i] && dist[i]<min) {
                min=dist[i];
                next=i;
            }

        visited[next]=1;

        for(int i=0;i<n;i++)
            if(!visited[i] &&
               min+cost[next][i] < dist[i])
                dist[i]=min+cost[next][i];
    }

    printf("Shortest Distances:\n");

    for(int i=0;i<n;i++)
        printf("%d -> %d = %d\n",
               source,i,dist[i]);
}
```

---

# 12. N-Queens Problem

```c
#include <stdio.h>
#include <stdlib.h>

int x[20],n;

int place(int k,int i){
    for(int j=1;j<k;j++)
        if(x[j]==i || abs(x[j]-i)==abs(j-k))
            return 0;
    return 1;
}

void queen(int k){
    for(int i=1;i<=n;i++){
        if(place(k,i)){
            x[k]=i;

            if(k==n){
                for(int j=1;j<=n;j++)
                    printf("%d ",x[j]);
                printf("\n");
            }
            else
                queen(k+1);
        }
    }
}

int main(){
    printf("Enter n: ");
    scanf("%d",&n);

    queen(1);
    return 0;
}
```

These versions are commonly accepted in **VTU / Anna University / Polytechnic Design & Analysis of Algorithms lab exams** because they are short, easy to write in record notebooks, and easy to explain during viva.
