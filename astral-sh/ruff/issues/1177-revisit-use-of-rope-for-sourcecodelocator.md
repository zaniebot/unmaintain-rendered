```yaml
number: 1177
title: "Revisit use of rope for `SourceCodeLocator`"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - performance
assignees: []
created_at: 2022-12-10T15:29:14Z
updated_at: 2022-12-31T00:39:35Z
url: https://github.com/astral-sh/ruff/issues/1177
synced_at: 2026-01-10T12:05:23Z
```

# Revisit use of rope for `SourceCodeLocator`

---

_Issue opened by @charliermarsh on 2022-12-10 15:29_

We use `ropey` to support random-access and slicing on source code (see: `source_code_locator.rs`). I suspect that this is non-optimal for certain kinds of access patterns.

The basic problem is that we need to be able to take any (row, column) location and map that to a byte location in the raw text. This requires us to determine where the newlines are, but also, where every column lines up, since (row, column) locations are based on _characters_, not bytes (think: Unicode).

We used to iterate over every character in the source code and build up this mapping in advance. Later, I moved it to `ropey`, which was faster empirically and led to fewer off-by-one bugs. But for cases in which we need to rapidly slice the same string (e.g., grabbing the text for every token), surely there are faster patterns.


---

_Label `good first issue` added by @charliermarsh on 2022-12-10 15:29_

---

_Label `performance` added by @charliermarsh on 2022-12-10 15:29_

---

_Comment by @charliermarsh on 2022-12-10 15:29_

This is a good first task because it's a relatively simple and contained interface -- the locator just takes source code (`&[&str]`) and has to support slicing it based on locations.

---

_Comment by @charliermarsh on 2022-12-11 15:36_

In theory, I _think_ most efficient would be to build up this mapping during lexing, since we have to visit every character anyway.


---

_Comment by @charliermarsh on 2022-12-11 22:31_

I should probably also re-benchmark the manual version of this. I'm confused why it'd be slower than a rope for unchanging text.

---

_Comment by @charliermarsh on 2022-12-15 02:35_

Changing my tune on this... I think it really depends on the access patterns. It takes ~100 accesses or so for the Rope to be slower than the manual iterator (where we iterate over every character and build up a map from (row, column) to byte index).  We're pretty unlikely to perform that many accesses on a single file with our current access patterns. But if, e.g., we needed to get the span for every _token_ (as in #1130), it could start to make a sense.


---

_Closed by @charliermarsh on 2022-12-31 00:39_

---
