---
title: hw3
math: true
description: studentische Lösungsskizze. Kann Fehler enthalten.
tags:
 - homework
---

# 2

```python
# Proof
# { T }
# { x + x + x = 3*x} # WEAK
y=x
# { x + x + y = 3*x } # ASG
y=x+x+y
# { y = 3*x } # ASG
```

# 3.3

```python
# Proof                    # Rules
# { T }                    # (PRE)
if a > b:
	# { T ∧ a > b } # IF
	max = a
    # { max = a ∧ max > b}
	# { max >= a ∧ max >= b }  # WEAK
else:
	# { T ∧ a <= b } # IF
	max = b
    # { max = b ∧ a <= max}
	# { max >= a ∧ max >= b}  # WEAK
# {max >= a ∧ max >= b} # IF
```

# 3.4

```python
# Proof                    # Rules
# { T }                    # (PRE)
if x < 0:
	# { T && x < 0 }       # (IF)
	y = -1
	# { x = 0 -> y = 0 }   # (WEAK)
elif x > 0:
	# { T && x > 0 }       # (IF)
	y = 1
	# { x = 0 -> y = 0 }   # (WEAK)
else:
	# { T && x = 0 }       # (IF)
	y = 0
	# { y = 0 && x = 0 }   # (ASG)
	# { x = 0 -> y = 0 }   # (WEAK)
# { x = 0 -> y = 0 }       # (IF)
```


# 4.5
```python
#{s = x +z} # PRE
while z != 0:
    #{s = x +z && z != 0} # WHILE
    #{s = x+1 +z-1}
    x=x+1
    #{s = x +z-1} # ASG
    z=z-1
    #{s = x + z} # ASG
#{s = x +z && z=0}  # WHILE
#{s = x +z} # WEAK
```

# 4.6
```python
# { x > 0}
# { 1 = 1}
y=1
# {y = 1}
# {y = 0!}
z=0
# {y = z!}
while (z != x):
    # {y = z! && z != x}
    # { y * (z + 1) = (z + 1)! } # (WEAK)
    z=z+1
    # { y * z = z! }             # (ASG)
    y=y*z
    # {y = z!} # ASG
# { y = z! && z=x}
# { y = x! } # WEAK

```

# 5
```python
# { x > 0}
# { 1 = 1}
y=1
# {y = 1}
# {y = 0!}
z=0
# {y = z! && 0 <= x-z}
while (z != x):
    # {y = z! && z != x && (0 <= x-z = x-z0)}
    # { y * (z + 1) = (z + 1)! && (0 <= x-(z+1) < x-z0) } # (WEAK)
    z=z+1
    # { y * z = z! && (0 <= x-z < x-z0)}             # (ASG)
    y=y*z
    # {y = z! && (0 <= x-z < x-z0)} # ASG
# { y = z! && z=x}
# { y = x! } # WEAK

```