```yaml
number: 660
title: Speed up version parsing for a 1.27±0.03 speedup in transformers-extras with conservative changes
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/speedup-version-parsing
created_at: 2023-12-15T12:37:02Z
updated_at: 2023-12-15T19:03:37Z
url: https://github.com/astral-sh/uv/pull/660
synced_at: 2026-01-10T15:44:44Z
```

# Speed up version parsing for a 1.27±0.03 speedup in transformers-extras with conservative changes

---

_Pull request opened by @konstin on 2023-12-15 12:37_

Two low-hanging fruits as optimizations for version parsing: A fast path for release only versions and removing the regex from version specifiers (still calling into version's parsing regex if required). This enables optimizing the serde format since we now see the serde part instead of only PEP 440 parsing. I intentionally didn't rewrite the full PEP 440 at this step.

```console
$ hyperfine --warmup 5 --runs 50 "target/profiling/puffin pip-compile scripts/requirements/transformers-extras.in" "target/profiling/main pip-compile scripts/requirements/transformers-extras.in"
  Benchmark 1: target/profiling/puffin pip-compile scripts/requirements/transformers-extras.in
    Time (mean ± σ):     217.1 ms ±   3.2 ms    [User: 194.0 ms, System: 55.1 ms]
    Range (min … max):   211.0 ms … 228.1 ms    50 runs

  Benchmark 2: target/profiling/main pip-compile scripts/requirements/transformers-extras.in
    Time (mean ± σ):     276.7 ms ±   5.7 ms    [User: 252.4 ms, System: 54.6 ms]
    Range (min … max):   268.9 ms … 303.5 ms    50 runs

  Summary
    target/profiling/puffin pip-compile scripts/requirements/transformers-extras.in ran
      1.27 ± 0.03 times faster than target/profiling/main pip-compile scripts/requirements/transformers-extras.in
```

---

_Review requested from @BurntSushi by @konstin on 2023-12-15 12:37_

---

_Review requested from @charliermarsh by @konstin on 2023-12-15 12:37_

---

_Renamed from "Speed up version parsing for a 1.27±0.03speedup in transformers-extras with conservative changes" to "Speed up version parsing for a 1.27±0.03 speedup in transformers-extras with conservative changes" by @konstin on 2023-12-15 12:37_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:1271 on 2023-12-15 13:28_

Should this be `unreachable!("expected error, but didn't get one")`.

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:697 on 2023-12-15 14:05_

I think this probably treats some version strings as valid that are not valid. For example, `+1.  +2  .+3` is not valid, but this will parse it as `[1, 2, 3]`. I think you can fix this by just ensuring the `number` only consists of ASCII digits.

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:722 on 2023-12-15 14:05_

Same here as above.

---

_@BurntSushi reviewed on 2023-12-15 14:08_

Nice! I think this does make the parser a little more permissive, but it should be an easy fix.

---

_Merged by @BurntSushi on 2023-12-15 19:03_

---

_Closed by @BurntSushi on 2023-12-15 19:03_

---

_Branch deleted on 2023-12-15 19:03_

---
