```yaml
number: 2034
title: "`exclude` looks at path components beyond \"project root\" (née does `--force-exclude` work like it should?)"
type: issue
state: closed
author: akx
labels:
  - core
assignees: []
created_at: 2023-01-20T16:13:57Z
updated_at: 2023-02-03T13:24:26Z
url: https://github.com/astral-sh/ruff/issues/2034
synced_at: 2026-01-10T11:09:44Z
```

# `exclude` looks at path components beyond "project root" (née does `--force-exclude` work like it should?)

---

_Issue opened by @akx on 2023-01-20 16:13_

I don't think `--force-exclude` does what it should; this is especially obvious when using `pre-commit-ruff` (which is supposed to be used with that flag).

In a repository where there are no excludes, or extend-excludes, running `ruff --force-exclude .` claims there are no Python files. AFAIU this shouldn't happen since nothing is excluded. This happens with `--isolated` too.

```shell
~/b/ruff-force-exclude-test $ ruff --force-exclude --isolated foo/a.py
warning: No Python files found under the given path(s)
~/b/ruff-force-exclude-test $ ruff --isolated foo/a.py
foo/a.py:1:8: F401 `time` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.
~/b/ruff-force-exclude-test $
```

- A minimal code snippet that reproduces the bug:
  - https://github.com/akx/ruff-force-exclude-test
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag:
  - `ruff --force-exclude --isolated foo/a.py` (or without `--isolated`)
- The current Ruff settings (any relevant sections from your `pyproject.toml`):
  - ```toml
    [tool.ruff]
    target-version = "py37"
    select = ["F401"]
    ```
- The current Ruff version (`ruff --version`):
  - `ruff 0.0.227` (versions before 0.0.204 don't show the warning, but they also don't do anything).


---

_Comment by @charliermarsh on 2023-01-20 16:23_

This may be a regression. I can take a look later today.

---

_Comment by @charliermarsh on 2023-01-20 16:30_

(Certainly looks like a regression based on the summary :))

---

_Comment by @akx on 2023-01-20 16:36_

Okay! It looks like this may have regressed [between 0.0.191 and 0.0.192](https://github.com/charliermarsh/ruff/compare/v0.0.191...v0.0.192), and 0.0.204 adds the warning:

```fish
$ for ruff in (seq 185 227); pip install --quiet ruff==0.0.$ruff; ruff --version; ruff --force-exclude foo/a.py; end
ruff 0.0.185
error: Found argument '--force-exclude' which wasn't expected, or isn't valid in this context
ruff 0.0.186
error: Found argument '--force-exclude' which wasn't expected, or isn't valid in this context
ruff 0.0.187
error: Found argument '--force-exclude' which wasn't expected, or isn't valid in this context
ruff 0.0.188
error: Found argument '--force-exclude' which wasn't expected, or isn't valid in this context
ruff 0.0.189
Found 1 error(s).
foo/a.py:1:8: F401 `time` imported but unused
1 potentially fixable with the --fix option.
ruff 0.0.190
Found 1 error(s).
foo/a.py:1:8: F401 `time` imported but unused
1 potentially fixable with the --fix option.
ruff 0.0.191
foo/a.py:1:8: F401 `time` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.
ruff 0.0.192
ruff 0.0.193
ruff 0.0.194
ruff 0.0.195
ruff 0.0.196
ERROR: No matching distribution found for ruff==0.0.197
ruff 0.0.198
ruff 0.0.199
ruff 0.0.200
ruff 0.0.201
ruff 0.0.202
ruff 0.0.203
ruff 0.0.204
warning: No Python files found under the given path(s)
ruff 0.0.205
warning: No Python files found under the given path(s)
ruff 0.0.206
warning: No Python files found under the given path(s)
^C
```

---

_Comment by @charliermarsh on 2023-01-20 16:59_

Thanks! I'll be able to take a look at this in an hour or so. If it's indeed a regression, I'll fix and include in a release this afternoon :)

---

_Comment by @charliermarsh on 2023-01-20 17:52_

This actually _does_ work for me, even in your test repo (which is awesome, and hugely appreciated).

Is it possible that you have a global `.gitignore` or something that's getting in the way? E.g., if I set `~/.gitignore` as my global ignore, and add `foo` to it, I see the behavior you're describing.

---

_Label `question` added by @charliermarsh on 2023-01-20 17:52_

---

_Comment by @akx on 2023-01-20 18:33_

Well, I do have a global `excludesfile`, but that shouldn't matter here considering the contents...

```
$ git config --global --list | grep ign
core.excludesfile=/Users/akx/.gitignore
$ cat /Users/akx/.gitignore
.DS_Store
.idea
```
Moving that file out of the way doesn't change things, so it's not that, anyway.

I'll dig a bit deeper, maybe add some debugging around how the files are enumerated, since I can (thankfully) repro this with my working copy of ruff too :)

Thanks for verifying that it's just me though! I can't repro this in a container either, now that you gave me the idea to :)

```shell
$ docker run -it python:3.11 bash
root@cf9034209872:/# git clone https://github.com/akx/ruff-force-exclude-test
root@cf9034209872:/# cd ruff-force-exclude-test/
root@cf9034209872:/ruff-force-exclude-test# pip install ruff
Successfully installed ruff-0.0.227
root@cf9034209872:/ruff-force-exclude-test# ruff --force-exclude .
foo/a.py:1:8: F401 `time` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.
root@cf9034209872:/ruff-force-exclude-test#
```

---

_Comment by @akx on 2023-01-20 18:43_

... oh _pfff_. 😂 

```
~/b/ruff-force-exclude-test (master) $ ruff --force-exclude --no-respect-gitignore --isolated --show-files --verbose foo/a.py
[2023-01-20][20:39:28][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-20][20:39:28][ruff::resolver][DEBUG] Ignored path via `exclude`: "/Users/akx/build"
warning: No Python files found under the given path(s)
```

I keep my projects under `/Users/akx/build` (for legacy raisins); the `~/b/` in `~/b/ruff-force-exclude-test` is the Fish shell contracting `build`.

`build` is, of course, one of the default excludes:

https://github.com/charliermarsh/ruff/blob/9e704a7c639f8ad5f7751fb062c46144caec5bb7/src/settings/defaults.rs#L44

I guess we should root `exclude` patterns to e.g. where the `pyproject.toml` file is found?

---

_Comment by @charliermarsh on 2023-01-20 23:33_

Yeah I need to think on this. I agree it's surprising behavior in this case. I think, maybe, in `is_file_excluded`, we need to avoid recurring past the "project root", to avoid looking at directories outside of the `pyproject.toml`.

---

_Label `question` removed by @charliermarsh on 2023-01-20 23:33_

---

_Label `core` added by @charliermarsh on 2023-01-20 23:33_

---

_Renamed from "Does `--force-exclude` work like it should?" to "`exclude` looks at path components beyond "project root" (née does `--force-exclude` work like it should?)" by @akx on 2023-01-21 16:54_

---

_Comment by @akx on 2023-02-02 12:25_

@charliermarsh How about https://github.com/charliermarsh/ruff/pull/2471? Seems almost deceptively simple though...

---

_Comment by @charliermarsh on 2023-02-03 13:24_

Closed by #2471.

---

_Closed by @charliermarsh on 2023-02-03 13:24_

---
