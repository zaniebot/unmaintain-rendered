---
number: 13217
title: Retain history of package versions through updates
type: issue
state: open
author: juhaszp95
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-04-30T08:33:11Z
updated_at: 2025-05-04T12:50:47Z
url: https://github.com/astral-sh/uv/issues/13217
synced_at: 2026-01-07T13:12:18-06:00
---

# Retain history of package versions through updates

---

_Issue opened by @juhaszp95 on 2025-04-30 08:33_

### Summary

Hi There,

I'm occasionally upgrading packages in my package environment using `uv`. `conda` has a `conda list --revisions` command which is very useful, as it shows you which packages changed and how through updates. This makes it easy to roll back if needed via `conda install --revision=REVNUM`.

It would be great if `uv` could support a similar feature (at least keeping a history).

Of course this could be done by committing `uv.lock` after each update to version control, but realistically that doesn't always happen.

### Example

_No response_

---

_Label `enhancement` added by @juhaszp95 on 2025-04-30 08:33_

---

_Label `wish` added by @charliermarsh on 2025-05-04 12:50_

---
