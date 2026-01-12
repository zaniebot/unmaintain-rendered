```yaml
number: 3589
title: "Avoid removing comment hash for noqa's with trailing content"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/syntax
created_at: 2023-03-17T22:29:38Z
updated_at: 2023-03-18T19:06:23Z
url: https://github.com/astral-sh/ruff/pull/3589
synced_at: 2026-01-12T15:55:13Z
```

# Avoid removing comment hash for noqa's with trailing content

---

_@charliermarsh_

## Summary

Right now, if you have a comment like `# noqa bar`, then when removing the blanket `# noqa`, we leave `bar` behind -- which is a syntax error.

It if the comment that trails the `noqa` is not itself a "partial comment" (like `# noqa # bar`), then we need to shift the deletion to _exclude_ the `#`.

Closes #3586.


---

_Review requested from @konstin by @charliermarsh on 2023-03-17 22:29_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-17 22:29_

---

_Label `bug` added by @charliermarsh on 2023-03-17 22:29_

---

_Comment by @github-actions[bot] on 2023-03-17 22:42_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+18, -0, 0 error(s))

<details><summary>airflow (+6, -0)</summary>
<p>

```diff
+ airflow/providers/google/cloud/operators/dataflow.py:1378:9: PLR1711 [*] Useless `return` statement at end of function
+ airflow/providers/microsoft/azure/sensors/data_factory.py:134:9: PLR1711 [*] Useless `return` statement at end of function
+ airflow/utils/db.py:1475:5: PLR1711 [*] Useless `return` statement at end of function
+ dev/check_files.py:223:5: PLR1711 [*] Useless `return` statement at end of function
+ dev/check_files.py:237:5: PLR1711 [*] Useless `return` statement at end of function
+ tests/jobs/test_scheduler_job.py:1924:17: PLR1711 [*] Useless `return` statement at end of function
```

</p>
</details>
<details><summary>bokeh (+10, -0)</summary>
<p>

```diff
+ src/bokeh/application/application.py:236:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/application/application.py:249:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/client/connection.py:281:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/contexts.py:301:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/contexts.py:321:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/tornado.py:715:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:223:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:262:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:288:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:335:9: PLR1711 [*] Useless `return` statement at end of function
```

</p>
</details>
<details><summary>zulip (+2, -0)</summary>
<p>

```diff
+ zerver/lib/streams.py:243:5: PLR1711 [*] Useless `return` statement at end of function
+ zerver/lib/test_helpers.py:122:9: PLR1711 [*] Useless `return` statement at end of function
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.06ms     2.8 MB/sec    1.02     14.8±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.3 MB/sec    1.01      3.9±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.1±1.65µs     6.6 MB/sec    1.01    452.5±2.66µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.01ms     4.0 MB/sec    1.02      6.6±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.04      8.4±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1756.2±2.61µs     9.5 MB/sec    1.03   1807.5±2.75µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.7±0.82µs    16.0 MB/sec    1.03    189.5±0.29µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.8 MB/sec    1.03      3.9±0.01ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.4±0.08ms     2.6 MB/sec    1.01     15.5±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.8±4.61µs     6.4 MB/sec    1.00    463.6±4.65µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.02ms     3.7 MB/sec    1.01      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.08ms     4.7 MB/sec    1.00      8.7±0.04ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1861.0±20.85µs     8.9 MB/sec    1.00   1858.2±8.29µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.5±1.01µs    15.4 MB/sec    1.01    194.0±2.37µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.03ms     6.4 MB/sec    1.01      4.0±0.04ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/noqa.rs`:164 on 2023-03-18 08:38_

Let's use `skip_while(char::is_whitespace)` or call `lines[row][end...].trim_start()` to handle  `# noqua     # comment` correctly.


Can you also add a test case for it. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/noqa.rs`:173 on 2023-03-18 08:40_

Is my understanding correct that we'll change `#noqa comment` to `# comment`. What's the reasoning behind this choice?



---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/noqa.rs`:164 on 2023-03-18 08:40_

Nit: I would find some comments at the top of each case explaining the case they handle and what fix they produce useful.
```suggestion
                            } 
                            // example: `# noqua # another comment`
                            else if lines[row].chars().nth(end + 1).map_or(false, |c| c == '#') {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/noqa.rs`:263 on 2023-03-18 08:43_

Nit: Is this section a 1:1 copy of the above? Can we extract it in a private-function 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/noqa.rs`:254 on 2023-03-18 08:43_

Handle extra whitespace between the two comments 

---

_@MichaReiser approved on 2023-03-18 08:44_

---

_@charliermarsh reviewed on 2023-03-18 18:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/noqa.rs`:173 on 2023-03-18 18:26_

Yeah this seems wrong -- will change.

---

_@charliermarsh reviewed on 2023-03-18 18:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/noqa.rs`:164 on 2023-03-18 18:26_

Will do, added test.

---

_Merged by @charliermarsh on 2023-03-18 18:48_

---

_Closed by @charliermarsh on 2023-03-18 18:48_

---

_Branch deleted on 2023-03-18 18:48_

---

_Comment by @charliermarsh on 2023-03-18 18:52_

Thanks for such a thorough review, I know this is a tricky area.

---
