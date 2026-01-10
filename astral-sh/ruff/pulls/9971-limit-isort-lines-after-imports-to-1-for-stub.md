```yaml
number: 9971
title: "Limit `isort.lines-after-imports` to 1 for stub files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - isort
assignees: []
merged: true
base: main
head: isort-lines-after-imports-in-stub-files
created_at: 2024-02-13T08:33:10Z
updated_at: 2024-02-28T16:36:52Z
url: https://github.com/astral-sh/ruff/pull/9971
synced_at: 2026-01-10T22:57:09Z
```

# Limit `isort.lines-after-imports` to 1 for stub files

---

_Pull request opened by @MichaReiser on 2024-02-13 08:33_

## Summary

Setting the `isort.lines-after-imports` setting to a value larger than 1 leads to formatter incompatibilities when formatting typing stub files because
the formatter enforces at most one blank line for stub files according to [typeshed's style guide](https://github.com/python/typeshed/blob/main/CONTRIBUTING.md#stub-file-coding-style).

This PR changes our `isort` implementation to limit `lines-after-imports` to 1 for stub files the same as isort does for black ([commit](https://github.com/PyCQA/isort/commit/b228c373ab3bc5c571ba0a616f13cac8f03ca99f), [code](https://github.com/PyCQA/isort/blob/eed5d059a8ea5442266be892322f8815074800a5/isort/output.py#L211-L225)). 

Fixes https://github.com/astral-sh/ruff/issues/9353

## Breaking Change?

This is a breaking change and we may need to wait for 0.3 to merge. The only "excuse" not to consider this a breaking change is that we say our `isort` configuration is intended to be black/ruff formatter compatible by default (`isort` profile black). In this case, this is a bug fix.

I did a code search on GitHub for [`lines-after-imports=2`](https://github.com/search?type=code&auto_enroll=true&q=%22lines-after-imports+%3D+2%22+path%3Apyproject.toml+language%3Atoml) (I didn't find any usages with values > 2). Most repositories don't have `pyi` files and are unaffected by the change. The following repositories contain pyi files and use `lines-after-imports=2`

* https://github.com/Ayan-Bandyopadhyay/BentoML: Fork of https://github.com/bentoml/BentoML The `lines-after-imports` setting was removed upstream in https://github.com/bentoml/BentoML/pull/3864 -> No longer an issue
* https://github.com/python-poetry/poetry/ The pyi files are excluded from linting. Running this branch does not raise any new lint errors.
* https://github.com/conda/conda-lock Pyi file locations are excluded. 
* https://github.com/pytest-dev/pytest/ All `pyi` files are empty `__init__.pyi` files (contain no imports)
* **https://github.com/sdss/lvmgort** They have `__init__.pyi` files that contain imports. They added two blank lines when migrating to ruff ;( https://github.com/sdss/lvmgort/commit/ca8206a68107c6ee689e427f7a13d1ec2f016cff
* https://github.com/bugbakery/whispercpp They exclude the typing directory
* **https://github.com/sdss/yao** Use two blank lines after imports in typing files. They don't use the formatter

From that it seems safe to conclude that using `lines-after-imports=2` with typing files isn't common, but at least two projects would see new errors after upgrading.

## Test Plan

Added test


---

_Label `isort` added by @MichaReiser on 2024-02-13 08:34_

---

_Comment by @github-actions[bot] on 2024-02-13 08:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-13 09:02_

---

_Review requested from @zanieb by @MichaReiser on 2024-02-13 09:02_

---

_Marked ready for review by @MichaReiser on 2024-02-13 09:02_

---

_Comment by @charliermarsh on 2024-02-13 18:18_

To confirm my understanding: if a project today doesn't set `lines-after-imports`, we _do_ use 1 as a default in `.pyi` files, right?

---

_Comment by @MichaReiser on 2024-02-13 20:26_

> To confirm my understanding: if a project today doesn't set `lines-after-imports`, we _do_ use 1 as a default in `.pyi` files, right?

Is this a very friendly hint that my implementation is wrong because it is wrong ;) It uses -1 which uses 2 empty lines if the following statement is a class or function. I have to add a test and fix the implementation. 

---

_Comment by @MichaReiser on 2024-02-14 14:10_

> To confirm my understanding: if a project today doesn't set `lines-after-imports`, we _do_ use 1 as a default in `.pyi` files, right?

I think I know understand why you asked this question... Yes, the change isn't just a breaking change when using `lines-after-imports=2` but also when using the default configuration if the imports are directly followed by a class or function declaration (which I assume is fairly common). 

I think that's reason enough for us to wait with merging until 0.3 

---

_Label `breaking` added by @MichaReiser on 2024-02-14 14:10_

---

_Added to milestone `v0.3.0` by @MichaReiser on 2024-02-14 14:21_

---

_Comment by @charliermarsh on 2024-02-15 00:48_

I asked because my guess was that it _wasn't_ a breaking change for files that _don't_ set `lines-after-imports`, because I thought we already limited to 1 in stub files. But it sounds like my guess was wrong :D

---

_Merged by @MichaReiser on 2024-02-28 16:36_

---

_Closed by @MichaReiser on 2024-02-28 16:36_

---

_Branch deleted on 2024-02-28 16:36_

---
