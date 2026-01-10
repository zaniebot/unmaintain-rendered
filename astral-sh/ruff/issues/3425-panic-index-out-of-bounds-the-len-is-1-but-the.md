```yaml
number: 3425
title: "[panic]: index out of bounds: the len is 1 but the index is 2"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-09T21:00:33Z
updated_at: 2023-03-13T04:02:45Z
url: https://github.com/astral-sh/ruff/issues/3425
synced_at: 2026-01-10T11:09:46Z
```

# [panic]: index out of bounds: the len is 1 but the index is 2

---

_Issue opened by @qarmin on 2023-03-09 21:00_

0.0.254
Config with all rules

```
thread 'main' panicked at 'index out of bounds: the len is 1 but the index is 2', crates/ruff/src/source_code/locator.rs:71:9
```

[_bgcolor (8th copy)15.py.zip](https://github.com/charliermarsh/ruff/files/10936022/_bgcolor.8th.copy.15.py.zip)


---

_Comment by @qarmin on 2023-03-09 21:01_

I have more crashes in other lines in locator.rs, but probably this are duplicates of this issue

---

_Comment by @charliermarsh on 2023-03-09 21:15_

Def agree that we shouldn't be panicking, but is that valid Python code? How would you describe the contents of that file? I'm trying to make sure I'm not misinterpreting the issue.

![Screen Shot 2023-03-09 at 4 14 38 PM](https://user-images.githubusercontent.com/1309177/224161327-6d648b80-7657-4314-a5ec-0a45b8b7c0fb.png)



---

_Comment by @qarmin on 2023-03-09 21:21_

This is found by fuzzer.
I provided valid python file and changed some bytes/charaters to find possible crashes

---

_Label `bug` added by @charliermarsh on 2023-03-10 05:30_

---

_Comment by @qarmin on 2023-03-11 06:27_

Smaller reproducer - [a.py.zip](https://github.com/charliermarsh/ruff/files/10947795/a.py.zip)


---

_Comment by @charliermarsh on 2023-03-13 04:02_

Okay, this is now resolved via #3454 and #3439.

---

_Closed by @charliermarsh on 2023-03-13 04:02_

---
