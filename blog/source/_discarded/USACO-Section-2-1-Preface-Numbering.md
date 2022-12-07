title: '[USACO] Section 2.2 Preface Numbering'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-12-03 13:40:00
---

模擬題
為了可讀性 故意寫的很智障XD

```c++
/*
ID: penyi191
TASK: preface
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>
#include <map>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("preface.out");
ifstream fin ("preface.in");

int N;
int sum[7]; // I, V, X, L. C, D, M

class Roman {
  int I, V, X, L, C, D, M;
public:
  Roman(int i, int v, int x, int l, int c, int d, int m) :
    I(i), V(v), X(x), L(l), C(c), D(d), M(m) {}
  Roman() : I(0), V(0), X(0), L(0), C(0), D(0), M(0) {}
  Roman operator + (Roman const &rhs) {
    Roman tmp = Roman(this->I + rhs.I, 
                      this->V + rhs.V, 
                      this->X + rhs.X,
                      this->L + rhs.L,
                      this->C + rhs.C,
                      this->D + rhs.D,
                      this->M + rhs.M);
    return tmp;
  }
  friend ostream& operator<<(ostream&, const Roman&);
};

ostream& operator<<(ostream& os, const Roman& roman) {
  if (roman.I > 0)
    os << "I "<< roman.I  << endl;
  if (roman.V > 0)
    os << "V "<< roman.V  << endl;
  if (roman.X > 0)
    os << "X "<< roman.X  << endl;
  if (roman.L > 0)
    os << "L "<< roman.L  << endl;
  if (roman.C > 0)
    os << "C "<< roman.C  << endl;
  if (roman.D > 0)
    os << "D "<< roman.D  << endl;
  if (roman.M > 0)
    os << "M "<< roman.M  << endl;
  return os;
}

int main() {
  cin >> N;
  
  map<int, Roman> RomanMap;
  
  //                     I, V, X, L, C, D, M
  RomanMap[1]    = Roman(1, 0, 0, 0, 0, 0, 0);
  RomanMap[2]    = Roman(2, 0, 0, 0, 0, 0, 0);
  RomanMap[3]    = Roman(3, 0, 0, 0, 0, 0, 0);
  RomanMap[4]    = Roman(1, 1, 0, 0, 0, 0, 0);
  RomanMap[5]    = Roman(0, 1, 0, 0, 0, 0, 0);
  RomanMap[6]    = Roman(1, 1, 0, 0, 0, 0, 0);
  RomanMap[7]    = Roman(2, 1, 0, 0, 0, 0, 0);
  RomanMap[8]    = Roman(3, 1, 0, 0, 0, 0, 0);
  RomanMap[9]    = Roman(1, 0, 1, 0, 0, 0, 0);
  
  //                     I, V, X, L, C, D, M
  RomanMap[10]   = Roman(0, 0, 1, 0, 0, 0, 0);
  RomanMap[20]   = Roman(0, 0, 2, 0, 0, 0, 0);
  RomanMap[30]   = Roman(0, 0, 3, 0, 0, 0, 0);
  RomanMap[40]   = Roman(0, 0, 1, 1, 0, 0, 0);
  RomanMap[50]   = Roman(0, 0, 0, 1, 0, 0, 0);
  RomanMap[60]   = Roman(0, 0, 1, 1, 0, 0, 0);
  RomanMap[70]   = Roman(0, 0, 2, 1, 0, 0, 0);
  RomanMap[80]   = Roman(0, 0, 3, 1, 0, 0, 0);
  RomanMap[90]   = Roman(0, 0, 1, 0, 1, 0, 0);
  
  //                     I, V, X, L, C, D, M
  RomanMap[100]  = Roman(0, 0, 0, 0, 1, 0, 0);
  RomanMap[200]  = Roman(0, 0, 0, 0, 2, 0, 0);
  RomanMap[300]  = Roman(0, 0, 0, 0, 3, 0, 0);
  RomanMap[400]  = Roman(0, 0, 0, 0, 1, 1, 0);
  RomanMap[500]  = Roman(0, 0, 0, 0, 0, 1, 0);
  RomanMap[600]  = Roman(0, 0, 0, 0, 1, 1, 0);
  RomanMap[700]  = Roman(0, 0, 0, 0, 2, 1, 0);
  RomanMap[800]  = Roman(0, 0, 0, 0, 3, 1, 0);
  RomanMap[900]  = Roman(0, 0, 0, 0, 1, 0, 1);
  
  RomanMap[1000] = Roman(0, 0, 0, 0, 0, 0, 1);
  RomanMap[2000] = Roman(0, 0, 0, 0, 0, 0, 2);
  RomanMap[3000] = Roman(0, 0, 0, 0, 0, 0, 3);

  Roman sum = Roman();
  for (int i = 1; i <= N; i++) {
    int tmp0 = i;
    int tmp1 = 1;
    while(tmp0 > 0) {
      sum = sum + RomanMap[(tmp0 % 10) * tmp1];
      tmp0 = tmp0 / 10;
      tmp1 = tmp1 * 10;
    }
  }
  
  cout << sum;
  
  return 0;
}


```