```yaml
number: 2252
title: Add mise usage instructions to installation guide
type: pull_request
state: merged
author: kvokka
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-12-29T01:19:31Z
updated_at: 2026-01-10T00:04:30Z
url: https://github.com/astral-sh/ty/pull/2252
synced_at: 2026-01-10T02:34:11Z
```

# Add mise usage instructions to installation guide

---

_Pull request opened by @kvokka on 2025-12-29 01:19_

The tool was added to [mise](https://github.com/jdx/mise) registry with https://github.com/jdx/mise/pull/7495 , and this commit updates the docs

---

_@zanieb reviewed on 2025-12-29 04:42_

---

_Review comment by @zanieb on `docs/installation.md`:53 on 2025-12-29 04:42_

If we include this (which I think will require consensus from other maintainers too), I think it should go below all the official distribution methods instead of here. The title should also probably be consistent? e.g., "Installing with mise"?

---

_@zanieb reviewed on 2025-12-29 04:43_

---

_Review comment by @zanieb on `docs/installation.md`:66 on 2025-12-29 04:43_

Having multiple instructions in a single block doesn't match the other examples on this page.

---

_@zanieb reviewed on 2025-12-29 04:43_

---

_Review comment by @zanieb on `docs/installation.md`:59 on 2025-12-29 04:43_

I don't think we need to include this, we don't explain how to list versions for any of the other installers.

---

_@MichaReiser reviewed on 2025-12-29 07:29_

---

_Review comment by @MichaReiser on `docs/installation.md`:53 on 2025-12-29 07:29_

I like how it's done in uv https://docs.astral.sh/uv/getting-started/installation/#homebrew or we could even have a single headline, `Or your package manager of choice` (or similar) and have all of them listed ina single section. 

---

_Label `documentation` added by @MichaReiser on 2025-12-29 07:30_

---

_@MichaReiser requested changes on 2025-12-29 07:30_

Based on Zanie's feedback

---

_Comment by @kvokka on 2025-12-29 07:42_

Made this one consistent with other installation methods.
But it feels like the question of how to organise the docs in terms of the different installation methods is a matter for another PR

---

_@MichaReiser reviewed on 2025-12-29 09:36_

---

_Review comment by @MichaReiser on `docs/installation.md`:54 on 2025-12-29 09:36_

I think this should come after `pipx`, but definitely after the standalone installer.

---

_@kvokka reviewed on 2025-12-29 09:56_

---

_Review comment by @kvokka on `docs/installation.md`:54 on 2025-12-29 09:56_

Moved

---

_@MichaReiser reviewed on 2025-12-29 10:10_

---

_Review comment by @MichaReiser on `docs/installation.md`:152 on 2025-12-29 10:10_

Should this use `mise use --global ty` instead (I don't know mise but that's what i found in the documentation)

---

_@MichaReiser approved on 2025-12-29 10:10_

---

_@dhruvmanila reviewed on 2025-12-29 12:09_

---

_Review comment by @dhruvmanila on `docs/installation.md`:153 on 2025-12-29 12:09_

I think it would be useful to have these two commands in separate code blocks so that it's easier for the user to directly copy-paste it using the square icon on the top-right corner in the rendered documentation. And, the text can be moved outside:


````suggestion
```shell
mise install ty
```

To set it globally:

```shell
mise use --global ty
```
````

---

_@carljm reviewed on 2026-01-09 22:49_

---

_Review comment by @carljm on `docs/installation.md`:149 on 2026-01-09 22:49_

Sorry for yet another reviewer coming in late -- but I don't understand what "To set it globally" means, or how it differs from "Install ty globally" above?


---

_@kvokka reviewed on 2026-01-09 23:14_

---

_Review comment by @kvokka on `docs/installation.md`:149 on 2026-01-09 23:14_

With `mise`, you install something once, and then it might be linked to some project or be the default choice. So the user set the installed tool for the environment.

In contrast, with tools like npm, the package will be installed for the specific project (or globally), keeping multiple copies on the disk. 

Hope that this explains the chosen wording 

---

_@carljm reviewed on 2026-01-10 00:04_

---

_Review comment by @carljm on `docs/installation.md`:149 on 2026-01-10 00:04_

Works for me -- I assume it will be clear enough to a mise user.

---

_Comment by @carljm on 2026-01-10 00:04_

This already has an approving review, and feedback has been addressed; going ahead and merging it.

---

_Merged by @carljm on 2026-01-10 00:04_

---

_Closed by @carljm on 2026-01-10 00:04_

---
