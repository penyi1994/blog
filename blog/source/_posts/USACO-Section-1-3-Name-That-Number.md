title: '[USACO] Section 1.3 Name That Number'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-15 23:44:00
---

簡單題

```c++
/*
ID: penyi191
TASK: namenum
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>
#define DICTSIZE 5100

#define cin fin
#define cout fout

using namespace std;

bool check(string number, string name, int mapping[26]) {
  if(number.length() != name.length())
    return false;
  
  int len = number.length();
  
  for (int i = 0; i < len; i++)
    if(number[i] - '0' != mapping[name[i] - 'A'])
      return false;
  return true;
}

int main() {
  ofstream fout ("namenum.out");
  ifstream fin ("namenum.in");
  ifstream dictin ("dict.txt");
  
  int mapping[26];
  
  for(int i = 0, j = 0; i < 26; i++) {
    mapping[i] = j / 3 + 2;
    if (i != ('Q'- 'A') && i != ('Z' - 'A'))
      j++;
  }
  
  int dictSize = 0;
  string dict[DICTSIZE];
  string input;
    
  while(dictin >> dict[dictSize++]);
  while(cin >> input) {
    bool hasValidName = false;
    
    for (int i = 0; i < dictSize; i++)
      if (check(input, dict[i], mapping)) {
        cout << dict[i] << endl;
        hasValidName = true;
      }
    
    if (!hasValidName)
      cout << "NONE" << endl;
  }
  return 0;
}

```