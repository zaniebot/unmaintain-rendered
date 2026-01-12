```yaml
number: 3002
title: Add tests for binary file detection when using memory maps
type: pull_request
state: closed
author: Erio-Harrison
labels:
  - rollup
assignees: []
base: master
head: add-mmap-binary-tests
created_at: 2025-02-27T11:10:55Z
updated_at: 2025-08-18T01:10:07Z
url: https://github.com/BurntSushi/ripgrep/pull/3002
synced_at: 2026-01-12T18:23:14Z
```

# Add tests for binary file detection when using memory maps

---

_@Erio-Harrison_

The original code contained a TODO comment to add tests for binary file detection
when using memory maps. This PR implements those tests, noting the differences
between mmap and no-mmap behavior regarding binary detection.

The tests show that with memory maps:
- NUL bytes are only searched for in the first few KB and in matches
- Explicit file paths behave differently than glob patterns
- Matches can appear both before and after NUL bytes
- Binary detection is more permissive with memory maps

---

_Review comment by @BurntSushi on `tests/binary.rs`:44 on 2025-08-17 20:17_

This wasn't quite right. This test doesn't use memory maps. When you use the `-g` flag without an explicit file path, that puts ripgrep in recursive search mode. In that mode, ripgrep generally never uses memory maps.

---

_Review comment by @BurntSushi on `tests/binary.rs`:79 on 2025-08-17 20:19_

I think this test would be more effective if it also included a pattern that matched after the NUL byte.

---

_Review comment by @BurntSushi on `tests/binary.rs`:104 on 2025-08-17 20:23_

This test doesn't check what is written in the comments. Memory maps aren't used here. You can force it by adding the `--mmap` flag. Indeed, when memory maps are used here, the result changes!

---

_Review comment by @BurntSushi on `tests/binary.rs`:118 on 2025-08-17 20:23_

This test includes matches after **and before** the NUL byte.

---

_@BurntSushi reviewed on 2025-08-17 20:24_

Thanks! I've left feedback, but please don't work on it. I've already resolved the feedback in my rollup branch.

---

_Label `rollup` added by @BurntSushi on 2025-08-17 20:24_

---

_Comment by @Erio-Harrison on 2025-08-18 01:06_

> Thanks! I've left feedback, but please don't work on it. I've already resolved the feedback in my rollup branch.

That's cool! Thank you for your work.

---

_Closed by @Erio-Harrison on 2025-08-18 01:10_

---
