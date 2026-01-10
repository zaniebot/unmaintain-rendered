```yaml
number: 1866
title: Consider sharding py-fuzzer CI job to run with multiple Python versions
type: issue
state: open
author: AlexWaygood
labels:
  - ci
  - fuzzer
assignees: []
created_at: 2025-12-12T14:34:48Z
updated_at: 2025-12-12T14:35:00Z
url: https://github.com/astral-sh/ty/issues/1866
synced_at: 2026-01-10T01:55:00Z
```

# Consider sharding py-fuzzer CI job to run with multiple Python versions

---

_Issue opened by @AlexWaygood on 2025-12-12 14:34_

> I'd be inclined to hard code the Python version to 3.8 or shard the benchmark to check multiple versions (both seem equally likely to catch bugs IMO because we default to an old Python version in mdtests)

_Originally posted by @MichaReiser in https://github.com/astral-sh/ruff/pull/21844#pullrequestreview-3552259205_

I did the first bit of this suggestion in https://github.com/astral-sh/ruff/pull/21844 (the `--python-version` argument for the fuzzer is currently hardcoded to 3.9, typeshed's lowest supported Python version). But we could consider doing the second suggestion as well: we could run two instances of the fuzzer script on every PR. Once with `--python-version=3.9`, and once with `--python-version=3.14`

---

_Label `ci` added by @AlexWaygood on 2025-12-12 14:35_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-12 14:35_

---
