title: '[USACO] Section 1.4 Barn Repair'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-19 23:35:00
---

滿有趣的一題

題意:
給一個長度S的整數的數線, 從1到S
有C個點有放牛
給你最多M個線段
要把有牛的點都蓋住
求最少幾個點會被這M個線段蓋住

原本想了一個對單一線段最大長度二分搜的作法
結果發現從左到右去蓋 
如果有木板沒用完的話 是可以回頭修正的
所以是錯的QAQ

正確的做法是
直接排序所有牛的間隔
能用M個線段就代表前M-1大的間隔可以被消掉

當然
如果能用的線段比牛的數目多的話
就是每個牛都用長度為1的線段蓋住

```c++
/*
ID: penyi191
TASK: barn1
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <algorithm>
#define MAXSIZE 201

#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("barn1.out");
  ifstream fin ("barn1.in");
  
  int M, S, C;
  int stall[MAXSIZE];
  int gap[MAXSIZE];
  
  while(cin >> M >> S >> C) {
    for (int i = 0; i < C; i++) 
      cin >> stall[i];
    
    if (M >= C) {
      cout << C << endl;
      continue;
    }
    
    sort(stall, stall + C);
    
    for (int i = 0; i < C - 1; i++) 
      gap[i] = stall[i + 1] - stall[i] - 1;
    
    int gapSize = C - 1; 
    int ans = stall[C - 1] - stall[0] + 1;
    
    sort(gap, gap + gapSize);
  
    for (int i = gapSize - 1; i > (gapSize - 1) - (M - 1); i--)
      ans -= gap[i];
    
    cout << ans << endl;
  }
  return 0;
}

```