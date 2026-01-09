---
number: 3336
title: "[Panic] Expected docstring to start with a valid triple- or single-quote prefix"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-04T13:20:08Z
updated_at: 2023-03-06T02:37:15Z
url: https://github.com/astral-sh/ruff/issues/3336
synced_at: 2026-01-07T13:12:14-06:00
---

# [Panic] Expected docstring to start with a valid triple- or single-quote prefix

---

_Issue opened by @qarmin on 2023-03-04 13:20_

Ruff 0.0.254

Steps to reproduce 
```
mkdir tt
cd tt
wget https://github.com/charliermarsh/ruff/files/10888419/AAA.zip
unzip AAA.zip
python3 -m venv venv
source venv/bin/activate
pip install packaging
pip install -r requirements.txt --no-deps
ruff .
```
causes to crash with message
```
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'internal error: entered unreachable code: Expected docstring to start with a valid triple- or single-quote prefix', crates/ruff/src/rules/pydocstyle/helpers.rs:24:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Aborted (core dumped)
```

Requirements file contains ~ 1000 the most popular python packages, mostly < 20 MB in size(I disabled some packages, that took need to download 500MB like nvidia cuda)

I suggest to check even more packages from time to time because this is quite easy way to find possible bugs in real code.



---

_Comment by @qarmin on 2023-03-04 13:29_

Found minimal example
```
﻿""" SAM macro definitions """

```

---

_Label `bug` added by @charliermarsh on 2023-03-04 15:02_

---

_Comment by @charliermarsh on 2023-03-04 15:13_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-04 16:07_

---

_Comment by @charliermarsh on 2023-03-04 16:10_

That minimal example doesn't panic for me, but I'll try for the full repro based on the zip file (thanks for that).

---

_Comment by @charliermarsh on 2023-03-04 17:04_

This is a very useful exercise!

---

_Comment by @qarmin on 2023-03-04 17:10_

I forgot about config file(by default there is no crash)
```
select = ["D"]
```

---

_Comment by @charliermarsh on 2023-03-04 17:13_

Is it possible that the file uses CR line endings? (Not CRLF, but CR.) I can't repro based on the snippet alone.

---

_Comment by @charliermarsh on 2023-03-04 17:21_

Alternatively, if you know which file this is in, that works too.

---

_Comment by @qarmin on 2023-03-04 17:33_

In notepad I see that file uses CL line ending and crash still happens - 
[ww.zip](https://github.com/charliermarsh/ruff/files/10889266/ww.zip)


---

_Comment by @charliermarsh on 2023-03-04 17:35_

Awesome, thank you, _that_ I can reproduce.

---

_Comment by @charliermarsh on 2023-03-04 17:42_

There's a zero-width non-breaking space at the start of the file...

https://www.compart.com/en/unicode/U+FEFF

---

_Comment by @charliermarsh on 2023-03-04 17:56_

We should handle this gracefully, but it's typically an encoding or corruption error AFAICT?

https://www.freecodecamp.org/news/a-quick-tale-about-feff-the-invisible-character-cd25cd4630e7/
https://stackoverflow.com/questions/1972362/why-is-my-bash-script-adding-feff-to-the-beginning-of-files

---

_Referenced in [astral-sh/ruff#3343](../../astral-sh/ruff/pulls/3343.md) on 2023-03-04 19:16_

---

_Closed by @charliermarsh on 2023-03-06 02:37_

---

_Referenced in [astral-sh/ruff#3721](../../astral-sh/ruff/issues/3721.md) on 2023-03-26 14:44_

---
