# Union-Find 알고리즘

### Union-Find 알고리즘이란?
서로 중복되지 않는 부분 집합 (Disjoint Set)을 표현할 때 사용하는 자료구조

초기화 / 합치기 (Union) / 찾기 (Find) 세 가지 연산을 사용한다

최소 스패닝 트리를 구현하는 크루스칼 알고리즘에 사용되며, 사이클을 만들지 않고 모든 노드를 방문할 수 있다

### Union-Find의 구현
- 초기화 : N 개의 원소가 각각의 집합에 포함되어 있도록 초기화한다 -> 각각을 유일한 원소로 가지는 집합 생성
- Union (합치기) : 두 원소 a, b 가 각각 속한 두 집합을 하나로 합친다 -> 두 개의 집합을 하나로 연결하는 역할
- Find (찾기) : 원소 a 가 주어질 때, 이 원소가 속한 집합을 루트노드를 반환한다 -> a가 어느 집합에 속해있는지 찾는 역할 

### 코드
Init
```
    public static void init(int n){
        parent = new int[n+1]; // 1 ~ n번 노드까지 사용하기 위해 n + 1
        for(int i=0;i<=n;i++){
            parent[i]=i;
        }
    }
```

Find
```
    public static int find(int x){
        // 부모가 자기 자신이라면 최상단노드이므로 반환
        if(x==parent[x]){
            return x;
        }
        else{
            // 부모와 자기 자신이 다르다면 최상단 노드를 찾아가자.
            // 근데 이때 최 상단노드를 값으로 저장하자
            return parent[x]=find(parent[x]);
        }
    }
```

Union
```
    public static void union(int x, int y){
        x = find(x);
        y = find(y);

        if(x<y){
            parent[y]=x;
        }else{
            parent[x]=y;
        }
    }
```

isEqual
```
    public static boolean isEqual(int x, int y){
        x = find(x);
        y = find(y);

        if(x==y) return true;
        else return false;
    }
```
