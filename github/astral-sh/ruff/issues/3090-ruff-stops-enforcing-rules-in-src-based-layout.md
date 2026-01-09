---
number: 3090
title: Ruff stops enforcing rules in src/ based layout
type: issue
state: closed
author: ashb
labels:
  - question
assignees: []
created_at: 2023-02-21T18:14:38Z
updated_at: 2023-02-21T19:32:18Z
url: https://github.com/astral-sh/ruff/issues/3090
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff stops enforcing rules in src/ based layout

---

_Issue opened by @ashb on 2023-02-21 18:14_

This is a slightly odd one to track down, but ruff has stopped enforcing any rules in my `src/` folder!

I've got a private repo (which makes it hard to share, at least publicly).

The ruff rules have been constant for the life of the repo (it's about 6 weeks old), so I tried using `git bisect`

I've managed to track down the commit that breaks things, using this `git bisect run` script:

```
bash -c '(cat src/myproject/app.py; echo -e "\n\nf\"a\"" ) | ruff check --no-fix --stdin-filename src/myproject/app.py  - --diff 2>&1 | grep --fixed "+\"a\""'
```

And looking at the changed files in the breaking commit, it doesn't "add up":

```
 src/myproject/__main__.py                                                  | 18 +++++++++++++++---
 src/myproject/api/routers/health.py                                        | 11 -----------
 src/myproject/apiserver/router.py                                          |  8 ++++++++
 src/myproject/app.py                                                       | 46 +++++++++++++++-------------------------------
 src/myproject/{metascheduler => common}/versions/alembic.py                |  0
 src/myproject/{metascheduler => common}/versions/v2_2_4.py                 |  0
 src/myproject/{metascheduler => common}/versions/v2_5.py                   |  0
 src/myproject/common/models/dsn.py                                         | 23 +++++++++++++++++++++++
 src/myproject/dependencies.py                                              |  7 -------
 src/myproject/hypervisor/__init__.py                                       |  0
 src/myproject/hypervisor/dependencies.py                                   |  7 +++++++
 src/myproject/hypervisor/router.py                                         | 26 ++++++++++++++++++++++++++
 src/myproject/hypervisor/routers/health.py                                 | 11 +++++++++++
 src/myproject/logging.py                                                   |  5 +++--
 src/myproject/component/__init__.py                                        |  1 -
 src/myproject/settings.py                                                  | 14 ++++++++++++++
```

I'd be happy to share access to the repo for the commits in question if it helps debug. (For future me, the breaking commit is b06f117fc and b06f117fc^ is good)

Through-out this time errors are still flaged up in `tests/`

<details>
<summary>Ruff config in pyproject.toml</summary>
```
[tool.ruff]
line-length = 110
extend-select = [
    "W",    # pycodestyle warnings
    "I",    # isort
    "C90",  # Complexity
    "C",    # flake8-comprehensions
    "ISC",  # flake8-implicit-str-concat
    "T10",  # flake8-debugger
    "A",    # flake8-builtins
    "UP",   # pyupgrade
    "RUF",  # un-needed noqa stanzas
]
extend-ignore = ["A002"]
extend-exclude = [
    "__pycache__",
]
target-version = "py311"
fix = true

[tool.ruff.isort]
combine-as-imports = true
known-first-party = ["myproject", "tests"]

[tool.ruff.mccabe]
max-complexity = 6
```
</details>

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-21 18:19_

---

_Label `question` added by @charliermarsh on 2023-02-21 18:19_

---

_Comment by @charliermarsh on 2023-02-21 18:22_

I'm not immediately sure what's going on. The first thing I'd wonder is if some of the files are being unintentionally excluded by the default exclusions list:

```rust
pub static EXCLUDE: Lazy<Vec<FilePattern>> = Lazy::new(|| {
    vec![
        FilePattern::Builtin(".bzr"),
        FilePattern::Builtin(".direnv"),
        FilePattern::Builtin(".eggs"),
        FilePattern::Builtin(".git"),
        FilePattern::Builtin(".hg"),
        FilePattern::Builtin(".mypy_cache"),
        FilePattern::Builtin(".nox"),
        FilePattern::Builtin(".pants.d"),
        FilePattern::Builtin(".pytype"),
        FilePattern::Builtin(".ruff_cache"),
        FilePattern::Builtin(".svn"),
        FilePattern::Builtin(".tox"),
        FilePattern::Builtin(".venv"),
        FilePattern::Builtin("__pypackages__"),
        FilePattern::Builtin("_build"),
        FilePattern::Builtin("buck-out"),
        FilePattern::Builtin("build"),
        FilePattern::Builtin("dist"),
        FilePattern::Builtin("node_modules"),
        FilePattern::Builtin("venv"),
    ]
});
```

E.g., that might happen if you moved files into a `build` subdirectory. But based on the layout above, I'm not seeing anything that would trigger that behavior.

The other possibility is that the automatic `gitignore` behavior somehow thinks those files should be ignored.

Are you able to share the output of running with `--verbose`?


---

_Comment by @ashb on 2023-02-21 18:27_

```
❯ ruff --version
ruff 0.0.249

❯ ruff check --no-fix src/laminar/app.py --verbose
[2023-02-21][18:26:29][ruff::commands::run][DEBUG] Identified files to lint in: 1.6477ms
[2023-02-21][18:26:29][ruff::diagnostics][DEBUG] Cache hit for: /home/ash/code/astro/laminar/src/laminar/app.py
[2023-02-21][18:26:29][ruff::commands::run][DEBUG] Checked 1 files in: 10.105801ms

❯ rm -r .ruff_cache

❯ ruff check --no-fix src/laminar/app.py --verbose
[2023-02-21][18:26:41][ruff::commands::run][DEBUG] Identified files to lint in: 1.64271ms
[2023-02-21][18:26:41][ruff::commands::run][DEBUG] Checked 1 files in: 9.527063ms
```

And to force an error (isort and f-string without replacements):

```
diff --git a/src/laminar/app.py b/src/laminar/app.py
index 8370d1b..400aa8b 100644
--- a/src/laminar/app.py
+++ b/src/laminar/app.py
@@ -1,5 +1,5 @@
-import structlog
 from fastapi import APIRouter, FastAPI
+import structlog

 app = FastAPI()

@@ -22,3 +22,5 @@ def build_app() -> FastAPI:
     log.info("Starting", app=module.__name__)
     app.include_router(module.router)
     return app
+
+f"a"
```

---

_Comment by @ashb on 2023-02-21 18:33_

(Happy to work out a way to get you a copy of the working and broken commits privately if it helps)

---

_Comment by @charliermarsh on 2023-02-21 18:42_

It would help a lot, if you're open to it. Do you want to send a zip by email (charlie.r.marsh@gmail.com)? What works best?

---

_Comment by @ashb on 2023-02-21 18:59_

You've got mail!

---

_Comment by @charliermarsh on 2023-02-21 19:01_

I forgot that I shipped a feature in latest Ruff that silently turns it off for all of _your_ projects specifically.

---

_Comment by @ashb on 2023-02-21 19:03_

😿 

---

_Comment by @charliermarsh on 2023-02-21 19:13_

Is the issue not that `app.py` now contains a `match` statement? Which is suppressed with `# noqa: E999`. (But the presence of the `match` means we can't check the AST.)

If you remove the `noqa`, you get a warning, like: `error: Failed to parse src/laminar/app.py: invalid syntax. Got unexpected token 'settings' at line 16 column 11`.

If you add an error that's not based on the AST (like a long line), you get: `src/laminar/app.py:26:89: E501 Line too long (91 > 88 characters)`.

If so, this should all be moot anyway since `match` support just merged. But maybe I'm misdiagnosing here.


---

_Comment by @ashb on 2023-02-21 19:22_

Ooooh shiny. Testing out a main build now.

I guess it makes sense that with the syntax errors it can't parse the AST to enforce more of the rules.

---

_Comment by @ashb on 2023-02-21 19:32_

Yup, confirmed. Main fixes it.

---

_Closed by @ashb on 2023-02-21 19:32_

---
