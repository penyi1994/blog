title: '[USACO] Section 2.1 Sorting a Three-Valued Sequence'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-29 00:39:00
---

不簡單題

最直觀的做法就是O(n^2)的貪心
優先換互相在正確位置的配對
真的不行才換不成對的配對

O(n)的作法
一樣是先換互相在正確位置的配對
然後運用一個觀察: 如果不成對的配對交換, 那可以保證再換一次就一定可以到正確位置
例如: 把在「2區間」的1換到「1區間」, 如果換回「2區間」的是3, 而不是1
那一定剛好會有一個在「3區間」的2等著你

苦思良久
沒有自己想出來 可惡QAQ
google搜了三頁 
國人的解題報告作法都超醜XDD
總算在stack overflow看到這個順眼又直觀的解法

推廣:
如果是固定k distinct value sequence最少交換幾次可以排序好?

```c++
/*
ID: penyi191
TASK: sort3
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>
#define SIZE 1001

#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("sort3.out");
  ifstream fin ("sort3.in");
  
  int N;
  int ans;
  int arr[SIZE];
  int ref[SIZE];
  int cnt[4] = {0};
  
  // wrong[i][j] : he is i, he is at the position belongs to j
  int wrong[4][4] = {0};

  cin >> N;
  
  for (int i = 1; i <= N; i++) {
    cin >> arr[i];
    cnt[arr[i]]++;
  }
  
  for (int i = 1; i <= N; i++) {
    if (i <= cnt[1])
      ref[i] = 1;
    else if (i > cnt[1] && i <= cnt[1] + cnt[2])
      ref[i] = 2;
    else if (i > cnt[1] + cnt[2])
      ref[i] = 3;
  }
  
  for (int i = 1; i <= N; i++)
    wrong[arr[i]][ref[i]]++;
  
  ans = min(wrong[1][2], wrong[2][1]) + 
        min(wrong[1][3], wrong[3][1]) +
        min(wrong[2][3], wrong[3][2]) +
        (max(wrong[1][2], wrong[2][1]) - min(wrong[1][2], wrong[2][1])) * 2;

  cout << ans << endl;
  
  return 0;
}

```