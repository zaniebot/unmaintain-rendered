```yaml
number: 18163
title: Fix rendering of admonition in docs
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/admonition-docs
created_at: 2025-05-18T15:55:16Z
updated_at: 2025-07-15T19:38:29Z
url: https://github.com/astral-sh/ruff/pull/18163
synced_at: 2026-01-12T15:56:13Z
```

# Fix rendering of admonition in docs

---

_@MichaReiser_

## Summary

GitHub and mkdocs use different syntaxes for admonition. The mkdocs admonition have more features but we have some markdown files that are both rendered on GitHub and mkdocs.
This PR includes a mkdocs plugin that transforms GitHub admonition to mkdocs admonitions. 

An alternative to the extension is to extend `generate_mkdocs` to also replace admonitions. Given that the plugin already exists, this seemed easier. 

I had to tag a specific commit because the pypi release doen't include the fix for https://github.com/PGijsbers/admonitions/issues/3

Fixes https://github.com/astral-sh/ruff/issues/18158

## Test Plan

Build the docs locally


---

_Assigned to @MichaReiser by @MichaReiser on 2025-05-18 15:55_

---

_Label `documentation` added by @MichaReiser on 2025-05-18 15:55_

---

_@dhruvmanila reviewed on 2025-05-18 15:58_

---

_Review comment by @dhruvmanila on `docs/requirements-insiders.txt`:4 on 2025-05-18 15:58_

Does this work? I'm asking because the repository is private under the Astral org on GitHub and cloning that would require the SSH keys when `git+ssh` is being used and username/password for the `https` protocol.

---

_@MichaReiser reviewed on 2025-05-18 16:02_

---

_Review comment by @MichaReiser on `docs/requirements-insiders.txt`:4 on 2025-05-18 16:02_

I reverted it. I always need to change it because I never set up SSH auth :)

---

_Merged by @MichaReiser on 2025-05-20 06:22_

---

_Closed by @MichaReiser on 2025-05-20 06:22_

---

_Branch deleted on 2025-05-20 06:22_

---

_Comment by @corneliusroemer on 2025-07-15 19:38_

Commit has now landed in a release so pin can be removed (in the future, it might be good to add a comment to the requirements.txt providing the reason for a pinned dependency)

<img width="518" height="186" alt="image" src="https://github.com/user-attachments/assets/a0b4ea1d-4adf-46f3-9867-e4c6182128d8" />


---
