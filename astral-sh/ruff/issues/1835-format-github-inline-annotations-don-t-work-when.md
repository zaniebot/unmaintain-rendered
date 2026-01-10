```yaml
number: 1835
title: "`--format=github` inline annotations don't work when running in a subdirectory"
type: issue
state: closed
author: jbbakst
labels:
  - bug
assignees: []
created_at: 2023-01-12T22:14:03Z
updated_at: 2023-10-09T14:35:04Z
url: https://github.com/astral-sh/ruff/issues/1835
synced_at: 2026-01-10T11:09:44Z
```

# `--format=github` inline annotations don't work when running in a subdirectory

---

_Issue opened by @jbbakst on 2023-01-12 22:14_

Issue:
I have a multi-language monorepo with a Github Action to lint on `pull_request`. Since ruff is running inside a subdirectory, Github doesn't properly resolve file names to display inline annotations.

Potential fix:
Update file names in output to be absolute instead of relative paths

`ruff --version`
0.0.219

Repro:
```
name: CI
on: push
jobs:
  build:
    runs-on: ubuntu-latest

    defaults:
        run:
            working-directory: subdir

    steps:
      - uses: actions/checkout@v3
      - name: Install Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.11"
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install ruff
      # Include `--format=github` to enable automatic inline annotations.
      - name: Run Ruff
        run: ruff --format=github .
```

---

_Comment by @charliermarsh on 2023-01-12 22:36_

Yeah we can probably always use absolute paths for `--format=github`. In the meantime, you might be able to invoke Ruff from outside the subdirectory? We don't rely on the current working directory to pick up configuration, so `ruff subdir` should be identical to what you have above.

---

_Label `bug` added by @charliermarsh on 2023-01-12 22:36_

---

_Comment by @charliermarsh on 2023-01-12 22:36_

But, I can change this pretty easily.

---

_Comment by @charliermarsh on 2023-01-12 22:37_

Actually, I'm not really familiar -- does GitHub respect absolute paths? What does the path even look like on Actions? :) 

---

_Comment by @jbbakst on 2023-01-12 22:42_

Yeah I think it should respect absolute paths — ESLint outputs absolute paths and the inline annotations work out of the box, even in a subdirectory.

Re: running outside the subdirectory, in my actual setup I'm using poetry to ensure that package versions are the same in CI as they are in my dev environment, so this gets a bit tricky since my `pyproject.toml` and `poetry.lock` files are both located in the subdirectory. I guess for now I could just relax that requirement, install ruff directly (risking some version drift), and run it from outside the subdirectory while I wait for an update :)

---

_Comment by @jbbakst on 2023-01-12 22:47_

> What does the path even look like on Actions?

Just ran the following action:
```
name: pwd

on:
    - push

jobs:
    pwd:
        name: pwd
        runs-on: ubuntu-latest
        steps:
            - run: pwd
```

And got this (replace `<repo-name>` with the name of the repo):
`/home/runner/work/<repo-name>/<repo-name>`

---

_Comment by @charliermarsh on 2023-01-12 22:50_

Ok cool. Regardless, I'll fix this.

---

_Comment by @charliermarsh on 2023-01-12 22:50_

(Thanks for trying that.)

---

_Closed by @charliermarsh on 2023-01-12 22:54_

---

_Comment by @jbbakst on 2023-01-12 22:57_

Amazing, thank you! 🙌

---

_Comment by @charliermarsh on 2023-01-12 23:01_

No prob! This will go out in v0.0.220 which will be kicked off in a sec (so should be available within, IDK, an hour or so?).

I don't personally use Ruff within GitHub Actions right now just given where I'm spending my time (lots of Rust!), so definitely appreciate feedback on this kind of stuff! And of course, if this fix doesn't work for you or as intended, just LMK.


---

_Comment by @levrik on 2023-10-09 14:26_

~Mhh. Absolute paths don't seem to work. I manually echoed examples with absolute and with relative path and only the relative one worked.~

Seems to be related to my specific setup of running Ruff inside Docker instead of on the host system directly. This obviously yields different absolute paths which GitHub isn't able to map. Relative paths are working fine in this case.

I wonder if Ruff could switch to relative paths if `GITHUB_ACTIONS` env var isn't available and output absolute ones if it's not?
In the end paths must be absolute on the GitHub Actions host system or relative to `GITHUB_WORKSPACE` env var. Maybe this could also be used. If `GITHUB_WORKSPACE` is available make paths relative to this directory, if not simply output relative paths from working directory as a best guess.

---
