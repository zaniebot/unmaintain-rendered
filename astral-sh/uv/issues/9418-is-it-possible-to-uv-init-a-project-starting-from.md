---
number: 9418
title: "Is it possible to `uv init` a project starting from a template TOML?"
type: issue
state: open
author: NicholasPini
labels:
  - needs-design
  - needs-decision
assignees: []
created_at: 2024-11-25T14:18:20Z
updated_at: 2025-08-01T13:33:19Z
url: https://github.com/astral-sh/uv/issues/9418
synced_at: 2026-01-10T01:24:40Z
---

# Is it possible to `uv init` a project starting from a template TOML?

---

_Issue opened by @NicholasPini on 2024-11-25 14:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am in the process of creating a bunch of projects that all require the same index settings and `constraint-dependencies`. I was wondering it is possible to create a project with `uv init` and pass some kind of option to use a file as a template. I tried `--config-file` passing a `uv.toml` with the following content:
```
[[index]]
name = "some_index"
url = "some_url"
explicit = true

[sources]
some_dep = { index = "some_index" }

# see https://github.com/astral-sh/uv/issues/7703
constraint-dependencies = ["kaleido!=0.2.1.post1"]
```

but it did nothing. Note that using a workspace is not an option for me.


---

_Comment by @zanieb on 2024-11-25 22:34_

This isn't possible yet. Why isn't a workspace an option?

---

_Label `needs-design` added by @zanieb on 2024-11-25 22:34_

---

_Label `needs-decision` added by @zanieb on 2024-11-25 22:34_

---

_Comment by @ultrapoci on 2024-11-26 07:07_

I see. I'm not using a workspace because I need all projects to be independent from each other and with their own venv folder.

---

_Comment by @scanny on 2025-03-24 21:10_

Just some input on this for design purposes. Somewhat different use-case I suppose, happy to move if there's a better spot for it:

I find myself lately **creating a lot of small projects**; probably has something to do with AI tools like Cursor allowing creating a project to explore something instead of just using a free-standing script. Even though they're small I want `uv` (already indispensable) and Git, etc., since these "side projects" have a habit of growing beyond initial expectations.

When I create a new project, I find myself always **copying `ruff`, `pyright`, and `pytest` settings into `pyproject.toml` from a prior project** since I use mostly the same settings on most all my projects; line length, strict mode, which lints to use/exclude, etc.

So:
> As a developer frequently using `uv` to create a new project
> I would like to configure `pyproject.toml` settings to be added to each new project during `uv init`
> Such that hand-customization of a new project with routine details is minimized

I'm sure there are other settings that would be worthwhile to "templatize", but this is the aspect that I see drawing my attention most frequently in recent experience.

---

_Comment by @NicholasPini on 2025-03-25 09:04_

Yeah, this would be really useful. uv is great for managing many small projects, even when not using workspaces. Having some kind of `pytemplate.toml` in a parent directory and/or in the home config folder, which automatically gets picked up by uv init and adds the configuration from `pytemplate.toml`, can be useful. 

---

_Comment by @tim-hilde on 2025-04-22 08:23_

Maybe not the ideal solution, but I have been using a [cookiecutter](https://github.com/cookiecutter/cookiecutter) template to have this functionality: https://github.com/tim-hilde/dotfiles/tree/main/cookiecutter/python_minimal

---

_Comment by @xmercerweiss on 2025-06-09 21:42_

Bump. Just started using uv, immediately looked for this feature; sad it doesn't exist

---

_Comment by @AndreaRiboni on 2025-08-01 13:29_

I second this

---
