```yaml
number: 6553
title: "[WIP] add --python-implementation to pip install"
type: pull_request
state: closed
author: schlamar
labels: []
assignees: []
base: main
head: main
created_at: 2024-08-23T22:24:48Z
updated_at: 2024-08-24T07:51:37Z
url: https://github.com/astral-sh/uv/pull/6553
synced_at: 2026-01-12T16:07:25Z
```

# [WIP] add --python-implementation to pip install

---

_@schlamar_

## Summary

Closes #6246

I did some experiments for my feature request.

In theory it works. However overriding implementation without specifying the abi doesn't make sense I would say.

Right now with my changes `uv pip install --python-implementation pp --python-version 3.10` would generate the following tags:

```
pp310-pypy310_pp312-win_amd64
py310-none-win_amd64
py3-none-win_amd64
py39-none-win_amd64
py38-none-win_amd64
py37-none-win_amd64
py36-none-win_amd64
...
```

I guess adding a `--python-abi` should be added, too? Otherwise, this feature doesn't makes sense for me.

In pip the following works:

```
pip install --implementation pp --python-version "3.10" --abi pypy310_pp73 --target test --only-binary=:all: numpy 
```

About `--implementation py`. That is some sort of implicit behavior in pip. Maybe there should be a separate flag for this kind of feature in uv? `--implementation-agnostic` or something like that.

I hope my first draft is going in the right direction and you can provide some feedback.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @schlamar on 2024-08-24 07:51_

Needs better spec, see #6246 

---

_Closed by @schlamar on 2024-08-24 07:51_

---
