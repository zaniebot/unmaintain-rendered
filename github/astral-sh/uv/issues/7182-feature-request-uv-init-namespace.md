---
number: 7182
title: "Feature request: `uv init --namespace`"
type: issue
state: open
author: Afoucaul
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-09-07T21:44:33Z
updated_at: 2025-07-08T06:07:51Z
url: https://github.com/astral-sh/uv/issues/7182
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature request: `uv init --namespace`

---

_Issue opened by @Afoucaul on 2024-09-07 21:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I mostly work in monorepo/workspace set-ups. In that context, I like to create all my packages under a common namespace - say `org`.
When I run `uv init --lib libs/foo`, I really want to create a lib named `org-foo`, with all the code under `src/org/foo`.
But because `uv init --lib` uses a standard layout, it creates a placeholder lib directly under `src/foo`, so I have to manually create `src/org`, and move `foo/` there.

In a monorepo, creating a lib is a pretty common task, so this can become tedious.
I see two options: a new `--namespace` flag (eg `uv init --namespace org`), and/or a `namespace` section in the `pyproject.toml` (that would make `uv init` automatically use the right structure).

---

_Renamed from "Feature request: namespace management" to "Feature request: `uv init --namespace`" by @Afoucaul on 2024-09-07 21:44_

---

_Comment by @inoa-jboliveira on 2024-09-08 18:38_

I was thinking about the need of "uv init" templates and stumble upon your issue.

I think it is also doable with something like that. It would be a mix of `uvx` with `uv init`

E.g. This would both initialize pyproject.toml from uv and run django-admin startproject (plus `uv add django`) 
```
uv init --with django@4.2 myproject
```

Simillarly 
```
uv init --with namespace lib123
```

Where anyone could develop this "namespace" lib, push to pypi and it have the config you need. 

This is the sort of application where there is not much benefit from having the code written in rust for speed and removes the burden of astral team supplying every single variant of project initialization in the market 


---

_Label `enhancement` added by @charliermarsh on 2024-09-16 02:54_

---

_Label `cli` added by @charliermarsh on 2024-09-16 02:54_

---

_Comment by @tpanum on 2024-12-23 13:16_

@Afoucaul: Until a more suitable solution is available, how do you handle this currently?

I ideally wish to do the same as you, but wouldn't want to enforce the use of `setuptools` as a build system to discover the namespace correctly.

---

_Referenced in [astral-sh/uv#11787](../../astral-sh/uv/issues/11787.md) on 2025-03-12 23:54_

---

_Referenced in [astral-sh/uv#10960](../../astral-sh/uv/issues/10960.md) on 2025-07-08 06:04_

---

_Comment by @Spenhouet on 2025-07-08 06:07_

In Poetry this is supported via poetry-multiproject-plugin and the `--with-top-namespace` option.

---
