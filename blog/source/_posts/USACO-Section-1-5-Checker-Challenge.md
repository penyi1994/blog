title: '[USACO] Section 1.5 Checker Challenge'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-20 11:34:00
---

念念不忘的八皇后 backtrack + bitmask
不知為何從USACO拿掉了
可能是太多人作弊了直接印答案XDD

隨手找了一個中國的OJ來送code
https://www.luogu.com.cn/problem/P1219

基本backtrack解法: 
用bool陣列表示狀態
N-Queen除了有N個Row之外
斜率1跟斜率-1各有N\*2-1條

進階的解法 :
用bitmask表示狀態
第i個bit是0, 表示目前row的第i格還是空的
第i個bit是1, 表示目前row的第i格會被之前放的皇后殺掉

每次枚舉bitmask中還是0的bit

用三個bitmask分別表示row, 斜率1 跟 斜率-1
遞迴下去的時候
斜率1要左移, 斜率-1要右移, 代表下個row不行放的位置

不要被過於精簡的bit operation嚇到了
其實backtrack的本質是一樣的

先用基本的backtrack印出前三個排列
再用bitmask的作法算出可能的方法數

```c++
#include <iostream>
#include <cstring>
#define SIZE 20

using namespace std;

int N;
int cnt;
int FullState;
int arr[SIZE];
bool isLower[SIZE];
bool isLowerL[SIZE];
bool isLowerR[SIZE];

void backtrack(int idx) {
  if (cnt > 3)
    return;
  if (idx >= N) {
    cnt++;
    if (cnt <= 3) {
      cout << arr[0];
      for (int i = 1; i < N; i++)
        cout << " " << arr[i];
      cout << endl;
    }
    return;
  }
  
  for (int i = 1; i <= N; i++) {
    int LowerL = (idx + i) % (N * 2 - 1);
    int LowerR = (idx - i + (N * 2 - 1)) % (N * 2 - 1);
    if (!isLower[i] && !isLowerL[LowerL] && !isLowerR[LowerR]) {
      isLower[i] = isLowerL[LowerL] = isLowerR[LowerR] = true;
      arr[idx] = i;
      backtrack(idx + 1);
      isLower[i] = isLowerL[LowerL] = isLowerR[LowerR] = false;
    }
  }
}

void backtrackFast(int Row, int LowerL, int LowerR) {
  if (Row == FullState) {
    cnt++;
    return;
  }
  
  int EmptyPos =  FullState & ~(Row | LowerL | LowerR);
  while (EmptyPos != 0) {
    // the right most 1
    int p = EmptyPos & -EmptyPos;
    EmptyPos -= p;
    backtrackFast(Row | p, (LowerL | p) << 1, (LowerR | p) >> 1);
  }
}

int main() {
  cin >> N;
  
  // Find first 3 answer
  cnt = 0;
  memset(isLower, false, sizeof(isLower));
  memset(isLowerL, false, sizeof(isLowerL));
  memset(isLowerR, false, sizeof(isLowerR));
  backtrack(0);
  
  // Find the number of all answers
  cnt = 0;
  FullState = (1 << N) - 1;
  backtrackFast(0, 0, 0);
  
  cout << cnt << endl;
  return 0;
}



```