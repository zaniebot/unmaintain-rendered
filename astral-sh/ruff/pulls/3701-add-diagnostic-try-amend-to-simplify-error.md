```yaml
number: 3701
title: "Add `Diagnostic.try_amend()` to simplify error handling"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: refactor-diagnostic-amend
created_at: 2023-03-23T23:21:49Z
updated_at: 2023-03-24T22:38:31Z
url: https://github.com/astral-sh/ruff/pull/3701
synced_at: 2026-01-12T15:55:13Z
```

# Add `Diagnostic.try_amend()` to simplify error handling

---

_@JonathanPlasse_

_No description provided._

---

_@JonathanPlasse reviewed on 2023-03-23 23:23_

---

_Review comment by @JonathanPlasse on `crates/ruff_diagnostics/src/diagnostic.rs`:56 on 2023-03-23 23:23_

Handle logging errors from failed auto-fix here.

---

_Comment by @github-actions[bot] on 2023-03-23 23:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.11ms     2.9 MB/sec    1.01     14.3±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.01      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.0±1.25µs     6.8 MB/sec    1.01    439.0±2.91µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.01      6.3±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.01ms     5.3 MB/sec    1.02      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1704.4±1.59µs     9.8 MB/sec    1.00   1697.9±2.71µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.5±0.20µs    17.0 MB/sec    1.02    176.2±0.76µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     15.5±0.05ms     2.6 MB/sec    1.00     15.0±0.16ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    448.6±9.29µs     6.6 MB/sec    1.00    449.2±5.34µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.9±0.16ms     3.7 MB/sec    1.00      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.07      8.8±0.09ms     4.6 MB/sec    1.00      8.2±0.13ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1845.8±48.64µs     9.0 MB/sec    1.00  1747.1±22.62µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.02    184.0±5.35µs    16.0 MB/sec    1.00    180.8±3.83µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.04ms     6.4 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/diagnostic.rs`:56 on 2023-03-24 00:13_

I'd like to get @MichaReiser's opinion, it strikes me as potentially unusual to have an API accept `Result` (since, in _most_ cases, we know the value is `Ok`), though the ergonomic benefits are clear.

Without looking at other APIs for inspiration... perhaps something like this would be more idiomatic?

```rust
pub fn try_amend(&mut self, fn: FnOnce() -> Result<Fix>) {
  ...
}
```

Don't act on that proposal yet though, since @MichaReiser may have a different suggestion.


---

_@charliermarsh reviewed on 2023-03-24 00:13_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/diagnostic.rs`:56 on 2023-03-24 07:44_

I am undecided on the API. Mainly because I don't yet understand how we want to support this in the long-term where we shouldn't use `error` if a fix fails, but instead add a diagnostic to the output that gets emitted the same as any other diagnostic. 

But that's still far in the future and it should not prevent us from improving the API in the short term. I like @charliermarsh's suggestion of adding a second `try_amend` that accepts a callback. I find this more idiomatic than passing `Result` but mainly like it because it doesn't require changing all `amend` calls where the parameter always is `Ok` 


Unrelated: What's the reason for the name `amend`. How about renaming the function to `with_fix` or `try_with_fix` in another PR?

---

_@MichaReiser reviewed on 2023-03-24 07:44_

---

_Merged by @charliermarsh on 2023-03-24 21:10_

---

_Closed by @charliermarsh on 2023-03-24 21:10_

---

_@charliermarsh reviewed on 2023-03-24 21:10_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/diagnostic.rs`:56 on 2023-03-24 21:10_

Yeah good call. I'll handle the rename.

---

_Renamed from "Refactor Diagnostic.amend()" to "Add `Diagnostic.try_amend()` to simplify error handling" by @charliermarsh on 2023-03-24 21:10_

---

_Branch deleted on 2023-03-24 22:38_

---
