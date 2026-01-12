```yaml
number: 3380
title: Updated forced-separate type from Rust to abstract
type: pull_request
state: merged
author: aacunningham
labels: []
assignees: []
merged: true
base: main
head: change-forced-separate-type-in-docs
created_at: 2023-03-07T06:18:16Z
updated_at: 2023-03-07T18:54:05Z
url: https://github.com/astral-sh/ruff/pull/3380
synced_at: 2026-01-12T04:39:44Z
```

# Updated forced-separate type from Rust to abstract

---

_Pull request opened by @aacunningham on 2023-03-07 06:18_

Really small documentation change. I noticed that most of the types listed in the docs aren't Rust types but more generic, except for `forced-separate` (https://beta.ruff.rs/docs/settings/#forced-separate). It looks like the only that was `Vec<String>` instead of `list[str]`, and I _think_ this is how that is generated?

Let me know if I missed anything or if there is anything else I can throw in here.

---

_Comment by @charliermarsh on 2023-03-07 14:35_

Thanks :) This is great.

---

_Merged by @charliermarsh on 2023-03-07 14:35_

---

_Closed by @charliermarsh on 2023-03-07 14:35_

---

_Comment by @aacunningham on 2023-03-07 17:33_

Thanks for the merge! Glad I could contribute something, this project has been amazing to review.

---

_Comment by @charliermarsh on 2023-03-07 18:54_

Thank you for the kind words :)

---
