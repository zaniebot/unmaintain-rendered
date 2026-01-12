```yaml
number: 16178
title: Warn on invalid noqa even when there are no diagnostics
type: pull_request
state: merged
author: dylwil3
labels:
  - cli
  - suppression
assignees: []
merged: true
base: main
head: invalid-noqa-always-warn
created_at: 2025-02-15T17:22:15Z
updated_at: 2025-02-16T19:58:18Z
url: https://github.com/astral-sh/ruff/pull/16178
synced_at: 2026-01-12T15:55:53Z
```

# Warn on invalid noqa even when there are no diagnostics

---

_@dylwil3_

On `main` we warn the user if there is an invalid noqa comment[^1] and at least one of the following holds:

- There is at least one diagnostic
- A lint rule related to `noqa`s is enabled (e.g. `RUF100`)

This is probably strange behavior from the point of view of the user, so we now show invalid `noqa`s even when there are no diagnostics.

Closes #12831

[^1]: For the current definition of "invalid noqa comment", which may be expanded in #12811 . This PR is independent of loc. cit. in the sense that the CLI warnings should be consistent, regardless of which `noqa` comments are considered invalid.


---

_Label `cli` added by @dylwil3 on 2025-02-15 17:22_

---

_Label `suppression` added by @dylwil3 on 2025-02-15 17:22_

---

_Comment by @github-actions[bot] on 2025-02-15 17:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-02-15 20:21_

It seems @charliermarsh introduced the check for performance reasons in https://github.com/astral-sh/ruff/pull/1898

Can you tell me a bit more: Is it possible that `check_noqa` creates warnings even if there are no diagnostics? Isn't it just iterating over the diagnostics? Or are those warnings emitted by the noqa parsings? Skipping the empty diagnostics does seem reasonable.

I'm not sure if we should skip the check if noqa is disabled because it gets initialized by the `--ignore-noqa` cli setting and it is documented as

```
    --ignore-noqa
          Ignore any `# noqa` comments
```

Showing warning in this case seems not in the spirit of this flag. Unless I'm misunderstanding something. 

---

_@MichaReiser approved on 2025-02-15 20:25_

Oh shoot. I thought you removed the entire check but you didn't. I think this looks correct. Assuming that those warnings are emitted as part of the noqa parsing logic

---

_Comment by @dylwil3 on 2025-02-15 22:36_

Yep they are emitted as part of the parsing of `noqa` comments implicit in `check_noqa`. 

I think the UX is worth the potential perf hit (I also think the situation you'd have to be in for the perf hit to be noticeable seems somewhat rare - something like a cold start on a large codebase with _no lint errors_ and lots of comments?) But I can try to run some kind of benchmark tomorrow to double check.

---

_Comment by @dylwil3 on 2025-02-16 19:57_

This seems okay to me... and again, I think it's a relatively rare situation. After deleting files with syntax errors, I found a code that had no diagnostics (`YTT101`) and ran the below benchmark.

(Also, the absolute numbers below are probably misleading since I didn't do any work to make sure my CPU was idle etc. So it's only the relative difference that's of interest).

```shell
❯ hyperfine --warmup 10 \
  "./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated --no-cache -e" "ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated --no-cache -e"
Benchmark 1: ./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated --no-cache -e
  Time (mean ± σ):      78.9 ms ±   1.6 ms    [User: 648.6 ms, System: 89.3 ms]
  Range (min … max):    76.3 ms …  82.9 ms    36 runs

Benchmark 2: ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated --no-cache -e
  Time (mean ± σ):      78.5 ms ±   1.5 ms    [User: 644.1 ms, System: 87.9 ms]
  Range (min … max):    75.9 ms …  82.3 ms    37 runs

Summary
  ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated --no-cache -e ran
    1.00 ± 0.03 times faster than ./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated --no-cache -e
```

And with a warm cache it's even more of a non-issue:

```shell
❯ hyperfine --warmup 10 \
  "./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated -e" "ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated -e"
Benchmark 1: ./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated -e
  Time (mean ± σ):      15.3 ms ±   1.0 ms    [User: 21.4 ms, System: 46.2 ms]
  Range (min … max):    13.3 ms …  18.1 ms    167 runs

Benchmark 2: ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated -e
  Time (mean ± σ):      15.1 ms ±   0.9 ms    [User: 21.8 ms, System: 46.2 ms]
  Range (min … max):    13.0 ms …  18.0 ms    167 runs

Summary
  ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated -e ran
    1.01 ± 0.09 times faster than ./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ --select YTT101 --isolated -e
```

---

_Merged by @dylwil3 on 2025-02-16 19:58_

---

_Closed by @dylwil3 on 2025-02-16 19:58_

---
