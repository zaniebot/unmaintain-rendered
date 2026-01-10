```yaml
number: 5883
title: "`ruff check --diff` writes summary message to stdout instead of stderr"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-07-19T14:28:04Z
updated_at: 2023-07-19T14:28:28Z
url: https://github.com/astral-sh/ruff/issues/5883
synced_at: 2026-01-10T11:09:48Z
```

# `ruff check --diff` writes summary message to stdout instead of stderr

---

_Issue opened by @zanieb on 2023-07-19 14:28_

When running `ruff check --diff` the diff _and_ summary message ("Would fix 2 errors") is written to standard output.

```shell
$ ruff check --select F,W,RUF015 --no-cache example.py --diff 2> /dev/null
--- example.py
+++ example.py
@@ -1,10 +1,9 @@
 from typing import List
 
-import os
 
 def sum_even_numbers(numbers: List[int]) -> int:
     """Given a list of integers, return the sum of all even numbers in the list."""
     return sum(num for num in numbers if num % 2 == 0)
 
 x = range(10)
-list(x)[0]
\ No newline at end of file
+list(x)[0]

Would fix 2 errors.
```

I presumed this would cause problems with displaying or applying the patch but it turns out it does not since the number of lines is specified in the diff.

e.g.

You can use `ydiff` for a pretty diff and the summary is separate:
```
$ ruff check --select F,W,RUF015 --no-cache example.py --diff | ydiff -s
--- example.py
+++ example.py
@@ -1,10 +1,9 @@
 1 from typing import List                                                           1 from typing import List
 2                                                                                   2 
 3 import os    
 4                                                                                   3 
 5 def sum_even_numbers(numbers: List[int]) -> int:                                  4 def sum_even_numbers(numbers: List[int]) -> int:
 6     """Given a list of integers, return the sum of all even numbers in the list>  5     """Given a list of integers, return the sum of all even numbers in the list>
 7     return sum(num for num in numbers if num % 2 == 0)                            6     return sum(num for num in numbers if num % 2 == 0)
 8                                                                                   7 
 9 x = range(10)                                                                     8 x = range(10)
10 list(x)[0]                                                                        9 list(x)[0]

Would fix 2 errors.
```

Or you can use `git apply` to apply the diff:
```
 $ ruff check --select F,W,RUF015 --no-cache example.py --diff | git apply -
 ```

---

_Comment by @zanieb on 2023-07-19 14:28_

As far as I can tell, this is not an issue. Feel free to chime in if it affects your use-case!

---

_Closed by @zanieb on 2023-07-19 14:28_

---
