title: '[USACO] Section 1.6 Superprime Rib'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-19 18:22:00
---

暴搜

Chapter1 搞定!!

```c++
/*
ID: penyi191
TASK: sprime
LANG: C++
*/
#include <iostream>
#include <fstream>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("sprime.out");
ifstream fin ("sprime.in");
  
int N;
int arr[10];

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

void backtrack(int idx) {
  if (idx >= N){
    for (int i = 0; i < N; i++)
      cout << arr[i];
    cout << endl;
    return;
  }

  for (int i = 0; i <= 9; i++) {
    if (idx == 0 && (i == 0 || i ==1))
      continue;
    arr[idx] = i;
    if (!isPrime(makeNumber(idx + 1)))
      continue;
    backtrack(idx + 1);
  }
}

int main() {
  cin >> N;
  backtrack(0);
  return 0;
}


```