```yaml
number: 10341
title: "Buffer file reads in `serde_json::from_reader`"
type: pull_request
state: merged
author: ajtulloch
labels: []
assignees: []
merged: true
base: main
head: tulloch/240106-fix-buffering
created_at: 2025-01-07T06:42:15Z
updated_at: 2025-01-07T13:02:33Z
url: https://github.com/astral-sh/uv/pull/10341
synced_at: 2026-01-10T11:44:43Z
```

# Buffer file reads in `serde_json::from_reader`

---

_Pull request opened by @ajtulloch on 2025-01-07 06:42_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

https://docs.rs/serde_json/latest/serde_json/fn.from_reader.html suggests that

> When reading from a source against which short reads are not efficient, such as a [File](https://doc.rust-lang.org/std/fs/struct.File.html), you will want to apply your own buffering because serde_json will not buffer the input. See [std::io::BufReader](https://doc.rust-lang.org/std/io/struct.BufReader.html).

Without this buffering, we observe a sequence of single byte reads which can be quite inefficient depending on the underlying filesystem.

This adds buffering with `std::io::BufReader` to resolve this.


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Unit tests cover this code.

<!-- How was it tested? -->


---

_Renamed from "Buffer file reads in serde_json::from_reader" to "Buffer file reads in `serde_json::from_reader`" by @ajtulloch on 2025-01-07 06:45_

---

_Review requested from @BurntSushi by @konstin on 2025-01-07 09:25_

---

_@BurntSushi approved on 2025-01-07 13:02_

Makes sense to me! Nice find!

---

_Merged by @BurntSushi on 2025-01-07 13:02_

---

_Closed by @BurntSushi on 2025-01-07 13:02_

---
