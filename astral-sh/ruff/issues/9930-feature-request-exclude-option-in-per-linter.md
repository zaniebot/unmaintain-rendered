```yaml
number: 9930
title: "[Feature Request] Exclude option in per-linter configuration"
type: issue
state: closed
author: AAraKKe
labels:
  - question
assignees: []
created_at: 2024-02-11T13:45:45Z
updated_at: 2024-02-25T11:39:56Z
url: https://github.com/astral-sh/ruff/issues/9930
synced_at: 2026-01-10T11:09:52Z
```

# [Feature Request] Exclude option in per-linter configuration

---

_Issue opened by @AAraKKe on 2024-02-11 13:45_

## Description
Hi team! I started with Ruff a couple of days ago for a side project, and I love it. Not only because of how fast it is but also because it removes the need to maintain several configuration files since now everything can live in the `pyproject.toml` file of the project (I am looking to you, flake8...).

I have been looking into the configuration and have not found the option I am asking for here, if it is there, forgive me for the issue. Just point in the right direction, and thanks a lot! 

I am having an issue with isort and the lack of `exclude` option at a per-linter level. We have an `exclude` top-level option to tell Ruff to exclude given files from everything. Then we have a dedicated lint `exclude` option so we can decide what files not to lint. However, given that Ruff provides so many _linters_ when linting, it would be nice if we could also have exclude for each one of them. When running each feature, each one would run on any file that is not excluded by a combination of any `exclude` up to its node.

I am saying this because, while isort is great, imports sometimes can be sensitive to sorting. I know that is not ideal, and modules should be importable in a stand-alone manner, but there are use cases where importing things in a `__init__` in a certain way is done on purpose for specific reasons. I wouldn't like to exclude `__init__` files from linting completely, but I would love to be able to not run isort in them.

## Example use case
As an example, I am building an app in which there are hundreds of handlers. These handlers then need to be registered on the app, and we have built it in such a way that just defining the handler in the package and decorating it with a `Registry.register` method registers them. This removes the need to keep registering handlers. However, in order for this to happen automagically, the init needs to be written with imports in such a way that when the handler is imported, the decorator already exists to avoid circular dependencies.

This is known and documented in the project and everyone is onboard with the solution as it saves a huge amount of boilerplate, but we need to disable linting in `__init__` files to avoid isort breaking this feature.


Thanks a lot!

---

_Renamed from "[Feature Request] Exclude option for isort configuration" to "[Feature Request] Exclude option in per-linter configuration" by @AAraKKe on 2024-02-11 13:56_

---

_Comment by @charliermarsh on 2024-02-11 14:51_

This is sort of the intent of [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) -- have you tried that? E.g., you can disable the isort rules in `__init__.py` files with:

```toml
[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["I"]
```

---

_Label `question` added by @charliermarsh on 2024-02-11 14:52_

---

_Comment by @AAraKKe on 2024-02-12 19:23_

Actually I tried it but I thought it was not working as expected. But it does! I was validated whether the VSCode extension reports something to validate the configuration and for some reason the extension misses this configuration. Is it because there are options the extension does not recognize?

The block of imports appears underlined as if it needed to be formated but if I run Ruff it is not sorted, as expected.

---

_Comment by @MichaReiser on 2024-02-23 21:28_

I'm glad to hear that `per-file-ignores` works for you. 

The VS Code extension supports all configuration settings.  Can you share a screenshot of what you're seeing? Do you have the `isort` plugin (not ruff, the standalone isort plugin) installed and it conflicts with ruff's settings?

---

_Comment by @AAraKKe on 2024-02-25 11:10_

Nevermind! All good, I just noticed when answering to your question that the `per-file-ignores` seem to have been removed from `pyproject.toml` by a coworker for some reason. If I add it back it is all working fine. Not sure what happened there. I will figure it out with him, thanks a lot!

---

_Comment by @MichaReiser on 2024-02-25 11:39_

Thanks for reporting back. I'll close the issue but feel free to comment again or to open a new issue if you have more questions. 

---

_Closed by @MichaReiser on 2024-02-25 11:39_

---
