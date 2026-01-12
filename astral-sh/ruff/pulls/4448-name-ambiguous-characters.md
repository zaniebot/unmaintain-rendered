```yaml
number: 4448
title: Name ambiguous characters
type: pull_request
state: merged
author: covracer
labels: []
assignees: []
merged: true
base: main
head: name-confusables
created_at: 2023-05-16T09:50:35Z
updated_at: 2023-05-22T17:36:25Z
url: https://github.com/astral-sh/ruff/pull/4448
synced_at: 2026-01-12T15:55:15Z
```

# Name ambiguous characters

---

_@covracer_

Name the ambiguous characters in prompts, to help clarify why one should be used over the other.

---

_Comment by @github-actions[bot] on 2023-05-16 10:02_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+53, -53, 0 error(s))

<details><summary>airflow (+23, -23)</summary>
<p>

```diff
+ airflow/providers/google/cloud/hooks/bigquery.py:256:49: RUF002 [*] Docstring contains ambiguous `–` (EN DASH). Did you mean `-` (HYPHEN-MINUS)?
- airflow/providers/google/cloud/hooks/bigquery.py:256:49: RUF002 [*] Docstring contains ambiguous unicode character `–` (did you mean `-`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:47:21: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:47:21: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:47:22: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:47:22: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:47:27: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:47:27: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:47:28: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:47:28: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:64:21: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:64:21: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:64:22: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:64:22: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:64:27: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:64:27: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:64:28: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:64:28: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:21: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:21: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:22: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:22: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:27: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:27: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:28: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:28: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:43: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:43: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:44: RUF001 [*] String contains ambiguous `ˆ` (MODIFIER LETTER CIRCUMFLEX ACCENT). Did you mean `^` (CIRCUMFLEX ACCENT)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:44: RUF001 [*] String contains ambiguous unicode character `ˆ` (did you mean `^`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:49: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:49: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/kubernetes/test_kubernetes_helper_functions.py:81:50: RUF001 [*] String contains ambiguous `˜` (SMALL TILDE). Did you mean `~` (TILDE)?
- tests/kubernetes/test_kubernetes_helper_functions.py:81:50: RUF001 [*] String contains ambiguous unicode character `˜` (did you mean `~`?)
+ tests/serialization/test_dag_serialization.py:1679:21: RUF001 [*] String contains ambiguous `╱` (BOX DRAWINGS LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT). Did you mean `/` (SOLIDUS)?
- tests/serialization/test_dag_serialization.py:1679:21: RUF001 [*] String contains ambiguous unicode character `╱` (did you mean `/`?)
+ tests/serialization/test_dag_serialization.py:1680:19: RUF001 [*] String contains ambiguous `╱` (BOX DRAWINGS LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT). Did you mean `/` (SOLIDUS)?
- tests/serialization/test_dag_serialization.py:1680:19: RUF001 [*] String contains ambiguous unicode character `╱` (did you mean `/`?)
+ tests/serialization/test_dag_serialization.py:1683:26: RUF001 [*] String contains ambiguous `╱` (BOX DRAWINGS LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT). Did you mean `/` (SOLIDUS)?
- tests/serialization/test_dag_serialization.py:1683:26: RUF001 [*] String contains ambiguous unicode character `╱` (did you mean `/`?)
+ tests/serialization/test_dag_serialization.py:1686:19: RUF001 [*] String contains ambiguous `╱` (BOX DRAWINGS LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT). Did you mean `/` (SOLIDUS)?
- tests/serialization/test_dag_serialization.py:1686:19: RUF001 [*] String contains ambiguous unicode character `╱` (did you mean `/`?)
+ tests/serialization/test_dag_serialization.py:1689:27: RUF001 [*] String contains ambiguous `╱` (BOX DRAWINGS LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT). Did you mean `/` (SOLIDUS)?
- tests/serialization/test_dag_serialization.py:1689:27: RUF001 [*] String contains ambiguous unicode character `╱` (did you mean `/`?)
+ tests/serialization/test_dag_serialization.py:1690:25: RUF001 [*] String contains ambiguous `╱` (BOX DRAWINGS LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT). Did you mean `/` (SOLIDUS)?
- tests/serialization/test_dag_serialization.py:1690:25: RUF001 [*] String contains ambiguous unicode character `╱` (did you mean `/`?)
```

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

```diff
+ examples/plotting/histogram.py:44:43: RUF001 [*] String contains ambiguous `σ` (GREEK SMALL LETTER SIGMA). Did you mean `o` (LATIN SMALL LETTER O)?
- examples/plotting/histogram.py:44:43: RUF001 [*] String contains ambiguous unicode character `σ` (did you mean `o`?)
+ examples/plotting/histogram.py:57:47: RUF001 [*] String contains ambiguous `σ` (GREEK SMALL LETTER SIGMA). Did you mean `o` (LATIN SMALL LETTER O)?
- examples/plotting/histogram.py:57:47: RUF001 [*] String contains ambiguous unicode character `σ` (did you mean `o`?)
```

</p>
</details>
<details><summary>zulip (+28, -28)</summary>
<p>

```diff
+ docs/conf.py:23:18: RUF001 [*] String contains ambiguous `–` (EN DASH). Did you mean `-` (HYPHEN-MINUS)?
- docs/conf.py:23:18: RUF001 [*] String contains ambiguous unicode character `–` (did you mean `-`?)
+ docs/conf.py:23:43: RUF001 [*] String contains ambiguous `–` (EN DASH). Did you mean `-` (HYPHEN-MINUS)?
- docs/conf.py:23:43: RUF001 [*] String contains ambiguous unicode character `–` (did you mean `-`?)
+ scripts/lib/zulip_tools.py:49:22: RUF003 [*] Comment contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- scripts/lib/zulip_tools.py:49:22: RUF003 [*] Comment contains ambiguous unicode character `’` (did you mean ```?)
+ tools/lib/provision.py:449:68: RUF003 [*] Comment contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- tools/lib/provision.py:449:68: RUF003 [*] Comment contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/lib/onboarding.py:239:23: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/lib/onboarding.py:239:23: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/lib/rate_limiter.py:448:51: RUF003 [*] Comment contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/lib/rate_limiter.py:448:51: RUF003 [*] Comment contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/tests/test_i18n.py:103:33: RUF001 [*] String contains ambiguous `с` (CYRILLIC SMALL LETTER ES). Did you mean `c` (LATIN SMALL LETTER C)?
- zerver/tests/test_i18n.py:103:33: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
+ zerver/tests/test_i18n.py:103:34: RUF001 [*] String contains ambiguous `е` (CYRILLIC SMALL LETTER IE). Did you mean `e` (LATIN SMALL LETTER E)?
- zerver/tests/test_i18n.py:103:34: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
+ zerver/tests/test_i18n.py:115:33: RUF001 [*] String contains ambiguous `с` (CYRILLIC SMALL LETTER ES). Did you mean `c` (LATIN SMALL LETTER C)?
- zerver/tests/test_i18n.py:115:33: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
+ zerver/tests/test_i18n.py:115:34: RUF001 [*] String contains ambiguous `е` (CYRILLIC SMALL LETTER IE). Did you mean `e` (LATIN SMALL LETTER E)?
- zerver/tests/test_i18n.py:115:34: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
+ zerver/tests/test_i18n.py:131:33: RUF001 [*] String contains ambiguous `с` (CYRILLIC SMALL LETTER ES). Did you mean `c` (LATIN SMALL LETTER C)?
- zerver/tests/test_i18n.py:131:33: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
+ zerver/tests/test_i18n.py:131:34: RUF001 [*] String contains ambiguous `е` (CYRILLIC SMALL LETTER IE). Did you mean `e` (LATIN SMALL LETTER E)?
- zerver/tests/test_i18n.py:131:34: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
+ zerver/tests/test_tutorial.py:168:19: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/tests/test_tutorial.py:168:19: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/views/video_calls.py:73:38: RUF003 [*] Comment contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/views/video_calls.py:73:38: RUF003 [*] Comment contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/jira/tests.py:220:154: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/jira/tests.py:220:154: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/jira/tests.py:220:176: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/jira/tests.py:220:176: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/jira/tests.py:225:141: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/jira/tests.py:225:141: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/jira/tests.py:225:163: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/jira/tests.py:225:163: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/jira/tests.py:230:185: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/jira/tests.py:230:185: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/jira/tests.py:235:187: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/jira/tests.py:235:187: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:105:6: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:105:6: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:109:13: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:109:13: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:130:11: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:130:11: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:136:6: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:136:6: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:140:13: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:140:13: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:75:8: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:75:8: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/lidarr/tests.py:99:11: RUF001 [*] String contains ambiguous `’` (RIGHT SINGLE QUOTATION MARK). Did you mean ``` (GRAVE ACCENT)?
- zerver/webhooks/lidarr/tests.py:99:11: RUF001 [*] String contains ambiguous unicode character `’` (did you mean ```?)
+ zerver/webhooks/slack_incoming/tests.py:155:1: RUF001 [*] String contains ambiguous `➕` (HEAVY PLUS SIGN). Did you mean `+` (PLUS SIGN)?
- zerver/webhooks/slack_incoming/tests.py:155:1: RUF001 [*] String contains ambiguous unicode character `➕` (did you mean `+`?)
```

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF001 | 96 | 48 | 48 |
| RUF003 | 8 | 4 | 4 |
| RUF002 | 2 | 1 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.15ms     2.8 MB/sec    1.02     14.7±0.13ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.8 MB/sec    1.01      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.1±0.63µs     7.0 MB/sec    1.01    423.6±2.37µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.02      6.0±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.05      7.0±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1449.7±2.23µs    11.5 MB/sec    1.03   1490.6±5.32µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.2±0.25µs    18.4 MB/sec    1.02    164.1±0.24µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.04      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.01      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1022.3±3.49µs    16.3 MB/sec    1.00   1026.6±0.41µs    16.2 MB/sec
parser/numpy/globals.py                    1.00    104.5±0.18µs    28.2 MB/sec    1.00    105.0±0.24µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.3±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.6±0.10ms     2.4 MB/sec    1.00     16.4±0.15ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.9±4.85µs     6.8 MB/sec    1.00    433.6±4.22µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     4.9 MB/sec    1.00      8.2±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1700.9±18.85µs     9.8 MB/sec    1.01  1719.0±13.31µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.6±1.54µs    16.3 MB/sec    1.00    182.4±3.11µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.8 MB/sec    1.00      3.7±0.02ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.04      6.9±0.03ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1266.7±5.86µs    13.1 MB/sec    1.04   1312.2±9.75µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    131.4±0.84µs    22.5 MB/sec    1.03    135.6±0.74µs    21.8 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.02      2.9±0.01ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-05-16 10:37_

Thank you. By how much does this increase the ruff release binary size?

---

_@youknowone reviewed on 2023-05-16 16:43_

---

_Review comment by @youknowone on `crates/ruff/Cargo.toml`:73 on 2023-05-16 16:43_

Since RustPython is using custom version of this crate, using this will be helpful to reduce depdendencies
```suggestion
unicode_names2 = { version = "0.6.0", git = "https://github.com/youknowone/unicode_names2.git", rev = "4ce16aa85cbcdd9cc830410f1a72ef9a235f2fde" }
```

---

_Comment by @youknowone on 2023-05-16 16:45_

@MichaReiser It will not increase binary size by using same crate with RustPython. This is an essential crate to parse unicode character by name.

---

_@JonathanPlasse reviewed on 2023-05-16 17:01_

---

_Review comment by @JonathanPlasse on `crates/ruff/Cargo.toml`:73 on 2023-05-16 17:01_

How can it be kept up to date to avoid regression of binary size?

---

_Review comment by @covracer on `crates/ruff/Cargo.toml`:73 on 2023-05-17 11:40_

@youknowone do you know how to resolve the pre-commit failure?

https://github.com/charliermarsh/ruff/actions/runs/4994650061/jobs/8945512983?pr=4448#step:8:71

---

_@covracer reviewed on 2023-05-17 11:40_

---

_@MichaReiser reviewed on 2023-05-17 11:50_

---

_Review comment by @MichaReiser on `crates/ruff/Cargo.toml`:73 on 2023-05-17 11:50_

We could use clippy's [cargo lint](https://rust-lang.github.io/rust-clippy/master/index.html#multiple_crate_versions) but it would require us to first solve all existing duplicate dependencies, which may not be possible. 

```
 cargo tree -d
```

---

_@youknowone reviewed on 2023-05-17 15:24_

---

_Review comment by @youknowone on `crates/ruff/Cargo.toml`:73 on 2023-05-17 15:24_

That's because only `Cargo.toml` is updated but `Cargo.lock` is not.
Pull the changes to local, and build once. Then you will see the updated Cargo.lock. Commiting it will fix the build.

---

_@charliermarsh reviewed on 2023-05-22 14:34_

---

_Review comment by @charliermarsh on `crates/ruff/Cargo.toml`:73 on 2023-05-22 14:34_

@MichaReiser - Are you cool with merging this as-is or would you want to get cargo lint in place?

---

_@MichaReiser reviewed on 2023-05-22 16:06_

---

_Review comment by @MichaReiser on `crates/ruff/Cargo.toml`:73 on 2023-05-22 16:06_

I'm okay with merging as is. I don't think that setting up `cargo lint` is trivial. 

Another (non trivial) idea would be to build our binary in CI in release mode (nightly?) and issue a warning if the binary size increased significantly. 

---

_@zanieb reviewed on 2023-05-22 16:10_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__confusables.snap`:10 on 2023-05-22 16:10_

Thoughts on tweaking how this is displayed?

```suggestion
  = help: Replace `𝐁` (MATHEMATICAL BOLD CAPITAL B) with `B` (LATIN CAPITAL LETTER B)
```

---

_Merged by @charliermarsh on 2023-05-22 17:16_

---

_Closed by @charliermarsh on 2023-05-22 17:16_

---
