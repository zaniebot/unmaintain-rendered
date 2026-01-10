```yaml
number: 4991
title: ruff_textwrap accepts illegal Python whitespace
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-09T21:23:41Z
updated_at: 2023-06-10T02:17:53Z
url: https://github.com/astral-sh/ruff/issues/4991
synced_at: 2026-01-10T11:09:47Z
```

# ruff_textwrap accepts illegal Python whitespace

---

_Issue opened by @addisoncrump on 2023-06-09 21:23_

Python does not consider `U+00a0` to be whitespace, but this character *is* whitespace according to Rust's `trim_start` (which uses Unicode character classes): https://doc.rust-lang.org/std/string/struct.String.html#method.trim_start

In addition, Rust's str::len() method returns the length in _bytes_, not in characters. This combination can lead to a confusion in ruff_textwrap: https://github.com/astral-sh/ruff/blob/main/crates/ruff_textwrap/src/lib.rs#L95

This issue potentially indicates incorrect processing of leading whitespace in ruff_textwrap::dedent. For example, the mixing of tabs and spaces, the use of non-ASCII whitespace, etc. will be misprocessed by dedent currently.

----

#### How this was discovered

Since trim_start removes UTF-8 whitespace, `U+00a0` used as leading whitespace leads to a panic if an odd number of single-byte whitespace (e.g., space) characters are used as leading whitespace later. The [string slicing](https://github.com/astral-sh/ruff/blob/main/crates/ruff_textwrap/src/lib.rs#L117) attempts to slice between UTF-8 code points because `prefix_len` is computed by number of bytes, not in characters.

Here is a reproducing (illegal python syntax, don't pay attention to that) file which triggers this behaviour: [illegal-whitespace.txt](https://github.com/astral-sh/ruff/files/11710187/illegal-whitespace.txt)



---

_Label `bug` added by @charliermarsh on 2023-06-09 21:38_

---

_Comment by @charliermarsh on 2023-06-09 21:38_

Thank you!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-10 00:49_

---

_Closed by @charliermarsh on 2023-06-10 01:44_

---

_Comment by @addisoncrump on 2023-06-10 02:09_

Looks like this was a potentially incomplete fix; I am still able to trigger this behaviour. See this line: https://github.com/astral-sh/ruff/blob/main/crates/ruff_textwrap/src/lib.rs#L117

---

_Comment by @charliermarsh on 2023-06-10 02:17_

I'm confused, I fixed that and added a test all in that same PR, but somehow it didn't end up in the diff on GitHub? Operator error somewhere.

---
