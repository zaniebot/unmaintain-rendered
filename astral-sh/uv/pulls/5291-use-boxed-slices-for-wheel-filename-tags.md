```yaml
number: 5291
title: Use boxed slices for wheel filename tags
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
base: main
head: charlie/box
created_at: 2024-07-22T14:50:35Z
updated_at: 2024-09-23T23:30:53Z
url: https://github.com/astral-sh/uv/pull/5291
synced_at: 2026-01-12T16:06:44Z
```

# Use boxed slices for wheel filename tags

---

_@charliermarsh_

## Summary

Easy trick to reduce the size of the struct, since these are never mutated.


---

_Label `performance` added by @charliermarsh on 2024-07-22 14:50_

---

_@charliermarsh reviewed on 2024-07-22 14:50_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/wheel.rs`:100 on 2024-07-22 14:50_

This has since been fixed, but I forgot to remove the TODO at the time.

---

_Comment by @codspeed-hq[bot] on 2024-07-22 14:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/box)

### Merging #5291 will **degrade performances by 24.1%**

<sub>Comparing <code>charlie/box</code> (2fb6638) with <code>main</code> (d11d11e)</sub>



### Summary

`❌ 3` regressions
`✅ 10` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/box)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/box` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_parsing[flyte-long-compatible]` | 9.9 µs | 11.5 µs | -14.37% |
| ❌ | `wheelname_parsing[flyte-short-compatible]` | 6.2 µs | 8.2 µs | -24.05% |
| ❌ | `wheelname_parsing[flyte-short-incompatible]` | 6.3 µs | 8.3 µs | -24.1% |


---

_Comment by @charliermarsh on 2024-07-22 15:01_

That's surprising, will take a look.

---

_@BurntSushi approved on 2024-07-23 14:45_

I like it.

We could probably do more work to shrink this type if we think it's causing issues.

---

_Comment by @charliermarsh on 2024-07-23 14:46_

Why do you think the benchmarks regressed? :(

When I just did collect and didn't pre-allocator the vector by scanning the string twice, it regressed even more.

---

_Comment by @BurntSushi on 2024-07-23 14:53_

> Why do you think the benchmarks regressed? :(
> 
> When I just did collect and didn't pre-allocator the vector by scanning the string twice, it regressed even more.

I spent some time looking at the flamegraph diff, and nothing sticks out at me. So for this, I think my next step would be to:

1. Run the benchmarks locally by measuring wall clock time instead of instruction count, which is what I believe codspeed is doing, right? If the wall clock time has no difference, then I'd ship it.
2. If there is a difference, then I'd look to see if profiles locally say more than the codspeed flamegraphs. Failing that, I'd start looking at the actual codegen differences.

In terms of _why_ this is happening, I don't actually have a great guess. Maybe this change increases instruction counts by doing the `Vec -> Box` conversion and that in turn causes the regression? That guess is making _a lot_ of assumptions though (codspeed's cost model, that `Vec -> Box` conversion is even adding instructions and probably more) that aren't necessarily correct.

---

_Closed by @charliermarsh on 2024-09-23 23:30_

---
