title: '[USACO] Section 2.2 Runaround Numbers'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-12-04 21:23:00
---

簡單模擬題
學了一招std::to_string

```c++
/*
ID: penyi191
TASK: runround
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>

#define cin fin
#define cout fout

using namespace std;

bool isValid(int x) {
  string s = to_string(x);
  int Len = s.length();
  bool used[10] = {0};
  
  for (int i = 0; i < Len; i++) {
    if (s[i] == '0')
      return false;
    else if (!used[s[i] - '0'])
      used[s[i] - '0'] = true;
    else if (used[s[i] - '0'])
      return false;
  }
  return true;
}

bool isRunaround(int x) {
  if (!isValid(x))
    return false;
  
  string s = to_string(x);
  int Len = s.length();
  bool used[10] = {0};
  char first = s[0];
  
  int idx = 0;
  for (int i = 1; i <= Len; i++) {
    idx = (idx + (s[idx] - '0')) % Len;
    if (used[s[idx] - '0'])
      return false;
    else {
      used[s[idx] - '0'] = true;
    }
  }
  
  return true;
}

int main() {
  ofstream fout ("runround.out");
  ifstream fin ("runround.in");
  
  int M;

  cin >> M;
  while(!isRunaround(++M));
  cout << M << endl;
  return 0;
}


```