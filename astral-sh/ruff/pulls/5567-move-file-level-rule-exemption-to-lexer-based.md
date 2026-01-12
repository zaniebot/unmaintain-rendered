```yaml
number: 5567
title: Move file-level rule exemption to lexer-based approach
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/exemption-parser
created_at: 2023-07-06T18:10:26Z
updated_at: 2023-07-07T16:04:05Z
url: https://github.com/astral-sh/ruff/pull/5567
synced_at: 2026-01-12T03:36:55Z
```

# Move file-level rule exemption to lexer-based approach

---

_Pull request opened by @charliermarsh on 2023-07-06 18:10_

## Summary

In addition to `# noqa` codes, we also support file-level exemptions, which look like:

- `# flake8: noqa` (ignore all rules in the file, for compatibility)
- `# ruff: noqa` (all rules in the file)
- `# ruff: noqa: F401` (ignore `F401` in the file, Flake8 doesn't support this)

This PR moves that logic to something that looks a lot more like our `# noqa` parser. Performance is actually quite a bit _worse_ than the previous approach (lexing `# flake8: noqa` goes from 2ns to 11ns; lexing `# ruff: noqa: F401, F841` is about the same`; lexing `# type: ignore # noqa: E501` fgoes from 4ns to 6ns), but the numbers are very small so it's... maybe worth it?

The primary benefit here is that we now properly support flexible whitespace, like: `#flake8:noqa`. Previously, we required exact string matching, and we also didn't support all case-insensitive variants of `noqa`.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-06 18:10_

---

_Comment by @charliermarsh on 2023-07-06 18:11_

I thought this might end up being faster, but given that it's slower, IDK, open to not merging. I do think handling `#flake8: noqa` (for example) is nice though.

---

_Comment by @github-actions[bot] on 2023-07-06 18:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.03ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1753.3±2.03µs     9.5 MB/sec    1.00   1756.2±3.49µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    197.9±0.85µs    14.9 MB/sec    1.01    199.6±0.52µs    14.8 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.00ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.06     14.5±1.27ms     2.8 MB/sec    1.00     13.6±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.10ms     4.8 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.9±0.47µs     6.7 MB/sec    1.00    437.4±1.19µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.03ms     4.2 MB/sec    1.00      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1481.7±3.54µs    11.2 MB/sec    1.00   1468.8±3.68µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.7±0.21µs    17.4 MB/sec    1.00    170.5±1.10µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.02ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.03ms     5.3 MB/sec    1.00      7.6±0.03ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1592.3±13.92µs    10.5 MB/sec    1.01  1601.8±11.61µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    170.8±1.46µs    17.3 MB/sec    1.01    172.0±3.35µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.01ms     7.2 MB/sec    1.00      3.6±0.02ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.04     13.0±0.15ms     3.1 MB/sec    1.00     12.6±0.04ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.02ms     5.1 MB/sec    1.00      3.2±0.01ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    347.7±3.18µs     8.5 MB/sec    1.00    346.2±6.27µs     8.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.04ms     4.7 MB/sec    1.00      5.5±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.02      6.7±0.10ms     6.0 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1339.7±13.14µs    12.4 MB/sec    1.00  1330.5±20.62µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    147.8±2.21µs    20.0 MB/sec    1.00    145.7±1.00µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.8 MB/sec    1.00      2.9±0.01ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-07 09:29_

Neat. As mentioned in the other PR. An alternative would have been to write a more "traditional" lexer that returns a sequence of Tokens instead (with their ranges). See `SimpleTokenizer` for an example. But this works too.

---

_@konstin reviewed on 2023-07-07 10:10_

---

_Review comment by @konstin on `crates/ruff/src/noqa.rs`:260 on 2023-07-07 10:10_

nit: `ParsedFileExemption` -> `Self`

---

_@konstin reviewed on 2023-07-07 10:16_

---

_Review comment by @konstin on `crates/ruff/src/noqa.rs`:349 on 2023-07-07 10:16_

I'm surprised there isn't a better way to write this, but at least i found the issue for the missing methos https://github.com/rust-lang/rfcs/issues/2566

---

_Comment by @konstin on 2023-07-07 10:17_

How did you compute those ns timings?

---

_@MichaReiser reviewed on 2023-07-07 11:19_

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:349 on 2023-07-07 11:19_

We could match on the byte slice because we only compare with byte characters. But you then still need to do the invariant which is annoying?

```
match line.as_str().bytes() {
	['n' | 'N', 'o' | 'O', 'q' | 'Q',  'a' | 'A'] => tada,
	_ => nope
}
```

---

_@konstin reviewed on 2023-07-07 12:04_

---

_Review comment by @konstin on `crates/ruff/src/noqa.rs`:349 on 2023-07-07 12:04_

the problem is that we want to select the string after `"noqa"` and converting back from bytes isn't safe.

Which reminds me, would `line.strip_prefix("noqa")` work?

---

_@charliermarsh reviewed on 2023-07-07 13:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:349 on 2023-07-07 13:56_

I don't think `line.strip_prefix("noqa")` matches case-insensitively, right?

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:349 on 2023-07-07 13:57_

> the problem is that we want to select the string after "noqa" and converting back from bytes isn't safe.

I think it would be safe here right because we know that all variations of `noqa` have an exact length of 4 bytes

Anyway. I think the current code is fine. it's verbose but does the job

---

_@MichaReiser reviewed on 2023-07-07 13:57_

---

_Comment by @charliermarsh on 2023-07-07 15:30_

> How did you compute those ns timings?

I use `cargo bench` in the `ruff` crate. I add something like:

```rust
use criterion::{black_box, criterion_group, criterion_main, BenchmarkId, Criterion};

use ruff::noqa::ParsedFileExemption;

pub fn directive_benchmark(c: &mut Criterion) {
    let mut group = c.benchmark_group("Directive");
    for i in [
        "# noqa: F401",
        "# noqa: F401, F841",
        "# flake8: noqa: F401, F841",
        "# ruff: noqa: F401, F841",
        "# flake8: noqa",
        "# ruff: noqa",
        "# noqa",
        "# type: ignore # noqa: E501",
        "# type: ignore # nosec",
        "# some very long comment that # is interspersed with characters but # no directive",
    ]
    .iter()
    {
        group.bench_with_input(BenchmarkId::new("Regex", i), i, |b, _i| {
            b.iter(|| ParsedFileExemption::try_regex(black_box(i)))
        });
        group.bench_with_input(BenchmarkId::new("Lexer", i), i, |b, _i| {
            b.iter(|| ParsedFileExemption::try_extract(black_box(i)))
        });
    }
    group.finish();
}

criterion_group!(benches, directive_benchmark);
criterion_main!(benches);
```

In `crates/ruff/benches/benchmark.rs`. Then add:

```toml
[[bench]]
name = "benchmark"
harness = false
```

To `crates/ruff/Cargo.toml`, along with `criterion = "0.5.1".

Then `cargo bench` in `crates/ruff` gives you benchmarks!

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:349 on 2023-07-07 15:33_

I like this, I changed it.

---

_@charliermarsh reviewed on 2023-07-07 15:33_

---

_Merged by @charliermarsh on 2023-07-07 15:41_

---

_Closed by @charliermarsh on 2023-07-07 15:41_

---

_Branch deleted on 2023-07-07 15:41_

---
