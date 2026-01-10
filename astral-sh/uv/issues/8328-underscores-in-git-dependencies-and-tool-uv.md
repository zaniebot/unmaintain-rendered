```yaml
number: 8328
title: "Underscores in Git dependencies and [tool.uv.sources]"
type: issue
state: closed
author: cgravill
labels:
  - question
assignees: []
created_at: 2024-10-18T13:54:17Z
updated_at: 2024-10-19T14:18:30Z
url: https://github.com/astral-sh/uv/issues/8328
synced_at: 2026-01-10T04:45:10Z
```

# Underscores in Git dependencies and [tool.uv.sources]

---

_Issue opened by @cgravill on 2024-10-18 13:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This is reproduced from a private repository.

https://github.com/cgravill/example_python_library
https://github.com/cgravill/example_python_app

We've some packages that are named with underscores then referenced as git dependencies. Some of the dependencies are hand written so match the exact names like this:

https://github.com/cgravill/example_python_app/blob/24b446b856475297785d5d240a3dc08086fa51b5/pyproject.toml#L7-L12

In general use this all works, we can use `uv run`, `uv lock` and others. What goes wrong is if we try use management commands:

```
uv remove https://github.com/cgravill/example_python_library.git --verbose
error: invalid value 'https://github.com/cgravill/example_python_library.git' for '<PACKAGES>...': Not a valid package or extra name: "https://github.com/cgravill/example_python_library.git". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.

For more information, try '--help'.
```

I think what's happening is that you folks are applying normalization prior commands: https://peps.python.org/pep-0503/#normalized-names which is totally reasonable. I'm mainly reporting as the behaviour caused quite a lot of confusion in the project (migrated from Poetry) and it might be possible to adjust the errors to lead folks down better paths.

There are also various other surprises if you say use ` uv add https://github.com/cgravill/example_python_library.git` as you'll then get conflicting TOML parsing with the overlapping entries.

Personally I think we're doing something ill-advised, though it's permitted by https://peps.python.org/pep-0503 so we're planning to just move all our project-names over to `-` as it'll just be simpler that way.

Also to note if everything is done with `uv add` and `uv remove` it works as the normalized names are written to pyproject.toml.

uv 0.4.24 (b9cd54913 2024-10-17)

---

_Comment by @tpgillam on 2024-10-18 14:23_

Another example of an "other suprise" that we came across:

```bash
gh repo clone cgravill/example_python_app
cd example_python_app
uv remove example_python_lib
git diff pyproject.toml
```

we see:
```diff
diff --git a/pyproject.toml b/pyproject.toml
index 71c2ebc..b043916 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -4,9 +4,7 @@ version = "0.1.0"
 description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.13"
-dependencies = [
-    "example_python_library",
-]
+dependencies = []

 [tool.uv.sources]
 example_python_library = { git = "https://github.com/cgravill/example_python_library.git" }
```

i.e. it has removed the dependency from `[project.dependencies]`, but not the source entry from `[tool.uv.sources]`.

---

_Comment by @charliermarsh on 2024-10-18 16:32_

Hmm, I think `uv remove` requires a package name and not a dependency specifier (i.e., `uv remove https://github.com/cgravill/example_python_library.git` isn't accepted, but `uv remove example_python_library` is). Does that seem wrong to you?

---

_Label `question` added by @charliermarsh on 2024-10-18 17:58_

---

_Comment by @cgravill on 2024-10-18 20:24_

@charliermarsh you're right I've confused the issue with that, I got into that when debugging around the issue @tpgillam more clearly reported (and has been extracted as [separate issue](#8330)). Since I've mentioned that though: I do somewhat expect symmetry between `add` and `remove` but that would be a different request. What I'd actually go for in that case is detecting special cases like GitHub repositories and erroring inviting people to `uv remove example_python_library` or `presumably uv remove example-python-library` for clarity

The other issue of adding the dependency again:

```
 uv add https://github.com/cgravill/example_python_library.git
 Updated https://github.com/cgravill/example_python_library.git (2ada4bd)
error: Failed to parse `pyproject.toml`
  Caused by: TOML parse error at line 11, column 1
   |
11 | [tool.uv.sources]
   | ^^^^^^^^^^^^^^^^^
duplicate sources for package `example-python-library`
```

and

```diff
git diff pyproject.toml
diff --git a/pyproject.toml b/pyproject.toml
index 71c2ebc..33d2eb3 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -5,8 +5,9 @@ description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.13"
 dependencies = [
-    "example_python_library",
+    "example-python-library",
 ]

 [tool.uv.sources]
 example_python_library = { git = "https://github.com/cgravill/example_python_library.git" }
+example-python-library = { git = "https://github.com/cgravill/example_python_library.git" }
```
is also an annoyance. The slightly more involved context that comes from is we also specify git `rev` so I was trying to update the handwritten entry. The failure made me incorrectly conclude that `uv` couldn't handle updates at all. Depending how the fix for #8330 goes it might also fix this!

---

_Closed by @zanieb on 2024-10-19 13:26_

---

_Comment by @cgravill on 2024-10-19 14:18_

Thanks all!

---
