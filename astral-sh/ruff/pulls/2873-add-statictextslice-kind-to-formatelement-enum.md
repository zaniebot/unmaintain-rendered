```yaml
number: 2873
title: "Add `StaticTextSlice` kind to `FormatElement` enum"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/formatter-ii
created_at: 2023-02-13T23:32:01Z
updated_at: 2023-02-15T03:27:54Z
url: https://github.com/astral-sh/ruff/pull/2873
synced_at: 2026-01-12T15:55:11Z
```

# Add `StaticTextSlice` kind to `FormatElement` enum

---

_@charliermarsh_

Given our current parser abstractions, we need the ability to tell `ruff_formatter` to print a pre-defined slice from a fixed string of source code, which we've introduced here as `FormatElement::StaticTextSlice`.

---

_Comment by @charliermarsh on 2023-02-13 23:32_

\cc @MichaReiser 

---

_Label `autoformatter` added by @charliermarsh on 2023-02-13 23:37_

---

_@not-my-profile reviewed on 2023-02-14 05:25_

---

_Review comment by @not-my-profile on `crates/ruff_formatter/src/format_element.rs`:54 on 2023-02-14 05:25_

Shouldn't this rather be `std::ops::Range<usize>`?

This would let us change e.g. `*start..*end` to `range.clone()`.

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element.rs`:54 on 2023-02-14 08:58_

I recommend you to use `TextRange` here. Documents larger than 4GB should be rare. Using `TextRange` helps keep the overall size of `FormatElement` low, which is essential for good formatter performance (fewer bytes to write when formatting, fewer bytes to read when printing, better cache locality...) and low memory usage. 

Using `TextSize` and `TextRange` proved good practice when working on Rome because it encodes that these are byte offsets into strings rather than offsets into some other data structure. That's why Rome uses them consistently when slicing into source code.

---

_@MichaReiser reviewed on 2023-02-14 08:58_

---

_Comment by @MichaReiser on 2023-02-14 10:49_

Do you have an example on how this `FormatElement` is used? You may also want to add a builder to `builder.rs` that writes a `FormatElement::StaticTextSlice`. 

---

_Comment by @charliermarsh on 2023-02-15 03:24_

I have a builder implemented in #2883 but it hinges on some details from the RustPython AST. Added a generic builder here.

---

_Merged by @charliermarsh on 2023-02-15 03:27_

---

_Closed by @charliermarsh on 2023-02-15 03:27_

---

_Branch deleted on 2023-02-15 03:27_

---
