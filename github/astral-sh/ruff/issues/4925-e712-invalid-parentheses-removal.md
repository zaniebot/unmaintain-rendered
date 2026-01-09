---
number: 4925
title: "E712: Invalid parentheses removal"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-07T12:08:12Z
updated_at: 2023-08-17T15:51:11Z
url: https://github.com/astral-sh/ruff/issues/4925
synced_at: 2026-01-07T13:12:15-06:00
---

# E712: Invalid parentheses removal

---

_Issue opened by @addisoncrump on 2023-06-07 12:08_

E712 fix may remove parentheses in a way which causes invalid syntax:

```py
def a(x):
  return x == 5

if(a(2)) == True:
  print("it's five")
```

Becomes:

```py
def a(x):
  return x == 5

ifa(2) is True:
  print("it's five")
```

Discovered by #4822.

---

_Label `bug` added by @charliermarsh on 2023-06-07 21:21_

---

_Comment by @charliermarsh on 2023-06-07 21:22_

\cc @MichaReiser - A parenthesized node would solve this (just tabulating).

In the meantime, we'd need to rewrite with LibCST I think.

---

_Comment by @MichaReiser on 2023-06-08 05:06_

> \cc @MichaReiser - A parenthesized node would solve this (just tabulating).
> 
> 
> 
> In the meantime, we'd need to rewrite with LibCST I think.

Thanks for the ping. Mutating the tree instead of manually creating text edits would solve that too

---

_Comment by @charliermarsh on 2023-06-08 15:45_

Ideally the edit would be "just" replacing `==` with `is`, rather than rewriting the entire expression :joy:

---

_Comment by @addisoncrump on 2023-06-08 15:47_

Sadly would not work for the opposite reason:
```py
if a==True:
  ...
```

would become
```py
if aisTrue:
  ...
```

---

_Referenced in [astral-sh/ruff#4972](../../astral-sh/ruff/issues/4972.md) on 2023-06-08 19:55_

---

_Comment by @jasikpark on 2023-06-17 05:56_

Just repro'ed another version of this w/ #4822 in libafl mode 😁 

```python
if(True) == TrueElement or x == TrueElement:
    pass

assert (not Soo) in bar
assert {"x": not foo} in bar
```

```shell
cargo run --bin ruff -- --fix ./minimized-from-crash-2c30da3ded030419
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff --fix ./minimized-from-crash-2c30da3ded030419`
warning: Detected debug build without --no-cache.
error: Autofix introduced a syntax error in `minimized-from-crash-2c30da3ded030419` with rule codes E712: invalid syntax. Got unexpected token Newline at byte offset 42
---
ifTrue is TrueElement or x == TrueElement:
    pass

assert (not Soo) in bar
assert {"x": not foo} in bar

---
minimized-from-crash-2c30da3ded030419:1:4: E712 Comparison to `True` should be `cond is True` or `if cond:`
minimized-from-crash-2c30da3ded030419:1:13: F821 Undefined name `TrueElement`
minimized-from-crash-2c30da3ded030419:1:28: F821 Undefined name `x`
minimized-from-crash-2c30da3ded030419:1:33: F821 Undefined name `TrueElement`
minimized-from-crash-2c30da3ded030419:3:14: F821 Undefined name `Soo`
minimized-from-crash-2c30da3ded030419:3:22: F821 Undefined name `bar`
minimized-from-crash-2c30da3ded030419:4:18: F821 Undefined name `foo`
minimized-from-crash-2c30da3ded030419:4:26: F821 Undefined name `bar`
Found 8 errors.
```

---

_Comment by @addisoncrump on 2023-07-31 23:36_

I found another, more exciting example of this; consider the following snippet:

```py
x = range(0, 10)

def y():
  global x

  for i in x:
    if (yield i) == True:
      print("even")
    else:
      print("odd")

v = y()
next(v)

for i in range(1, 10):
  print(v.send(i % 2 == 0))
```

This is a really dumb program that uses generators wrong, but it gets the point across. Ruff attempts to fix this to:

```py
x = range(0, 10)

def y():
  global x

  for i in x:
    if yield i is True:
      print("even")
    else:
      print("odd")

v = y()
next(v)

for i in range(1, 10):
  print(v.send(i % 2 == 0))
```

Which is invalid because the `yield` must be surrounded by parentheses. I think the answer to this is to simply _not_ remove the parentheses, as it seems to be not a totally sane fix.

---

_Comment by @dhruvmanila on 2023-08-01 04:35_

> Which is invalid because the `yield` must be surrounded by parentheses. I think the answer to this is to simply _not_ remove the parentheses, as it seems to be not a totally sane fix.

This seems like a regression as I can only reproduce this on `v0.0.279` onwards and not on any previous versions.


---

_Comment by @dhruvmanila on 2023-08-01 04:53_

A bisection search gives me 875e04e36900114a3b7fdefe84e5ea9c4afdb5b7.

Right, so the patch generation was moved from `Generator`-based to `Locator`-based which is not aware of the parenthesis while the `Generator`-based is.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-14 21:57_

---

_Referenced in [astral-sh/ruff#6575](../../astral-sh/ruff/pulls/6575.md) on 2023-08-14 22:30_

---

_Closed by @charliermarsh on 2023-08-17 15:51_

---
