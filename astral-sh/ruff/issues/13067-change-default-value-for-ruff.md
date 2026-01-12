```yaml
number: 13067
title: "Change default value for `ruff.configurationPreference` to `\"filesystemFirst\"`"
type: issue
state: open
author: dhruvmanila
labels:
  - breaking
  - configuration
  - needs-decision
  - server
assignees: []
created_at: 2024-08-23T04:13:40Z
updated_at: 2025-05-13T12:35:39Z
url: https://github.com/astral-sh/ruff/issues/13067
synced_at: 2026-01-12T15:54:52Z
```

# Change default value for `ruff.configurationPreference` to `"filesystemFirst"`

---

_@dhruvmanila_

There have been multiple requests to change the default value for [`ruff.configurationPreference`](https://docs.astral.sh/ruff/editors/settings/#configurationpreference) server setting to `"filesystemFirst"`:
* https://github.com/astral-sh/ruff/issues/12514
* https://github.com/astral-sh/ruff-vscode/issues/425
* https://github.com/astral-sh/ruff-vscode/issues/3

I'm opening this issue to welcome any discussion / suggestion for or against this change.

This is a breaking change and would go in a minor release.

---

_Label `breaking` added by @dhruvmanila on 2024-08-23 04:13_

---

_Label `configuration` added by @dhruvmanila on 2024-08-23 04:13_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-23 04:13_

---

_Label `server` added by @dhruvmanila on 2024-08-23 04:13_

---

_Comment by @T-256 on 2024-12-12 15:40_

Hey, Did you decide about this change? good to have it on `0.9` :)

---

_Comment by @dhruvmanila on 2024-12-13 10:38_

Not yet, I'm still on the fence on whether this should be changed or not given that most users are (I think) finding the default value useful. This is mainly from the amount of upvotes in this issue and given that it can be easily updated using the setting.

---

_Comment by @ianzone on 2025-04-23 08:30_

I'm in favor of this change. 

---

_Comment by @nbaju1 on 2025-05-13 12:35_

I see very few reasons as to why `editorFirst` should be the default value, compared to `filesystemFirst`. If a project is configured with specific linter and/or formatting rules it is safe to assume that these are the preferred and agreed upon settings for that project. I see very few, if any, scenarios in which the preferences of individual developers should be prioritized over the project settings. Especially not on formatting rules as this can result in a mess of commits.

---
