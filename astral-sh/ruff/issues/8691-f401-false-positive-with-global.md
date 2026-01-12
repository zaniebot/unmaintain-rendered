```yaml
number: 8691
title: F401 false positive with global
type: issue
state: open
author: hauntsaninja
labels:
  - bug
assignees: []
created_at: 2023-11-15T10:40:13Z
updated_at: 2024-05-07T17:33:24Z
url: https://github.com/astral-sh/ruff/issues/8691
synced_at: 2026-01-12T15:54:48Z
```

# F401 false positive with global

---

_@hauntsaninja_

It's weird code, but we had some code doing a thing like this and the autofix broke it:
```
λ cat x.py   
import random
from math import pi

def make_math_cool():
    global pi
    if random.random() < 0.5:
        pi = 4
    print(pi)

make_math_cool()

λ ruff check x.py
x.py:2:18: F401 [*] `math.pi` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Label `bug` added by @charliermarsh on 2023-11-15 16:23_

---

_Comment by @charliermarsh on 2023-11-15 16:24_

I'm not sure if the right solution here is to make the fix unsafe when an import is potentially-shadowed, or avoid flagging F401 at all. If the latter, we'll almost certainly introduce false negatives.

---

_Comment by @hauntsaninja on 2023-11-15 20:26_

I think the issue here is that the symbol isn't shadowed, there is no `pi` local variable (of course if we remove `global`, then F401 is fully correct). I think use of `global` is rare enough that false negatives wouldn't really be a problem, but maybe I misunderstand.

(In any case, I'm not unhappy with ruff's existing behaviour and wouldn't want to trade off too much, tbh this idiom deserves whatever happens to it)

---

_Comment by @lgammo on 2024-05-07 17:33_

Any language processor that applies language rules to fix a program in that language has to understand the semantics of that language. Thus, for ruff to ignore the semantics of global declarations indicates that ruff is indeed rough, and not to be trusted.

You can't say tough don't use globals. This type of fixes should definitely be made unsafe, but better still if global semantics were understood by ruff.

---
