```yaml
number: 11517
title: Update contributing guide for newcomers
type: pull_request
state: closed
author: ekohilas
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2024-05-23T15:02:26Z
updated_at: 2024-12-10T15:02:24Z
url: https://github.com/astral-sh/ruff/pull/11517
synced_at: 2026-01-10T20:42:26Z
```

# Update contributing guide for newcomers

---

_Pull request opened by @ekohilas on 2024-05-23 15:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR updates the contributor documentation to make it more clear what to do step by step, and utilize existing tools.


---

_Converted to draft by @ekohilas on 2024-05-23 15:03_

---

_@zanieb reviewed on 2024-05-23 15:21_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:71 on 2024-05-23 15:21_

Why remove all of this?

---

_@zanieb reviewed on 2024-05-23 15:21_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:71 on 2024-05-23 15:21_

I see it moved down, I'm not sure that's clearer.

---

_@ekohilas reviewed on 2024-05-23 16:00_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:71 on 2024-05-23 16:00_

Sorry I didn't have the chance to add comments yet.

This was because during setup, users are doing each thing step by step.

But at this point, the user isn't up to the step where the repo is downloaded, to be able to setup the pre-commit hooks in.

---

_@ekohilas reviewed on 2024-05-23 16:05_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:89 on 2024-05-23 16:05_

It wasn't clear when this was run that the `/path/to/file.py` was an example.

This change updates this by clearing up the sentence, and also pointing to an example file that exists.

---

_@ekohilas reviewed on 2024-05-23 16:06_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:98 on 2024-05-23 16:06_

code blocks give the user an ability to copy and paste into a terminal, but doing this doesn't work since the trailing comments break the newlines.

---

_@ekohilas reviewed on 2024-05-23 16:07_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:101 on 2024-05-23 16:07_

ideally this would be in something like `cargo pre-test` alias, similar to `npm run` scripts, but this doesn't seem to natively exist in cargo

---

_@ekohilas reviewed on 2024-05-23 16:09_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:203 on 2024-05-23 16:09_

Makes it more clear that `ruff.schema.json` is auto updated, and what it's for

---

_@ekohilas reviewed on 2024-05-23 16:09_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:196 on 2024-05-23 16:09_

easier to read

---

_@ekohilas reviewed on 2024-05-23 16:11_

---

_Review comment by @ekohilas on `CONTRIBUTING.md`:166 on 2024-05-23 16:11_

being able to run a script to reduce the amount of things needed for new users will make new contributions easier to make

---

_Marked ready for review by @ekohilas on 2024-05-23 16:11_

---

_Review requested from @zanieb by @ekohilas on 2024-05-25 04:35_

---

_Label `documentation` added by @MichaReiser on 2024-05-27 11:05_

---

_Comment by @zanieb on 2024-12-10 15:02_

I think this is stale now, sorry we lost track of it.

---

_Closed by @zanieb on 2024-12-10 15:02_

---
