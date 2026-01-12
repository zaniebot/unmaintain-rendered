```yaml
number: 11927
title: "ENH copyright-notice: check in the first 4096 bytes instead of 1024"
type: pull_request
state: merged
author: adrinjalali
labels:
  - rule
assignees: []
merged: true
base: main
head: 4096
created_at: 2024-06-18T14:48:13Z
updated_at: 2024-06-18T18:15:51Z
url: https://github.com/astral-sh/ruff/pull/11927
synced_at: 2026-01-12T15:55:39Z
```

# ENH copyright-notice: check in the first 4096 bytes instead of 1024

---

_@adrinjalali_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
related to https://github.com/astral-sh/ruff/issues/5306

The check right now only checks in the first 1024 bytes, and that's really not enough when there's a docstring at the beginning of a file.

A more proper fix might be needed, which might be more complex (and I don't have the `rust` skills to implement that). But this temporary "fix" might enable more users to use this.

Context: We want to use this rule in https://github.com/scikit-learn/scikit-learn/ and we got blocked because of this hardcoded rule (which TBH took us quite a while to figure out why it was failing since it's not documented).

## Test Plan

This is already kinda tested, modified the test for the new byte number.

<!-- How was it tested? -->


---

_Renamed from "4096" to "ENH copyright-notice: check in the first 4096 bytes instead of 1024" by @adrinjalali on 2024-06-18 15:05_

---

_@zanieb approved on 2024-06-18 15:50_

Seems okay to me. Thanks for contributing! cc @MichaReiser 

---

_Label `rule` added by @zanieb on 2024-06-18 15:50_

---

_@MichaReiser approved on 2024-06-18 16:03_

Looks okay, although I'm not sure why the limit even exists. The only performance issue that I can see is if the regex uses a wildcard at the start. Any regex without a wildcard at the start should end by just testing a few characters.

---

_Merged by @zanieb on 2024-06-18 16:04_

---

_Closed by @zanieb on 2024-06-18 16:04_

---

_Comment by @MichaReiser on 2024-06-18 16:07_

Okay it seems that the somewhat arbitrary limit is coming from the original lint rule. I think the limit there makes sense because the rule actually reads the file. The situation for ruff is different because we already have the file in memory. 

Anyway, thanks for contributing!

---

_Comment by @zanieb on 2024-06-18 16:07_

I'm not sure either cc @BurntSushi 

Not a lot of context in the original implementation discussion at https://github.com/astral-sh/ruff/pull/4701


---

_Comment by @BurntSushi on 2024-06-18 16:44_

Here's the regex:

https://github.com/astral-sh/ruff/blob/2e7c3454e0d4ef24b6d46c7ec5fac7eab56ae907/crates/ruff_linter/src/rules/flake8_copyright/settings.rs#L18

Specifically:

```
(?i)Copyright\s+((?:\(C\)|©)\s+)?\d{4}((-|,\s)\d{4})*
```

And the search is using `Regex::find`, which is an unanchored search. So the regex behaves as if there is a `(?s:.)*?` at the beginning. So putting a limit on how much is searched seems sensible if you only expect it to be near the beginning of file _and_ it's plausible that this could be used to search very large files. I doubt 4096 and 1024 will make much of a difference, so this change seems okay.

---

_Branch deleted on 2024-06-18 18:15_

---
