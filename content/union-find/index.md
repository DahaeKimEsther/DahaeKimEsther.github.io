---
emoji: ๐ช
title: (์๊ณ ๋ฆฌ์ฆ) Disjoint Set ๊ตฌ์กฐ์ Union Find ์๊ณ ๋ฆฌ์ฆ
date: '2019-05-19 02:00:00'
author: ์ค์ฝ๋ฉ
tags: algorithm
categories: ์๊ณ ๋ฆฌ์ฆ
---

## Disjoint-set ๊ตฌ์กฐ์ Union-Find ์๊ณ ๋ฆฌ์ฆ์ด๋?

- Disjoint set ์ด๋ ์ฐ๊ฒฐ์ด ๋์ด์ง ์์๋ค์ ์งํฉ์ ์๋ฏธ ํ๋ค.
- ์ด ๋ฐ์ดํฐ ๊ตฌ์กฐ๋ฅผ ์ํด์ Union-Find ์๊ณ ๋ฆฌ์ฆ์ด 2๊ฐ์ง ์ฃผ์ํ operation์ ์ ๊ณตํ๋ค.

1. Find : ์ด๋ค ์์๊ฐ ์ด๋ ์งํฉ์ ์๋์ง ์ฐพ์์ค๋ค. ์ฃผ๋ก ๋๊ฐ์ element๊ฐ ๊ฐ์ ์งํฉ์ ์๋์ง ํ์ธํ๋๋ฐ ์ฌ์ฉ๋๋ค.
2. Union : 2๊ฐ์ ์งํฉ์ ํ๋์ ์งํฉ์ผ๋ก ํฉ์ณ์ค๋ค.

**์ด ์๊ณ ๋ฆฌ์ฆ์ Cycle์ ์ฐพ๋๋ฐ ์์ฃผ ์ฉ์ดํ๋ค.**

## Union-Find์ ํ์ฉ ์ฝ๋ (์ฌ์ดํด ์ฐพ๊ธฐ)

![์ฌ์ง](https://raw.githubusercontent.com/zoomKoding/zoomKoding.github.io/source/assets/_posts/union-find-1.png)

์์ ๊ฐ์ ๊ทธ๋ํ ์ผ ๋ ์ฌ์ดํด ์ฌ๋ถ๋ฅผ Union-find๋ฅผ ์ด์ฉํด์ ํ์ธํด๋ณด์!

```cpp
//๊ฐ Edge์ ์ ๋ณด๊ฐ ๋ด๊ธด struct์ด๋ค.
struct Edge{
    int src, dest;
};

//๊ฐ Graph์ ์ ๋ณด๊ฐ ๋ด๊ธด struct์ด๋ค.
struct Graph{
    int V, E;
    struct Edge* edge;
};

//Graph๋ฅผ ์์ฑํด์ ๋ฐํํด์ฃผ๋ ์ญํ ์ ํ๋ ํจ์์ด๋ค.
//V๋ vertex ๊ฐ์, E๋ edge์ ๊ฐ์๋ฅผ ์๋ฏธํ๋ค.
struct Graph* createGraph(int V, int E){

    //๊ทธ๋ํ ๊ณต๊ฐ ํ ๋น!
    struct Graph* graph = (struct Graph*) malloc( sizeof ( struct Graph));

    //๊ทธ๋ํ์ ํฌ๊ธฐ๋ฅผ ์ฃผ์ด์ง ์ ๋ณด๋ฅผ ์ด์ฉํด ์ ํด์ค๋ค.
    graph->V = V;
    graph->E = E;

    //๊ทธ๋ํ์ edge ๊ฐ์๋งํผ Edge์ ๊ณต๊ฐ์ ํ ๋นํด์ค๋ค.
    graph->edge = (struct Edge*) malloc ( graph -> E * (struct Edge));

    //๋ง๋  ๊ทธ๋ํ๋ฅผ ๋ฐํํด์ค๋ค.
    return graph;
}

//ํด๋น vertex๊ฐ ์ด๋ ์งํฉ์ ์ํ๋์ง ํ์ธ์์ผ์ฃผ๋ ๋ธ๋์ด๋ค.
//๋๊ฐ์ vertex๊ฐ ๊ฐ์ ์งํฉ์ ์๋ค๋ฉด find์ ๊ฒฐ๊ณผ ๊ฐ์ด ๊ฐ์ ๊ฒ์ด๋ค.
int find(int parent[], int i){

    //๊ณ์ ํ๊ณ  ํ๊ณ  ๊ฐ์ ์ง๊ธ parent๊ฐ ์๋ ์น๊ตฌ๋ฅผ ์ฐพ์๋ณด์(์ฆ ๊ทธ๋ํ์ child node)
    if(parent[i] == -1) return i;
    return find(parent, parent[i]);
}

//๋ง์ฝ ๋๊ฐ์ child node๊ฐ ๋ค๋ฅด๋ค๋ฉด ๋ ๋ธ๋๋ฅผ ์ด์ด์ค์๋ค. ํ๋์ ๊ทธ๋ํ๊ฐ ๋ค๋ฅธ ๊ทธ๋ํ ๋ฐ์ผ๋ก ์ ๋ค์ด๊ฐ๊ฒ ๋๋ค.
void union(int parent[], int x, int y){

    //๋์ childnode๋ฅผ ์ฐพ๊ณ  ๋ค๋ฅด๋ฉด ํ๋๊ฐ ๋ค๋ฅธ ํ๋๋ฅผ ๋จน์.
    int xset = find(parent, x);
    int yset = find(parent, y);
    if(xset != yset){
        parent[xset] = yset;
    }
}

//Cycle์ ํ์ธํด์ฃผ๋ ํจ์
int isCycle(struct Graph* graph){

    //parent๋ผ๋ ์ด๋ ์ด ๋ณ์๋ฅผ ์์ฑํ๊ณ  ์ด๊ธฐ ๊ฐ์ -1, ์ฆ ๋ถ๋ชจ๊ฐ ์๋ ์ํ๋ก ๋์
    int *parent = (int*) malloc( graph->V * sizeof(int) );
    memset(parent, -1, sizeof(int) * graph->V);

    //๊ทธ๋ํ์ Edge๋ฅผ ์ญ ํ์ด๋ด์๋ค.
    for(int i = 0; i < graph->E; ++i)
    {
        //Edge์ src์ dest node๊ฐ ๊ฐ์ graph์ ์ํด์๋ ๋ด์๋ค.
        int x = find(parent, graph->edge[i].src);
        int y = find(parent, graph->edge[i].dest);

        //๊ฐ์ผ๋ฉด Cycle์ด ์๋๊ฒ๋๋ค.
        if (x == y) return 1;

        //์๋๋ฉด ๋ ๊ทธ๋ํ๋ฅผ ํฉ์ณ์!
        Union(parent, x, y);
    }
    return 0;
}

int main(){
    int V = 3, E = 3;
    struct Graph* graph = createGraph(V, E);

    // add edge 0-1
    graph->edge[0].src = 0;
    graph->edge[0].dest = 1;

    // add edge 1-2
    graph->edge[1].src = 1;
    graph->edge[1].dest = 2;

    // add edge 0-2
    graph->edge[2].src = 0;
    graph->edge[2].dest = 2;

    if(isCycle(graph)) printf("์ฌ์ดํด ์์ด์~");
    else printf("์ฌ์ดํด ์์ด์~");
}
```

์ฃผ์์ ํ์ธํ๋ฉด ํจ์ฌ ์ดํด๊ฐ ๋น ๋ฅผ ๊ฒ์ด๋ค.

## ํท๊ฐ๋ฆผ ์ฃผ์!!

union์ ํ๊ฒ ๋๋ฉด ๊ฐ์ ์งํฉ์ ์๋ ์์์ child node๊ฐ ๊ฐ์์ง๊ฒ ๋๋ค. ๋ฐ์ ๊ทธ๋ฆผ์ ์ฐธ๊ณ ํ์:)

![์ฌ์ง](https://raw.githubusercontent.com/zoomKoding/zoomKoding.github.io/source/assets/_posts/union-find-2.jpeg)

## ๋๋์ 

- ๋ด๊ฐ bfs๋ก ์งฐ์ ๋๋ณด๋ค ํจ์ฌ ํจ์จ์ ์ธ์ง๋ ๋ชจ๋ฅด๊ฒ ์ง๋ง ์ผ๋จ ๊ต์ฅํ ์ง๊ด์ ์ด๋ค.
- ๋๊ฐ์ ์งํฉ์ ํฉ์น๋ ๋ฐฉ๋ฒ๋ ์ด๋ป๊ฒ ํ์ง ํ๋๋ฐ child node๋ฅผ ์ด์ด์ฃผ๋ ๊ฒ ๋ง์ผ๋ก ์ด๊ฒ์ด ๊ฐ๋ฅํด๋ค๋ ๋ฐ์๋ ๋๋ฌด ์ข๋ค.
- ์์ง ํ์คํ ๊ฐ์ ์์ค์ง๋ง ํ๋ฒ ๋์ ํด๋ณผ๋ง ํ๋ค.
