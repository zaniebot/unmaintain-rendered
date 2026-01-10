```yaml
number: 565
title: Add test cases for editable installs
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konstin/editable-installs-test-cases
created_at: 2023-12-05T12:58:01Z
updated_at: 2023-12-07T21:28:07Z
url: https://github.com/astral-sh/uv/pull/565
synced_at: 2026-01-10T15:44:44Z
```

# Add test cases for editable installs

---

_Pull request opened by @konstin on 2023-12-05 12:58_

One test case with a mixed python/native module using maturin, one using pure python using poetry. I'm avoiding adding one using legacy setuptools.

---

_Comment by @konstin on 2023-12-05 12:58_

Current dependencies on/for this PR:
* main
  * **PR #564** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/564?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #565** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/565?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #566** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/566?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #567** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/567?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #587** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/565?utm_source=stack-comment).

---

_Marked ready for review by @konstin on 2023-12-06 16:34_

---

_Comment by @zanieb on 2023-12-06 16:39_

Why avoid setuptools? Isn't it still very common?

---

_@zanieb approved on 2023-12-06 16:40_

---

_Comment by @konstin on 2023-12-06 17:07_

With legacy setuptools i mean `python setup.py develop`, which is pre-PEP standard and even the setuptools devs don't like anymore (https://blog.ganssle.io/articles/2021/10/setup-py-deprecated.html). We could add a setuptools test case through build-backend, but from my experience poetry&co are more standards compliant and more helpful when something goes wrong; I'd defer till we need regression tests.

---

_Merged by @konstin on 2023-12-06 17:21_

---

_Closed by @konstin on 2023-12-06 17:21_

---

_Branch deleted on 2023-12-06 17:21_

---
