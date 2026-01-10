```yaml
number: 7603
title: "Formatter instability: own-line comments on function parameter defaults"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - formatter
  - accepted
assignees: []
created_at: 2023-09-22T16:16:46Z
updated_at: 2023-10-12T15:50:15Z
url: https://github.com/astral-sh/ruff/issues/7603
synced_at: 2026-01-10T11:09:49Z
```

# Formatter instability: own-line comments on function parameter defaults

---

_Issue opened by @charliermarsh on 2023-09-22 16:16_

Given:

```python
def f2(x:e2=
    #
"foo") -> r2:
    pass

def f3(#
 x:e3#=
  = #
    #
 123#
#
) -> r3:
    pass
```

We first format as:

```python
def f2(
    x: e2 = #
    "foo",
) -> r2:
    pass


def f3(  #
    x: e3 = #  # =  #
    123,  #
    #
) -> r3:
    pass
```

And then as:

```python
def f2(
    x: e2 = "foo",  #
) -> r2:
    pass


def f3(  #
    x: e3 = 123,  #  # =  #  #
    #
) -> r3:
    pass
```

We probably need to treat those comments as dangling, as for comments around colons.

Sourced from https://github.com/astral-sh/ruff/issues/7590 (`A_870428921595896654.py`).

---

_Label `bug` added by @charliermarsh on 2023-09-22 16:16_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 16:16_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-09-22 16:16_

---

_Comment by @MichaReiser on 2023-09-22 16:31_

It may also be possible (at last for case 1) to enforce a hard line break if the default value has a leading comment.

---

_Comment by @charliermarsh on 2023-09-22 16:32_

The challenge there is that the leading comment could be within parentheses.

---

_Comment by @MichaReiser on 2023-09-22 16:39_

> The challenge there is that the leading comment could be within parentheses.

I see. Would be good if we have a solution for this that works consistently. E.g. a `has_leading_comments_outside_parentheses`.  There's another issue around this as well but I can't find it right now

---

_Comment by @MichaReiser on 2023-09-22 16:53_

As a diff

```diff
---
--- Formatted once
+++ Formatted twice
@@ -1,13 +1,11 @@
 def f2(
-    x: e2 = #
-    "foo",
+    x: e2 = "foo",  #
 ) -> r2:
     pass
 
 
 def f3(  #
-    x: e3 = #  # =  #
-    123,  #
+    x: e3 = 123,  #  # =  #  #
     #
 ) -> r3:
     pass
---

```

---

_Comment by @charliermarsh on 2023-09-27 16:00_

Keeping this for the Beta. It's likely uncommon but should be fixed.

---

_Label `accepted` added by @charliermarsh on 2023-09-27 18:45_

---

_Comment by @charliermarsh on 2023-09-27 18:49_

(I'd suggest taking a look at #7493 and #7495, we can handle similarly to those.)

---

_Assigned to @konstin by @konstin on 2023-10-09 13:29_

---

_Closed by @konstin on 2023-10-12 15:50_

---
