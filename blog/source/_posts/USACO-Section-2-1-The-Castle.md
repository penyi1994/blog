title: '[USACO] Section 2.1 The Castle'
author: PenYi
tags:
  - USACO
categories: []
date: 2022-11-27 13:56:00
---

DFS簡單題

題目出的超爛
到底優先要拿哪個牆壁?
farthest to the west 到底是越西越好還是越東越好?
真心看不懂
卡超久
最後直接找解答照抄找牆壁的部分才過ZZZZZ

```c++
/*
ID: penyi191
TASK: castle
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <cstring>

#define cin fin
#define cout fout

using namespace std;

int M, N;
int castle[101][101];

int cnt;

int RoomLabel[101][101];
int RoomSize[101 * 101];
int RoomNumber;

int MaxRoomSize;
int MaxMergeRoomSize;
int RemoveWall_X;
int RemoveWall_Y;
int RemoveWall;

bool vis[101][101];
int dx[4] = {-1, 0,  0, 1};
int dy[4] = { 0, 1, -1, 0};
int mask[4] = {2, 4, 1, 8};
char direction[4] = {'N', 'E', 'W', 'S'};

void dfs(int x, int y) {
  RoomLabel[x][y] = RoomNumber;
  cnt++;
  vis[x][y] = true;
  for (int i = 0; i < 4; i++) {
    if ((castle[x][y] & mask[i]) == 0) {
      int dst_x = x + dx[i];
      int dst_y = y + dy[i];
      if (dst_x >= 1 && dst_x <= M && dst_y >= 1 && dst_y <= N && !vis[dst_x][dst_y]) {
        dfs(dst_x, dst_y);
      }
    }
  }
}

int main() {
  ofstream fout ("castle.out");
  ifstream fin ("castle.in");
  
  // N and M are confusing
  cin >> N >> M;
  memset(castle, 0, sizeof(castle));
  for (int i = 1; i <= M; i++)
    for (int j = 1; j <= N; j++)
      cin >> castle[i][j];
  
  // Solve RoomNumber
  RoomNumber = 0;
  memset(vis, 0, sizeof(vis));
  memset(RoomLabel, 0, sizeof(RoomLabel));
  for (int i = 1; i <= M; i++) {
    for (int j = 1; j <= N; j++) {
      if (!vis[i][j]) {
        RoomNumber++;
        cnt = 0;
        dfs(i, j);
        RoomSize[RoomNumber] = cnt;
      }
    }
  }
  
  // Solve MaxRoomSize
  MaxRoomSize = 0;
  for (int i = 1; i <= RoomNumber; i++)
    MaxRoomSize = max(MaxRoomSize, RoomSize[i]);
  
  // Solve MaxMergeRoomSize
  MaxMergeRoomSize = 0;
  for (int y = 1; y <= N; y++) {
    for (int x = M; x >= 1; x--) {
      for (int i = 0; i <= 1; i++) {
        int dst_x = x + dx[i];
        int dst_y = y + dy[i];
        if ((castle[x][y] & mask[i]) == mask[i] && RoomLabel[x][y] != RoomLabel[dst_x][dst_y]) {
          int sum = RoomSize[RoomLabel[x][y]] + RoomSize[RoomLabel[dst_x][dst_y]];
          if (sum > MaxMergeRoomSize) {
            MaxMergeRoomSize = sum;
            RemoveWall_X = x;
            RemoveWall_Y = y;
            RemoveWall = i;
          }
        } 
      }
    }
  }
  
  cout << RoomNumber << endl;
  cout << MaxRoomSize << endl;
  cout << MaxMergeRoomSize << endl;
  cout << RemoveWall_X << " " << RemoveWall_Y << " " << direction[RemoveWall] << endl;
  
  return 0;
}

```