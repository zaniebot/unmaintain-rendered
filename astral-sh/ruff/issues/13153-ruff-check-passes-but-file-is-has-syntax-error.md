```yaml
number: 13153
title: Ruff check passes, but file is has syntax error
type: issue
state: closed
author: uweschmitt
labels: []
assignees: []
created_at: 2024-08-29T18:04:29Z
updated_at: 2024-08-29T20:08:06Z
url: https://github.com/astral-sh/ruff/issues/13153
synced_at: 2026-01-12T15:54:52Z
```

# Ruff check passes, but file is has syntax error

---

_@uweschmitt_

The following program is syntactially not correct, but accepted by `ruff`:

```
# demo.py
x = [3]
print(f"{x["0"]}")
```

```
$ ruff check demo.py
All checks passed!
```
```
$ python demo.py
  File "/home/schmittu/slurk-chat-bot-setup-descil/proto.py", line 2
    print(f"{x["0"]}")
                ^
SyntaxError: f-string: unmatched '['
```


```
$ ruff --version
ruff 0.6.3
```



---

_Comment by @charliermarsh on 2024-08-29 18:10_

Thanks -- this is tracked in #6591. Ruff's parser is more permissive. So all valid programs will be accepted by Ruff's parser, but it may not error on some invalid programs. We plan to emit diagnostics for that in the future (per the linked issue).

---

_Closed by @charliermarsh on 2024-08-29 20:07_

---

_Comment by @charliermarsh on 2024-08-29 20:08_

(Combining with that other issue.)

---
