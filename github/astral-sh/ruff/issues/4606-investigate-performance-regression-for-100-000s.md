---
number: 4606
title: Investigate performance regression for 100,000s of violations
type: issue
state: closed
author: charliermarsh
labels:
  - question
  - performance
assignees: []
created_at: 2023-05-23T15:12:06Z
updated_at: 2023-05-24T14:34:49Z
url: https://github.com/astral-sh/ruff/issues/4606
synced_at: 2026-01-07T13:12:14-06:00
---

# Investigate performance regression for 100,000s of violations

---

_Issue opened by @charliermarsh on 2023-05-23 15:12_

As of #3931, most rule categories (on the CPython benchmark) got faster, but a few got slower, especially D, ANN, PT, and Q. So running `cargo build --release && hyperfine --ignore-failure --warmup 10 "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent --select D,ANN,PT,Q"` went from ~436ms to ~2s on my machine.

I think the categories themselves are irrelevant -- it just turns out that these categories have many, many violations (e.g., `Q000` has 143,807 violations).

The inverse (`--select ALL --ignore D,ANN,PT,Q`) went from 452ms to 764ms, despite the fact that running any of the categories _individually_ got faster.

Though most projects (and most Ruff invocations) don't have hundreds of thousands of violations :) it'd be good to understand where this slowdown is coming from. We're running with `--no-cache` and `--silent`, so we shouldn't be serializing the diagnostics in any way.


---

_Label `question` added by @charliermarsh on 2023-05-23 15:12_

---

_Label `performance` added by @charliermarsh on 2023-05-23 15:12_

---

_Comment by @MichaReiser on 2023-05-23 16:37_

What changed is that we now always include the source code with the `Diagnostic`, regardless if `silent` is specified. This can have a significant cost if there are many diagnostics. 

I remember the previous version had a fast-path for when silent is enabled. 

I think the solution to this is to make `source_code` on `Message` optional as described in #4571. We could then omit the source file when `--silent`  has been passed. 

Nit: I recommend using ruff's `-e` flag so that hyperfine `--ignore-failures` argument isn't necessary. It prevents that you profile failing ruff runs.

---

_Comment by @charliermarsh on 2023-05-23 16:43_

For my understanding: why does including the source code come with a cost? Isn't it just refcounted?

---

_Comment by @MichaReiser on 2023-05-23 16:53_

> For my understanding: why does including the source code come with a cost? Isn't it just refcounted?

The cost comes from cloning the `str` to a `String` once for every file. I'm not 100% sure if that's the only reason for the regression but it probably adds quiet a bit of overhead

---

_Comment by @charliermarsh on 2023-05-24 01:02_

But the number of files for which we're generating violations doesn't change between these rule sets. I just want to sanity check that we're not doing something unintentional like cloning the `str` to `String` for every _diagnostic.

---

_Comment by @Boshen on 2023-05-24 02:15_

Probably relevant:
* https://github.com/Boshen/oxc/issues/166

Diagnostic printing is probably where you want to look at.

---

_Comment by @charliermarsh on 2023-05-24 02:28_

`--silent` should mean that we're not printing diagnostics at all :joy:

---

_Comment by @MichaReiser on 2023-05-24 06:18_

Here's a [flamegraph](https://share.firefox.dev/3pWDYUr) running the above on main (without hyperfine). You can see that ruff spends the most time in the first thread, sorting diagnostics. 

[This is a profile](https://share.firefox.dev/3pWDYUr) from before the PR, it doesn't show the long sorting at the end of the analysis on the main thread. 

Edit: I investigated this more, and the culprit is that I included the `diagnostic.kind.rule()` in the comparison because I noticed that we have non-deterministic ordering when multiple rules emit a diagnostic in the same file and position. 

https://github.com/charliermarsh/ruff/blob/3644695bf2377e7dc7fd8432161a76bb4a1f4db9/crates/ruff/src/message/mod.rs#L79-L85

Removing the rule comparison cuts the runtime down from 1s to 320ms

I think there are two cheaper fixes:

* Use `sort` instead of `sort_unstable` so that we maintain the order in which the diagnostics were generated. This should be deterministic
* And / or: Use `diagnostic.range().ordering(other.range())` to also sort by the diagnostic's end position



---

_Comment by @MichaReiser on 2023-05-24 10:34_

I'll follow up with a pr. i have an idea how we could speed this up even more 

---

_Referenced in [astral-sh/ruff#4624](../../astral-sh/ruff/pulls/4624.md) on 2023-05-24 11:32_

---

_Closed by @MichaReiser on 2023-05-24 14:34_

---
