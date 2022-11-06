title: '[USACO] Section 1.4 Prime Cryptarithm'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-05 22:51:00
---

快一個月沒寫了QAQ

暴搜
簡單題

```c++
/*
ID: penyi191
TASK: crypt1
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>

#define cin fin
#define cout fout


using namespace std;

bool isLegalLine(int line, int length, bool* isAllowed) {
  int tmp = 0;
  while (line != 0) {
      if (!isAllowed[line % 10])
        return false;
      line = line / 10;
      tmp++;
  }
  
  if (tmp != length)
    return false;
  
  return true;
}

bool isLegalDigit(int line_1, int line_2, bool* isAllowed) {
  int line_3 = line_1 * (line_2 % 10);
  int line_4 = line_1 * (line_2 / 10);
  int line_5 = line_3 + line_4 * 10;
  
  if (isLegalLine(line_1, 3, isAllowed) && 
      isLegalLine(line_2, 2, isAllowed) &&
      isLegalLine(line_3, 3, isAllowed) &&
      isLegalLine(line_4, 3, isAllowed) &&
      isLegalLine(line_5, 4, isAllowed))
    return true;
  
  return false;
}

int main() {
  ofstream fout ("crypt1.out");
  ifstream fin ("crypt1.in");
  
  int N;
  int ans;
  bool isAllowed[10];
  
  while(cin >> N) {
    int tmp;
    memset(isAllowed, 0, sizeof(isAllowed));
    for (int i = 0; i < N; i++) {
      cin >> tmp;
      isAllowed[tmp] = true;
    }
    
    ans = 0;
    for (int i = 100; i <= 999; i++)
      for (int j = 10; j < 99; j++)
        if (isLegalDigit(i, j, isAllowed))
          ans++;
    
    cout << ans << endl;
  }
  
  return 0;
}

```