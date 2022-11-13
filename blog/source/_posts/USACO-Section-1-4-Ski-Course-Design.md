title: '[USACO] Section 1.4 Ski Course Design'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-12 14:23:00
---
簡單題
Section 1-4 搞定~

```c++
/*
ID: penyi191
TASK: skidesign
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

int getCost(int* arr, int N, int low, int high) {
  int cost = 0;
  for (int i = 0; i < N; i++) {
    if (arr[i] < low)
      cost += (low - arr[i]) * (low - arr[i]);
    if (arr[i] > high)
      cost += (high - arr[i]) * (high - arr[i]);
  }
  return cost;
}

int main() {
  ofstream fout ("skidesign.out");
  ifstream fin ("skidesign.in");
  
  int N;
  int ans;
  int height[1000];
  
  while(cin >> N) {
    for (int i = 0; i < N; i++)
      cin >> height[i];
    ans = 99999999;
    for (int i = 0; i < 100; i++)
      ans  = min(ans, getCost(height, N, i, i + 17));
    cout << ans << endl;
  }
  return 0;
}

```