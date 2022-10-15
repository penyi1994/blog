title: '[USACO] Section 1.2 Broken Necklace'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-10 17:18:00
---
簡單題
注意有可能繞一圈的情況

C++的單引號跟雙引號不一樣
單引號才是char
乾 python寫太多了 = ="

Section 1.2 完成!!

```c++
/*
ID: penyi191
TASK: beads
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>
#include <map>
#include <algorithm>
#define cin fin
#define cout fout

using namespace std;

ofstream fout ("beads.out");
ifstream fin ("beads.in");

int goLeft(int idx, char color, string s) {
  int tmp = 0;
  for (int i = idx; i >= 0; i--)
    if (s[i] == color || s[i] == 'w')
      tmp++;
    else
      return tmp;
  return tmp;
}

int goRight(int idx, char color, string s) {
  int tmp = 0;
  for (int i = idx; i < s.length(); i++)
    if (s[i] == color || s[i] == 'w')
      tmp++;
    else
      return tmp;
  return tmp;
}

int main() {
  string s;
  int N;
  int ans;
  
  while(cin >> N >> s) {
    s = s + s;
    ans = 0;
    for (int i = 0; i < N * 2; i++)
      ans = max(ans, max(goLeft(i, 'r', s), 
                         goLeft(i, 'b', s)) + 
                     max(goRight(i + 1, 'r', s), 
                         goRight(i + 1, 'b', s)));
    if (ans > N)
      ans = N;
    cout << ans << endl;
  }
}

```