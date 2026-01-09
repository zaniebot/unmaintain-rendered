---
number: 3127
title: "Add `flake8-obey-import-goat`"
type: issue
state: open
author: bellini666
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-02-22T15:07:46Z
updated_at: 2023-07-10T01:26:06Z
url: https://github.com/astral-sh/ruff/issues/3127
synced_at: 2026-01-07T13:12:14-06:00
---

# Add `flake8-obey-import-goat`

---

_Issue opened by @bellini666 on 2023-02-22 15:07_

Ref: https://pypi.org/project/flake8-obey-import-goat

---

_Label `plugin` added by @charliermarsh on 2023-02-22 15:24_

---

_Comment by @henryiii on 2023-02-23 02:31_

Isn't this the same thing as TID? https://beta.ruff.rs/docs/rules/#flake8-tidy-imports-tid ?

---

_Comment by @charliermarsh on 2023-02-23 02:32_

Yeah this looks identical to [`banned-api`](https://beta.ruff.rs/docs/rules/banned-api/).

---

_Comment by @bellini666 on 2023-02-23 15:15_

> Yeah this looks identical to [`banned-api`](https://beta.ruff.rs/docs/rules/banned-api/).

Not necessarily. What it it does it to ensure that some imports are done in a specific way.

For example, if you want for someone to always `import os` and use `os.path` instead of `from is import path`.

The `banned-api`, if I understood that correctly, will totally prevent it from being used

---

_Comment by @JonathanPlasse on 2023-02-23 16:16_

With `banned-api`, you cannot specify with a glob which modules are concerned for the ban.

---

_Comment by @JonathanPlasse on 2023-02-23 16:26_

The configuration of `flake8-obey-import-goat` look like this:

```ini
[flake8]
forbidden-imports =
    *: datetime.datetime, stdlib modules should be imported as a module
    *: typing.Optional, we use T | None instead of Optional[T]
    users.*: foo.*, users module should not use foo module
    *.implementation.*: *.domain.*, implementation layer should not use domain layer
```

With the `banned-api` plugin, the configuration would look like this:

```toml
[tool.ruff.flake8-tidy-imports.banned-api]
"datetime.datetime".msg = "stdlib modules should be imported as a module"
"typing.Optional".msg = "we use T | None instead of Optional[T]"
"foo.*".msg = "users module should not use foo module"
"*.domain.*".msg = "implementation layer should not use domain layer"
```

`banned-api`, could be extended by adding a `path` like this, by default would be `*`.

```toml
[tool.ruff.flake8-tidy-imports.banned-api]
"datetime.datetime".msg = "stdlib modules should be imported as a module"
"typing.Optional".msg = "we use T | None instead of Optional[T]"
"foo.*".msg = "users module should not use foo module"
"foo.*".path = "users.*"
"*.domain.*".msg = "implementation layer should not use domain layer"
"*.domain.*".path = "*.implementation.*"
```


---

_Comment by @JonathanPlasse on 2023-02-23 17:00_

Is a pull request welcome with this new behavior?
It would be backward compatible.
How should we handle that this is a merge of two plugins in the documentation?
Should we mention `flake8-obey-import-goat` in `flake8-tidy-imports.banned-api`?
Or, should we only add the license `flake8-obey-import-goat`?

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---
