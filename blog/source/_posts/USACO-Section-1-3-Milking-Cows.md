title: '[USACO] Section 1.3 Milking Cows'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-11 22:18:00
---
對start time由小到大排序

掃一遍所有區間
紀錄目前最長連續工作時間的最左邊跟最右邊

如果「當前區間」被包含在「目前最長區間」裡, 則
不理她

如果「當前區間」可以延長「目前最長區間, 則
更新目前的最右邊, 並且更新「最久工作紀錄」

如果「當前區間」跟「目前最長區間」沒有交集, 則
更新「最久不工作紀錄」, 並且把「當前區間」當成「目前最長區間」,繼續往後掃 

```c++
/*
ID: penyi191
TASK: milk2
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <algorithm>
#define cin fin
#define cout fout

using namespace std;

struct TimeInterval {
  int start;
  int end;
  bool operator<(const TimeInterval& rhs) const { 
    return (start < rhs.start) || ((start == rhs.start) && (end <= rhs.end));
  }
};

int main() {
  ofstream fout ("milk2.out");
  ifstream fin ("milk2.in");

  int N;
  TimeInterval time[5001];
  
  while(cin >> N) {
    for (int i = 0; i < N; i++)
      cin >> time[i].start >> time[i].end;
    
    sort(time, time + N);
    
    int curLeft = time[0].start;
    int curRight = time[0].end;
    
    int maxWorking = curRight - curLeft;
    int maxNotWorking = 0;
    
    for (int i = 1; i < N; i++) {
      if(time[i].start > curRight) {
        maxNotWorking = max(maxNotWorking, time[i].start - curRight);
        curLeft = time[i].start;
        curRight = time[i].end;
      }
      else if (time[i].end > curRight) {
        maxWorking = max(maxWorking, time[i].end - curLeft);
        curRight = time[i].end;
      }
    }
    cout << maxWorking << " " << maxNotWorking << endl;
  }
  return 0;
}

```