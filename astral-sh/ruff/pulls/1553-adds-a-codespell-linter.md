```yaml
number: 1553
title: Adds a codespell linter
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: codespell
created_at: 2023-01-02T12:18:42Z
updated_at: 2023-01-02T20:57:59Z
url: https://github.com/astral-sh/ruff/pull/1553
synced_at: 2026-01-12T05:36:31Z
```

# Adds a codespell linter

---

_Pull request opened by @colin99d on 2023-01-02 12:18_

Fixes #1536

---

_Marked ready for review by @colin99d on 2023-01-02 12:20_

---

_Comment by @not-my-profile on 2023-01-02 14:17_

The log says

> Running codespell on '' with the following options...

so is it actually doing anything?

When I run codespell locally on the `src` directory it reports a bunch of useless `crate ==> create` errors.

---

_Comment by @colin99d on 2023-01-02 14:19_

Good catch. I thought all of the crate issues were in the `./target` folder, which isnt in the repo. But it looks like there are a couple in other places. Will fix now.

---

_Comment by @colin99d on 2023-01-02 14:58_

Linter is working now! Its currently failing on a couple errors (showing the value). Once @charliermarsh confirms he wants it I will resolve those errors and get this thing merged.

---

_Review comment by @andersk on `.github/workflows/ci.yaml`:202 on 2023-01-02 19:08_

This action doesn’t interact with `poetry.lock`.

---

_@andersk reviewed on 2023-01-02 19:09_

There’s an official GitHub Action from the Codespell project that might make more sense: https://github.com/codespell-project/actions-codespell

---

_Comment by @charliermarsh on 2023-01-02 19:09_

@colin99d - I def appreciate this, but I'd like to keep the development tooling within the Rust ecosystem where possible. Would you mind modifying this to use the [`typos`](https://github.com/crate-ci/typos) crate instead?

---

_Comment by @colin99d on 2023-01-02 19:24_

@charliermarsh I definitely agree, I just switched it and some more errors were found. Let me fix the spelling mistakes and then we should be good to go.

---

_Comment by @charliermarsh on 2023-01-02 19:38_

Thanks!

---

_@charliermarsh reviewed on 2023-01-02 19:40_

---

_Review comment by @charliermarsh on `_typos.toml`:3 on 2023-01-02 19:40_

Should we just change those to some valid word, to avoid false negatives?

---

_@colin99d reviewed on 2023-01-02 19:45_

---

_Review comment by @colin99d on `_typos.toml`:3 on 2023-01-02 19:45_

Im on it.

---

_@charliermarsh reviewed on 2023-01-02 19:46_

---

_Review comment by @charliermarsh on `_typos.toml`:3 on 2023-01-02 19:46_

Thanks tons

---

_@colin99d reviewed on 2023-01-02 19:57_

---

_Review comment by @colin99d on `_typos.toml`:3 on 2023-01-02 19:57_

Np. Done.

---

_Merged by @charliermarsh on 2023-01-02 20:57_

---

_Closed by @charliermarsh on 2023-01-02 20:57_

---
