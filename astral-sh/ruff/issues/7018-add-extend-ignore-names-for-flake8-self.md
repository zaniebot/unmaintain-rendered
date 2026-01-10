```yaml
number: 7018
title: "Add `extend-ignore-names` for `flake8-self`"
type: issue
state: closed
author: jaap3
labels:
  - configuration
assignees: []
created_at: 2023-08-31T11:28:53Z
updated_at: 2023-09-09T14:04:51Z
url: https://github.com/astral-sh/ruff/issues/7018
synced_at: 2026-01-10T11:09:49Z
```

# Add `extend-ignore-names` for `flake8-self`

---

_Issue opened by @jaap3 on 2023-08-31 11:28_

The [default](https://beta.ruff.rs/docs/settings/#flake8-self-ignore-names) for `ignore-names` is `["_make", "_asdict", "_replace", "_fields", "_field_defaults", "_name_", "_value_"]`.

For a Django project I want to **add**  `["_base_manager", "_default_manager",  "_meta"]`. This means I need to copy over the defaults, and then add my overrides. 

`pep8-naming` has a `extend-ignore-names` configuration option. Is it possible to provide a similar option for `flake8-self`?

---

_Label `configuration` added by @charliermarsh on 2023-08-31 13:29_

---

_Comment by @charliermarsh on 2023-08-31 13:30_

It would make sense to support this for consistency, though I'm also wondering if we should just add these to the defaults.

---

_Comment by @dhruvmanila on 2023-09-06 02:58_

I think I would go with adding a new config option as oppose to adding library specific names.

---

_Comment by @MichaReiser on 2023-09-06 06:14_

> I think I would go with adding a new config option as oppose to adding library specific names.

Another idea. What if users could configure their used frameworks and we would use that information to derive the most meaningful defaults? The configuration could even be optional in the future when ruff understands the project dependencies. 

---

_Comment by @dhruvmanila on 2023-09-09 14:04_

This was added in #7194

---

_Closed by @dhruvmanila on 2023-09-09 14:04_

---
