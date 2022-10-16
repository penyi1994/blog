title: '[USACO] Section 1.4 Mixing Milk'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-16 22:28:00
---

簡單題

```c++
/*
ID: penyi191
TASK: milk
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <algorithm>
#define MAXSIZE 5001

#define cin fin
#define cout fout

using namespace std;

class Item {
public:
  int price;
  int amount;
  bool operator<(const Item& rhs) const { 
    return price < rhs.price;
  }
};

int main() {
  ofstream fout ("milk.out");
  ifstream fin ("milk.in");
  
  int N, M;
  Item item[MAXSIZE];
  
  while(cin >> N >> M) {
    for (int i = 0; i < M; i++)
      cin >> item[i].price >> item[i].amount;
    
    sort(item, item + M);
    
    int cost = 0;
    for(int i = 0; i < M && N > 0; i++) {
      if (N > item[i].amount) {
        N -= item[i].amount;
        cost += item[i].amount * item[i].price;
      }
      else {
        cost += item[i].price * N;
        break;
      }
    }
    cout << cost << endl;
  }
  return 0;
}

```