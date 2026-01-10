```yaml
number: 12025
title: Add dependabot config for uv to docs
type: pull_request
state: open
author: grepme
labels:
  - documentation
assignees: []
base: main
head: docs/dependabot
created_at: 2025-03-06T22:09:11Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/12025
synced_at: 2026-01-10T11:10:39Z
```

# Add dependabot config for uv to docs

---

_Pull request opened by @grepme on 2025-03-06 22:09_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

uv is now fully supported in dependabot: https://github.blog/changelog/2025-03-13-dependabot-version-updates-now-support-uv-in-general-availability/

Former description:
dependabot now supports the uv package ecosystem provided the `enable-beta-ecosystems` flag is true. `uv.lock` and dependency groups are not yet supported.

Relevant PR: https://github.com/dependabot/dependabot-core/pull/11687

Mentions of this change:
https://github.com/dependabot/dependabot-core/pull/10040#issuecomment-2696978430
https://github.com/dependabot/dependabot-core/issues/10478#issuecomment-2691330949

## Test Plan

For the dependabot configuration, I updated several of my company's repositories with the new config and dependabot then updated its PRs.

For documentation changes, I tested the changes locally and then ran:
`npx prettier --prose-wrap always --write "**/*.md" `

---

_Comment by @edgarrmondragon on 2025-03-07 15:49_

This should probably wait until https://github.com/dependabot/dependabot-core/issues/10478#issuecomment-2703953413 (updating `uv.lock`) works

---

_Comment by @grepme on 2025-03-14 15:25_

@edgarrmondragon Looks like we are good now: https://github.blog/changelog/2025-03-13-dependabot-version-updates-now-support-uv-in-general-availability/

---

_Label `documentation` added by @konstin on 2025-03-14 16:30_

---

_Review requested from @zanieb by @konstin on 2025-03-14 16:30_

---

_Review comment by @john-s-lin on `docs/guides/integration/dependency-bots.md`:85 on 2025-03-14 16:49_

```suggestion
[Dependabot's documentation](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference)
```

I think it might be a typo but the backticks are making the hyperlink invalid in markdown

---

_@john-s-lin reviewed on 2025-03-14 16:49_

---

_Comment by @edgarrmondragon on 2025-03-14 17:07_

> @edgarrmondragon Looks like we are good now: https://github.blog/changelog/2025-03-13-dependabot-version-updates-now-support-uv-in-general-availability/

announcements apart, it still seems a bit unstable: https://github.com/dependabot/dependabot-core/issues/10478#issuecomment-2724041214

---

_@grepme reviewed on 2025-03-14 18:06_

---

_Review comment by @grepme on `docs/guides/integration/dependency-bots.md`:85 on 2025-03-14 18:06_

@john-s-lin yes, good catch and thank you for reviewing, added your commit.

---
