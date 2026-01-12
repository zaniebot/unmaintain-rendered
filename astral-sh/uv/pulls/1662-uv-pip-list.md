```yaml
number: 1662
title: "`uv pip list`"
type: pull_request
state: merged
author: sbrugman
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: pip-list
created_at: 2024-02-18T18:00:28Z
updated_at: 2024-02-25T19:46:39Z
url: https://github.com/astral-sh/uv/pull/1662
synced_at: 2026-01-12T16:04:41Z
```

# `uv pip list`

---

_@sbrugman_

Hi, love your work on `uv` 👋! 

Opening a Draft PR early to check if there are any existing rust table formatting libs that I am unaware of (either already in `uv`/`ruff`, or the rust ecosystem) before spending much time on inventing the wheel myself and cleaning it up. Any other pointers are also welcome (e.g. on the editable filtering).

Editable project locations in `uv pip list` include the file scheme (`file://`), where they are omitted in `pip list`. Is this desired, or should it replicate pip?

## Summary

Implementation for #1401 
`--editable` flag is implemented.

`--outdated` and `--uptodate` out of scope for this PR (requires latest version information, and type wheel/sdist)

## Test Plan

Not yet implemented as I couldn't locate the tests for `uv pip freeze`.
We can compare to `pip` in `scripts/compare_with_pip/compare_with_pip.py`?


---

_Comment by @charliermarsh on 2024-02-18 18:54_

I think it's fine for us to write our own table-formatting logic (@MichaReiser tends to have good opinions on when we should use a crate vs. do things ourselves). I don't feel strongly about `file://` but my initial reaction is that it'd be nice to strip them.

---

_Comment by @charliermarsh on 2024-02-18 18:54_

Great to see you on the repo @sbrugman, hi again!

---

_Label `enhancement` added by @zanieb on 2024-02-18 21:06_

---

_Label `cli` added by @zanieb on 2024-02-18 21:06_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-19 16:17_

---

_Comment by @sbrugman on 2024-02-20 08:16_

@BurntSushi Does self-assigning mean you will take it from here or shall I still continue?

---

_Comment by @BurntSushi on 2024-02-20 12:31_

@sbrugman Ah no sorry, I should have commented about that. Self-assigning meant I'll take ownership of reviewing.

As for your question about table formatters, if you can get away with it, my [`tabwriter`](https://docs.rs/tabwriter) crate is very simple and might be enough. With that said, we probably should try to match the output format of `pip list` since it seems like some folks parse/grep it.

---

_Comment by @sbrugman on 2024-02-20 16:21_

Thanks for the reply and the pointer. I'll continue with `tabwriter` (great to see the Springsteen references 😄 ) and test consistency with `pip list`

---

_Comment by @sbrugman on 2024-02-20 18:42_

@BurntSushi `tabwriter` misses functionality for the border below the header. We need it to replicate `pip list`:
```text
Package                    Version         Editable project location
-------------------------- --------------- ---------------------------------------
aiohttp                    3.8.1
aiosignal                  1.3.1
(...)
```

Assuming it's not in scope to extend `tabwriter` (correct me if wrong), I can check out some other packages, e.g.:
https://docs.rs/cli-table/latest/cli_table/
https://docs.rs/papergrid/latest/papergrid/

---

_Comment by @BurntSushi on 2024-02-20 19:51_

Ah yeah, `tabwriter` is pretty basic. It really only does tab expansion.

If it were me, I'd probably just hand code it:

1. Build each column in a `Vec`.
2. Find the max width of each column using [`unicode-width`](https://docs.rs/unicode-width/latest/unicode_width/).
3. Insert padding and the dashed lines as appropriate.

It shouldn't be much code and shouldn't be too tricky to write.

---

_Marked ready for review by @sbrugman on 2024-02-24 15:34_

---

_Comment by @sbrugman on 2024-02-24 15:36_

@BurntSushi Took a bit longer than hoped, ready for review! The `--outdated` and `--uptodate` flags can be added later in follow-up work, as they require latest version and type (wheel/sdist) information. 

Thanks for all the pointers, much appreciated.

(Will fix the Windows tests)

---

_Comment by @charliermarsh on 2024-02-24 15:36_

Thanks @sbrugman!

---

_Comment by @sbrugman on 2024-02-25 14:26_

Windows test fixed. 

Turned out that the Windows `file:///` prefix has an additional forward slash. Changed the tests to support workdir paths to be shorter than the replacement (as is the case on Windows). Finally needed to change the filter order to start with `INSTA_FILTERS` to rewrite Windows paths before replacement.

There are some more args that we could implement after the basic functionality is merged.


---

_@charliermarsh approved on 2024-02-25 19:19_

Excellent!

---

_Merged by @charliermarsh on 2024-02-25 19:42_

---

_Closed by @charliermarsh on 2024-02-25 19:42_

---

_Branch deleted on 2024-02-25 19:46_

---
