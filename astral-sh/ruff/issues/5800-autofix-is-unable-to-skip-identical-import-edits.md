```yaml
number: 5800
title: Autofix is unable to skip identical import edits
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-07-16T09:03:44Z
updated_at: 2023-07-29T12:11:59Z
url: https://github.com/astral-sh/ruff/issues/5800
synced_at: 2026-01-10T11:09:48Z
```

# Autofix is unable to skip identical import edits

---

_Issue opened by @dhruvmanila on 2023-07-16 09:03_

There are 22 violations in this [test file](https://github.com/astral-sh/ruff/blob/d692ed08961cbdc56a9f254fba906bd6d3b711d8/crates/ruff/resources/test/fixtures/ruff/RUF013_0.py) for `RUF013`. The autofix for the same contains an edit created using `get_or_import_symbol`. This creates a [no-op edit](https://github.com/astral-sh/ruff/blob/d692ed08961cbdc56a9f254fba906bd6d3b711d8/crates/ruff/src/importer/mod.rs#L205-L220) for every violation. The autofix module is unable to detect that this was already applied and thus not skipping it. This means that there are multiple edits on the same line, thus it skips applying the other fix for the current iteration. This means it applies fix one at a time for each iteration from the 22 violations, meaning it requires 22 iterations to apply all of the fixes.

### How to reproduce?

1. Update the `MAX_ITERATIONS` to 21 (1 less than the total violations reported) here:

https://github.com/astral-sh/ruff/blob/d692ed08961cbdc56a9f254fba906bd6d3b711d8/crates/ruff/src/linter.rs#L250

2. Run the following command:

	```console
    cargo run --bin ruff -- check --select=RUF013 --no-cache --isolated crates/ruff/resources/test/fixtures/ruff/RUF013_0.py --target-version=py39 --fix --diff
	```

Output:
```
debug error: Failed to converge after 21 iterations in `crates/ruff/resources/test/fixtures/ruff/RUF013_0.py` with rule codes RUF013:---
```

Or, for the test case:

1. Update the `MAX_ITERATIONS` to 21 here:

https://github.com/astral-sh/ruff/blob/d692ed08961cbdc56a9f254fba906bd6d3b711d8/crates/ruff/src/test.rs#L86

2. Run the following command:

	```console
	cargo test --package ruff --lib --all-features -- rules::ruff::tests::implicit_optional_py39 --nocapture
	```

---

_Label `bug` added by @dhruvmanila on 2023-07-16 09:03_

---

_Label `autofix` added by @dhruvmanila on 2023-07-16 09:03_

---

_Closed by @charliermarsh on 2023-07-29 12:11_

---
