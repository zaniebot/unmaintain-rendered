---
number: 1220
title: "`exclude` and `extend-exclude` options seem ignored when running with pre-commit"
type: issue
state: closed
author: WilliamJamieson
labels: []
assignees: []
created_at: 2022-12-12T20:55:08Z
updated_at: 2024-11-01T18:51:23Z
url: https://github.com/astral-sh/ruff/issues/1220
synced_at: 2026-01-10T01:22:39Z
---

# `exclude` and `extend-exclude` options seem ignored when running with pre-commit

---

_Issue opened by @WilliamJamieson on 2022-12-12 20:55_

`ruff` is really awesome, and I have begun to use it in many of my projects as it is orders of magnitude faster than `flake8` and others like it.

In one of my projects I tried to add `ruff` to be run by pre-commit, see spacetelescope/jwst#7395. This project has a list of directories to be excluded listed in its `pyproject.toml` file. However, when running `ruff` via pre-commit `ruff` found errors in files within these excluded directories, despite trying both `exclude` and `extend-exclude` in the `pyproject.toml`. The only solution I found was to additionally exclude these directories in the `.pre-commit-config.yaml` for the `ruff` hook, which is not ideal.

Is this a user error or a bug in `ruff` (or its pre-commit hook)?

---

_Comment by @charliermarsh on 2022-12-12 20:56_

Let me take a look...

---

_Comment by @charliermarsh on 2022-12-12 21:04_

I think the problem is that `ruff` still checks files if they're passed directly, e.g.:

```
~/workspace/ruff on * main
∴ cargo run jwst/jwst/fits_generator/
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff jwst/jwst/fits_generator/`

~/workspace/ruff on * main
∴ cargo run jwst/jwst/fits_generator/main.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff jwst/jwst/fits_generator/main.py`
Found 3 error(s).
jwst/jwst/fits_generator/main.py:31:24: F401 `astropy.io.fits` imported but unused
jwst/jwst/fits_generator/main.py:115:17: E722 Do not use bare `except`
jwst/jwst/fits_generator/main.py:280:20: F401 `docutils` imported but unused
2 potentially fixable with the --fix option.
```

And the way pre-commit works is that it passes all changed files (IIRC) to the command directly.

I _think_ this is the same behavior as Black. If I add this to `pyproject.toml`:

```
[tool.black]
exclude = 'jwst/fits_generator'
```

Then I see:

```
∴ black jwst/fits_generator
No Python files are present to be formatted. Nothing to do 😴
~/workspace/ruff/jwst on * feature/pre-commit
∴ black jwst/fits_generator/generators.py
reformatted jwst/fits_generator/generators.py

All done! ✨ 🍰 ✨
1 file reformatted.
```

So not 100% sure how to resolve, need to look into the options...


---

_Comment by @charliermarsh on 2022-12-12 21:07_

There's a workaround [here](https://github.com/psf/black/issues/438#issuecomment-424813931) that's discouraged but running Ruff over your codebase is fast enough that I personally think this is ok.

---

_Comment by @charliermarsh on 2022-12-12 21:07_

Otherwise, I do think you have to duplicate the exclusions in the hook definition, like in the [Black example](https://github.com/psf/black/issues/438#issue-348511789):

```
repos:
-   repo: https://github.com/ambv/black
    rev: stable
    hooks:
    - id: black
      language_version: python3.6
      exclude: migrations
```

---

_Comment by @WilliamJamieson on 2022-12-12 21:18_

I know from experience with `black` in `astropy` that [`force-exclude`](https://github.com/astropy/astropy/blob/d31499bb3d4d4cf02981c9b7fdd35bc37c62ad54/pyproject.toml#L49-L63) does work as expected for `astropy`'s [`.pre-commit-config.yaml`](https://github.com/astropy/astropy/blob/d31499bb3d4d4cf02981c9b7fdd35bc37c62ad54/.pre-commit-config.yaml#L82-L85).

I don't think `ruff` has a `force-exclude` configuration option right now. However, that might be one way to handle this case. 

---

_Comment by @charliermarsh on 2022-12-12 21:30_

Oh cool. We could probably support that...

---

_Comment by @charliermarsh on 2022-12-13 03:59_

I guess the downside of `force-exclude` (apart from added complexity / a thing to implement + support) is that you're kind of providing an escape hatch that goes against the intention of the tool ("ruff path/to/file should check file no matter what"), in order to workaround a limitation in another tool. But I do see the dilemma...


---

_Comment by @WilliamJamieson on 2022-12-13 20:44_

 > I guess the downside of `force-exclude` (apart from added complexity / a thing to implement + support) is that you're kind of providing an escape hatch that goes against the intention of the tool ("ruff path/to/file should check file no matter what"), in order to workaround a limitation in another tool. But I do see the dilemma...

One compromise to this issue would be to have a flag (`--no-exclude`?) which would prevent `ruff` from considering any of the set exclusion criteria.

I imagine the discussion in psf/black#438 and psf/black#1032 may shed light on why `force-exclude` maybe useful.



---

_Comment by @charliermarsh on 2022-12-20 00:49_

@WilliamJamieson - I'm going to make `force-exclude` a boolean flag. That way you can keep using `exclude`, but pass `force-exclude` as an argument to the pre-commit hook.

---

_Referenced in [astral-sh/ruff#1295](../../astral-sh/ruff/pulls/1295.md) on 2022-12-20 01:08_

---

_Closed by @charliermarsh on 2022-12-20 01:09_

---

_Referenced in [fpgmaas/cookiecutter-poetry#81](../../fpgmaas/cookiecutter-poetry/issues/81.md) on 2023-02-14 12:28_

---

_Comment by @egeres on 2024-11-01 18:51_

I had this issue because I was only adding `include` to the dirs I wanted to check. As @charliermarsh said, the pre-commit ignores that and feeds ruff with files that shouldn't. Adding an `exclude` list fixed this issue for me. So I went from this:

```toml
include = ["app/**/*.py", "tests/**/*.py", "examples/**/*.py"]
```

to this:

```toml
exclude = ["sketches/**"]
```

---
