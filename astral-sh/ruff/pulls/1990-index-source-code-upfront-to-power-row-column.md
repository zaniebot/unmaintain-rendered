```yaml
number: 1990
title: Index source code upfront to power (row, column) lookups
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unrope
created_at: 2023-01-19T05:10:44Z
updated_at: 2023-01-23T19:20:21Z
url: https://github.com/astral-sh/ruff/pull/1990
synced_at: 2026-01-12T04:51:59Z
```

# Index source code upfront to power (row, column) lookups

---

_Pull request opened by @charliermarsh on 2023-01-19 05:10_

## Summary

The problem: given a (row, column) number (e.g., for a token in the AST), we need to be able to map it to a precise byte index in the source code. A while ago, we moved to `ropey` for this, since it was faster in practice (mostly, I think, because it's able to defer indexing). However, at some threshold of accesses, it becomes faster to index the string in advance, as we're doing here.

(We can get a further speedup by doing an `is_ascii` check upfront. The vast majority of Python source code is ASCII, and it's much faster to index if we don't have to read every character's byte length, etc.)

## Benchmark

This newer version is a ~10.5% speedup in the ALL case, and a ~1% slowdown in the default case.

Before:

![main](https://user-images.githubusercontent.com/1309177/213883581-8f73c61d-2979-4171-88a6-a88d7ff07e40.png)

After:

![Screen Shot 2023-01-21 at 5 33 27 PM](https://user-images.githubusercontent.com/1309177/213889593-551c1812-577e-488d-a006-ba6bfa2227e8.png)


---

_@messense reviewed on 2023-01-19 10:49_

---

_Review comment by @messense on `src/source_code/locator.rs`:51 on 2023-01-19 10:49_

Use `char_indices()` might be cleaner?

```suggestion
pub fn compute_offsets(contents: &str) -> Vec<Vec<usize>> {
    let mut offsets = Vec::new();
    let mut current_row = Vec::new();
    for (index, char) in contents.char_indices() {
        current_row.push(index);
        if char == '\n' {
            offsets.push(current_row);
            current_row = Vec::new();
        }
    }
    offsets.push(current_row);
    offsets
}
```

---

_Renamed from "Revert use of Rope for raw source code extraction" to "Index source cope upfront to power (row, column) lookups" by @charliermarsh on 2023-01-21 19:26_

---

_Renamed from "Index source cope upfront to power (row, column) lookups" to "Index source code upfront to power (row, column) lookups" by @charliermarsh on 2023-01-21 19:26_

---

_Review comment by @Stranger6667 on `src/source_code/locator.rs`:13 on 2023-01-21 21:48_

As an idea - could it be `Vec<Range<usize>>`? I assume that it will be faster to build (as there are fewer vectors involved). Then `trunc` will need a slight change:

```rust
fn truncate(location: Location, offsets: &[Range<usize>], contents: &str) -> usize {
    if (location.row() - 1 == offsets.len() && location.column() == 0)
        || (location.row() - 1 == offsets.len() - 1
            && location.column() == offsets[location.row() - 1].len())
    {
        contents.len()
    } else {
        offsets[location.row() - 1].start + location.column()
    }
}
```

Also, I assume that it would be faster too - there will be no memory access into the nested vector.


---

_@Stranger6667 reviewed on 2023-01-21 21:48_

---

_@Stranger6667 reviewed on 2023-01-21 21:54_

---

_Review comment by @Stranger6667 on `src/source_code/locator.rs`:13 on 2023-01-21 21:54_

Or, do we even need `Range` there? Maybe it could be `Vec<usize>` with row start indexes - calculating line size would be the difference with the next element (or `content.len()` for the last one).

---

_@charliermarsh reviewed on 2023-01-21 21:54_

---

_Review comment by @charliermarsh on `src/source_code/locator.rs`:13 on 2023-01-21 21:54_

Does that not assume that every character in the line is a single byte?

---

_Review comment by @charliermarsh on `src/source_code/locator.rs`:13 on 2023-01-21 21:56_

What I'm considering is to use that exact strategy for ascii source code:

```rs
pub fn compute_offsets(contents: &str) -> Offset {
    if contents.is_ascii() {
        let mut offsets = Vec::with_capacity(48);
        let mut total = 0;
        offsets.push(total);
        while let Some(index) = contents[total..].find('\n') {
            offsets.push(total + index + 1);
            total += index + 1;
        }
        Offset::Ascii(offsets)
    } else {
        let mut offsets = Vec::with_capacity(48);
        let mut current_row = Vec::with_capacity(48);
        let mut current_byte_offset = 0;
        let mut previous_char = '\0';
        for char in contents.chars() {
            current_row.push(current_byte_offset);
            if char == '\n' {
                if previous_char == '\r' {
                    current_row.pop();
                }
                offsets.push(current_row);
                current_row = Vec::with_capacity(48);
            }
            current_byte_offset += char.len_utf8();
            previous_char = char;
        }
        offsets.push(current_row);
        Offset::Utf8(offsets)
    }
}
```

---

_@charliermarsh reviewed on 2023-01-21 21:56_

---

_@charliermarsh reviewed on 2023-01-21 21:56_

---

_Review comment by @charliermarsh on `src/source_code/locator.rs`:13 on 2023-01-21 21:56_

(For UTF-8, I think we need to know the index of every character.)

---

_@Stranger6667 reviewed on 2023-01-21 22:03_

---

_Review comment by @Stranger6667 on `src/source_code/locator.rs`:13 on 2023-01-21 22:03_

Oh, you're right, it won't properly work for multibyte characters :(

---

_@charliermarsh reviewed on 2023-01-21 22:09_

---

_Review comment by @charliermarsh on `src/source_code/locator.rs`:13 on 2023-01-21 22:09_

The vast majority of Python source code is single-byte though, so it'd be nice to exploit that :)

---

_@Stranger6667 reviewed on 2023-01-21 22:10_

---

_Review comment by @Stranger6667 on `src/source_code/locator.rs`:13 on 2023-01-21 22:10_

Yep, I assume it should be the case! :)

---

_Comment by @charliermarsh on 2023-01-21 22:34_

Alright, newer version is a ~10.5% speedup in the `ALL` case, and a ~1% slowdown in the default case.

![Screen Shot 2023-01-21 at 5 33 27 PM](https://user-images.githubusercontent.com/1309177/213889593-551c1812-577e-488d-a006-ba6bfa2227e8.png)

(Editing PR summary to include this outcome...)


---

_Merged by @charliermarsh on 2023-01-21 22:56_

---

_Closed by @charliermarsh on 2023-01-21 22:56_

---

_Branch deleted on 2023-01-21 22:56_

---
