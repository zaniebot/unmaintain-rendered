```yaml
number: 9551
title: "False-positive between `ERA001` and `RUF100`"
type: issue
state: open
author: flbraun
labels:
  - bug
assignees: []
created_at: 2024-01-16T14:11:37Z
updated_at: 2025-05-05T19:44:50Z
url: https://github.com/astral-sh/ruff/issues/9551
synced_at: 2026-01-10T11:09:51Z
```

# False-positive between `ERA001` and `RUF100`

---

_Issue opened by @flbraun on 2024-01-16 14:11_

ruff 0.1.13

Take this piece of code:
```python
response = '...'  # imagine requests.get(...) here
# response.json() example:
# {
#   'foo': 1,
#   'bar': 2,
#   'baz': 3,
# }
bar = response.json()['bar']
```

`ruff check --select ERA001 --select RUF100` rightfully complains about commented-out code:
```
test.py:3:1: ERA001 Found commented-out code
test.py:4:1: ERA001 Found commented-out code
test.py:5:1: ERA001 Found commented-out code
test.py:6:1: ERA001 Found commented-out code
test.py:7:1: ERA001 Found commented-out code
Found 5 errors.
```

Then add noqa directives for ERA001, because we actually want to keep the comment:
```python
response = '...'  # imagine requests.get(...) here
# response.json() example:
# {                 # noqa: ERA001
#   'foo': 1,       # noqa: ERA001
#   'bar': 2,       # noqa: ERA001
#   'baz': 3,       # noqa: ERA001
# }                 # noqa: ERA001
bar = response.json()['bar']
```

`ruff check --select ERA001 --select RUF100` then complains about unused noqa directives, although they are definitely required to fully satisfy ERA001:
```
test.py:3:21: RUF100 [*] Unused `noqa` directive (unused: `ERA001`)
test.py:7:21: RUF100 [*] Unused `noqa` directive (unused: `ERA001`)
Found 2 errors.
```

Removing the supposedly unused noqa directives causes `ruff check --select ERA001 --select RUF100` to complain about ERA001 again:
```python
response = '...'  # imagine requests.get(...) here
# response.json() example:
# {
#   'foo': 1,       # noqa: ERA001
#   'bar': 2,       # noqa: ERA001
#   'baz': 3,       # noqa: ERA001
# }
bar = response.json()['bar']
```
```
test.py:3:21: RUF100 [*] Unused `noqa` directive (unused: `ERA001`)
test.py:7:21: RUF100 [*] Unused `noqa` directive (unused: `ERA001`)
Found 2 errors.
```

---

_Comment by @charliermarsh on 2024-01-16 19:39_

Ah yeah this is tricky. The presence of the `# noqa` comments is causing Ruff _not_ to consider those lines as commented-out code.

---

_Label `bug` added by @charliermarsh on 2024-01-16 19:39_

---

_Comment by @charliermarsh on 2024-01-16 19:40_

Thanks for the clear write-up.

---

_Closed by @dylwil3 on 2024-11-10 03:30_

---

_Reopened by @dylwil3 on 2024-11-10 03:30_

---

_Comment by @buhtz on 2025-05-05 19:44_

I do observe similliar behavior with `# noqa: E402` and `RUF100`.

---
