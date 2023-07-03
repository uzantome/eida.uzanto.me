---
title: hw5
math: true
description: studentische Lösungsskizze. Kann Fehler enthalten.
tags:
 - homework
---

# 1
```python
def lexi_sort(arr, pivot_func):
    adapted_arr = []
    
    for i in range(len(arr)):
        adapted_arr.appen([arr[i], num_to_list(arr[i])])
        
    adapted_arr = quicksort(adapted_arr, pivot_func)
    
    result = []
    for i in range(len(adapted_arr)):
        result.append(adapted_arr[i][0])
    
    return result

def quicksort(arr, pivot_func):
    if len(arr) < 1:
        return arr
        
    pivot = pivot_func(arr)
    left, mid, right = partition(arr, pivot)

    left = quicksort(left, pivot_func)
    right = quicksort(right, pivot_func)
    arr = left + mid + right

    return arr
```

```python
def partition(arr, pivot):
    smallerList = []
    biggerList = []

    for i in range(0, len(arr)):
        if i == pivot[0]: continue

        if compare_lexicographically(arr[i],pivot[1]): #arr_element < pivot
            smallerList.append(arr[i])
        else:
            biggerList.append(arr[i])

    return smallerList, [pivot[1]], biggerList

def compare_lexicographically(a, b):
    x = 0
    while True:
        if len(a) == x:
            return True
        if len(b) == x:
            return False
        if a[x] < b[x]:
            return True
        elif a[x] > b[x]:
            return False
        else:
            x = x + 1
            
def num_to_list(x):
    return [x] if x < 10 else num_to_list(x//10) + [x%10]

def first_pivot(arr):
    return 0

def last_pivot(arr):
    if len(arr) == 0:
        return -1
    return len(arr) - 1

def random_pivot(arr):
    return random.randrange(0, len(arr))

def middle_pivot(arr):
    return len(arr)//2

def median_first_middle_last_pivot(arr):
    if len(arr) <= 2:
        return first_pivot(arr)
    
    left, right, mid = 0, len(arr)-1, len(arr)//2
    pivot_index = median_of_three(arr,left, right, mid)
    return pivot_index
```
#	1.2
Subtract-and-Conquer:

$T(n)=aT(n-b)+f(n)$
$T(n-b)=aT(n-2b)+f(n-b)$
$T(n-2b)=aT(n-3b)+f(n-2b)$
$T(n-3b)=aT(n-4b)+f(n-3b)$

Einsetzen in $T(n)$:

$$T(n) = a^2T(n-2b)+af(n-b)+af(n)$$
$$T(n) = a^3T(n-3b)+a^2f(n-2b)+af(n-b)+af(n)$$
$$\vdots$$
$$T(n) = \sum_{i=0}^{n/b} a^if(n-ib)  + T(0)=\mathcal{O}(n^k \sum_{i=0}^{n/b} a^i)$$
Fall 1: $a\lt 1$: $$\sum_{i=0}^{n/b} a^i = \mathcal{O}(1),\space T(n) = \mathcal{O}(n^k+1)$$
Fall 2: $a= 1$: $$\sum_{i=0}^{n/b} a^i =\sum_{i=0}^{n/b} 1 = \mathcal{O}(n),\space T(n) = \mathcal{O}(n^k \times n)$$
Fall 3: $a\gt 1$: $$\sum_{i=0}^{n/b} a^i = \mathcal{O}(a^{n\over b}), T(n) = \mathcal{O}(n^k \times a^{n\over b})$$
$T_{qs}(n)=T(p)+T(n-p-1)+\Theta (n)$

Worst-Case: Pivot-Element ist größtes Element

$\longrightarrow\space n-1 = p$ (alle Elemente sind kleiner und in derselben Teilmenge nach Partitionsschritt)
$$T_{qs}(n)=T(n-1)+T(n-(n-1)-1)+\Theta (n) = T(n-1)+T(0)+\Theta (n)$$
$$\Longrightarrow \space a=1, b=1, f(n) = \mathcal{O}(n)$$

Anwenden des Master-Theorems für Subtract-and-Conquer:

Fall 2: 
$$T_{qs}(n)=\mathcal{O}(n)\mathcal{O}(f(n)) = \mathcal{O}(n^2)$$

# 2

```python
def merge(left, right):
    merged_arr = []

    while len(left) > 0 and len(right) > 0:
        if left[0] <= right[0]:
            merged_arr += [left[0]]
            left = left[1:]
        else:
            merged_arr += [right[0]]
            right = right[1:]

    merged_arr += left + right

    return merged_arr

def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    middle = len(arr) // 2

    l = merge_sort(arr[0:middle])
    r = merge_sort(arr[middle:len(arr)])

    return merge(l, r)

def hybrid_sort(arr: list[int], num_buckets: int):
    buckets = [[] for _ in range(num_buckets)]
    lower, upper = min(arr), max(arr)

    # Divide into buckets
    for e in arr:
        # index = (e - lower) / bucket_size
        # bucket_size = (upper - lower + 1) / num_buckets
        index = num_buckets * (e - lower) // (upper - lower + 1)
        buckets[min(index, num_buckets - 1)].append(e)

    # Sort and concatenate buckets
    i = 0
    for bucket in buckets:
        sorted_bucket = merge_sort(bucket)
        for e in sorted_bucket:
            arr[i] = e
            i += 1

    return arr


```