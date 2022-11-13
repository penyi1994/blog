title: '[USACO] Section 1.5 Mother''s Milk'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-13 20:38:00
---

BFS

```c++
/*
ID: penyi191
TASK: milk3
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <algorithm>
#include <queue>

#define cin fin
#define cout fout

using namespace std;

class State {
public:
  State(int A, int B, int C, 
        int maxA, int maxB, int maxC){
    milk[0] = A;
    milk[1] = B;
    milk[2] = C;
    maxMilk[0] = maxA;
    maxMilk[1] = maxB;
    maxMilk[2] = maxC;
  }
  int milk[3], maxMilk[3];
};

State pour(State state, int from, int to) {
  state.milk[to] += state.milk[from];
  state.milk[from] = 0;
  if (state.milk[to] > state.maxMilk[to]) {
    state.milk[from] = state.milk[to] - state.maxMilk[to];
    state.milk[to] = state.maxMilk[to];
  }
  return state;
}


int main() {
  ofstream fout ("milk3.out");
  ifstream fin ("milk3.in");
  
  int A, B, C;
  bool used[21][21][21] = {0};
  
  cin >> A >> B >> C;
  
  State startSate = State(0, 0, C, A, B, C);
  used[0][0][C] = true;
  
  queue<State> Q;
  
  Q.push(startSate);
  
  while(!Q.empty()) {
    State curState = Q.front();
    Q.pop();
    used[curState.milk[0]][curState.milk[1]][curState.milk[2]] = true;
    
    // pour A into B
    for (int i = 0; i < 3; i++)
      for (int j = 0; j < 3; j++)
        if (i != j) {
          State tmpState = pour(curState, i, j);
          if (!used[tmpState.milk[0]][tmpState.milk[1]][tmpState.milk[2]]) 
            Q.push(tmpState);
        }
  }
  
  bool first = true;
  for (int c = 0; c <= C; c++) {
    for (int b = 0; b <= B; b++) {
      if (used[0][b][c]) {
        if (first) {
          cout << c;
          first = false;
        }
        else
          cout << " " << c;
        break;
      }
    }
  }
  cout << endl;
  
  return 0;
}

```