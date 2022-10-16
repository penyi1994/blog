title: '[USACO] Section 1.3 Palindromic Squares'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-16 19:42:00
---

簡單題
本題重點 : override operator << ostream

```c++
/*
ID: penyi191
TASK: palsquare
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
  friend ostream& operator<<(ostream&, const Number&);
};

ostream& operator<<(ostream& os, const Number& number) {
  int len = number.length;
  
  for (int i = len - 1; i >= 0; i--)
    if (number.data[i] < 10)
      os << number.data[i];
    else
      os << char('A' + (number.data[i] - 10));
  
  return os;
}

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
  ofstream fout ("palsquare.out");
  ifstream fin ("palsquare.in");

  int N;
  
  while(cin >> N) {
    for (int i = 1; i <= 300; i++) {
      Number square = convertBase(i * i, N);
      if (isPalindromic(square)) {
        Number tmp = convertBase(i, N);
        cout << tmp << " " << square << endl;
      }
    }
  }
  
  return 0;
}

```