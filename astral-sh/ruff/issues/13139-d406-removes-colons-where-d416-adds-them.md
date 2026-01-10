```yaml
number: 13139
title: D406 removes colons where D416 adds them
type: issue
state: closed
author: maxschulz-COL
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-08-28T12:11:22Z
updated_at: 2024-08-29T15:33:19Z
url: https://github.com/astral-sh/ruff/issues/13139
synced_at: 2026-01-10T11:09:55Z
```

# D406 removes colons where D416 adds them

---

_Issue opened by @maxschulz-COL on 2024-08-28 12:11_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.


"section-name-ends-in-colon", "D416", "removed", "autofix"


* A minimal code snippet that reproduces the bug.
Ruff seems to auto-fix

```py
def foo(a: str) -> str:
    """Foo bar.

    Args:
        a: Some argument.

    Examples
        Some explanation here.
        >>> bla bla bla

    """
    return a
```
to
```py
def foo(a: str) -> str:
    """Foo bar.

    Args:
        a: Some argument.

    Examples:
        Some explanation here.
        >>> bla bla bla

    """
    return a
```
, but it changes (in my eyes erroneously?):
```py
def foo(a: str) -> str:
    """Foo bar.

    Examples:
        Some explanation here.
        >>> bla bla bla

    """
    return a
```
to 
```py
def foo(a: str) -> str:
    """Foo bar.

    Examples
        Some explanation here.
        >>> bla bla bla

    """
    return a
```

Should this be the case?

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
Any ruff command with autofix

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
```toml
[tool.ruff]
line-length = 120
target-version = "py38"

[tool.ruff.lint]
# see: https://beta.ruff.rs/docs/rules/
ignore = [
  "D104",  # undocumented-public-package
  "D401",  # first-line should be in imperative mood
  # D407 needs to be ignored as it otherwise messes up the formatting in our API docs
  "D407"  # missing dashed underline after section
]
ignore-init-module-imports = true  # TODO: remove as deprecated soon
select = [
  "E",  # pycodestyle errors
  "W",  # pycodestyle warnings
  "F",  # pyflakes
  "I",  # isort
  "D",  # pydocstyle
  "T201",  # print
  "C4",  # flake8-comprehensions
  "RUF",  # Ruff-specific rules
  "PL"  # pylint
]
```


* The current Ruff version (`ruff --version`).
`0.6.2`

---

_Comment by @AlexWaygood on 2024-08-28 15:43_

Ahhh, it took me a little bit of time to reproduce this, because it seems that while it's D416 _adding_ the colon to your first snippet, it's D406 that's _removing_ them from your second snippet! So `cargo run -- check --select=D416 --no-cache --isolated --fix --diff foo.py` is e.g. not sufficient to reproduce this.

Anyway, I've now reproduced it!

---

_Label `docstring` added by @AlexWaygood on 2024-08-28 15:43_

---

_Renamed from "D416 removes colons where it should actually add them" to "D406 removes colons where D416 adds them" by @AlexWaygood on 2024-08-28 15:59_

---

_Comment by @AlexWaygood on 2024-08-28 16:39_

So I think the reason for this is that Ruff is inferring the incorrect [docstring convention](https://docs.astral.sh/ruff/settings/#lint_pydocstyle_convention). Not sure yet whether it's inferring `pep257` or `numpy` -- but if you run e.g. `ruff check baz.py --no-cache --select=D406 --fix --diff --config 'lint.pydocstyle.convention = "google"'` (where `baz.py` has your second snippet in it), you'll find that no errors are emitted. Whereas if you run either `ruff check baz.py --no-cache --select=D406 --fix --diff --config 'lint.pydocstyle.convention = "pep257"'` or `ruff check baz.py --no-cache --select=D406 --fix --diff --config 'lint.pydocstyle.convention = "numpy"'`, the error you're reporting is emitted. As a quick fix (that will avoid this whole category of errors), you can explicitly set the `lint.pydocstyle.convention` setting in your `pyproject.toml` file to `"google"`.

I'll keep this open for now though, since I believe we infer the docstring convention in some of our other rules... if so, we should maybe try to do that here as well. (Or maybe we already do? If so, we should do it better, since it's pretty obvious to me that you're using `google` conventions!)

---

_Comment by @AlexWaygood on 2024-08-28 16:48_

Okay, inserting these debug prints confirms that because you haven't explicitly selected a docstring convention in your `pyproject.toml` file, we try to infer the convention -- and then for some reason we infer the convention as `numpy`, when it clearly should be `google`.

```diff
--- a/crates/ruff_linter/src/docstrings/styles.rs
+++ b/crates/ruff_linter/src/docstrings/styles.rs
@@ -2,7 +2,7 @@ use crate::docstrings::google::GOOGLE_SECTIONS;
 use crate::docstrings::numpy::NUMPY_SECTIONS;
 use crate::docstrings::sections::SectionKind;
 
-#[derive(Copy, Clone)]
+#[derive(Copy, Clone, Debug)]
 pub(crate) enum SectionStyle {
     Numpy,
     Google,
diff --git a/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs b/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs
index 95e0c46af..9fdb344b1 100644
--- a/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs
+++ b/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs
@@ -1327,10 +1327,10 @@ pub(crate) fn sections(
     section_contexts: &SectionContexts,
     convention: Option<Convention>,
 ) {
-    match convention {
+    match dbg!(convention) {
         Some(Convention::Google) => parse_google_sections(checker, docstring, section_contexts),
         Some(Convention::Numpy) => parse_numpy_sections(checker, docstring, section_contexts),
-        Some(Convention::Pep257) | None => match section_contexts.style() {
+        Some(Convention::Pep257) | None => match dbg!(section_contexts.style()) {
             SectionStyle::Google => parse_google_sections(checker, docstring, section_contexts),
             SectionStyle::Numpy => parse_numpy_sections(checker, docstring, section_contexts),
```

This seems like a clear bug to me.

---

_Label `bug` added by @AlexWaygood on 2024-08-28 16:48_

---

_Comment by @maxschulz-COL on 2024-08-28 16:51_

Wow well investigated!!!

So I can fix the issue by setting the template, but it is also a bug that other user might benefit from having fixed?

---

_Comment by @AlexWaygood on 2024-08-28 16:52_

Exactly! I'm working on a fix now.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-28 16:52_

---

_Closed by @AlexWaygood on 2024-08-29 15:33_

---
