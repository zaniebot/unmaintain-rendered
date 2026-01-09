---
number: 2677
title: "Freeze generated requirements aren't properly formatted for git and local dependencies"
type: issue
state: closed
author: tonydavis629
labels:
  - needs-mre
assignees: []
created_at: 2024-03-26T21:13:37Z
updated_at: 2024-03-27T14:12:39Z
url: https://github.com/astral-sh/uv/issues/2677
synced_at: 2026-01-07T13:12:17-06:00
---

# Freeze generated requirements aren't properly formatted for git and local dependencies

---

_Issue opened by @tonydavis629 on 2024-03-26 21:13_

I have a project with dependencies installed from github and from local disk.

```
uv pip freeze > requirements.txt
```
generates a requirements.txt with lines such as 
```
cutie==1.0.0 (from file:///home/jovyan/clips-labeler/Cutie)
segment-anything==1.0 (from git+https://github.com/facebookresearch/segment-anything.git@6fdee8f2727f4506cfbbe553e23b895e27956588)
```
I try to install from this requirements.txt
```
uv pip install -r requirements.txt
```
and it seems the requirements.txt is not in the right format for these types of dependencies.
```
error: Couldn't parse requirement in `requirements.txt` at position 250
  Caused by: Trailing `(from file:///home/jovyan/clips-labeler/Cutie)` is not allowed
cutie==1.0.0 (from file:///home/jovyan/clips-labeler/Cutie)
```
and 
```
error: Couldn't parse requirement in `requirements.txt` at position 1848
  Caused by: Trailing `(from git+https://github.com/facebookresearch/segment-anything.git@6fdee8f2727f4506cfbbe553e23b895e27956588)` is not allowed
segment-anything==1.0 (from git+https://github.com/facebookresearch/segment-anything.git@6fdee8f2727f4506cfbbe553e23b895e27956588)
```

I can manually modify these lines to use the '@' for uv, but I feel like this should be the default.

---

_Label `bug` added by @zanieb on 2024-03-26 21:15_

---

_Comment by @zanieb on 2024-03-26 21:15_

Thanks for the report!

---

_Comment by @charliermarsh on 2024-03-26 21:22_

Do you know what uv version you're on? This was fixed long ago.

---

_Label `bug` removed by @charliermarsh on 2024-03-26 21:22_

---

_Label `needs-mre` added by @charliermarsh on 2024-03-26 21:22_

---

_Comment by @charliermarsh on 2024-03-26 21:23_

See:

- https://github.com/astral-sh/uv/issues/1931
- https://github.com/astral-sh/uv/pull/1936

---

_Comment by @tonydavis629 on 2024-03-27 14:11_

Indeed I was on an older version. You guys are moving so fast you think 3 weeks is a long time!

---

_Closed by @tonydavis629 on 2024-03-27 14:11_

---

_Comment by @charliermarsh on 2024-03-27 14:12_

Thanks so much for following up :)

---
