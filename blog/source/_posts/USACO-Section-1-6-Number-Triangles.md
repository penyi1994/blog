title: '[USACO] Section 1.6 Number Triangles'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-13 22:40:00
---

數字三角形
經典DP
dp\[i]\[j] = max(dp\[i - 1]\[j - 1], dp\[i - 1]\[j]) + arr\[i]\[j]

```c++
/*
ID: penyi191
TASK: numtri
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("numtri.out");
  ifstream fin ("numtri.in");
  
  int R;
  int ans;
  int dp[1001][1001];
  int arr[1001][1001];
  
  cin >> R;
  
  for (int i = 1; i <= R; i++) {
    for (int j = 1; j <= i; j++) {
      cin >> arr[i][j];
    }
  }
  
  ans = dp[1][1] = arr[1][1];
  
  for (int i = 2; i <= R; i++) {
    for (int j = 1; j <= i; j++) {
      if (j == 1)
        dp[i][j] = dp[i - 1][j] + arr[i][j];
      else if (j == i)
        dp[i][j] = dp[i - 1][j - 1] + arr[i][j];
      else
        dp[i][j] = max(dp[i - 1][j - 1], dp[i - 1][j]) + arr[i][j];
      
      ans = max(ans, dp[i][j]);
    }
  }
  
  cout << ans << endl;
  return 0;
}


```