```yaml
number: 2957
title: Allow profiling tests with tracing instrumentation
type: pull_request
state: merged
author: konstin
labels:
  - performance
  - internal
  - testing
assignees: []
merged: true
base: main
head: konsti/allow-profiling-test-runs
created_at: 2024-04-10T10:06:17Z
updated_at: 2024-04-10T10:15:28Z
url: https://github.com/astral-sh/uv/pull/2957
synced_at: 2026-01-10T14:43:31Z
```

# Allow profiling tests with tracing instrumentation

---

_Pull request opened by @konstin on 2024-04-10 10:06_

To get more insights into test performance, allow instrumenting tests with tracing-durations-export.

Usage:

```shell
# A single test
TRACING_DURATIONS_TEST_ROOT=$(pwd)/target/test-traces cargo test --features tracing-durations-export --test pip_install_scenarios no_binary -- --exact
# All tests
TRACING_DURATIONS_TEST_ROOT=$(pwd)/target/test-traces cargo nextest run --features tracing-durations-export
```

Then we can e.g. look at `target/test-traces/pip_install_scenarios::no_binary.svg` and see the builds it performs:

![image](https://github.com/astral-sh/uv/assets/6826232/40b4e094-debc-4b22-8aa3-9471998674af)



---

_Label `performance` added by @konstin on 2024-04-10 10:06_

---

_Label `internal` added by @konstin on 2024-04-10 10:06_

---

_Label `testing` added by @konstin on 2024-04-10 10:06_

---

_Merged by @konstin on 2024-04-10 10:15_

---

_Closed by @konstin on 2024-04-10 10:15_

---

_Branch deleted on 2024-04-10 10:15_

---
