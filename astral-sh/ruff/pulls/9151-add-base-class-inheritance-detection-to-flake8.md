```yaml
number: 9151
title: Add base-class inheritance detection to flake8-django rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/django
created_at: 2023-12-15T17:52:56Z
updated_at: 2023-12-15T18:07:49Z
url: https://github.com/astral-sh/ruff/pull/9151
synced_at: 2026-01-10T23:31:11Z
```

# Add base-class inheritance detection to flake8-django rules

---

_Pull request opened by @charliermarsh on 2023-12-15 17:52_

## Summary

As elsewhere, this only applies to classes defined within the same file.

Closes https://github.com/astral-sh/ruff/issues/9150.


---

_Label `bug` added by @charliermarsh on 2023-12-15 17:55_

---

_Merged by @charliermarsh on 2023-12-15 18:01_

---

_Closed by @charliermarsh on 2023-12-15 18:01_

---

_Branch deleted on 2023-12-15 18:01_

---

_Comment by @github-actions[bot] on 2023-12-15 18:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+19 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L1895'>zerver/models.py:1895:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L2171'>zerver/models.py:2171:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L2382'>zerver/models.py:2382:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L2677'>zerver/models.py:2677:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3207'>zerver/models.py:3207:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3216'>zerver/models.py:3216:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3279'>zerver/models.py:3279:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3382'>zerver/models.py:3382:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3393'>zerver/models.py:3393:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3503'>zerver/models.py:3503:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3508'>zerver/models.py:3508:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3808'>zerver/models.py:3808:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3829'>zerver/models.py:3829:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L4456'>zerver/models.py:4456:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L4943'>zerver/models.py:4943:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L191'>zilencer/models.py:191:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L210'>zilencer/models.py:210:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L235'>zilencer/models.py:235:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L254'>zilencer/models.py:254:7:</a> DJ008 Model does not define `__str__` method
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DJ008 | 14 | 14 | 0 | 0 | 0 |
| DJ012 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+19 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L1895'>zerver/models.py:1895:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L2171'>zerver/models.py:2171:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L2382'>zerver/models.py:2382:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L2677'>zerver/models.py:2677:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3207'>zerver/models.py:3207:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3216'>zerver/models.py:3216:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3279'>zerver/models.py:3279:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3382'>zerver/models.py:3382:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3393'>zerver/models.py:3393:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3503'>zerver/models.py:3503:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3508'>zerver/models.py:3508:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3808'>zerver/models.py:3808:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L3829'>zerver/models.py:3829:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L4456'>zerver/models.py:4456:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zerver/models.py#L4943'>zerver/models.py:4943:5:</a> DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L191'>zilencer/models.py:191:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L210'>zilencer/models.py:210:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L235'>zilencer/models.py:235:7:</a> DJ008 Model does not define `__str__` method
+ <a href='https://github.com/zulip/zulip/blob/fb5137f8b5fbc3d83d0c7c4526fb6ed0b01d8803/zilencer/models.py#L254'>zilencer/models.py:254:7:</a> DJ008 Model does not define `__str__` method
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DJ008 | 14 | 14 | 0 | 0 | 0 |
| DJ012 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---
