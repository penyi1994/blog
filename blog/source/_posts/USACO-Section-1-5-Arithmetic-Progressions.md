title: '[USACO] Section 1.5 Arithmetic Progressions'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-12 22:15:00
---
題意:
給定N, M
從「兩數平方和」(p^2 + q^2)的集合中, 0 <= p,q <= M
有哪些長度至少N的等差數列?

直觀的暴搜題

稍微把重複的事情省下來就不會超時了
我做了兩件事

(1) 
枚舉公差時, 沒有必要 1 ~ M * M * 2都枚舉
如果要形成長度N的等差數列
至少會有N-1個公差的距離
否則第N個元素會直接大於M * M * 2
所以枚舉1 ~ M * M * 2 / (N - 1) 即可

(2)
第一個維度枚舉公差
第二個維度枚舉首項

首項也是等差數列的一部分
必須也是「兩數平方和」

天真的作法是
每次枚舉新的公差進來
都判斷一次0 ~ M* M * 2是不是「兩數平方和」

完全可以事先把首項準備好

然後注意枚舉到大於M * M * 2的數字時會access memory out of bound
所以我故意宣告成M * M * 3

```c++
/*
ID: penyi191
TASK: ariprog
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("ariprog.out");
  ifstream fin ("ariprog.in");
  
  int N, M;
  bool isBiSquare[250 * 250 * 3 + 1] = {0};
  bool hasProgression = false;
  int start[250 * 250 * 3 + 1];
  int startSize = 0;
  
  cin >> N >> M;
  
  for (int i = 0; i <= M; i++)
    for (int j = 0; j <= M; j++) {
      if (!isBiSquare[i * i + j * j]) {
        isBiSquare[i * i + j * j] = true;
        start[startSize] = i * i + j * j;
        startSize++;
      }
    }
  
  sort(start, start + startSize);
  
  for (int diff = 1; diff <= (M * M * 2) / (N - 1); diff++) {
    for (int i = 0; i < startSize; i++) {
      int startNumber = start[i];
      bool isProgression = true;
      for (int j = 0; j < N; j++) {
        if (!isBiSquare[startNumber + diff * j]) {
          isProgression = false;
          break;
        }
      }
      
      if (isProgression) {
        cout << startNumber << " " << diff << endl;
        hasProgression = true;
      }
    }
  }
  
  if (!hasProgression)
    cout << "NONE" << endl;
    
  return 0;
}

```