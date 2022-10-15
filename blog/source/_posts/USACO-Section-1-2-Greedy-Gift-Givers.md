title: '[USACO] Section 1.2 Greedy Gift Givers'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-10 02:16:00
---
簡單題

```c++
/*
ID: penyi191
TASK: gift1
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>
#include <map>
#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("gift1.out");
  ifstream fin ("gift1.in");
  
  string name[20];
  int N;
  map<string, int> money;
  
  while(cin >> N) {
    money.clear();
    for (int i = 0; i < N; i++) {
      cin >> name[i];
      money[name[i]] = 0;
    }
    
    for (int i = 0; i < N; i++) {
      string giver;
      int a, b;
      
      cin >> giver;
      cin >> a >> b;
      
      string receiver;
      if (b > 0) {
        money[giver] = money[giver] - a + (a % b);
        for (int j = 1; j <= b; j++) {
          cin >> receiver;
          money[receiver] += (a / b);
        }
      }
    }
    
    for (int i = 0; i < N; i++)
      cout << name[i] << " " << money[name[i]] << endl;
  }
  return 0;
}

```