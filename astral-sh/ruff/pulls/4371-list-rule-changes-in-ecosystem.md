```yaml
number: 4371
title: List rule changes in ecosystem
type: pull_request
state: merged
author: calumy
labels: []
assignees: []
merged: true
base: main
head: list-rule-change-count
created_at: 2023-05-11T11:14:15Z
updated_at: 2023-05-11T14:46:44Z
url: https://github.com/astral-sh/ruff/pull/4371
synced_at: 2026-01-12T03:56:39Z
```

# List rule changes in ecosystem

---

_Pull request opened by @calumy on 2023-05-11 11:14_

Currently, the ecosystem check lists all changes between two versions of ruff as a diff. This PR counts the changes for each rule so the impact of the change can be determined. 

A use case where this could be useful is when adding a set of new rules to determine if one rule will cause a particularly large number of new rule violations to be raised. For example, the addition of the new flake8-todo rules in #3921 raised [a discussion ](https://github.com/charliermarsh/ruff/pull/3921#discussion_r1190257550)about the impact of the rule TDO003 which checks that TODOs have links present.

## Example output

When comparing the current build to v0.0.264 the example output would be.

```bash
scripts/check_ecosystem.py env/bin/ruff target/release/ruff
```

ℹ️ ecosystem check **detected changes**. (+12, -0, 0 error(s))

<details><summary>typeshed (+12, -0)</summary>
<p>

```diff
+ stdlib/typing.pyi:735:1: PYI042 Type alias `_get_type_hints_obj_allowed_types` should be CamelCase
+ stubs/cffi/_cffi_backend.pyi:113:5: PYI042 Type alias `buffer` should be CamelCase
+ stubs/cffi/cffi/api.pyi:13:1: PYI042 Type alias `basestring` should be CamelCase
+ stubs/cffi/cffi/api.pyi:18:5: PYI042 Type alias `buffer` should be CamelCase
+ stubs/pika/pika/spec.pyi:9:1: PYI042 Type alias `_str` should be CamelCase
+ stubs/pywin32/pythoncom.pyi:10:1: PYI042 Type alias `error` should be CamelCase
+ stubs/pywin32/win32/odbc.pyi:9:1: PYI042 Type alias `_odbcError` should be CamelCase
+ stubs/pywin32/win32comext/adsi/adsi.pyi:7:1: PYI042 Type alias `error` should be CamelCase
+ stubs/pywin32/win32comext/propsys/propsys.pyi:6:1: PYI042 Type alias `error` should be CamelCase
+ stubs/pywin32/win32comext/shell/shell.pyi:7:1: PYI042 Type alias `error` should be CamelCase
+ stubs/redis/redis/typing.pyi:17:1: PYI043 Private type alias `_StringLikeT` should not be suffixed with `T` (the `T` suffix implies that an object is a `TypeVar`)
+ stubs/requests/requests/compat.pyi:22:1: PYI042 Type alias `builtin_str` should be CamelCase
```

</p>
</details>
Rules changed: 2

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI042 | 11 | 11 | 0 |
| PYI043 | 1 | 1 | 0 |

---

_Comment by @github-actions[bot] on 2023-05-11 11:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.53ms     2.4 MB/sec    1.05     17.8±0.88ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     4.0 MB/sec    1.05      4.4±0.24ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.4±20.18µs     5.6 MB/sec    1.00   531.1±20.93µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.33ms     3.4 MB/sec    1.00      7.4±0.27ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.27ms     4.8 MB/sec    1.01      8.5±0.73ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1786.8±73.32µs     9.3 MB/sec    1.00  1767.7±77.25µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.6±6.80µs    13.7 MB/sec    1.03   221.7±14.37µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.13ms     6.8 MB/sec    1.01      3.8±0.16ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.8±0.43ms     6.0 MB/sec    1.01      6.8±0.24ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1289.6±64.73µs    12.9 MB/sec    1.07  1379.5±55.78µs    12.1 MB/sec
parser/numpy/globals.py                    1.00    132.8±9.22µs    22.2 MB/sec    1.04    137.7±5.14µs    21.4 MB/sec
parser/pydantic/types.py                   1.02      3.0±0.25ms     8.5 MB/sec    1.00      2.9±0.11ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.35ms     2.4 MB/sec    1.01     16.9±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.06ms     3.8 MB/sec    1.00      4.3±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    443.4±7.65µs     6.7 MB/sec    1.00    439.2±4.41µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.13ms     3.5 MB/sec    1.00      7.2±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.14ms     4.8 MB/sec    1.01      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1795.7±45.98µs     9.3 MB/sec    1.00  1790.4±23.56µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.3±2.17µs    15.4 MB/sec    1.00    191.5±3.25µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.8±0.07ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.8±0.04ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1290.0±12.52µs    12.9 MB/sec    1.00   1294.2±8.33µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.4±1.16µs    22.5 MB/sec    1.02    134.0±1.26µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.01      2.9±0.04ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:303 on 2023-05-11 12:01_

just finding the first match is sufficient here

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:310 on 2023-05-11 12:02_

i'd use a [match group](https://docs.python.org/3/howto/regex.html#grouping) here

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:326 on 2023-05-11 12:08_

i'd only print this box if `rule_changes` is non-empty to shorten the comment a bit

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:331 on 2023-05-11 12:09_

you can skip the `dict().items()` part and just iterated over the sorted tuples

---

_@konstin reviewed on 2023-05-11 12:10_

This is a good addition, i've left some code comments

---

_Review comment by @calumy on `scripts/check_ecosystem.py`:326 on 2023-05-11 12:38_

Addressed in 5400a18

---

_@calumy reviewed on 2023-05-11 12:38_

---

_Review comment by @calumy on `scripts/check_ecosystem.py`:303 on 2023-05-11 12:45_

Addressed in 9a3c499

---

_@calumy reviewed on 2023-05-11 12:45_

---

_Review comment by @calumy on `scripts/check_ecosystem.py`:310 on 2023-05-11 12:45_

Addressed in 9a3c499

---

_@calumy reviewed on 2023-05-11 12:45_

---

_Review comment by @calumy on `scripts/check_ecosystem.py`:331 on 2023-05-11 12:55_

Addressed in b7c5e95

---

_@calumy reviewed on 2023-05-11 12:55_

---

_@konstin reviewed on 2023-05-11 13:03_

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:310 on 2023-05-11 13:03_

sorry, i wasn't precise enough: my idea was to put the rule into a group and then just use the group so you don't need to manually trim

---

_@calumy reviewed on 2023-05-11 13:51_

---

_Review comment by @calumy on `scripts/check_ecosystem.py`:310 on 2023-05-11 13:51_

Something more along the lines of 

```python
                for line in diff_str.splitlines():
                    # Find rule change for current line or construction
                    # + <rule>/<path>:<line>:<column>: <rule_code> <message>
                    matches = re.search(r": ([A-Z]{1,3}[0-9]{3,4})", line)

                    if matches is None:
                        # Handle case where there are no regex matches e.g.
                        # +                 "?application=AIRFLOW&authenticator=TEST_AUTH&role=TEST_ROLE&warehouse=TEST_WAREHOUSE" # noqa: E501, ERA001
                        # Which was found in local testing
                        continue

                    rule_code = matches.group(1)
```

I have pushed this in a commit, but due to the current [GitHub issues](https://www.githubstatus.com/), it does not appear to be showing up.

---

_@calumy reviewed on 2023-05-11 14:06_

---

_Review comment by @calumy on `scripts/check_ecosystem.py`:310 on 2023-05-11 14:06_

Updated in f9e09c5. I'm not sure why this is not showing up in this PR

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:310 on 2023-05-11 14:30_

yeah yesterday CI has also been flaky all day :/

thanks for fixing!

---

_@konstin reviewed on 2023-05-11 14:30_

---

_@konstin approved on 2023-05-11 14:32_

---

_Merged by @konstin on 2023-05-11 14:33_

---

_Closed by @konstin on 2023-05-11 14:33_

---

_Branch deleted on 2023-05-11 14:46_

---
