---
layout: post
title:  "알고리즘 다익스트라"
date:   2018-03-29 10:22:00 +0900
categories: jekyll update
---

---
### 다익스트라 알고리즘
출발 지점을 하나 선택하면 나머지 다른 모든 지점까지의 최단 경로를 알려주는 단일 시작점 최단 경로 알고리즘이다. 프림 알고리즘과 마찬가지로 최단 경로의 정점을 선택하는 방법이 알고리즘의 성능을 좌우한다. 간선<u,w>의 가중치를 weight(u, w)라 하면.
```
distance(w) = minumum{distance(w), distance(u) + weight(u, w)}
earlier(w) = u if distance(w)가 변경
```

```
void dijkstra_GM(int graph[][MAX], int n, int s)
{
  bool SP_V[MAX];
  int distance[MAX], earlier[MAX], i, u, w;
  for(i = 0; i < n; i++)//초기화
  {
    SP_V[i] = false;
    distance[i] = graph[s][i];
    earlier[i] = s;
  }
  SP_V[s] = true;
  loop(n-2) //n-2번 반복
  {
    u = select_min(distance, n, SP_V);
    SP_V[u] = true;
    for(w = 0; w < n; w++)
    {
      if(SP_V[w] == flase)
      {
        if(distance[u]+graph[u][w] < distance[w])
        {
          distance[w] = distance[u] + graph[u][w];
          earlier[w] = u;
        }
      }
    }
  }
}
```

프림 알고리즘과 다른 부분은 변수 이름을 제외하고는 distance정의와 관련된 안쪽 if문 뿐이다. select_min함수도 프림알고리즘과 같다. 복잡도 역시 Θ(n^2)이다.

---
### 활용과 한계

그래프의 간선 가중치가 음수가 된다면 이미 결정했던 최단 경로가 바뀌게 되어 알고리즘의 원칙이 깨지게 된다.
