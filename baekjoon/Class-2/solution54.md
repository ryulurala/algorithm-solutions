---
title: "나이순 정렬(10814)"
category: 백준[Class-2]
tags: [C++, JavaScript, 백준]
date: "2021-03-20"
---

## 문제 링크

[나이순 정렬(10814)](https://www.acmicpc.net/problem/10814)

## C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool cmp(pair<int, string> a, pair<int, string> b){
    return a.first < b.first;   // 나이 순
}

// 문제 풀이 함수
void solution(){
    int n, age;
    string name;
    cin >> n;

    vector<pair<int, string> > people;
    while(cin >> age >> name){
        people.push_back({age, name});
    }

    stable_sort(people.begin(), people.end(), cmp);

    for(auto p: people){
        cout<<p.first<<" "<<p.second<<"\n";
    }
}

bool exists(const char* fileName){
    FILE* fp;
    if((fp = fopen(fileName, "r"))){
        fclose(fp);
        return true;
    }
    return false;
}

int main() {
    if(exists("stdin")){
        freopen("stdin", "r", stdin);
        solution();
        fclose(stdin);
    }
    else{
        solution();
    }

    return 0;
}
```

## JavsScript

```js
const fs = require("fs");
// split 조절
const input = fs.readFileSync("dev/stdin").toString().trim().split("\n");

// 문제 풀이
const n = +input[0];
const people = input.filter((v, i) => i > 0).map((v) => v.split(" "));

// 정렬
people.sort((a, b) => (+a[0] < +b[0] ? -1 : 1));

// print
console.log(people.map((v) => v.join(" ")).join("\n"));
```
