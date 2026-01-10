---
number: 2769
title: "Allow `pyproject.toml` discovery to be disabled for subdirectories"
type: issue
state: closed
author: isaacharrisholt
labels:
  - configuration
assignees: []
created_at: 2023-02-11T16:28:52Z
updated_at: 2023-02-12T10:44:06Z
url: https://github.com/astral-sh/ruff/issues/2769
synced_at: 2026-01-10T01:22:41Z
---

# Allow `pyproject.toml` discovery to be disabled for subdirectories

---

_Issue opened by @isaacharrisholt on 2023-02-11 16:28_

## Context

I'm working on a project templating tool in Python, using Ruff for linting (love it, by the way!). It's using a `pyproject.toml` file for config, as I'm using Poetry for environment management.

I'm storing the templates in the same directory as the tool, and one of those templates is a generic Poetry project template with some pre-commit hooks and tooling set up. This, of course, uses Ruff and includes a `pyproject.toml` file with Ruff config.

## My dilemma

Essentially, every time I run Ruff in the root of the project, two `.ruff_cache` directories are created - one in the root, and one in the directory where the Poetry project template lies. This isn't ideal, as this template then includes a `.ruff_cache` directory, which gets copied into new projects when the tool is run.

My current workaround is to exclude `.ruff_cache` directories from the Poetry build, but it would be nice to have a way to prevent the hierarchical behaviour in subdirectories so that this doesn't happen. I'm aware that there are other ways of solving my problem, but having 'default' `pyproject.toml` files is not an uncommon thing, so it would be nice to see a solution implemented in Ruff, if possible.

## The solution

I'm not familiar with the Ruff source code, so I can't suggest a technical solution, but my suggestion would be something along the lines of adding the following config option to the `pyproject.toml`:

```toml
[tool.ruff]
disable_pyproject_discovery = true
```

This would disable `pyproject.toml` discovery only in subdirectories of this `pyproject.toml` which would allow people to disable the feature in specific parts of their application, if they so desire.

Let me know what you think!


---

_Label `configuration` added by @charliermarsh on 2023-02-11 16:59_

---

_Comment by @not-my-profile on 2023-02-12 03:12_

Doesn't `extend-exclude = ["templates/"]` work? (assuming your directory is called `templates/`) ... if this doesn't work I think we should make it work rather than introducing yet another config option.

---

_Comment by @isaacharrisholt on 2023-02-12 07:33_

Technically yes, but then I don't get linting in my templates! The package I'm writing allows arbitrary Python to be run upon rendering the template, and that Python is kept in the template directory. It would be great if that (and other Python in the directory) was linted.

---

_Comment by @not-my-profile on 2023-02-12 07:38_

Ah ... in that case I think it would be better if you'd just rename those template files e.g. `pyproject.toml.template` or  `pyproject.toml.jinja2`. Or alternatively you could move these `pyproject.toml` files to a subdirectory.

---

_Comment by @isaacharrisholt on 2023-02-12 07:42_

That makes sense, and I do understand that I have a very niche problem, but I can't imagine I'm the only person who would want to disable the default behaviour here?

Happy to close this issue if you disagree!

---

_Comment by @not-my-profile on 2023-02-12 07:54_

With this setting `ruff` would still use the `pyproject.toml` files when `ruff` was invoked in the subdirectory containing the file ... this might be a bit confusing.

If renaming the file works well for you, then I'd personally rather have us refrain from introducing yet another setting since every setting we add makes ruff more complex and also increases the mental load when deciding which settings to use.

---

_Comment by @isaacharrisholt on 2023-02-12 10:26_

No worries! I've got a workaround anyway, I just figured that not everyone will want automatic `pyproject.toml` discovery, and may prefer to have it disabled in part of their project.

---

_Closed by @isaacharrisholt on 2023-02-12 10:26_

---

_Comment by @not-my-profile on 2023-02-12 10:44_

If other people end up wanting the same feature, we can always reconsider this :)

---
