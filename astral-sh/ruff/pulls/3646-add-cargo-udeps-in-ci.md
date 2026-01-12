```yaml
number: 3646
title: Add cargo-udeps in CI
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: add-cargo-udeps-in-ci
created_at: 2023-03-21T13:00:42Z
updated_at: 2023-03-22T16:56:49Z
url: https://github.com/astral-sh/ruff/pull/3646
synced_at: 2026-01-12T15:55:13Z
```

# Add cargo-udeps in CI

---

_@JonathanPlasse_

- Close #3643

---

_Comment by @github-actions[bot] on 2023-03-21 13:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.03     16.8±0.08ms     2.4 MB/sec     1.00     16.4±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.02ms     3.9 MB/sec     1.00      4.3±0.01ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    587.9±1.26µs     5.0 MB/sec     1.01    590.9±2.12µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.4±0.05ms     3.5 MB/sec     1.00      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.8±0.03ms     4.6 MB/sec     1.00      8.7±0.02ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1974.1±117.34µs     8.4 MB/sec    1.00   1954.8±2.42µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.6±0.52µs    13.8 MB/sec     1.03    219.4±0.68µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.01ms     6.2 MB/sec     1.00      4.1±0.01ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     17.9±0.88ms     2.3 MB/sec    1.00     16.9±0.56ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.7±0.28ms     3.5 MB/sec    1.00      4.7±0.22ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.05   644.9±38.36µs     4.6 MB/sec    1.00   612.5±37.68µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.9±0.34ms     3.2 MB/sec    1.00      7.6±0.38ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      9.8±0.46ms     4.1 MB/sec    1.00      9.6±0.48ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.09ms     7.8 MB/sec    1.00      2.1±0.11ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   243.6±19.81µs    12.1 MB/sec    1.00   238.3±18.62µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.17ms     5.7 MB/sec    1.00      4.4±0.23ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:189 on 2023-03-21 13:57_

Nit: Do you think it would be useful to output the result to [the job summary](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#adding-a-job-summary) like this

```suggestion
        run: cargo +nightly udeps | tee $GITHUB_STEP_SUMMARY
```

---

_@MichaReiser reviewed on 2023-03-21 13:59_

Oh wow... I didn't expect this to be picked up and be implemented so quickly! Thank you.

Can you add a test plan to your PR summary that shows that the job fails if there's an unused dependency (you can push a change to this PR and then add a link to the action-run to PR summary).

---

_Converted to draft by @JonathanPlasse on 2023-03-21 17:05_

---

_Comment by @JonathanPlasse on 2023-03-21 19:11_

[Successful `cargo-udeps` CI](https://github.com/charliermarsh/ruff/actions/runs/4482793688?pr=3646#summary-12171428582)
> No unused dependencies found

[Failed `cargo-udeps` CI](https://github.com/charliermarsh/ruff/actions/runs/4482452843#summary-12170358105)
> Unused dependencies found
> ```console
> `ruff_diagnostics v0.0.0 (/home/runner/work/ruff/ruff/crates/ruff_diagnostics)`
> └─── dependencies
>      └─── "clap"
> `ruff_formatter v0.0.0 (/home/runner/work/ruff/ruff/crates/ruff_formatter)`
> └─── dev-dependencies
>      └─── "insta"
> `ruff_python_formatter v0.0.0 (/home/runner/work/ruff/ruff/crates/ruff_python_formatter)`
> └─── dev-dependencies
>      └─── "test-case"
> `ruff_wasm v0.0.0 (/home/runner/work/ruff/ruff/crates/ruff_wasm)`
> └─── dev-dependencies
>      └─── "wasm-bindgen-test"
> Note: They might be false-positive.
>       For example, `cargo-udeps` cannot detect usage of crates that are only used in doc-tests.
>       To ignore some dependencies, write `package.metadata.cargo-udeps.ignore` in Cargo.toml.
> ```

---

_Marked ready for review by @JonathanPlasse on 2023-03-21 19:11_

---

_Review requested from @MichaReiser by @JonathanPlasse on 2023-03-21 19:17_

---

_Review comment by @MichaReiser on `.github/workflows/pr-comment.yaml`:57 on 2023-03-22 07:52_

The ecosystem and benchmark jobs use the comment action because they don't automatically fail when there's a regression. The assessment is left to the user. Using a comment also has the downside that it creates noise because GitHub sends a notification every time the comment is created or updated. 

The fact the udeps step has a clear failure/pass condition and the added noise of the comment are the reasons why I would prefer to keep the job similar to the clippy/test/fmt jobs, where it fails (this also generates a notification depending on your settings) without creating/updating an additional comment. 



---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:197 on 2023-03-22 07:53_

I only learned yesterday that `$GITHUB_STEP_SUMMARY` is a file. Meaning, we could directly pipe to the file and upload that file instead of creating another temporary file.

---

_@MichaReiser reviewed on 2023-03-22 07:55_

Nice polished summary! I'm unsure about adding the PR result to the comment action because of the added complexity, and that I don't think it is necessary. My inline comment goes into details why. What was your motivation when you added it?

---

_Comment by @JonathanPlasse on 2023-03-22 08:50_

> Nice polished summary! I'm unsure about adding the PR result to the comment action because of the added complexity, and that I don't think it is necessary. My inline comment goes into details why. What was your motivation when you added it?

It was to make the cargo-udeps summary more visible.
But, as you said, it would cause noise and it should not happen very often.

---

_Merged by @MichaReiser on 2023-03-22 14:53_

---

_Closed by @MichaReiser on 2023-03-22 14:53_

---

_Branch deleted on 2023-03-22 16:56_

---
