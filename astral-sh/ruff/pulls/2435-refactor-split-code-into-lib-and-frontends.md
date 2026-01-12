```yaml
number: 2435
title: "refactor: Split code into `lib/` and `frontends/`"
type: pull_request
state: closed
author: not-my-profile
labels: []
assignees: []
draft: true
base: main
head: crates-dir
created_at: 2023-02-01T09:06:14Z
updated_at: 2023-02-05T13:49:08Z
url: https://github.com/astral-sh/ruff/pull/2435
synced_at: 2026-01-12T04:52:00Z
```

# refactor: Split code into `lib/` and `frontends/`

---

_Pull request opened by @not-my-profile on 2023-02-01 09:06_

The PR #2088 implements the following structure:

```
├── crates/
│   ├── flake8_to_ruff/
│   ├── ruff/
│   ├── ruff_cli/
│   └── ruff_macros/
├── playground/
│   └── ....
```

This PR implements an alternative structure, namely:

```
├── frontends/
│   ├── flake8_to_ruff/
│   ├── playground/
│   └── ruff_cli/
├── lib/
│   ├── ruff/
│   └── ruff_macros/
```

The reasoning being that I find this grouping to be more intuitive. 

Left over TODOs:

* [ ] move most part of `CONTRIBUTING.md` to `lib/ruff/CONTRIBUTING.md`

Part of #2059.

---

_Renamed from "refactor: Introduce `crates/` directory" to "refactor: Split code into `lib/` and `frontends/`" by @not-my-profile on 2023-02-01 11:08_

---

_Closed by @not-my-profile on 2023-02-05 13:49_

---
