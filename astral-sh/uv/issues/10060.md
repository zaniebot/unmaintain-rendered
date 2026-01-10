```yaml
number: 10060
title: uv python install consumes a lot of memory
type: issue
state: closed
author: jooon
labels:
  - bug
assignees: []
created_at: 2024-12-20T15:38:22Z
updated_at: 2024-12-20T18:52:44Z
url: https://github.com/astral-sh/uv/issues/10060
synced_at: 2026-01-10T04:36:21Z
```

# uv python install consumes a lot of memory

---

_Issue opened by @jooon on 2024-12-20 15:38_

While investigating #10057 and rerunning the uv python install command to get a shorter test case I noticed uv became slower and slower the more I used it. It seems to read and rewrite a _sysconfigdata__linux_x86_64-linux-gnu.py file which keeps getting bigger and bigger for every run.

```
$ uv --version
uv 0.5.11
$ uv python uninstall --all
Searching for Python installations
No Python installations found
$ time (for i in {1..25}; do uv python install 3.12.8; done)
Installed Python 3.12.8 in 1.06s
 + cpython-3.12.8-linux-x86_64-gnu

real	0m27.687s
user	0m17.309s
sys	0m8.580s
$ cd /home/jon/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12/
$ du -hs _sysconfigdata__linux_x86_64-linux-gnu.py
2.3G	_sysconfigdata__linux_x86_64-linux-gnu.py
```


---

_Comment by @jooon on 2024-12-20 15:42_

Tested some old versions. Looks like it was introduced in uv 0.5.9.

---

_Comment by @jooon on 2024-12-20 15:57_

That's a lot of backslashes :) 

I guess the bug is somewhere here, but I can't see it.

https://github.com/astral-sh/uv/blob/cf14a62de7bf1330c904d23135afe214cf61619c/crates/uv-python/src/sysconfig/parser.rs#L161

---

_Assigned to @charliermarsh by @konstin on 2024-12-20 15:58_

---

_Label `bug` added by @konstin on 2024-12-20 15:58_

---

_Comment by @zanieb on 2024-12-20 16:08_

Thanks for the report!

---

_Comment by @charliermarsh on 2024-12-20 16:08_

Noooooooo

---

_Closed by @charliermarsh on 2024-12-20 18:52_

---

_Closed by @charliermarsh on 2024-12-20 18:52_

---
