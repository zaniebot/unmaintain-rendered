```yaml
number: 5187
title: Add rule documentation URL to JSON output
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/code_description
created_at: 2023-06-19T16:35:52Z
updated_at: 2023-06-19T21:09:17Z
url: https://github.com/astral-sh/ruff/pull/5187
synced_at: 2026-01-12T03:43:30Z
```

# Add rule documentation URL to JSON output

---

_Pull request opened by @charliermarsh on 2023-06-19 16:35_

## Summary

I want to include URLs to the rule documentation in the LSP (the LSP has a native `code_description` field for this, which, if specified, causes the source to be rendered as a link to the docs). This PR exposes the URL to the documentation in the Ruff JSON output.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-19 16:35_

---

_Review requested from @konstin by @charliermarsh on 2023-06-19 16:35_

---

_Label `cli` added by @charliermarsh on 2023-06-19 16:35_

---

_Review comment by @konstin on `crates/ruff/src/registry.rs`:355 on 2023-06-19 16:49_

Does it make sense to use `env!("CARGO_PKG_HOMEPAGE")` here to have a single place to change the url? (could also be it's less confusing to have rust not depend on toml metadata)

---

_Review comment by @konstin on `crates/ruff/src/message/json.rs`:66 on 2023-06-19 16:50_

```suggestion
        "description_url": message.kind.rule().url(),
```
i'd add that this is a link

---

_@konstin approved on 2023-06-19 16:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/json.rs`:66 on 2023-06-19 16:58_

Hmm, torn -- I like adhering to the LSP schema.

---

_@charliermarsh reviewed on 2023-06-19 16:58_

---

_@charliermarsh reviewed on 2023-06-19 16:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/json.rs`:66 on 2023-06-19 16:58_

Oh, actually, it should be like this (which hopefully resolves this):

```ts
/**
 * Structure to capture a description for an error code.
 *
 * @since 3.16.0
 */
export interface [CodeDescription](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#codeDescription) {
	/**
	 * An URI to open with more information about the diagnostic error.
	 */
	href: [URI](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#uri);
}
```

---

_@konstin reviewed on 2023-06-19 17:00_

---

_Review comment by @konstin on `crates/ruff/src/message/json.rs`:66 on 2023-06-19 17:00_

oh sorry didn't realize there was a schema for that

---

_Comment by @github-actions[bot] on 2023-06-19 17:11_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.2±0.01ms     6.5 MB/sec    1.00      6.2±0.01ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   1307.7±4.95µs    12.7 MB/sec    1.00  1309.8±16.94µs    12.7 MB/sec
formatter/numpy/globals.py                 1.01    127.3±0.27µs    23.2 MB/sec    1.00    126.7±1.21µs    23.3 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.01ms    10.0 MB/sec    1.00      2.5±0.01ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.05ms     3.1 MB/sec    1.00     13.2±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.3±0.86µs     7.2 MB/sec    1.00    413.1±1.68µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.01ms     4.5 MB/sec    1.00      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1435.3±2.44µs    11.6 MB/sec    1.01   1451.8±3.34µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.2±1.64µs    18.5 MB/sec    1.03    163.3±0.94µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.7±0.38ms     3.8 MB/sec    1.01     10.8±0.54ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.07ms     7.9 MB/sec    1.02      2.1±0.10ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   213.7±13.63µs    13.8 MB/sec    1.01   215.4±25.23µs    13.7 MB/sec
formatter/pydantic/types.py                1.06      4.3±0.19ms     5.9 MB/sec    1.00      4.1±0.24ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.04     23.6±1.05ms  1768.7 KB/sec    1.00     22.6±0.86ms  1842.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.39ms     3.1 MB/sec    1.06      5.6±0.22ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   649.5±35.64µs     4.5 MB/sec    1.01   657.1±34.17µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.59ms     2.7 MB/sec    1.03      9.6±0.41ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     11.4±0.56ms     3.6 MB/sec    1.03     11.8±0.50ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.11ms     7.2 MB/sec    1.03      2.4±0.08ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   266.9±10.47µs    11.1 MB/sec    1.03   276.1±11.51µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.1±0.31ms     5.0 MB/sec    1.00      5.1±0.21ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add URL to JSON output via `code_description`" to "Add rule documentation URL to JSON output" by @charliermarsh on 2023-06-19 20:56_

---

_@charliermarsh reviewed on 2023-06-19 20:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/json.rs`:66 on 2023-06-19 20:57_

Okay, you were right, changed my mind after Micha _also_ brought this up.

---

_@charliermarsh reviewed on 2023-06-19 20:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/json.rs`:66 on 2023-06-19 20:57_

Apparently Mitte calls this `url` so running with that.

---

_@MichaReiser approved on 2023-06-19 20:57_

---

_Merged by @charliermarsh on 2023-06-19 21:09_

---

_Closed by @charliermarsh on 2023-06-19 21:09_

---

_Branch deleted on 2023-06-19 21:09_

---
