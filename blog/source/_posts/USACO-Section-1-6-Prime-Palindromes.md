title: '[USACO] Section 1.6 Prime Palindromes'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-19 17:55:00
---

根據題目的提示
天真的枚舉所有數字是不行的
暴搜出回文的數字再判斷是否是質數即可

```c++
/*
ID: penyi191
TASK: pprime
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("pprime.out");
ifstream fin ("pprime.in");
  
int A, B;
int cntA, cntB;
int ans;
int arr[10];

int getNumDigit(int x) {
  int cnt = 0;
  while(x > 0) {
    x /= 10;
    cnt++;
  }
  return cnt;
}

bool isPrime(int x) {
  for (int i = 2; i * i <= x; i++)
    if (x % i == 0)
      return false;
  return true;
}

int makeNumber(int Len) {
  int x = 0;
  for (int i = 0; i < Len; i++)
    x = x * 10 + arr[i];
  return x;
}

void backtrack(int idx, int Len) {
  if (idx > (Len - 1) / 2) {
    if (Len % 2 == 0)
      for (int i = idx, j = idx -1; i < Len; i++, j--)
        arr[i] = arr[j];
    else
      for (int i = idx, j = idx - 2; i < Len; i++, j--)
        arr[i] = arr[j];
    
    int x = makeNumber(Len);
    if (x >= A && x <= B && isPrime(x))
      cout << x << endl;
    return;
  }

  for (int i = 0; i <= 9; i++) {
    arr[idx] = i; 
    backtrack(idx + 1, Len);
  }
}

int main() {
  cin >> A >> B;
  
  cntA = getNumDigit(A);
  cntB = getNumDigit(B);
  for (int i = cntA; i <= cntB; i++)
    backtrack(0, i);
    
  return 0;
}


```