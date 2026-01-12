```yaml
number: 10627
title: "Remove `import re` from entrypoint wrapper scripts"
type: pull_request
state: merged
author: hukkin
labels:
  - performance
assignees: []
merged: true
base: main
head: rm-re-import
created_at: 2025-01-15T08:54:52Z
updated_at: 2025-01-15T18:51:11Z
url: https://github.com/astral-sh/uv/pull/10627
synced_at: 2026-01-12T16:09:24Z
```

# Remove `import re` from entrypoint wrapper scripts

---

_@hukkin_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This removes `import re` from the wrapper script of installed tools, so that small tools that never `import re` themselves are allowed to load faster.

This was offered to `distlib` but was rejected https://github.com/pypa/distlib/pull/239. That discussion includes all the relevant performance measurements for this change.

Here's the corresponding `pip` issue https://github.com/pypa/pip/issues/13165 and `pip` PR https://github.com/pypa/pip/pull/13166

The motivation for this change was [this discussion](https://discuss.python.org/t/make-more-of-the-standard-library-import-on-demand/76311) on Python Discourse.

## Test Plan

<!-- How was it tested? -->

Ran
```console
cargo build
./target/debug/uv tool install black
black --version
# observed that installed black runs succesfully
cat ~/.local/bin/black
# observed that the wrapper script no longer imports `re` module
```



---

_Review comment by @T-256 on `crates/uv-install-wheel/src/wheel.rs`:44 on 2025-01-15 13:36_

I think it's not compatible with Python < 3.10

---

_@T-256 reviewed on 2025-01-15 13:36_

---

_@hukkin reviewed on 2025-01-15 13:43_

---

_Review comment by @hukkin on `crates/uv-install-wheel/src/wheel.rs`:44 on 2025-01-15 13:43_

I've tried this on Python 2.7, it should work fine. What feature do you think is not compatible?

---

_@T-256 reviewed on 2025-01-15 13:52_

---

_Review comment by @T-256 on `crates/uv-install-wheel/src/wheel.rs`:44 on 2025-01-15 13:52_

I just know `str.endswith` added in Python 3.10.

---

_Review comment by @T-256 on `crates/uv-install-wheel/src/wheel.rs`:44 on 2025-01-15 13:56_

I think you could also do:
```suggestion
    if sys.argv[0][-11:] == "-script.pyw":
```

---

_@T-256 reviewed on 2025-01-15 13:56_

---

_@hukkin reviewed on 2025-01-15 13:57_

---

_Review comment by @hukkin on `crates/uv-install-wheel/src/wheel.rs`:44 on 2025-01-15 13:57_

Here's Python 2.6 docs referencing it https://docs.python.org/2.6/library/stdtypes.html#str.endswith

---

_@T-256 reviewed on 2025-01-15 14:03_

---

_Review comment by @T-256 on `crates/uv-install-wheel/src/wheel.rs`:44 on 2025-01-15 14:03_

Oh, forgive me, I almost losing my Python memories after working with Rust.

> I just know `str.endswith` added in Python 3.9.

(That was `removesuffix`.)


---

_@zanieb approved on 2025-01-15 16:30_

This seems reasonable to me.

cc @konstin 

---

_Comment by @charliermarsh on 2025-01-15 16:31_

Yeah this is good.

---

_@konstin approved on 2025-01-15 18:44_

nice work, thanks!

---

_Label `performance` added by @konstin on 2025-01-15 18:45_

---

_Merged by @konstin on 2025-01-15 18:45_

---

_Closed by @konstin on 2025-01-15 18:45_

---

_Branch deleted on 2025-01-15 18:51_

---
