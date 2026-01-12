```yaml
number: 4504
title: Implement S609, linux_commands_wildcard_injection
type: pull_request
state: merged
author: scop
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/bandit-wildcard-injection
created_at: 2023-05-18T19:24:29Z
updated_at: 2023-06-02T18:33:34Z
url: https://github.com/astral-sh/ruff/pull/4504
synced_at: 2026-01-12T03:50:03Z
```

# Implement S609, linux_commands_wildcard_injection

---

_Pull request opened by @scop on 2023-05-18 19:24_

_No description provided._

---

_@scop reviewed on 2023-05-18 19:27_

---

_Review comment by @scop on `crates/ruff/src/checkers/ast/mod.rs`:2738 on 2023-05-18 19:27_

"linux" in [bandit's`linux_commands_wildcard_injection` plugin name](https://bandit.readthedocs.io/en/latest/plugins/b609_linux_commands_wildcard_injection.html#module-bandit.plugins.injection_wildcard) is a misnomer as this applies to other unixy os's too. Therefore I chose to "fix" this in the name, but I don't know how truthfully we should follow upstream names here.

---

_Comment by @github-actions[bot] on 2023-05-18 20:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.6±0.20ms     2.5 MB/sec    1.00     16.4±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.01      4.1±0.09ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.9±4.68µs     5.9 MB/sec    1.00    504.4±3.41µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.03ms     3.6 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1790.9±10.85µs     9.3 MB/sec    1.00  1780.2±20.52µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±3.66µs    14.8 MB/sec    1.02    204.4±2.28µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.01      3.7±0.02ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.1±0.05ms     6.6 MB/sec    1.02      6.3±0.06ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1197.8±10.63µs    13.9 MB/sec    1.02  1226.4±11.02µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    124.8±2.43µs    23.7 MB/sec    1.04    129.8±0.26µs    22.7 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.02ms     9.7 MB/sec    1.01      2.7±0.03ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     22.4±0.71ms  1863.8 KB/sec    1.00     21.9±0.59ms  1905.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.5±0.19ms     3.0 MB/sec    1.00      5.4±0.19ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   640.7±25.68µs     4.6 MB/sec    1.00   636.3±23.82µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.4±0.34ms     2.7 MB/sec    1.00      9.2±0.37ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.9±0.28ms     3.7 MB/sec    1.00     10.8±0.34ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.08ms     7.2 MB/sec    1.00      2.3±0.11ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   262.5±16.26µs    11.2 MB/sec    1.00   263.0±18.49µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.16ms     5.2 MB/sec    1.01      4.9±0.99ms     5.2 MB/sec
parser/large/dataset.py                    1.00      8.1±0.21ms     5.0 MB/sec    1.00      8.1±0.18ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1524.8±51.98µs    10.9 MB/sec    1.00  1519.1±139.60µs    11.0 MB/sec
parser/numpy/globals.py                    1.00    154.0±7.26µs    19.2 MB/sec    1.00    153.8±7.34µs    19.2 MB/sec
parser/pydantic/types.py                   1.02      3.5±0.19ms     7.3 MB/sec    1.00      3.4±0.12ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:321 on 2023-05-19 07:19_

We can remove this check here because of the `if let Some(..)` on line 324
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:330 on 2023-05-19 07:20_

Nit:
```suggestion
                    .map(|elt| string_literal(elt).unwrap_or_default())
                    .join(" "),
                _ => String::from(string_literal(cmd).unwrap_or_default()),
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/snapshots/ruff__rules__flake8_bandit__tests__S609_S609.py.snap`:4 on 2023-05-19 07:22_

I assume you copied this message from the upstream plugin? I struggle to understand the message. Which part of my code is triggering the `wildcard` injection? Unfortunately, I also don't know how to improve the message much without our Diagnostic's supporting an `advice` that is shown to explain the rule in more detail.

---

_@MichaReiser reviewed on 2023-05-19 07:23_

---

_@scop reviewed on 2023-05-19 08:41_

---

_Review comment by @scop on `crates/ruff/src/rules/flake8_bandit/snapshots/ruff__rules__flake8_bandit__tests__S609_S609.py.snap`:4 on 2023-05-19 08:41_

Yes, the message comes from Bandit: https://bandit.readthedocs.io/en/latest/plugins/b609_linux_commands_wildcard_injection.html

I did not bother to include the called function name in the message, but I don't think doing so would make it any clearer. Anyway I don't think it's a particularly good one either. Something like "Potentially dangerous use of wildcard in shell call" could be better, but then again I'm not sure how close to upstream Ruff wants to stick.

---

_Comment by @scop on 2023-05-25 08:19_

Resolved conflicts.

---

_Label `rule` added by @charliermarsh on 2023-06-02 18:10_

---

_Merged by @charliermarsh on 2023-06-02 18:19_

---

_Closed by @charliermarsh on 2023-06-02 18:19_

---

_Branch deleted on 2023-06-02 18:26_

---
