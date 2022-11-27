title: '[USACO] Section 2.1 Ordered Fractions'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-27 23:57:00
---

簡單題

裝模作樣的練習了一下C++的基本概念
本題重點
(1) override << ostream operator, 可以直接把class餵給cout
(2) overide == operator, 可以直接做sort
(3) function call pass by reference
(4) range-based for loop
(5) STL vector, erase會調整size!! (但不會影響capacity)

```c++
/*
ID: penyi191
TASK: frac1
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("frac1.out");
ifstream fin ("frac1.in");

class Fraction {
public:
  int A; // numerator
  int B; // denominator
  Fraction(int a, int b) : A(a), B(b) {}
  friend ostream& operator<<(ostream&, const Fraction&);
  bool operator<(const Fraction& rhs) const { 
    return this->A * rhs.B < rhs.A * this->B;
  }
  bool operator==(const Fraction& rhs) const {
    return (this->A == rhs.A && this->B == rhs.B) || (this->A == 0 && rhs.A == 0);
  }
};

ostream& operator<<(ostream& os, const Fraction& frac) {
  os << frac.A << "/" << frac.B << endl;
  return os;
}

void reduction(Fraction &frac) {
  for (int i = 2; i <= frac.B; i++) {
    while (frac.A % i == 0 && frac.B % i == 0) {
      frac.A /= i;
      frac.B /= i;
    }
  }
}

int main() {
  int N;
  vector<Fraction> v;
  
  cin >> N;
  
  for (int i = 1; i <= N; i++)
    for (int j = 0; j <= i; j++)
      v.push_back(Fraction(j, i));
  
  for (auto &tmp : v)
    reduction(tmp);
  
  sort(v.begin(), v.end());
  
  // Eliminate redundant fraction in the sorted array
  for (int i = 0; i < v.size() - 1; i++)
    while (i + 1 < v.size() && v[i] == v[i + 1])
      v.erase(v.begin() + i + 1);
  
  for (auto &tmp : v)
    cout << tmp;
  
  return 0;
}

```