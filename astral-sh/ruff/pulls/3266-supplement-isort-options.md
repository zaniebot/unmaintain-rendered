```yaml
number: 3266
title: Supplement isort options
type: pull_request
state: closed
author: moreal
labels: []
assignees: []
draft: true
base: main
head: isort-options
created_at: 2023-02-28T03:47:49Z
updated_at: 2023-03-21T15:07:02Z
url: https://github.com/astral-sh/ruff/pull/3266
synced_at: 2026-01-12T15:55:12Z
```

# Supplement isort options

---

_@moreal_

This pull request is related to the https://github.com/charliermarsh/ruff/issues/1749 issue but it doesn't resolve the problem perfectly yet. I opened this pull request to check the intermediate stage with reviewing.

I referenced https://github.com/PyCQA/isort/blob/3a72e069635a865a92b8a0273aa829f630cbcd6f/isort/settings.py source file.

---

_@charliermarsh reviewed on 2023-02-28 20:51_

---

_Review comment by @charliermarsh on `crates/ruff/src/flake8_to_ruff/isort.rs`:34 on 2023-02-28 20:51_

These may all need the `alias = "single-line-exclusions", alias = "single_line_exclusions"` treatment as seen in `src_paths`, to respect isort's acceptance of either casing style. 

---

_Comment by @charliermarsh on 2023-02-28 20:51_

Seems like a reasonable start! I might suggest only implementing those options that we support within Ruff for now, to make things easier. see: https://beta.ruff.rs/docs/settings/#isort.

---

_@moreal reviewed on 2023-03-04 08:38_

---

_Review comment by @moreal on `crates/ruff/src/flake8_to_ruff/isort.rs`:34 on 2023-03-04 08:38_

I added such aliases too! Thanks! 🙏🏻 

---

_Converted to draft by @moreal on 2023-03-04 08:39_

---

_Converted to draft by @moreal on 2023-03-04 08:39_

---

_Comment by @moreal on 2023-03-04 09:11_

> Seems like a reasonable start! I might suggest only implementing those options that we support within Ruff for now, to make things easier. see: https://beta.ruff.rs/docs/settings/#isort.

Thank you for your suggestion. I added more options from https://beta.ruff.rs/docs/settings/#isort and clean up other options, not in the document. 🙏🏻 

---

_Review requested from @charliermarsh by @moreal on 2023-03-04 09:11_

---

_Marked ready for review by @moreal on 2023-03-04 09:11_

---

_Converted to draft by @moreal on 2023-03-04 14:37_

---

_Comment by @charliermarsh on 2023-03-21 15:07_

I'm going to close this for now, just to keep the open PR list tight. If you choose to return to this, we can always reopen!

---

_Closed by @charliermarsh on 2023-03-21 15:07_

---
