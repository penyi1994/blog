title: '[USACO] Section 1.2 Friday the Thirteenth'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-10-10 13:19:00
---

簡單題

```c++
/*
ID: penyi191
TASK: friday
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>
#include <map>
#define cin fin
#define cout fout

using namespace std;

ofstream fout ("friday.out");
ifstream fin ("friday.in");

class Date {
public:
  Date() {}
  Date(int y, int m, int d, int w) : year(y), month(m), day(d), week(w) {} 
  int year;
  int month;
  int day;
  int week;
  Date nextDay();
  bool isFinishYear(int);
};

int getDaysOfThisMonth(int month, int year) {
  if (month == 1  || month == 3  || month == 5 || month == 7 || 
      month == 8  || month == 10 || month == 12)
    return 31;
  
  if (month == 4 || month == 6 || month == 9 || month == 11)
    return 30;
  
  if (month == 2) {
    if (year % 400 == 0)
      return 29;
    if (year % 100 == 0)
      return 28;
    if (year % 4 == 0)
      return 29;
    return 28;
  }
  // unreachable
  cout << "illegal month" << endl;
  return 87;
}

Date Date::nextDay() {
  Date d = *this;
  
  d.day = d.day + 1;
  d.week = (d.week + 1) % 7;
  
  int daysOfThisMonth = getDaysOfThisMonth(d.month, d.year);
  
  if (d.day > daysOfThisMonth) {
    d.day = 1;
    d.month++;
  }
  
  if (d.month > 12) {
    d.year++;
    d.month = 1;
  }
  
  return d;
}

bool Date::isFinishYear(int N) {
  return this->year == N;
}

int main() {
  int N;
  int ans[7];
  
  while(cin >> N) {
    memset(ans, 0, sizeof(ans));
    Date d = Date(1900, 1, 1, 0);
    while(!d.isFinishYear(1900 + N)) {
      if (d.day == 13)
        ans[d.week] ++;
      d = d.nextDay();
    }
    
    cout << ans[5] << " " 
         << ans[6] << " " 
         << ans[0] << " " 
         << ans[1] << " "
         << ans[2] << " " 
         << ans[3] << " " 
         << ans[4] << endl;
  }
  return 0;
}

```