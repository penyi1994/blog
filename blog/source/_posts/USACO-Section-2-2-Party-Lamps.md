title: '[USACO] Section 2.2 Party Lamps'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-12-07 23:04:00
---

不簡單題

第一眼 會以為又是典型的BFS題
但看到Button可以按10000次就該知道事有蹊蹺

觀察1:
每個Button都是按兩次等於沒按
所以其實從頭到尾就只有2^4種可能
而這2^4種可能之中還有重複的, 但我就沒管它了, 直接bool擋掉

觀察2:
Button2是顛倒奇數
Button3是顛倒偶數
Button4是顛倒1, 4, 7, 11, ..., 3N+1
所以真正要維護的狀態只有6個bit

觀察3:
只要C大於「要按的Button個數」並且奇偶一樣, 才可以到達想要的狀態

最後要注意 答案要求的是「字典序的大小」
跟維護6個bit的「數字的大小」是不一樣的
所以我把string拿去std::sort

看似簡單的一題
但繞了好幾個彎
程式解題競賽果然是給年輕人玩的東西啊...

Note:
USACO的測資看起來滿弱的
不確定到底是否正解XD

```c++
/*
ID: penyi191
TASK: lamps
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>
#include <cstring>
#include <algorithm>

#define cin fin
#define cout fout

using namespace std;

ofstream fout ("lamps.out");
ifstream fin ("lamps.in");

int N, C;
int sizeON;
int sizeOFF;
int sizeANS;
int tmp;
int mustON[101];
int mustOFF[101];
int ans[20];
string ansString[20];
int result[2][2][2][2];
bool used[(1 << 6) - 1];

bool isValid(int x) {
  for (int i = 0; i < sizeON; i++)
    if ((x & (1 << ((mustON[i] - 1) % 6))) == 0)
      return false;

  for (int i = 0; i < sizeOFF; i++)
    if (x & (1 << ((mustOFF[i] - 1) % 6)))
      return false;
      
  return true;
}

string extendLamp(int x, int Len) {
  string s = "";
  for (int i = 0; i < Len; i++) {
    if (x & (1 << (i % 6)))
      s += "1";
    else
      s += "0";
  }
  return s;
}

int main() {
  cin >> N >> C;
  
  sizeON = 0;
  sizeOFF = 0;
  sizeANS = 0;
  
  while (cin >> tmp) {
    if (tmp == -1)
      break;
    mustON[sizeON++] = tmp;
  }
  
  while (cin >> tmp) {
    if (tmp == -1)
      break;
    mustOFF[sizeOFF++] = tmp;
  }
  
  for (int a = 0; a <= 1; a++)
    for (int b = 0; b <= 1; b++)
      for (int c = 0; c <= 1; c++)
        for (int d = 0; d <= 1; d++) {
          result[a][b][c][d] = 0b111111;
          if (a == 1)
            result[a][b][c][d] ^= 0b111111;
          if (b == 1)
            result[a][b][c][d] ^= 0b010101;
          if (c == 1)
            result[a][b][c][d] ^= 0b101010;
          if (d == 1)
            result[a][b][c][d] ^= 0b001001;
        }
  
  memset(used, false, sizeof(used));
  for (int a = 0; a <= 1; a++)
    for (int b = 0; b <= 1; b++)
      for (int c = 0; c <= 1; c++)
        for (int d = 0; d <= 1; d++)
          if (a + b + c + d <= C &&  (a + b + c + d) % 2 == C % 2)
            if (isValid(result[a][b][c][d]) && !used[result[a][b][c][d]]) {
              ans[sizeANS++] = result[a][b][c][d];
              used[result[a][b][c][d]] = true;
            }
  
  if (sizeANS == 0)
    cout << "IMPOSSIBLE" << endl;
  else {
    for (int i = 0; i < sizeANS; i++)
      ansString[i] = extendLamp(ans[i], N);
    
    sort(ansString, ansString + sizeANS);
    
    for (int i = 0; i < sizeANS; i++)
      cout << ansString[i] << endl;
  }

  return 0;
}


```