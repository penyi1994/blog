title: '[USACO] Section 1.4 Combination Lock'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-06 17:54:00
---
簡單題

記得不要拿負數去取餘數
C語言是向0方向
有些語言則是向負無窮方向

```c++
/*
ID: penyi191
TASK: combo
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>

#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("combo.out");
  ifstream fin ("combo.in");
  
  int N;
  int lockJohn[3];
  int LockMaster[3];
  bool used[101][101][101];
  int ans;
  
  while(cin >> N) {
    cin >> lockJohn[0] >> lockJohn[1] >> lockJohn[2];
    cin >> LockMaster[0] >> LockMaster[1] >> LockMaster[2];
    
    memset(used, 0, sizeof(used));
    
    ans = 0;
    int a, b, c, d, e, f;
    for (int i = -2; i <= 2; i++) {
      for (int j = -2; j <= 2; j++) {
        for (int k = -2; k <= 2; k++) {
          a = ((lockJohn[0] % N) + i + N) % N;
          b = ((lockJohn[1] % N) + j + N) % N;
          c = ((lockJohn[2] % N) + k + N) % N;
          d = ((LockMaster[0] % N) + i + N) % N;
          e = ((LockMaster[1] % N) + j + N) % N;
          f = ((LockMaster[2] % N) + k + N) % N;
          
          if (!used[a][b][c]) {
            used[a][b][c] = true;
            ans++;
          }
          
          if (!used[d][e][f]) {
            used[d][e][f] = true;
            ans++;
          }
        }
      }
    }
    cout << ans << endl;
  }
  
  return 0;
}

```