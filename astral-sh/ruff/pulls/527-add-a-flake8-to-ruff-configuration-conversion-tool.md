```yaml
number: 527
title: Add a Flake8-to-Ruff configuration conversion tool
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/flake8
created_at: 2022-10-31T14:41:42Z
updated_at: 2022-11-02T02:27:40Z
url: https://github.com/astral-sh/ruff/pull/527
synced_at: 2026-01-12T05:48:45Z
```

# Add a Flake8-to-Ruff configuration conversion tool

---

_Pull request opened by @charliermarsh on 2022-10-31 14:41_

This PR adds a `flake8_to_ruff` command to generate a Ruff `pyproject.toml` section from an existing Flake8 configuration file (setup.cfg, tox.ini, or .flake8).

See: #414, #423.


---

_Comment by @charliermarsh on 2022-10-31 14:42_

This needs tests, _and_ a strategy for actually distributing this tool. (It's just an example script in the repo right now.)


---

_@charliermarsh reviewed on 2022-10-31 14:42_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/parser.rs`:126 on 2022-10-31 14:42_

Wow, this was shockingly complicated, I guess it stems from INI being such a limited format. (This is basically a 1-to-1 port of the code from Flake8 for parsing the per-file-ignores.)

---

_Comment by @charliermarsh on 2022-10-31 14:48_

@fsouza - Do you have any opinion on whether this should be distributed as...

1. It's own PyPI package.
2. A subcommand on `ruff`.
3. A separate binary that's part of the same PyPI package (I don't know if this is possible).

---

_Comment by @charliermarsh on 2022-10-31 15:34_

(Merging for now, will expose in a separate PR.)

---

_Merged by @charliermarsh on 2022-10-31 15:34_

---

_Closed by @charliermarsh on 2022-10-31 15:34_

---

_Branch deleted on 2022-10-31 15:34_

---

_Comment by @fsouza on 2022-10-31 15:48_

> @fsouza - Do you have any opinion on whether this should be distributed as...
> 
>     1. It's own PyPI package.
> 
>     2. A subcommand on `ruff`.
> 
>     3. A separate binary that's part of the same PyPI package (I don't know if this is possible).

Oh that's an interesting one. I'm thinking a separate PyPI package since the common use case will likely be a one-off (or people like me that can't really force everyone in the company to migrate and have to wrap it in something like flake8-ruff-wrapper lol).

The reason I'm thinking that a separate PyPI package is better is because not everyone that uses ruff will use this tool? What do you think?

---

_Comment by @charliermarsh on 2022-10-31 15:51_

Yeah that makes sense. I do want to split ruff into subcommands, but even then it's probably not worth including this as a subcommand on ruff itself given that it's more of a compatibility tool.

---

_Review comment by @fsouza on `src/flake8_to_ruff/mod.rs`:25 on 2022-11-02 02:22_

It looks like this should be `max-line-length`/`max_line_length` instead?

https://flake8.pycqa.org/en/latest/user/options.html#cmdoption-flake8-max-line-length



---

_@fsouza reviewed on 2022-11-02 02:22_

---

_@charliermarsh reviewed on 2022-11-02 02:27_

---

_Review comment by @charliermarsh on `src/flake8_to_ruff/mod.rs`:25 on 2022-11-02 02:27_

Thanks!

---
