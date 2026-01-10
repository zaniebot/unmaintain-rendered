```yaml
number: 15302
title: Use uv consistently throughout the documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/u
created_at: 2025-01-06T15:32:10Z
updated_at: 2025-01-07T14:43:27Z
url: https://github.com/astral-sh/ruff/pull/15302
synced_at: 2026-01-10T20:34:00Z
```

# Use uv consistently throughout the documentation

---

_Pull request opened by @charliermarsh on 2025-01-06 15:32_

## Summary

Closes https://github.com/astral-sh/ruff/issues/15301#issuecomment-2573350821.


---

_Review requested from @zanieb by @charliermarsh on 2025-01-06 15:32_

---

_Review requested from @dhruvmanila by @charliermarsh on 2025-01-06 15:32_

---

_Label `documentation` added by @charliermarsh on 2025-01-06 15:32_

---

_@charliermarsh reviewed on 2025-01-06 15:32_

---

_Review comment by @charliermarsh on `docs/faq.md`:228 on 2025-01-06 15:32_

I'm tempted to remove this line.

---

_@charliermarsh reviewed on 2025-01-06 15:35_

---

_Review comment by @charliermarsh on `README.md`:168 on 2025-01-06 15:35_

For consistency...

---

_@charliermarsh reviewed on 2025-01-06 15:35_

---

_Review comment by @charliermarsh on `docs/installation.md`:3 on 2025-01-06 15:35_

We could also include instructions for installing uv, but it feels like that shouldn't live here.

---

_Review comment by @MichaReiser on `docs/installation.md`:7 on 2025-01-06 15:39_

Is it our recommended setup to install ruff globally vs as a project dependency? Having Ruff pinned seems like a big advantage to me and suggesting to install it globally feels like we're misguiding users to struggle with random CI failures etc. in the future

---

_@MichaReiser reviewed on 2025-01-06 15:39_

---

_Review comment by @dhruvmanila on `README.md`:122 on 2025-01-06 15:52_

Is this a typo? I don't think we should add a `$` for the comment.

They're added in multiple code blocks.

```suggestion
# With uv.
```

---

_@dhruvmanila reviewed on 2025-01-06 15:52_

---

_@dhruvmanila reviewed on 2025-01-06 15:53_

---

_Review comment by @dhruvmanila on `README.md`:123 on 2025-01-06 15:53_

Should we promote using `ruff@latest`?

---

_@dhruvmanila reviewed on 2025-01-06 15:54_

---

_Review comment by @dhruvmanila on `docs/faq.md`:227 on 2025-01-06 15:54_

Same as above, there are `$` signs before the comment.

---

_Review comment by @dhruvmanila on `docs/installation.md`:7 on 2025-01-06 15:56_

I think we could recommend both which is to either install it globally or add it as a project dependency with a specific version. Rooster should take care of updating the versions in the documentation (https://github.com/astral-sh/ruff/blob/d45c1ee44f53d9c606085e79da1830b558fc001f/pyproject.toml#L106-L114).

---

_@dhruvmanila reviewed on 2025-01-06 15:56_

---

_@dhruvmanila approved on 2025-01-06 15:56_

---

_@zanieb reviewed on 2025-01-06 17:21_

---

_Review comment by @zanieb on `README.md`:124 on 2025-01-06 17:21_

You can't use leading `$` in the GitHub README without breaking copy/paste.

---

_@zanieb reviewed on 2025-01-06 17:21_

---

_Review comment by @zanieb on `docs/installation.md`:6 on 2025-01-06 17:21_

We should only use a leading `$` in the documentation if we add the copy/paste fix javascript from uv.

---

_@charliermarsh reviewed on 2025-01-06 17:22_

---

_Review comment by @charliermarsh on `README.md`:124 on 2025-01-06 17:22_

Sad, thanks.

---

_@charliermarsh reviewed on 2025-01-06 17:22_

---

_Review comment by @charliermarsh on `docs/installation.md`:6 on 2025-01-06 17:22_

Okay thanks. We already use this in a few places, I think. I'll figure out what to do!

---

_Review comment by @zanieb on `docs/faq.md`:228 on 2025-01-06 17:24_

Should we be grouping into two separate installation concepts: Installing Ruff globally and adding Ruff to your project?

---

_@zanieb reviewed on 2025-01-06 17:24_

---

_@charliermarsh reviewed on 2025-01-06 18:14_

---

_Review comment by @charliermarsh on `docs/faq.md`:228 on 2025-01-06 18:14_

I'm not sure. It seems like a lot to try and introduce the concept of "projects" here. Are you suggesting that we'd have `uv tool` and `pipx` vs. `uv add` and `pip`?

---

_@charliermarsh reviewed on 2025-01-07 01:28_

---

_Review comment by @charliermarsh on `README.md`:122 on 2025-01-07 01:28_

Yes, it's necessary in uv (including the leading `$`). But it was a mistake to add it here.

---

_Review comment by @zanieb on `docs/faq.md`:228 on 2025-01-07 02:11_

Yeah that's the thought.

I don't know if we need to go into details, just "project" and "globally" 🤷‍♀️ 

---

_@zanieb reviewed on 2025-01-07 02:11_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-07 02:13_

---

_Review comment by @zanieb on `docs/faq.md`:228 on 2025-01-07 02:15_

I'm fine with an incremental improvement — just a thought on the grouping of the concepts instead of by tool.

---

_@zanieb reviewed on 2025-01-07 02:15_

---

_Merged by @charliermarsh on 2025-01-07 14:43_

---

_Closed by @charliermarsh on 2025-01-07 14:43_

---

_Branch deleted on 2025-01-07 14:43_

---
