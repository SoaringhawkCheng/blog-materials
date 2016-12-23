---
title: LeetCode题解 §9 广度搜索
date: 2016-12-21 14:26:29
categories: 编程
tags: Leetcode
---
他自己表达的，确实是他在日复一日的思虑和苦痛中凝结起来的东西，他想传达给对方的，也是长期经受等待和苦恋煎熬的景象。对方却相反，认为他那些感情都是俗套，他的痛苦俯拾即是，他的惆怅人皆有之。
						————阿贝尔·加缪

<!--more-->

## 127. Word Ladder

```
#include <stdio.h>
#include <iostream>
#include <unordered_set>
#include <queue>
using namespace std;

class Solution1 {//按书中方法一自己改写的方法
public:
    int ladderLength(string beginWord, string endWord, unordered_set<string>& wordList) {
        queue<pair<string,int>> q;
        unordered_set<string> visited;
        q.push(make_pair(beginWord, 1));
        while(!q.empty()) {
            pair<string,int> state=q.front();
            q.pop();
            vector<pair<string,int>> new_states;
            if(state.first==endWord) return state.second;
            for(int i=0;i<beginWord.size();++i){
                for(auto c='a';c<='z';++c){
                    if(c==state.first[i]) continue;
                    swap(c,state.first[i]);
                    if((wordList.find(state.first)!=wordList.end()||state.first==endWord)&&visited.find(state.first)==visited.end())
                        new_states.push_back(make_pair(state.first, state.second+1));
                    swap(c,state.first[i]);
                }
            }
            for(auto new_state:new_states) {
                q.push(new_state);
                visited.insert(new_state.first);
            }
        }
        return 0;
    }
};

class Solution2 {//书中方法二，双队列
public:
    int ladderLength(string beginWord, string endWord, unordered_set<string>& wordList) {
        queue<string> cur,next;
        unordered_set<string> visited;
        int level=0;
        cur.push(beginWord);
        visited.insert(beginWord);
        while(!cur.empty()) {
            ++level;
            while(!cur.empty()) {
                string state=cur.front();
                cur.pop();
                if(state==endWord) return level;
                unordered_set<string> new_states;
                for(int i=0;i<beginWord.size();++i){
                    for(char c='a';c<='z';++c){
                        if(c==state[i]) continue;
                        swap(c,state[i]);
                        if((wordList.find(state)!=wordList.end()||state==endWord)&&visited.find(state)==visited.end())
                            new_states.insert(state);
                        swap(c,state[i]);
                    }
                }
                for(auto new_state:new_states){
                    next.push(new_state);
                    visited.insert(new_state);
                }
            }
            swap(next,cur);
        }
        return 0;
    }
};
```

## 126. Word Ladder

```
#include <stdio.h>
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>
#include <string>
using namespace std;

class Solution1 {//书上单队列修改
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, unordered_set<string> &wordList) {
        queue<pair<string,int>> q;
        unordered_map<string,int> visited;
        unordered_map<string, vector<string>> fathers;
        vector<vector<string>> result;
        q.push({beginWord,1});
        while(!q.empty()) {
            auto state=q.front();
            q.pop();
            if(!result.empty()&&state.second>result[0].size()) break;
            if(state.first==endWord){
                vector<string> path;
                generate_path(fathers,beginWord,endWord,path,result);
                continue;//跳过后面的，不断pop队列q
            }
            vector<pair<string,int>> new_states;
            for(int i=0;i<beginWord.size();++i){
                for(auto c='a';c<='z';++c){
                    if(c==state.first[i]) continue;
                    swap(c,state.first[i]);
                    if(wordList.find(state.first)!=wordList.end()||state.first==endWord){//在wordList里或者是endWord，可以同阶重复是因为要计算父节点
                        auto iter=visited.find(state.first);
                        if(iter==visited.end()||(iter!=visited.end()&&iter->second==state.second+1))//未遍历或者同阶的后继节点
                            new_states.push_back(make_pair(state.first,state.second+1));
                    }
                    swap(c, state.first[i]);
                }
            }
            for(auto new_state:new_states){
                if(visited.find(new_state.first)==visited.end()){//不能重复推入
                    q.push(new_state);
                    visited.insert(new_state);
                }
                fathers[new_state.first].push_back(state.first);
            }
        }
        return result;
    }
private:
    void generate_path(unordered_map<string, vector<string>> &fathers,string &start,string &cur,vector<string> &path,vector<vector<string>> &result){
        path.push_back(cur);
        if(start==cur){
            if(result.empty()||path.size()==result[0].size()){
                result.push_back(path);
                reverse(result.back().begin(), result.back().end());
            }
        }
        else{
            for (auto &father:fathers[cur])
                generate_path(fathers, start, father, path, result);
        }
        path.pop_back();
    }
};

class Solution2 {//双队列
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, unordered_set<string> &wordList) {
        queue<pair<string,int>> cur;
        queue<pair<string,int>> next;
        unordered_map<string, int> visited;
        vector<vector<string>> result;
        unordered_map<string, vector<string>> fathers;
        cur.push({beginWord,1});
        while(!cur.empty()){
            while(!cur.empty()){
                auto state=cur.front();
                cur.pop();
                if(state.first==endWord){
                    vector<string> path;
                    generate_path(fathers, path, beginWord, endWord, result);
                    continue;
                }
                vector<pair<string,int>> new_states;
                for(int i=0;i<beginWord.size();++i){
                    for(auto c='a';c<='z';++c){
                        if(c==state.first[i]) continue;
                        swap(c,state.first[i]);
                        if(wordList.find(state.first)!=wordList.end()||state.first==endWord)
                            if(visited.find(state.first)==visited.end()||visited[state.first]==state.second+1)
                                new_states.push_back({state.first,state.second+1});
                        swap(c, state.first[i]);
                    }
                }
                for(auto new_state:new_states){
                    if(visited.find(new_state.first)==visited.end()){
                        visited.insert(new_state);
                        next.push(new_state);
                    }
                    fathers[new_state.first].push_back(state.first);
                }
            }
            swap(cur,next);
        }
        return result;
    }
private:
    void generate_path(unordered_map<string, vector<string>> &fathers,vector<string> &path,string start,string cur,vector<vector<string>> &result){
        path.push_back(cur);
        if(cur==start){
            if(result.empty()||path.size()==result[0].size()){
                result.push_back(path);
                reverse(result.back().begin(), result.back().end());
            }
        }
        else{
            for(auto &father:fathers[cur]) {
                generate_path(fathers, path, start, father, result);
            }
        }
        path.pop_back();
    }
};

```

## 130. Surrounded Regions

```
#include <stdio.h>
#include <vector>
#include <queue>
#include <iostream>
using namespace std;

class Solution {
public:
    void solve(vector<vector<char>> &board) {
        if(board.empty()) return;
        int m=board.size();
        int n=board[0].size();
        for(int i=0;i<n;++i){
            bfs(board,0,i);
            bfs(board,m-1,i);
        }
        for(int i=1;i<m-1;++i){
            bfs(board,i,0);
            bfs(board,i,n-1);
        }
        for(int i=0;i<m;++i)
            for(int j=0;j<n;++j){
                if(board[i][j]=='O')
                    board[i][j]='X';
                else if(board[i][j]=='+')
                    board[i][j]='O';
            }
    }
private:
    void bfs(vector<vector<char>> &board,int i,int j){
        queue<pair<int,int>> q;
        vector<pair<int, int>> orient={{0,1},{0,-1},{-1,0},{1,0}};
        int row=board.size();
        int col=board[0].size();
        if(i>=0&&i<row&&j>=0&&j<col&&board[i][j]=='O'){
            board[i][j]='+';
            q.push({i,j});
        }
        while(!q.empty()) {
            auto cur=q.front();
            q.pop();
            vector<pair<int,int>> result;
            for(int k=0;k<4;++k){
                int ii=cur.first+orient[k].first;
                int jj=cur.second+orient[k].second;
                if(ii>=0&&ii<row&&jj>=0&&jj<col&&board[ii][jj]=='O'){
                    board[ii][jj]='+';
                    result.push_back({ii,jj});
                }
            }
            for(auto res:result) q.push(res);
        }
    }
};

```
