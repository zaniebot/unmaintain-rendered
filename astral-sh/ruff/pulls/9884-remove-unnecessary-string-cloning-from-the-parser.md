```yaml
number: 9884
title: Remove unnecessary string cloning from the parser
type: pull_request
state: merged
author: charliermarsh
labels:
  - parser
assignees: []
merged: true
base: main
head: charlie/string-parser
created_at: 2024-02-07T22:05:35Z
updated_at: 2024-02-09T21:14:16Z
url: https://github.com/astral-sh/ruff/pull/9884
synced_at: 2026-01-10T22:57:09Z
```

# Remove unnecessary string cloning from the parser

---

_Pull request opened by @charliermarsh on 2024-02-07 22:05_

Closes https://github.com/astral-sh/ruff/issues/9869.

---

_Comment by @github-actions[bot] on 2024-02-07 22:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2024-02-08 03:56_

Hate this change, didn't even improve benchmarks!!!

---

_Comment by @MichaReiser on 2024-02-08 17:37_

> Hate this change, didn't even improve benchmarks!!!

That's because codspeed uses the fixed cost 0 for allocations....

---

_Marked ready for review by @charliermarsh on 2024-02-08 18:07_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-08 18:08_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-02-08 18:08_

---

_@charliermarsh reviewed on 2024-02-08 18:08_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string.rs`:293 on 2024-02-08 18:08_

@BurntSushi - Is this exposed in any crates that we already use? I had to vendor it from `bstr`.

---

_@BurntSushi reviewed on 2024-02-08 18:46_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/string.rs`:293 on 2024-02-08 18:46_

Yes. :P https://docs.rs/bstr/latest/bstr/trait.ByteSlice.html#method.find_non_ascii_byte

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:220 on 2024-02-08 19:46_

nit: the index could also include `{` or `}` as well, right? The variable name `before_with_slash` suggests otherwise unless I'm wrong.

---

_@dhruvmanila approved on 2024-02-08 20:07_

Nice!

---

_Label `parser` added by @dhruvmanila on 2024-02-08 20:07_

---

_@MichaReiser approved on 2024-02-08 20:42_

---

_@MichaReiser approved on 2024-02-08 20:44_

:guitar:

---

_Comment by @codspeed-hq[bot] on 2024-02-08 21:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/string-parser)

### Merging #9884 will **improve performances by 6.53%**

<sub>Comparing <code>charlie/string-parser</code> (58178b3) with <code>main</code> (7ca515c)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/string-parser` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 8.6 ms | 8 ms | +6.53% |


---

_Comment by @charliermarsh on 2024-02-09 02:46_

I'm gonna get to the bottom of this if it's the last thing I do.

---

_Converted to draft by @charliermarsh on 2024-02-09 04:01_

---

_Comment by @charliermarsh on 2024-02-09 04:50_

I see basically no improvement on local benchmarks either via criterion or hyperfine. I'm not sure why, to be honest. I guess I'll proceed with the change since it's both (1) a better API, and (2) intuitively better to avoid re-allocating here when we don't need to. But I'm a bit confused.

---

_Comment by @charliermarsh on 2024-02-09 04:51_

(Ignore current state of the PR, I duplicated the existing and updated code to do some benchmarking. I'll clean up later.)

---

_Comment by @charliermarsh on 2024-02-09 20:05_

By removing lexing from the parser benchmark, I'm now able to see a more consistent 1-2% improvement in parse time, so I'll move forward with this. (Benchmarking the individual string-parsing functions (rather than the parser as a whole) shows an enormous speed-up, like 2x.)

---

_Marked ready for review by @charliermarsh on 2024-02-09 20:46_

---

_Merged by @charliermarsh on 2024-02-09 21:03_

---

_Closed by @charliermarsh on 2024-02-09 21:03_

---

_Branch deleted on 2024-02-09 21:03_

---
