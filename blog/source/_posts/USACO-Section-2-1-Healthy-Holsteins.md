title: '[USACO] Section 2.1 Healthy Holsteins'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-30 23:52:00
---

暴搜所有組合

```c++
/*
ID: penyi191
TASK: holstein
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("holstein.out");
ifstream fin ("holstein.in");

int V;
int requirement[30];
int G;
int vitamin[20][30];
int ans;
int ans_list[20];

int arr[30];
int curVitamin[30];

bool isEnough(int idx) {
  for (int i = 1; i <= V; i++)
    if (curVitamin[i] < requirement[i])
      return false;
  return true;
}

void backtrack(int idx) {
  if (idx >= ans)
    return;
  if (idx < ans && isEnough(idx)) {
    ans = idx;
    for (int i = 1; i < ans; i++)
      ans_list[i] = arr[i];
    return;
  }
  for (int i = arr[idx - 1] + 1; i <= G; i++) {
    arr[idx] = i;
    for (int j = 1; j <= V; j++)
      curVitamin[j] += vitamin[i][j];
    backtrack(idx + 1);
    for (int j = 1; j <= V; j++)
      curVitamin[j] -= vitamin[i][j];
  }
}

int main() {
  cin >> V;
  for (int i = 1; i <= V; i++)
    cin >> requirement[i];
  
  cin >> G;
  for (int i = 1; i <= G; i++)
    for (int j = 1; j <= V; j++)
      cin >> vitamin[i][j];
  
  ans = 87;
  memset(curVitamin, 0, sizeof(curVitamin));
  backtrack(1);
  
  cout << ans - 1;
  for (int i = 1; i < ans; i++)
    cout << " " << ans_list[i];
  cout << endl;
  
  return 0;
}

```