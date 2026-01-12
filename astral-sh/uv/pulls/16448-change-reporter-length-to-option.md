```yaml
number: 16448
title: "Change reporter length to `Option`"
type: pull_request
state: merged
author: 0xllx0
labels:
  - internal
assignees: []
merged: true
base: main
head: issue/single-item-reporter
created_at: 2025-10-25T11:32:51Z
updated_at: 2025-10-27T16:17:14Z
url: https://github.com/astral-sh/uv/pull/16448
synced_at: 2026-01-12T16:12:15Z
```

# Change reporter length to `Option`

---

_@0xllx0_

## Summary

Changes the `length` parameter to `PythonDownloadReporter::new` to an `Option<u64>` to resolve the issue of setting a single-item reporter length.

Resolves: #15404

## Test Plan

- build test
- ran test suite

---

_Comment by @zanieb on 2025-10-25 14:14_

I think this came up w.r.t. `BinaryDownloadReporter`, we should make sure other reports are consistent.

Does this have any user-facing effect?

---

_Review requested from @konstin by @zanieb on 2025-10-25 14:15_

---

_Comment by @0xllx0 on 2025-10-25 23:33_

> I think this came up w.r.t. BinaryDownloadReporter, we should make sure other reports are consistent.

I changed all of the `*Reporter::new(.., length: ..)` methods to take an `Option`, hopefully I understood your suggestion.

The builder-like `with_length` methods I left alone, since presumably these are always meant to set the length, and not an optional length.

Also corrected the single reporter to pass a `Some(1)` argument, instead of the incorrect `None` that was there before (noticed after reviewing the other reporters).

> Does this have any user-facing effect?

I don't think it should, since all of these methods are `pub(crate)`. If there are manual UI/UX smoke tests I can run, just let me know.

---

_Renamed from "reporters: change reporter length to `Option`" to "Change reporter length to `Option`" by @konstin on 2025-10-27 09:06_

---

_Label `internal` added by @konstin on 2025-10-27 09:31_

---

_Comment by @konstin on 2025-10-27 09:33_

The current implementation still sets `Some(1)`, even though that's not the correct length, so we're not yet solving https://github.com/astral-sh/uv/issues/15404.

I checked with the example of the publish reporter, and it seems the `root` progress bar is created but then never used (so it's hidden), but that's a bigger change.

---

_Comment by @0xllx0 on 2025-10-27 14:42_

> The current implementation still sets Some(1), even though that's not the correct length, so we're not yet solving https://github.com/astral-sh/uv/issues/15404.

Right, so I originally set it to `None`, but wasn't sure if this was correct after the initial review.

I ran a little smoke test:

```rust
use indicatif::{ProgressBar, ProgressDrawTarget};

let bytes = b"implementation of io::Read";
let reader = bytes.as_ref();

let pb = ProgressBar::with_draw_target(
    Some(1),
    //Some(bytes.len() as u64),
    //None,
    ProgressDrawTarget::stdout());

let mut writer = Vec::new();

for r in pb.wrap_iter(reader.iter()) {
    writer.push(*r);
    std::thread::sleep(std::time::Duration::from_millis(100));
}
```

All variants of the length argument show the progress counter updates with the correct counter values.

- `Some(1)` argument lights up the entire progress bar
- `Some(bytes.len() as u64)` argument lights up the progress bar as progress is made
- `None` keeps the progress bar dark as progress is made

Now I see what you meant about it being an aesthetic choice. I think in absence of having the actual length of progress, `None` makes sense.

So, would you like me to change all of the `Some(1)` progress bars to `None`?

---

_Comment by @konstin on 2025-10-27 14:42_

Yes, we should go with `None`

---

_Comment by @konstin on 2025-10-27 16:16_

I did some manual testing and it looks like the unused root progress bar is still hidden after the change, as expected.

---

_@konstin approved on 2025-10-27 16:16_

---

_Merged by @konstin on 2025-10-27 16:17_

---

_Closed by @konstin on 2025-10-27 16:17_

---
