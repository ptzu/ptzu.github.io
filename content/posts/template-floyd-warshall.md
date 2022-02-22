---
title: Floyd Warshall
date: 2018-02-05 12:55:12
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

全點對最短路徑
時間複雜度: {%math%}O(n^{3}){%endmath%}

## Code
```cpp
void floyd(int V){
    for(int k = 0; k < V; k++){
        for(int i = 0; i < V; i++){
            for(int j = 0; j < V; j++){
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
}
```
