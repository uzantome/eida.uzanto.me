---
title: hw7
math: true
description: studentische Lösungsskizze. Kann Fehler enthalten.
tags:
 - homework
---



|  | A  | B | C | D | E |
| -------- | -------- | -------- |--- |---|---|
|      | / 0     | / ∞     |  / ∞| / ∞| / ∞ |
| | |A 6 | | A 1| | |
| | |D 3 | | | D 2 |
| | | | E 7| | 
| | | | | | 


|  | A  | B | C | D | E | F
| -------- | -------- | -------- |--- |---|---|---
|      | / 0     | A 6     |  A 5| B 7| C 9| D 9


```python
def floyd_warshall(graph):
    '''
    Eine leere n*m Matrix kann in Python wie folgt erstellt werden:
    m = [[0 for j in range(n)] for i in range(m)]
    Damit hat man n Spalten und m Zeilen
    Nutzen Sie dieses Prinzip um die Matrizen cost und pred zu erstellen cost soll die Kostenmatri sein und pred die Vorg ̈angermatrix
    '''
    n = len(graph)
    cost = [[INF for _ in range(n)] for _ in range(n)]
    pred = [[None for _ in range(n)] for _ in range(n)]
    for src in range(n):
        for dst in range(n):
            if graph[src][dst] != INF:
                cost[src][dst] = graph[src][dst]
                pred[src][dst] = src
    for i in range(n):
        cost[i][i] = 0
        pred[i][i] = i
    for k in range(n):
        for i in range(n):
            for j in range(n):
                if cost[i][j] > cost[i][k] + cost[k][j]:
                    cost[i][j] = cost[i][k] + cost[k][j]
                    pred[i][j] = k
    return cost, pred
```