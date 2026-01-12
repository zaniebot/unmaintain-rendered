```yaml
number: 4902
title: Omitted target_version when using ruff.toml
type: issue
state: closed
author: HealthyPear
labels:
  - documentation
assignees: []
created_at: 2023-06-06T13:43:12Z
updated_at: 2023-06-07T04:18:58Z
url: https://github.com/astral-sh/ruff/issues/4902
synced_at: 2026-01-12T15:54:45Z
```

# Omitted target_version when using ruff.toml

---

_@HealthyPear_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I am using `ruff.toml` for my configuration and my Python package is configured with `pyproject.toml`.

In my ruff configuration, I omitted `target_version`.

I am using `ruff v0.0.270`.

When I launch `ruff check -v .` from the root of my repository I get,

> [2023-06-06][13:32:44][ruff::settings::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.

If the expected behavior (infer from `pyproject.toml`) is not supported from `ruff.toml`, I think that the [documentation](https://beta.ruff.rs/docs/settings/#target-version) should be clearer.

If instead this is supposed to work maybe it's a bug or I missed something...


---

_Comment by @dhruvmanila on 2023-06-06 14:14_

This is the expected behavior although I'm not sure the reasoning behind this. @charliermarsh could provide more info about this. But I do agree that this should be added in the documentation.

---

_Comment by @charliermarsh on 2023-06-06 14:25_

This is the expected behavior -- I'll update the documentation. We don't want to reconcile settings inferred from multiple files, it adds a lot of complexity to the code and ultimately to the user-facing behavior. I think it's the right tradeoff, but it should be better documented here.

---

_Label `documentation` added by @charliermarsh on 2023-06-06 14:25_

---

_Comment by @HealthyPear on 2023-06-06 14:50_

Why do you say "inferred from multiple files"? You would infer anyway from `pyproject.toml`, no?

---

_Comment by @charliermarsh on 2023-06-06 19:05_

By "inferred from multiple files", I mean that we would need to identify both the `ruff.toml` and the `pyproject.toml`, and extract + reconcile information from both of them, rather than using a single configuration file as the source of truth.

---

_Closed by @charliermarsh on 2023-06-07 04:18_

---
