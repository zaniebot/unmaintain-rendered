---
number: 1924
title: "Error: wanted string or inline table for key `tool.ruff.target-version`"
type: issue
state: closed
author: thisfred
labels:
  - question
assignees: []
created_at: 2023-01-16T22:03:32Z
updated_at: 2023-01-17T18:28:55Z
url: https://github.com/astral-sh/ruff/issues/1924
synced_at: 2026-01-07T13:12:14-06:00
---

# Error: wanted string or inline table for key `tool.ruff.target-version`

---

_Issue opened by @thisfred on 2023-01-16 22:03_

Since today I'm suddenly getting this error when I run ruff (either standalone or under pre-commit):

```
Error: wanted string or inline table for key `tool.ruff.target-version`
```

The strange thing is, it makes no difference whether that string is present in my pyproject.toml, like:

```toml
[tool.ruff]
target-version = "py38"
```

or whether I remove it (or entirely remove pyproject.toml), and run either:

```bash
ruff --target-version=py38 some_file.py
```
or:
```bash
ruff some_file.py
```

I am entirely willing to believe I somehow messed up my virtual environment, but I've rebuilt it from scratch several times now, and also tried running ruff as part of pre-commit, which installs its own dependencies, so I'm really mystified.

I've tried going back versions, and it doesn't seem to make a difference either, always the same error.

On Friday this was all working for me. I did update to a newer version of ruff today, so I'm wondering if something changed, and that something is being cached outside of the python env even when I try to downgrade to older versions using pip.

versions I've tried:

0.0.223
0.0.219
0.0.209

---

_Comment by @thisfred on 2023-01-16 22:04_

I've tried running with `-v` or `--verbose` but no more information is output.

---

_Comment by @charliermarsh on 2023-01-16 22:07_

Very strange... What happens if you create a file like:

```toml
[tool.ruff]
target-version = "py38"
```

And then run explicitly with `ruff --config ./pyproject.toml some_file.py`?

---

_Comment by @thisfred on 2023-01-16 22:17_

Explicitly passing the config file does seem to work, so that's a  workaround, but I'm still completely baffled as to why it doesn't work with the default file location. (I am sure it is able to find the file there, because when in desperation I changed the option name to `target_version` (with an underscore instead of a dash), it complained that that option doesn't exist...

---

_Comment by @thisfred on 2023-01-16 22:32_

As another data point, if I pass pyproject.toml explicitly with --config, it doesn't complain, even when `target-version` is not set in that file at all, and additionally, if I name the file anything other than `pyproject.toml`, it fails with 

```
Unrecognized settings file: `/path/to/project/mypyproject.toml`
```

no matter the contents of that file.


---

_Comment by @charliermarsh on 2023-01-16 22:35_

(Yeah I believe we rely on the name, because we have to distinguish between `ruff.toml` and `pyproject.toml` files.)

---

_Comment by @charliermarsh on 2023-01-16 22:37_

Is it possible that you have a "user" pyproject.toml file in the global config dir specified [here](https://github.com/dirs-dev/directories-rs#basedirs)?

---

_Label `question` added by @charliermarsh on 2023-01-17 12:39_

---

_Comment by @thisfred on 2023-01-17 17:39_

I don't think so: I do have one, one directory level up from the project I'm working in, but that's above the git root, yet still below my home directory, and not in one of the locations specified there. I'll do some more digging.

What I wonder is if it is somehow related to `target-version` being specified in other (non-ruff) stanzas in the pyproject.toml. Black has its own key by that name, which takes an array rather than a string, for instance, but I would expect ruff's parser to ignore everything that doesn't start with `[tool.ruff...]`, so that's probably also not it.

---

_Comment by @thisfred on 2023-01-17 17:42_

oh, ok, actually your suggestion was correct. With the following directory structure:

```
~/
  all_projects/
    pyproject.toml
    the_project/
      .git
      pyproject.toml
```

the pyproject.toml one level above the working directory does seem to get picked up when running ruff from within `the_project/`.

---

_Comment by @thisfred on 2023-01-17 17:43_

Maybe this is intended behavior and I can see it being useful in some cases, but it did surprise me a little. Thanks for your patience in helping me figure this out, I appreciate it, and all your work on this amazing tool.

---

_Comment by @charliermarsh on 2023-01-17 18:28_

Oh, ok, great! (And thanks for the kind words :))

Yeah, that behavior _is_ intended. When we check a given file, we find the first `pyproject.toml` in the same directory, or any parent directory.

I totally understand how that could be surprising. It does have some benefits though. For one thing, it makes it possible to run Ruff across a bunch of separate Python projects with a single `ruff` invocation, since each file will use the "right" `pyproject.toml`. It also means that you can run `ruff` from any directory, and it'll lint any given file the same way (which wouldn't be true if, e.g., we used the `pyproject.toml` from the current working directory).

There's a little bit more on that strategy in [the docs](https://github.com/charliermarsh/ruff#pyprojecttoml-discovery). As always, suggestions welcome on how to make it clearer or less surprising :)


---

_Closed by @charliermarsh on 2023-01-17 18:28_

---
