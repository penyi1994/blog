title: '[USACO] Section 1.4  Wormholes'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-12 12:24:00
---

這題卡好久
瘋狂誤解題意 
題目實在太多廢話= =凸

如果配對黑洞(A,B)
從A進就會從B出, 從B進就會從A出
而且走到黑洞就一定會被傳走
除了黑洞之外只能往+x方向直直走
總而言之
只要配對好黑洞, 移動的路徑就固定了

本題重點 : 用backtrack暴搜出兩兩組合


```c++
/*
ID: penyi191
TASK: wormhole
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>

#define cin fin
#define cout fout

using namespace std;

int N;
int ans;
int warmHole[12][2];
int pairHole[12];
int rightHole[12];

int nextHole(int x) {
  if (rightHole[x] == -1)
    return -1;
  return pairHole[rightHole[x]];
}

bool hasCycle() {
  for (int start = 0; start < N; start++) {
    int tmp = start;
    while (true) {
      tmp = nextHole(tmp);
      if (tmp == -1)
        break;
      if (tmp == start)
        return true;
    }
  }
  return false;
}

void backtrack(int x) {
  if (x == N) {
    if (hasCycle())
      ans++;
    return;
  }

  if (pairHole[x] != -1) {
    backtrack(x + 1);
    return;
  }
  
  for (int i = x + 1; i < N; i++) {
    if (pairHole[i] == -1) {
      pairHole[x] = i;
      pairHole[i] = x;
      backtrack(x + 1);
      pairHole[x] = pairHole[i] = -1;
    }
  }
}

int main() {
  ofstream fout ("wormhole.out");
  ifstream fin ("wormhole.in");
  
  while(cin >> N) {
    for (int i = 0; i < N; i++)
      cin >> warmHole[i][0] >> warmHole[i][1];
    
    for (int i = 0; i < N; i++)
      pairHole[i] = rightHole[i] = -1;
    
    for (int i = 0; i < N; i++)
      for (int j = 0; j < N; j++)
        if (warmHole[i][1] == warmHole[j][1] && warmHole[i][0] < warmHole[j][0])
          if (rightHole[i] == -1 || warmHole[j][0] < warmHole[rightHole[i]][0])
            rightHole[i] = j;
    
    ans = 0;
    backtrack(0);
    
    cout << ans << endl;
  }
  return 0;
}

```