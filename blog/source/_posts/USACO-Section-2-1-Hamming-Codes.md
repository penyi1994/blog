title: '[USACO] Section 2.1 Hamming Codes'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-12-02 23:13:00
---

暴搜

```c++
/*
ID: penyi191
TASK: hamming
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("hamming.out");
ifstream fin ("hamming.in");

int N, B, D;
int arr[70];
bool foundAns;

int HammingDistance(int x, int y) {
  int cnt = 0;
  for (int i = 0; i < B; i++)
    if ((x & (1 << i)) ^ (y & (1 << i)))
      cnt++;
  return cnt;
}

bool isLegal(int idx, int x) {
  for (int i = 1; i < idx; i++)
    if (HammingDistance(arr[i], x) < D)
      return false;
  return true;
}

void backtrack(int idx) {
  if (foundAns)
    return;
  if (idx > N) {
    foundAns = true;
    for (int i = 1; i <= N; i++) {
      if (i % 10 == 1)
        cout << arr[i];
      else if (i % 10 == 0 && i != N)
        cout << " " << arr[i] << endl;
      else
        cout << " " << arr[i];
    }
    cout << endl;
    return;
  }
  
  for (int i = arr[idx - 1]; i <= (1 << B) - 1; i++) {
    if (isLegal(idx, i)) {
      arr[idx] = i;
      backtrack(idx + 1);
    }
  }
}

int main() {
  cin >> N >> B >>D;
  
  foundAns = false;
  memset(arr, 0, sizeof(arr));
  backtrack(1);
  
  return 0;
}

```