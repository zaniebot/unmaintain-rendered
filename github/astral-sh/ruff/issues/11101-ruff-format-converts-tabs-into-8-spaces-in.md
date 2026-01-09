---
number: 11101
title: Ruff format converts tabs into 8 spaces in docstring
type: issue
state: open
author: ibrokemypie
labels:
  - formatter
assignees: []
created_at: 2024-04-23T07:54:12Z
updated_at: 2024-04-26T01:48:26Z
url: https://github.com/astral-sh/ruff/issues/11101
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff format converts tabs into 8 spaces in docstring

---

_Issue opened by @ibrokemypie on 2024-04-23 07:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Ruff format converts tabs in docstrings to 8 spaces, and there does not seem to be any way to disable this.

I think that ruff format shouldn't touch docstrings at all by default, as they are data, not code, and at the very least this should be configurable. On another note, 8 spaces doesnt even match the configured indent-width.

Minimal repro on ruff 0.4.1:

test.py
```python
"""
foo
	bar
"""
```

```shell
> ruff format --verbose --check --diff test.py
[2024-04-23][17:49:57][ruff::resolve][DEBUG] Using Ruff default settings
[2024-04-23][17:49:57][ruff::commands::format][DEBUG] format_path; path=/home/noodle/test.py
[2024-04-23][17:49:57][tracing::span][DEBUG] Printer::print;
[2024-04-23][17:49:57][ruff::commands::format][DEBUG] Formatted 1 files in 343.02µs
--- test.py
+++ test.py
@@ -1,4 +1,4 @@
 """
 foo
-	bar
+        bar
 """

1 file would be reformatted
```

Have searched for existing issues with various combinations of the terms 'docstring', 'indent', 'tab', 'disable', 'format' and was not able to find any that were still open which applied.

---

_Comment by @dhruvmanila on 2024-04-23 08:09_

Hi, thanks for opening this issue.

Can you try using the [`indent-style`](https://docs.astral.sh/ruff/settings/#format_indent-style) config option?

```toml
[tool.ruff.format]
indent-style = "tab"
```

https://play.ruff.rs/9046b93e-f66b-4f06-b0dd-1f9b8eeeaa93

---

_Label `formatter` added by @dhruvmanila on 2024-04-23 08:09_

---

_Comment by @ibrokemypie on 2024-04-23 08:28_

Adding a pyproject.toml to configure indent-style to tab does prevent the docstring tabs from being replaced, however now the entire codebase is having every instance of indentations being converted from tabs to spaces which is not what we want at all. We want our code to be formatted to use spaces for indentation as is standard, and for docstrings to be left alone, or at least be left visually the same

test.py
```python
"""
foo
	bar
"""


def baz():
    print('hi')
```

pyproject.toml
```toml
[tool.ruff.format]
indent-style = "tab"
```

```shell
> ruff format --verbose --check --diff test.py
[2024-04-23][18:25:05][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/noodle/pyproject.toml
[2024-04-23][18:25:05][ruff::commands::format][DEBUG] format_path; path=/home/noodle/test.py
[2024-04-23][18:25:05][tracing::span][DEBUG] Printer::print;
[2024-04-23][18:25:05][ruff::commands::format][DEBUG] Formatted 1 files in 145.20µs
--- test.py
+++ test.py
@@ -5,4 +5,4 @@
 
 
 def baz():
-    print('hi')
+       print("hi")

1 file would be reformatted
```

---

_Comment by @dhruvmanila on 2024-04-23 08:37_

Oh I see. Thank you for providing the context. So, just to be clear, you'd want the docstrings indentation to be left as is but retain the 4-space indent for other parts of the codebase.

I think @MichaReiser might have more context on this but this seems similar to  https://github.com/astral-sh/ruff/issues/10396

---

_Comment by @ibrokemypie on 2024-04-23 08:41_

Correct, we want to use standard 4-space (or whatever is configured) indentation for the codebase, but leave the indentation within docstrings alone :+1: 

I did see and read through that issue, but as noted there `docstring-code-format` does not apply to non-code in docstrings (and is already disabled by default) and going through a very large codebase and adding `# fmt: skip` to every docstring (and probably needing a semgrep rule to enforce this going forward?) isn't really feasible

---

_Comment by @MichaReiser on 2024-04-23 16:01_

The existing behavior is intentional to match how Python displays docstrings at runtime. Python's `cleandoc` function changes all tabs to 8 spaces. Because of that, Ruff is forced to change tabs to 8 spaces. Using `indent-width` spaces instead would alter how Python interprets your comments at runtime. 

That being said. We could do better, but that requires that Ruff understands the semantics of docstrings to know if some leading whitespace is indentation or alignment (e.g. for an ASCII art where spacing matters). You can read more about this in https://github.com/astral-sh/ruff/issues/8430#issuecomment-1934209625

I'm sorry, but there's no option to disable docstring formatting at the moment and I unfortunately don't have a good recommendation on how you can migrate your code base easily without having to review the changes manually. D206 catches tab indentations inside docstrings, but it doesn't provide an autofix for it. You could try enabling the rule and change all tabs manually, I know that's cumbersome.

---

_Comment by @ibrokemypie on 2024-04-26 01:18_

We were able to replace the tabs in docstrings with spaces by hand to make them visually equivalent in editor, which has worked around the issue. I think the biggest thing to note here is that this behaviour doesn't at all match black, which doesn't touch the docstrings, and I wasn't able to find any mention of this in the known deviations page.

```python
"""
foo
	bar
    bar
"""


def baz():
    print("hi")
```

```shell
> ruff format --verbose --check --diff test.py
[2024-04-26][11:13:23][ruff::resolve][DEBUG] Using Ruff default settings
[2024-04-26][11:13:23][ruff::commands::format][DEBUG] format_path; path=/home/noodle/test.py
[2024-04-26][11:13:23][tracing::span][DEBUG] Printer::print;
[2024-04-26][11:13:23][ruff::commands::format][DEBUG] Formatted 1 files in 174.54µs
--- test.py
+++ test.py
@@ -1,6 +1,6 @@
 """
 foo
-	bar
+        bar
     bar
 """
 

1 file would be reformatted
```


```shell
> black -v --diff test.py
Identified `/` as project root containing a file system root.
Found input source: "test.py"
test.py already well formatted, good job.

All done! ✨ 🍰 ✨
1 file would be left unchanged.
```

---

_Comment by @charliermarsh on 2024-04-26 01:48_

To be clear: I believe Black does touch function and class docstrings, and _does_ convert them from tabs to spaces. But it doesn't touch _module_ docstrings.

---
