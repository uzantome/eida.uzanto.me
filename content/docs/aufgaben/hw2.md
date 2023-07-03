---
title: hw2
math: true
description: studentische Lösungsskizze. Kann Fehler enthalten.
tags:
 - homework
---


# 1.1

```python
def cal(n):
    if n % 2 == 0:
        return n / 2
    
    return 3 * n + 1

def col(i, n):
    if i == 0:
        return n

    return cal(col(i-1, n))
```

# 1.2
```python
cal_it = lambda n : n / 2 if n % 2 == 0 else 3 * n + 1

def col_it(i, n):
    while i > 0:
        n = cal_it(n)
        i -= 1

# Iterativ
# ---------------------------------------
# def col_it(i, n):
#   while i > 0:              c1    i + 1
#     n = cal_it(n)           c2    i
#     i -= 1                  c3    i
#  
#   return n                  c4    1
#
# Ergebnis: c = ((i + 1) * c1) + (i * c2) + (i * c3) + c4
#             = (i * c1) + c1 + (i * c2) + (i * c3) + c4
#             = (i * (c1 + c2 + c3)) + c1 + c4
#           quasi
#             = a * i + b
#
#           => col_it in O(n)


```

# 2

```python
def foo(array, start, end):
  if start == end:                                          
    return array[start]                                     
                                                             
  mid = math.floor((start + end) / 2)                       
                                                            
  return foo(array, start, mid) * foo(array, mid+1, end)    

```

# 2.6
```python
def bar(array, start, end):
    result = 1
    for i in range(start, end + 1):
        result *= array[i]
    return result
```
# a

$$\forall n > n_0: c_1 \cdot g(n) \leq f(n) \leq c_2 \cdot g(n)$$

$$\exists c_1', c_2', n_0' > 0: \forall n > n_0': c_1' \cdot f(n) \leq g(n) \leq c_2' \cdot f(n)$$

Wähle $c_1' = \frac{1}{c_2}, c_2' = \frac{1}{c_1}, n_0' = n_0$. Sei nun $n > n_0'$.

Somit gilt:

$$ c_1 \cdot g(n) \leq f(n) \implies g(n) \leq \frac{f(n)}{c_1} = c_2' \cdot f(n) $$

$$ f(n) \leq c_2 \cdot g(n) \implies c_1' \cdot f(n) = \frac{f(n)}{c_2} \leq g(n) $$