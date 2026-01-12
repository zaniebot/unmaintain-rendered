```yaml
number: 2288
title: Subcommand followups
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: subcommand-followups
created_at: 2023-01-28T03:18:40Z
updated_at: 2023-01-28T14:55:24Z
url: https://github.com/astral-sh/ruff/pull/2288
synced_at: 2026-01-12T15:55:07Z
```

# Subcommand followups

---

_@not-my-profile_

(See the commit messages for the reasoning.)

---

_Comment by @charliermarsh on 2023-01-28 03:35_

Are there any analogies we can draw to other tools, to help inform the user-facing API for the CLI?

E.g., `rustup` has `rustup toolchain list`. Should we have `ruff rule list` instead of just `ruff rule`?


---

_Comment by @not-my-profile on 2023-01-28 03:37_

Yes I think it's quite common for commands to default to listing when there's no more apt action, examples that come to mind:

* `git branch`
* `mount`
* `ip address`, `ip route` (etc. all the `ip` subcommands)
* `getent <database> [key]`, e.g. `getent hosts`

`ruff rule list` would be ambiguous in case there's a rule named `list` (which we'll probably never have since it would be a very non-descriptive rule name but it's still nice to avoid such ambiguities).

---

_Comment by @charliermarsh on 2023-01-28 03:44_

Appreciate it. I don't plan to release tonight either way :)

---

_Comment by @not-my-profile on 2023-01-28 06:18_

I just want to note that if we want to change `explain` to `rule` (I personally think it's a good idea) we should do that before releasing the subcommand refactor so that the release notes in `BREAKING_CHANGES.md` stay up to date.

To clarify my commit message: At some point in the future we may have:

`ruff config banned-api` to explain the `banned-api` config option

`ruff rule banned-api` to explain the `banned-api` rule

... if we only had `explain` it would have to report both, which would be confusing. And we also would have to introduce separate subcommands for listing linters, rules, config settings ... I think having a setup where one subcommand lists everything by default and shows an individual item with another argument makes more sense.

---

_Merged by @charliermarsh on 2023-01-28 12:26_

---

_Closed by @charliermarsh on 2023-01-28 12:26_

---

_Comment by @charliermarsh on 2023-01-28 14:37_

@not-my-profile - anything you think is missing / preventing us from cutting a release, now that this is merged?

---

_Comment by @not-my-profile on 2023-01-28 14:55_

No, I don't see anything else blocking a new release :)

---
