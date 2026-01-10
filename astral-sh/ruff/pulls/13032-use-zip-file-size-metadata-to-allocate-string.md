```yaml
number: 13032
title: Use ZIP file size metadata to allocate string
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: vendored-alloc-string-with-right-size
created_at: 2024-08-21T12:24:09Z
updated_at: 2024-08-21T12:58:20Z
url: https://github.com/astral-sh/ruff/pull/13032
synced_at: 2026-01-10T21:38:32Z
```

# Use ZIP file size metadata to allocate string

---

_Pull request opened by @MichaReiser on 2024-08-21 12:24_

## Summary

We spend about 1.4% in `VendoredFileSystem::read_to_string`. Let's pre-allocate the string with the right size by using the zip file metadata
as a heuristic (don't trust it!)

## Test Plan

`cargo test`

I verified that total spend in `VendoredFileSystem::read_to_string` goes down to 1.1% and it no longer shows up as a large contributor to `malloc`

---

_Review requested from @carljm by @MichaReiser on 2024-08-21 12:24_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-21 12:24_

---

_Comment by @AlexWaygood on 2024-08-21 12:27_

Hmm. In https://github.com/astral-sh/ruff/pull/11863#discussion_r1639337339, you argued against doing this

---

_Comment by @codspeed-hq[bot] on 2024-08-21 12:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/vendored-alloc-string-with-right-size)

### Merging #13032 will **improve performances by 8.11%**

<sub>Comparing <code>vendored-alloc-string-with-right-size</code> (170ffb4) with <code>main</code> (a35cdbb)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `vendored-alloc-string-with-right-size` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 183.9 µs | 170.1 µs | +8.11% |


---

_Comment by @MichaReiser on 2024-08-21 12:36_

Yeah, I now saw in the benchmarks (still need to verify that it helped) that I was wrong. 

Reading through the code:

```
    fn read_to_string(&mut self, buf: &mut String) -> Result<usize> {
        default_read_to_string(self, buf, None)
    }
```

the last argument is the size hint... Oh well, it is `None`

---

_Comment by @github-actions[bot] on 2024-08-21 12:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-21 12:38_

Thanks for explaining, sounds good!

(Is it worth adding a comment explaining the reasoning you just gave me?)

---

_Label `red-knot` added by @MichaReiser on 2024-08-21 12:42_

---

_Merged by @MichaReiser on 2024-08-21 12:48_

---

_Closed by @MichaReiser on 2024-08-21 12:48_

---

_Branch deleted on 2024-08-21 12:48_

---
