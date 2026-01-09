---
number: 16465
title: Linking project extras to maturin features
type: issue
state: closed
author: chitralverma
labels:
  - enhancement
assignees: []
created_at: 2025-10-27T06:17:58Z
updated_at: 2025-12-18T16:15:32Z
url: https://github.com/astral-sh/uv/issues/16465
synced_at: 2026-01-07T13:12:19-06:00
---

# Linking project extras to maturin features

---

_Issue opened by @chitralverma on 2025-10-27 06:17_

### Summary

I have python rust mixed pyo3 project (`mypackage`) which is built using maturin build-backend and I want to make certain Rust features compile only when optional Python dependencies (or “extras”) are installed.

I know what the dependencies and optional-dependencies section a pyproject.toml are to manipulate the included python packages. but I was wondering if there is a known pattern to use these to manipulate a feature gated (maturin) build itself.

Any help would be greatly appreciated, let me know if i need to raise this as a github issue.
thanks

### Example

It'll be awesome for the end users to do something like :

```
# base build with default features
uv pip install mypackage           

# enables "fast" Rust feature + includes any other py dependencies in fast extra
uv pip install mypackage[fast]     

# enables "something" and "fast" Rust feature + includes any other py dependencies in "something" and "fast" extra
uv pip install mypackage[fast, something]    
```

and have those optional extras map to Cargo features in your Cargo.toml so that maturin knows to build with those features.

---

_Label `enhancement` added by @chitralverma on 2025-10-27 06:18_

---

_Referenced in [apache/opendal#6729](../../apache/opendal/pulls/6729.md) on 2025-10-27 09:02_

---

_Comment by @konstin on 2025-10-27 19:05_

Unfortunately, this is not supported by extras. All wheels have the same extras, so we can't make compilation or wheel selection dependent on the activated extras. Extras are purely additive (you can select dependencies when a specific extra is active, but not really depend on another extra being inactive), so you couldn't have `mypackage[fast, something]` deselect something you'd use with `mypackage[fast]` either.

---

_Referenced in [apache/opendal#6748](../../apache/opendal/issues/6748.md) on 2025-11-04 03:26_

---

_Closed by @konstin on 2025-12-18 16:15_

---
