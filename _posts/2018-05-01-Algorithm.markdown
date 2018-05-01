---
layout: post
title:  "알고리즘 이항계수"
date:   2018-05-01 21:41:00 +0900
categories: jekyll update
---

---
### 문제의 순환적 정의

```
        |----C(n-1, k-1) + C(n-1, k)  if 0<k<n
C(n,k) =|
        |----1           if k = 0 or k = n
```


```
int bin_coef_DP(int n, int k)
{
  int C[MAX][MAX];
  int i, j, m;
  for(i = 0; i <= n; i++)
  {
    m = minimum(i,k);
    for(j = 0; k <= m; j++)
    {
      if(j == 0 || j == i) C[i][j] = 1;
      else C[i][j] = C[i-1][j-1] + C[i-1][j];
    }
  }
}
```

복잡도는 `Θ(nk)`
