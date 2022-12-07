title: '[USACO] Section 2.2 Subset Sums'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-12-04 20:25:00
---

經典DP題

arr[i]代表第i個數字
令f(x,y)代表用前x個數字 湊出總和y的方法數
則f(x, y) = f(x-1, y) + f(x-1, y-arr[i])

f(x-1, y) 代表不用第x個數字, 也能湊出y的方法數
f(x-1, y-arr[i]) 代表用到第x個數字

因為本題是問湊出總和一半的方法數
所以得出來的f(N, sum/2)要再除2

```c++
/*
ID: penyi191
TASK: subset
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>

#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("subset.out");
  ifstream fin ("subset.in");
  
  int N;
  int sum;
  long long int dp[40][(1 + 39) * 39 / 2];
  
  cin >> N;
  sum = (1 + N) * N / 2;
  
  if (sum % 2 != 0) {
    cout << "0" << endl;
  }
  else {
    memset(dp, 0, sizeof(dp));
    sum = sum / 2;
      
    dp[0][0] = 1;
    for (int i = 1; i <= N; i++) {
      for (int j = 0; j <= sum; j++) {
        if (j >= i)
          dp[i][j] = dp[i -1][j] + dp[i - 1][j - i];
        else
          dp[i][j] = dp[i - 1][j];
      }
    }
    cout << dp[N][sum] / 2 << endl;
  }
  
  return 0;
}




```