title: '[USACO] Section 1.3 Transformations'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-15 17:37:00
---
簡單模擬題

題目滿好笑的
如果是完全一樣的pattern
不能直接回答6
如果1到5會成立還是要回答1到5
滿智障的= ="

本題重點

(1)
override == operator
如果是定義在class內
那this只能是left hand side
改成friend定義在外面lhs跟rhs才能有交換律

(2) 二維陣列順時針轉90度意外的難呢...


```c++
/*
ID: penyi191
TASK: transform
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>
#define MAXSIZE 20
#define cin fin
#define cout fout


using namespace std;

class Pattern {
public:
  Pattern(){}
  Pattern(int x) : size(x) {}
  int size;
  string data[MAXSIZE];
  friend bool operator==(const Pattern&, const Pattern&);
};

bool operator==(const Pattern& p0, const Pattern& p1) {
  int N = p0.size;
  for (int i = 0; i < N; i++)
    for (int j = 0; j < N; j++)
      if (p0.data[i][j] != p1.data[i][j])
        return false;
  return true;
}

/*
rotate 90 degree
(i, j) ==> (j, N-i-1)
*/

Pattern Rotate90(Pattern p) {
  Pattern tmp = p;
  int N = tmp.size;
  for (int i = 0; i < N; i++)
    for (int j = 0; j < N; j++)
      tmp.data[j][N - i - 1] = p.data[i][j];
  return tmp;
}

Pattern Rotate180(Pattern p) {
  return Rotate90(Rotate90(p));
}

Pattern Rotate270(Pattern p) {
  return Rotate90(Rotate90(Rotate90(p)));
}

Pattern Reflect(Pattern p) {
  Pattern tmp = p;
  int N = tmp.size;
  for (int i = 0; i < N; i++)
    for (int j = 0; j < N; j++)
      tmp.data[i][j] = p.data[i][N - j - 1];
  return tmp;
}

int main() {
  ofstream fout ("transform.out");
  ifstream fin ("transform.in");
  
  int N;
  
  while(cin >> N) {
    Pattern inputPattern = Pattern(N);
    Pattern targetPattern = Pattern(N);
    
    for (int i = 0; i < N; i++)
      cin >> inputPattern.data[i];
    
    for (int i = 0; i < N; i++)
      cin >> targetPattern.data[i];
  
    if (targetPattern == Rotate90(inputPattern))
      cout << "1" << endl;
    else if (targetPattern == Rotate180(inputPattern))
      cout << "2" << endl;
    else if (targetPattern == Rotate270(inputPattern))
      cout << "3" << endl;
    else if (targetPattern == Reflect(inputPattern))
      cout << "4" << endl;
    else if (targetPattern == Rotate90(Reflect(inputPattern)))
      cout << "5" << endl;
    else if (targetPattern == Rotate180(Reflect(inputPattern)))
      cout << "5" << endl;
    else if (targetPattern == Rotate270(Reflect(inputPattern)))
      cout << "5" << endl;
    else if (targetPattern == inputPattern)
      cout << "6" << endl;
    else
      cout << "7" << endl;
  }
  return 0;
}

```