---
title: hw6
math: true
description: studentische Lösungsskizze. Kann Fehler enthalten.
tags:
 - homework
---

# 1
Beweis per Induktion über $|E|$:

* **Induktionsanfang:** $|E| = 0 \implies E = \emptyset$

   Kein Knoten hat einen benachbarten Knoten. Deshalb gilt:

   $$
   \sum_{u \in V} d_G(u) = 0 = 2 \cdot |E|
   $$

* **Induktionsvoraussetzung:** Das Theorem gilt für $|E'|$ Kanten mit einem Graph $G' = (V, E')$.

* **Induktionsschritt:** $|E| = |E'| + 1, E = E' \cup \{ \{ v, w \} \}$

   Im Vergleich zum Graphen $G'$ haben die Knoten $v$ und $w$ je einen benachbarten Knoten mehr. Es gilt:

   $$
   \sum_{u \in V} d_G(u) = 2 + \sum_{u \in V} d_{G'}(u) = 2 + 2 \cdot |E'| = 2 \cdot (|E'| + 1)
   $$
   
# 2.3

Angenommen, ein ungerichteter Graph $G = (V, E)$ hätte eine ungerade Anzahl von Knoten mit ungeradem Grad.

Sei $U$ die Menge der Knoten mit ungeradem Grad und $G$ die Menge der Knoten mit geradem Grad, sodass $V = U \cup G$. Dann gilt für einen Knoten $u \in V$ und eine Konstante $i_u \in \mathbb{N}$:

$$
d_G(u) =
	\begin{cases}
	2i_u, & \text{falls } u \in G \\
	2i_u - 1, & \text{falls } u \in U
	\end{cases}
$$

Somit ergibt sich:
$$
\begin{align*}
\sum_{u \in V} d_G(u) &= \left( \sum_{u \in G} 2i_u \right) + \left( \sum_{u \in U} 2i_u - 1 \right) \\
 &= 2 \cdot \left( \sum_{u \in G} i_u \right) + 2 \cdot \left( \sum_{u \in U} i_u \right) - |U| \\
 &= 2 \cdot \left( \sum_{u \in V} i_u \right) - |U|
\end{align*}
$$

Per Annahme ist $|U|$ ungerade, also $|U| = 2n - 1$ für ein $n$. Zusammengefasst gilt:

$$
\sum_{u \in V} d_G(u) = 2 \cdot \left( n + \sum_{u \in V} i_u \right) - 1
$$

Diese Summe ist offensichtlich ungerade. Aber per Theorem 1 gilt:

$$
\sum_{u \in V} d_G(u) = 2 \cdot |E|
$$

Das ist offensichtlich gerade. Widerspruch!
# 

```
for u ∈ G                 # Kosten: c_i  Anzahl: |V|+1
    u.explored = false               . # Anzahl: |V|
Q = ∅                                . # Anzahl: 1
root.explored = true                 . # Anzahl: 1 
enqueue(Q, root)                       # Anzahl: 1
while Q ̸= ∅                            # Anzahl: |V|+1
    u = dequeue(Q)                     # Anzahl: |V|
    for each v ∈ G.Adj[u]              # Anzahl: |V| + |E|
        if v.explored == false then    # Anzahl: |E|
            v.explored = true          # Anzahl: |V| - 1
            enqueue(Q,v)               # Anzahl: |V| - 1
```