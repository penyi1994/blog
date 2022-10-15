title: '[USACO] Section 1.2 Your Ride Is Here'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-10 02:16:00
---
心血來潮
來寫USACO

其實高中時也才寫了兩三個chapter而已吧(!?)


```C++
/*
ID: penyi191
TASK: ride
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#define cin fin
#define cout fout

using namespace std;

int main() {
  ofstream fout ("ride.out");
  ifstream fin ("ride.in");
  
  char A[10], B[10];
  
  while(cin >> A >> B) {
    int productA = 1;
    int productB = 1;
    int lenA = strlen(A);
    int lenB = strlen(B);
    
    for (int i = 0; i < lenA; i++) 
      productA *= (A[i] - 'A' + 1) % 47; 
    
    for (int i = 0; i < lenB; i++) 
      productB *= (B[i] - 'A' + 1) % 47; 
    
    productA = productA % 47;
    productB = productB % 47;
    
    if (productA == productB)
      cout << "GO" << endl;
    else
      cout << "STAY" << endl;
  }
  return 0;
}

```