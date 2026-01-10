---
number: 101
title: Identify bottlenecks in current PubGrub implementation
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-16T03:10:25Z
updated_at: 2024-02-02T20:08:50Z
url: https://github.com/astral-sh/uv/issues/101
synced_at: 2026-01-10T01:23:03Z
---

# Identify bottlenecks in current PubGrub implementation

---

_Issue opened by @charliermarsh on 2023-10-16 03:10_

_No description provided._

---

_Comment by @charliermarsh on 2023-10-17 15:57_

We seem to spend a ton of time computing version intersections. I'll just copy an email I wrote to Jacob.

---

When we find ourselves testing many consecutive versions of a given package, the term intersection increases linearly. For example, after testing and rejecting Django 5.0a1, we have:

```
[ 0a0.dev0, 5.0a1 [  [ 5.0a1.post0.dev0, ∞ [
```

We then test Django 4.2.6, which gives us:

```
[ 0a0.dev0, 4.2.6 [  [ 4.2.6.post0.dev0, ∞ [
```

We then take the intersection of these terms, which gives us:

```
[ 0a0.dev0, 4.2.6 [  [ 4.2.6.post0.dev0, 5.0a1 [  [ 5.0a1.post0.dev0, ∞ [
```

This continues until we have a range like:

```
[ 0a0.dev0, 4.2rc1 [  [ 4.2rc1.post0.dev0, 4.2 [  [ 4.2.post0.dev0, 4.2.1 [  [ 4.2.1.post0.dev0, 4.2.2 [  [ 4.2.2.post0.dev0, 4.2.3 [  [ 4.2.3.post0.dev0, 4.2.4 [  [ 4.2.4.post0.dev0, 4.2.5 [  [ 4.2.5.post0.dev0, 4.2.6 [  [ 4.2.6.post0.dev0, 5.0a1 [  [ 5.0a1.post0.dev0, ∞ [
```

If you're testing hundreds of versions, the terms continue to grow, and I suspect this leads to quadratic behavior, since we're increasing the number of terms linearly with the number of tested versions?

The intersections aren't "wrong", and it's possible that we're doing something wrong with our version formulations -- but could we take advantage of the fact that we know there are no versions in these partial ranges?

For example, given `[ 0a0.dev0, 5.0a1 [  [ 5.0a1.post0.dev0, ∞ [` and `[ 0a0.dev0, 4.2.6 [  [ 4.2.6.post0.dev0, ∞ [`, it would be preferable to reduce to `[ 0a0.dev0, 4.2.6 [`, if I'm understanding the syntax correctly.

---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Added to milestone `Future` by @charliermarsh on 2023-10-25 04:32_

---

_Comment by @konstin on 2024-02-02 20:08_

This is now better tracked in https://github.com/pubgrub-rs/pubgrub/issues/135 and has improved a lot. I'm closing this in favor of opening specific issues for specific problems.

---

_Closed by @konstin on 2024-02-02 20:08_

---
