---
title: hw8
math: true
description: studentische Lösungsskizze. Kann Fehler enthalten.
tags:
 - homework
---


# page-rank
```python
import networkx as nx
import matplotlib.pyplot as plt
import random

def pagerank(A, d, iterations):
    nodes = len(A)
    M = [[0 for j in range(0,nodes)] for i in range(0,nodes)]
    # Calculate M from A
    for i in range(0,nodes):
        count = 0
        for j in range(0,nodes):
            count += A[i][j]
        for j in range(0,nodes):
            if(A[i][j] == 1):
                M[i][j] = 1/count
    # Calculate M' = d * M + ...

    for i in range(0,nodes):
        for j in range(0,nodes):
            M[i][j] = M[i][j]*d + (1-d)*1/nodes

    # Normalize
    for i in range(0,nodes):
        count = 0
        for j in range(0,nodes):
            count += M[i][j]
        for j in range(0,nodes):
            M[i][j] /= count

    #Transponiere
    MT = [[0 for j in range(0,nodes)] for i in range(0,nodes)]
    for i in range(0,nodes):
        for j in range(0,nodes):
            MT[j][i] = M[i][j]
    # Initialize PR

    PR = [1/nodes for i in range(0,nodes)]

    # Power Iteration
    for l in range(0,iterations):
        PRsafe = [0 for i in range(0,nodes)]
        for i in range(0,nodes):
            for j in range(0,nodes):
                PRsafe[i] += MT[i][j]*PR[j]
        count = 0
        PR=PRsafe
        for i in range(0,nodes):
            count += PR[i]
        for i in range(0,nodes):
            PR[i] /= count
    
    return PR

def plot_graph(adj_matrix, pageranks):
    # create directed graph from adjacency matrix
    edges = []
    for i in range(len(adj_matrix)):
        for j in range(len(adj_matrix[i])):
            if adj_matrix[i][j] != 0:
                edges.append((i, j))
    G = nx.DiGraph(edges)
    for i in range(len(pageranks)):
        if not G.has_node(i):
            G.add_node(i)   
    # set node labels and sizes based on pageranks
    node_sizes, node_labels = [], {}
    for i in G.nodes:
        node_labels[i] = f'{i}\n{pageranks[i]:.2f}'
        node_sizes.append(max(1000, int(10000 * pageranks[i])))
    # set node colors based on pageranks
    node_colors = [(0.0 + pageranks[i], 0.0, 1.0 - pageranks[i], 1.0) for i in G.nodes]
    # plot graph
    pos = nx.spring_layout(G, seed=42, k=1, iterations=10)
    nx.draw_networkx_nodes(G, pos, node_size=node_sizes, node_color=node_colors)
    nx.draw_networkx_labels(G, pos, labels=node_labels, font_size=10)
    nx.draw_networkx_edges(G, pos, connectionstyle='arc3,rad=.1', node_size=node_sizes)
    plt.axis('off')
    plt.show()
    
def generate_random_digraph(V, E):
    adj_matrix = [[0 for j in range(V)] for i in range(V)]
    edges = set()
    while len(edges) < E:
        start_node, end_node = random.randint(0, V-1), random.randint(0, V-1)
        if start_node != end_node:
            edges.add((start_node, end_node))
    for start_node, end_node in edges:
        adj_matrix[start_node][end_node] = 1
    return adj_matrix
    
d, iterations = 0.85, 1000
# Wikipedia:
# Die Ergebnisse fuer das englische und deutsche wiki
# sind fuer den gleichen Graphen scheinbar different
# Dies koennte an der (fehlender) Normierung liegen
adj_matrix_wiki = [
    [0,0,0,0,0,0,0,0,0,0,0],#A
    [0,0,1,0,0,0,0,0,0,0,0],#B
    [0,1,0,0,0,0,0,0,0,0,0],#C
    [1,1,0,0,0,0,0,0,0,0,0],#D
    [0,1,0,1,0,1,0,0,0,0,0],#E
    [0,1,0,0,1,0,0,0,0,0,0],#F
    [0,1,0,0,1,0,0,0,0,0,0],#G
    [0,1,0,0,1,0,0,0,0,0,0],#H
    [0,1,0,0,1,0,0,0,0,0,0],#I
    [0,0,0,0,1,0,0,0,0,0,0],#J
    [0,0,0,0,1,0,0,0,0,0,0],#K
]
adj_matrix_standford = [
    [0,1,1],
    [1,0,1],
    [1,0,0],
]
adj_matrix = generate_random_digraph(8,12)
pageranks = pagerank(adj_matrix_wiki, d, iterations)
plot_graph(adj_matrix_wiki, pageranks)


```

# ford fulkerson

```python
from train_max_flow_backend import *

#
# 3.6 (a)
#

def ford_fulkerson_max_flow(graph, source, sink):
    #So in der Art ist die Methode
    flow = 0
    maxFlow = 0
    graph = transform_graph(graph.copy())
    resgraph = create_residual_graph(graph)
    path = find_augmenting_path(resgraph, source, sink)
    bahnhoefe = set()
    while len(path)>0:
        path = find_augmenting_path(resgraph, source, sink)
        flow = calculate_path_flow(resgraph, path)
        maxFlow += flow
        update_residual_graph(resgraph, path, flow)
        for bahnhof in path:
            bahnhoefe.add(bahnhof)

    return (maxFlow, bahnhoefe, resgraph)

#
# 3.6 (b)
#

def create_residual_graph(graph):
    dictPath = {}
    for node in graph:
        #Edge
        if not node["source"] in dictPath:
            dictPath[node["source"]] = {}
        dictPath[node["source"]][node["target"]] = node["capacity"]
        #Backedge
        if not node["target"] in dictPath:
            dictPath[node["target"]] = {}
        dictPath[node["target"]][node["source"]] = 0
    return dictPath

#
# 3.6 (c)
#

def find_augmenting_path(graph, source, sink):
    path = []
    visited = set()

    def dfs(u):
        visited.add(u)
        path.append(u)

        if u == sink:
            return True

        for v in graph[u].keys():
            if v not in visited and graph[u][v] > 0:
                if dfs(v):
                    return True
        path.pop()
        return

    dfs(source)

    return path

#
# 3.6 (d)
#

def calculate_path_flow(graph, path):
    if len(path) == 0:
        return 0
    smallestFlow = graph[path[0]][path[1]]
    for i in range(len(path)-1):
        if graph[path[i]][path[i+1]] < smallestFlow:
            smallestFlow = graph[path[i]][path[i+1]]
    return smallestFlow

#
# 3.6 (e)
#

def update_residual_graph(graph, path, path_flow):
    for i in range(len(path)-1):
        u = path[i]
        v = path[i+1]
        graph[u][v] -= path_flow
        graph[v][u] += path_flow

#
# 3.6 (f)
#

def transform_graph(graph):
    for u in graph:
        for v in graph:
            if u["source"] == v["target"] and v["source"] == u["target"]:
                helper = {"source": u["source"] + u["target"] + "helper", "target": u["target"]}
                graph.append(helper)
                x = city_positions[u["target"]][0] + (city_positions[u["source"]][0] - city_positions[u["target"]][0])/2
                y = city_positions[u["target"]][1] + (city_positions[u["source"]][1] - city_positions[u["target"]][1])/2
                city_positions[u["source"] + u["target"] + "helper"] = (x,y)
                u["target"] = u["source"] + u["target"] + "helper"
                
    add_capacities(graph)
    return graph

############################################################
# Testbereich beginnt:
############################################################

from train_data import Routes_Bavaria as Routes, city_positions_bavaria as city_positions

#from train_data import Routes_Bavaria_Extended as Routes
#from train_data import  city_positions_bavaria_extended as city_positions

#from train_data import Routes, city_positions

add_capacities(Routes)

maxflow = ford_fulkerson_max_flow(Routes, "Würzburg", "Passau")
print(maxflow)
#maxflow = ford_fulkerson_max_flow(Routes, "Vancouver", "Miami")

#from train_data import Simple_Test_Case as Routes
#from train_data import simple_test_case_positions as city_positions
#maxflow= ford_fulkerson_max_flow(Routes, "A", "D")

plot_routes(Routes, city_positions, maxflow)
```