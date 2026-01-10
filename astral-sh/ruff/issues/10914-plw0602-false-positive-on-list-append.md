```yaml
number: 10914
title: PLW0602 false positive on list.append
type: issue
state: closed
author: AaronDMarasco
labels: []
assignees: []
created_at: 2024-04-13T01:13:02Z
updated_at: 2024-04-13T01:23:40Z
url: https://github.com/astral-sh/ruff/issues/10914
synced_at: 2026-01-10T11:09:53Z
```

# PLW0602 false positive on list.append

---

_Issue opened by @AaronDMarasco on 2024-04-13 01:13_

It seems to only look for things like `=` or `+=` and doesn't recognize `list.append`.


* A minimal code snippet that reproduces the bug.

```python
probs = []
def y():
  global probs
  probs.append("problem")
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```sh
$ ruff check --isolated --config "lint.select=['PLW0602']" PLW0602.py
PLW0602.py:3:10: PLW0602 Using global for `probs` but no assignment is done
Found 1 error.
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
  * N/A
  
* The current Ruff version (`ruff --version`).
  * ruff 0.3.7




---

_Comment by @AaronDMarasco on 2024-04-13 01:21_

Never mind... because it's not rebinding the variable name and using the reference, it's cromulent...

```python
revragnarok@rugrinn:~/ruff$ cat PLW0602.py 
probs = []
def y():
  # global probs
  probs.append("problem")

y()
y()
y()
print(probs)

revragnarok@rugrinn:~/ruff$ python3 PLW0602.py
['problem', 'problem', 'problem']
```

---

_Closed by @AaronDMarasco on 2024-04-13 01:21_

---
