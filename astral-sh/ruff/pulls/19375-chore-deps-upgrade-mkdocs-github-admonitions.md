```yaml
number: 19375
title: "chore(deps): upgrade mkdocs-github-admonitions-plugin to v0.1.1"
type: pull_request
state: open
author: corneliusroemer
labels:
  - dependencies
assignees: []
base: main
head: unpin-adminitions-plugin
created_at: 2025-07-15T19:45:30Z
updated_at: 2025-07-16T08:30:56Z
url: https://github.com/astral-sh/ruff/pull/19375
synced_at: 2026-01-10T17:58:13Z
```

# chore(deps): upgrade mkdocs-github-admonitions-plugin to v0.1.1

---

_Pull request opened by @corneliusroemer on 2025-07-15 19:45_

## Summary

Renovate doesn't upgrade sha-pinned dependencies.

A sha pin on `└mkdocs-github-admonitions-plugin` was introduced due to a bug fix not being available in a released version in PR #18163

Now the bug fix has been released so the pin can be removed.

In the future, in such cases, it might be a good idea to add a comment
and/or set a reminder to look back and consider removing the pin
as renovate will not handle sha pins.

As renovate handles almost all dep upgrades, this provides a false sense
of "security" that deps are always kept up to date automatically.

Related to issue #19369

Thanks @ntBre for the pointer!

## Test Plan

Docs previews should be visually inspected for correct rendering of admonitions.

As I'm not an Astral insider, I unfortunately can't actually faithfully reproduce the docs (due to Material for MkDocs being closed source). I don't know if there are automatic preview builds in CI that I can inspect instead.

## Other

I also add a comment to remind the reader that renovate does not automatically update git/sha pinned dependencies. This was touched on in #19369 as well - I think this comment might be worth it but happy to remove of course if reviewers differ. It's not a big change, hence adding as piggyback commit to the PR that edits the same file.


---

_Review requested from @zanieb by @MichaReiser on 2025-07-15 19:46_

---

_Comment by @MichaReiser on 2025-07-15 19:47_

I'm not sure if we even use this version in the deployed docs but @zanieb will know better 

---

_Comment by @corneliusroemer on 2025-07-15 19:50_

@MichaReiser oh interesting, if the prod setup differs from what's on main, that might be a good thing to add the docs CONTRIBUTING.md section

Maybe as an addition to:
- https://github.com/astral-sh/ruff/pull/19373

---

_Label `dependencies` added by @ntBre on 2025-07-15 20:38_

---

_@MichaReiser requested changes on 2025-07-16 08:30_

Thank you. We should also update the regular `requirements.txt` file so that both insiders/non-insiders use the same versions (where possible)

---
