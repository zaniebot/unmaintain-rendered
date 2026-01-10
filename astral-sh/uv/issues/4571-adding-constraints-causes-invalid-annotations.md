---
number: 4571
title: Adding constraints causes invalid annotations (from extras) in uv pip compile 0.2.26
type: issue
state: closed
author: notatallshaw-gts
labels:
  - bug
assignees: []
created_at: 2024-06-26T22:54:16Z
updated_at: 2024-06-28T19:25:11Z
url: https://github.com/astral-sh/uv/issues/4571
synced_at: 2026-01-10T01:23:39Z
---

# Adding constraints causes invalid annotations (from extras) in uv pip compile 0.2.26

---

_Issue opened by @notatallshaw-gts on 2024-06-26 22:54_

When I upgraded to uv 0.2.26 there was a huge difference in the annotations produced by my requirements, at first I assumed this was bug fix and it was intentional, but focusing in on a single requirement it looked like it is a new bug, and after quite a bit of effort I now have a minimal reproducible example.

First create a constraints file called `constraints.txt` and put in the single requirement:

```
numpy<2
```

## uv 0.2.25

```
$ uv -V
uv 0.2.15

$ echo -e "ipython>=8.4.0\nnumpy<2" | uv pip compile --python-version 3.11 --annotation-style line - | grep "numpy=="
Resolved 18 packages in 56ms
numpy==1.26.4

echo -e "ipython>=8.4.0\nnumpy<2" | uv pip compile --python-version 3.11 --annotation-style line -c constraints.txt - | grep "numpy=="
Resolved 18 packages in 11ms
numpy==1.26.4             # via -c constraints.txt
```

## uv 0.2.26

```
$ uv -V
uv 0.2.16

$ echo -e "ipython>=8.4.0\nnumpy<2" | uv pip compile --python-version 3.11 --annotation-style line - | grep "numpy=="
Resolved 18 packages in 9ms
numpy==1.26.4

$ echo -e "ipython>=8.4.0\nnumpy<2" | uv pip compile --python-version 3.11 --annotation-style line -c constraints.txt - | grep "numpy=="
Resolved 18 packages in 8ms
numpy==1.26.4             # via ipython, -c constraints.txt
```


Suddendly `numpy` is now coming "via ipython" when it never was before. Where did this come from?

Well my best guess is taking a look at the metadata of that iPython release (https://files.pythonhosted.org/packages/c0/78/fae11aa2dd7e948fbf6283f8abaff267b37a4a3cba531d0aa4274137e793/ipython-8.25.0-py3-none-any.whl.metadata):

```
Requires-Dist: numpy >=1.23 ; extra == 'test_extra'
```

There is a numpy requirement for an extra "test_extra", but this extra is never added as a requirement in either the requirements or the contraints.

---

_Comment by @notatallshaw-gts on 2024-06-26 22:57_

Perhaps fixed by https://github.com/astral-sh/uv/pull/4570? (I should probably just wait always wait an hour to see if @charliermarsh fixes the problem before spending time producing a clear example myself)

---

_Comment by @charliermarsh on 2024-06-26 22:58_

Thanks, almost certainly the same thing. We're gonna cut another release tonight, sorry about that.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-26 22:58_

---

_Label `bug` added by @charliermarsh on 2024-06-26 22:58_

---

_Comment by @notatallshaw-gts on 2024-06-26 23:01_

Confirmed, fixed on main:

```
echo -e "ipython>=8.4.0\nnumpy<2" | cargo run -- pip compile --python-version 3.11 --annotation-style line -c constraints.txt - | grep "numpy=="
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.35s
     Running `target/debug/uv pip compile --python-version 3.11 --annotation-style line -c constraints.txt -`
Resolved 18 packages in 125ms
numpy==1.26.4             # via -c constraints.txt
```

---

_Closed by @notatallshaw-gts on 2024-06-26 23:01_

---

_Comment by @zanieb on 2024-06-26 23:09_

Will be released in a bit: https://github.com/astral-sh/uv/pull/4573

---

_Comment by @potiuk on 2024-06-28 19:25_

nice :)

---
