---
layout: post
title:  "알고리즘 허프만 코드"
date:   2018-04-03 21:07:00 +0900
categories: jekyll update
---

---
### 허프만 코드

무손실 압축에 쓰이는 엔트로피 부호화 방법. 일반 텍스트 문서 파일과 같은 경우는 반드시 무손실 압축 방법을 사용해야한다. 허프만 코드는 **시작부 코드(prefix code)** 를 생성 한다. 시작부 코드는 코드 집합의 어떤 코드도 다른 코드의 시작부분에 나타나지 않는다는 특징을 갖는다.

허프만 코딩은 각 문자의 빈도수에 따라 코드를 만들어 내는데 빈도수가 높을수록 짧은 코드를 배정하고 빈도수가 낮을수록 긴 코드를 배정한다. 각 문자에 이진 코드를 배정할 때 이진 트리를 사용하며 이를 허프만 트리라 한다.

---
### 해법
각 문자들중 가장 빈도수가 낮은 두 개의 트리를 선택하여 합쳐서 새로운 트리를 만들어내고 최종적으로 하나의 트리가 만들어지면 종료한다. 새로 만들어지는 트리의 루트노드는 선택한 두 트리를 자식 노드로 삼게 되며 두 트리의 빈도수의 합이 새트리의 빈도수가 된다.

허프만 트리의 각 노드에서 왼쪽 자식 노드는 0, 오른쪽 자식 노드는 1을 붙인 후 루트에서부터 해당 잎 노드까지의 경로를 따라가면 허프만 코드를 얻는다.

```
node huffman_GM(int frequency[], int n)
{
  min_heap heap;
  node root, left, right;
  init_min_heap(heap, frequency, n);
  loop(n-1)
  {
    left = delete_min_heap(heap);
    right = delete_min_heap(heap);
    root->left = left;
    root->right = right;
    root->frequency = left->frequency + right -> frequency;
    insert_min_heap(heap, root);
  }
  return delete_min_heap(heap);
}
```

---
### 복잡도

init_min_heap을 수행하는데 O(n)이 걸린다. 또한 반복 블록은 n-1회 반복하고, delete_min_heap과 insert_min_heap모두 O(logn)이므로 허프만 트리의 생성 알고리즘은 O(nlogn)이다.

---
### 압축 풀기

압축 풀기 역시 허프만 트리를 이용한다. 비트 스트링을 따라가며 허프만 트리의 단말 노드가 나오면 코드를 문자로 복원한다.

---
### 최적 부분 구조

허프만 트리 T1에서 빈도수가 가장 낮은 두 문자가 a와 b라고 하자. 주어진 문자 집합에서 a와 b를 빼고 새로운 문자 z를 추가했을 때의 허프만 트리를 T2라 하자. 만약 a와 b의 빈도수 합과 z의 빈도수가 같을 경우 T1에서 a와 b와 이 둘을 자식으로 하는 노드를 삭제하고 z로 대치하면 T2가 된다는 사실이 증명 되었다. 따라서 최적 부분 구조를 갖는다.
