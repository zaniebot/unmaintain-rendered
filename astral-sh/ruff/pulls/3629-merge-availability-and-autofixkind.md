```yaml
number: 3629
title: Merge Availability and AutofixKind
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: merge-availablity-and-autofixkind
created_at: 2023-03-20T16:05:20Z
updated_at: 2023-03-20T17:06:35Z
url: https://github.com/astral-sh/ruff/pull/3629
synced_at: 2026-01-12T04:39:45Z
```

# Merge Availability and AutofixKind

---

_Pull request opened by @JonathanPlasse on 2023-03-20 16:05_

Implement suggestion from @MichaReiser 
> NIt: Unrelated to this change: A `Availability::None` would remove the need for `Option<AutofixKind>`. It would then simply be `AUTOFX: AutofixKind = AutofixKind::NONE` (we could even inline `Availability` into `AutofixKind`

_Originally posted by @MichaReiser in https://github.com/charliermarsh/ruff/pull/3593#discussion_r1141763825_
            

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:47 on 2023-03-20 16:10_

Nit: We could implement `Display` for `AutofixKind`. You could then write this as
```suggestion
		let autofix = rule.autofixable();
		if matches!(autofix, AutofxKind::Sometimes | AutofixKind::Always) {
			writeln!(output, "{autofix}\n").unwrap(); // Writting to a string never fails except if the program OOms
		}
```

and then re-use the same logic in `generate_docs`

---

_@MichaReiser approved on 2023-03-20 16:11_

Nice. Thanks for working on this improvement :)

---

_Review comment by @JonathanPlasse on `crates/ruff_cli/src/commands/rule.rs`:47 on 2023-03-20 16:16_

I do not know the term `OOms`. What does it mean?

---

_@JonathanPlasse reviewed on 2023-03-20 16:16_

---

_Comment by @github-actions[bot] on 2023-03-20 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.05ms     2.9 MB/sec    1.00     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.4 MB/sec    1.00      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.3±2.45µs     6.9 MB/sec    1.00    425.3±1.98µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.02ms     5.2 MB/sec    1.00      7.7±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1689.1±5.22µs     9.9 MB/sec    1.00   1692.6±4.20µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.3±0.33µs    16.8 MB/sec    1.01    176.5±0.64µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.12ms     2.8 MB/sec    1.00     14.8±0.20ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.02      4.1±0.10ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.8±6.69µs     6.9 MB/sec    1.00    430.8±6.15µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.9 MB/sec    1.03      6.8±0.18ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.01      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1722.6±7.38µs     9.7 MB/sec    1.01  1733.8±12.06µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.4±1.73µs    17.1 MB/sec    1.01    173.6±2.02µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.8 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-03-20 16:19_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:47 on 2023-03-20 16:19_

Oh, sorry. I should avoid using acronyms, and that's my java background shining through. OOM is an acronym for Out of Memory (exception in Java). And *OOms* is the activity when the program runs out of memory. The equivalent in Python is probably the `MemoryError`

---

_@MichaReiser reviewed on 2023-03-20 16:34_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/violation.rs`:14 on 2023-03-20 16:34_

Sorry for not having been clear about this in my original proposal. We should write `Autofix is not available.` in the case of `None` because this is a generic `Display` implementation. 

---

_@JonathanPlasse reviewed on 2023-03-20 16:40_

---

_Review comment by @JonathanPlasse on `crates/ruff_diagnostics/src/violation.rs`:14 on 2023-03-20 16:40_

No problem, thank you for your review.

---

_Merged by @MichaReiser on 2023-03-20 16:45_

---

_Closed by @MichaReiser on 2023-03-20 16:45_

---

_Branch deleted on 2023-03-20 16:45_

---
