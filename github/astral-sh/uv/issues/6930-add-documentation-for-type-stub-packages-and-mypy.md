---
number: 6930
title: Add documentation for type stub packages and mypy invocation
type: issue
state: open
author: KDruzhkin
labels:
  - documentation
  - question
assignees: []
created_at: 2024-09-02T09:02:45Z
updated_at: 2025-09-22T10:20:03Z
url: https://github.com/astral-sh/uv/issues/6930
synced_at: 2026-01-07T13:12:17-06:00
---

# Add documentation for type stub packages and mypy invocation

---

_Issue opened by @KDruzhkin on 2024-09-02 09:02_

Type stubs can be provided in separate packages. For example, a package `foo` has a companion package `types-foo` or `foo-stubs`.

With poetry, a typical _pyproject.toml_ file looks like this:

```toml
[tool.poetry.dependencies]
pandas = ">=2.2.2"
psutil = ">=6.0.0"
tqdm = "^4.66.0"

[tool.poetry.group.dev.dependencies]
mypy = ">=1.10.0"
pandas-stubs = ">=2.2.2"
types-psutil = ">=6.0.0"
types-tqdm = ">=4.66.0"
```

So, with poetry, the development virtual environment contains both `mypy` and the stub packages (used by `mypy` to check the project).

How should I handle stub packages with `uv`? Do they belong in the tool environment for `mypy`? If yes, how do I put them there?


---

_Label `documentation` added by @charliermarsh on 2024-09-02 17:16_

---

_Comment by @charliermarsh on 2024-09-02 17:16_

Good question, we can document this. In general MyPy and the type stubs should all be in `[tool.uv.dev-dependencies]`.

---

_Label `question` added by @charliermarsh on 2024-09-02 17:16_

---

_Comment by @KDruzhkin on 2024-09-03 07:45_

So, `mypy` is different from most other tools: it does not work well in isolation.

To be precise, _if_ you want `mypy` to follow imports:

```toml
[tool.mypy]
follow_imports = "normal"
```
... then `uv tool run mypy` is not the way to go.

---

_Comment by @zanieb on 2024-09-03 17:35_

Yeah this is explicitly noted at https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run — do we need to provide more context somewhere?

---

_Comment by @KDruzhkin on 2024-09-04 14:51_

> Yeah this is explicitly noted at https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run — do we need to provide more context somewhere?

For any other tool, I would say, no.

But `mypy` is not _a_ tool among many, it is _the_ tool for type checking, and it deserves a section of its own.

---

_Comment by @zanieb on 2024-09-04 14:53_

We do literally mention `mypy` there though. I'm not quite sure a section that says "mypy is a dev dependency" makes sense for the project documentation. Maybe we could do an integration guide for it.

---

_Comment by @KDruzhkin on 2024-09-04 15:05_

Hmm…

It would be nice if [the tools section](https://docs.astral.sh/uv/concepts/tools/) _started_ with something like:

“… there are two general categories of tools: 1) tools which work in isolation, e.g. `ruff`, and 2) tools which require access the development environment, e.g. `mypy`, `pytest`. For the first category `uv` includes a dedicated interface…”

Yes, the docs say just so, but they say it at the _end_ of the section, under the subtle “Relationship to uv run”.

---

_Comment by @KDruzhkin on 2024-09-04 15:06_

Anyway, thanks for your fantastic work on `ruff` and `uv`!

---

_Assigned to @zanieb by @zanieb on 2024-09-04 15:09_

---

_Comment by @zanieb on 2024-09-04 15:09_

That makes sense, I think. I will look into that.

---

_Comment by @lgarron on 2024-11-19 12:10_

I was also very, very confused by this. Right now, the documentation gives me the expectation that I can run:

```shell
uv tool install mypy # optional
uv tool run mypy .
```

This tells you to run `mypy --install-types`, which works but does nothing.

But it seems I need to run:

```shell
uv add --dev mypy
uv tool run mypy .
```

It would be really nice for the documentation to spell this out, and to explicitly mention this for `mypy`.

(Even greater would be if `uv` could somehow abstract this kind of unhermetic ickiness and add a mechanism for `uv tool run mypy .` to work. At this point, I'd be happy even if it's just a hack for `mypy` explicitly. A caution message to the effect of "hey, we're running an abstraction layer to do what you probably do want, here's a link if you want the gnarly deets" would be more than enough for me. I'd much rather have my tools do the right things out of the box to contain `mypy`'s abstraction leakage, than to keep track of ecosystem quirks by hand. 😅)

---

_Renamed from "type stub packages (a documentation request)" to "Add documentation for type stub packages and mypy invocation" by @zanieb on 2025-01-07 20:14_

---

_Comment by @zanieb on 2025-01-07 20:16_

I think we could add a mypy integration guide that covers proper usage.

---

_Comment by @robsdedude on 2025-09-22 10:20_

Side note: there's also https://docs.astral.sh/uv/guides/tools/#requesting-extras with uses examples like 

> ```
> uvx --from 'mypy[faster-cache,reports]' mypy --xml-report mypy_report
> ```

I get that it's just an example, but people like to copy examples and that will lead to them ending up with using `uvx` for invoking `mypy` exactly as not recommended.

---
