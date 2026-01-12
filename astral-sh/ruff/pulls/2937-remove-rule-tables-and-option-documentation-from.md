```yaml
number: 2937
title: Remove rule tables and option documentation from the README
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: trim-readme
created_at: 2023-02-15T20:50:28Z
updated_at: 2023-02-17T12:55:51Z
url: https://github.com/astral-sh/ruff/pull/2937
synced_at: 2026-01-12T15:55:12Z
```

# Remove rule tables and option documentation from the README

---

_@not-my-profile_

As per https://github.com/charliermarsh/ruff/issues/2911#issuecomment-1430791769.

---

_Comment by @charliermarsh on 2023-02-16 14:00_

I'll try to review this in the afternoon. It of course makes sense to get rid of the huge README at some point, so I just need to decide if now is that point :)

Another question is: if we're removing these references, should we just remove "everything", and limit the README to "Getting started" + link to the docs? But, we could always do that separately.

---

_Comment by @charliermarsh on 2023-02-16 22:59_

Do you mind rebasing this and I'll merge it whenever it's ready?

---

_Comment by @not-my-profile on 2023-02-17 04:57_

> should we just remove "everything", and limit the README to "Getting started" + link to the docs? But, we could always do that separately.

I think we should split off *Editor Integrations* and *FAQ* into `docs/editor-integrations.md` and  `docs/faq.md` in a followup PR to fix #2911 and to fix the heading levels (currently h3 follows h1 ... the subsection headings should be h2).

---

_Merged by @charliermarsh on 2023-02-17 12:55_

---

_Closed by @charliermarsh on 2023-02-17 12:55_

---
