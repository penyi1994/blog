title: '[USACO] Section 1.3 Dual Palindromes'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-16 21:08:00
---
簡單題
而且跟上一題一模一樣= ="

Section 1-3 完成!!

```c++
/*
ID: penyi191
TASK: dualpal
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#define MAXSIZE 101

#define cin fin
#define cout fout

using namespace std;

class Number {
public:
  int length;
  int data[MAXSIZE];
};

Number convertBase(int x, int base) {
  Number tmp;
  int len = 0;
  
  while (x > 0) {
    tmp.data[len] = x % base;
    len++;
    x = x / base;
  }
  tmp.length = len;
  
  return tmp;
}

bool isPalindromic(Number x) {
  int len = x.length;
  
  for (int i = 0; i < len; i++) 
    if (x.data[i] != x.data[len - i - 1])
      return false;
  
  return true;
}

int main() {
  ofstream fout ("dualpal.out");
  ifstream fin ("dualpal.in");

  int N, S;
  
  while(cin >> N >> S) {
    int cnt = 0;
    for (int i = S + 1; cnt < N; i++) {
      bool once = false;
      for (int base = 2; base <= 10; base++) {
        if (isPalindromic(convertBase(i, base))) {
          if (!once)
            once = true;
          else {
            cnt++;
            cout << i << endl;
            break;
          }
        }
      }
    }
  }
  
  return 0;
}


```