```yaml
number: 15399
title: A005 stdlib-module-shadowing warns that utils.logging shadows python logging module
type: issue
state: closed
author: dwt
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-01-10T12:58:28Z
updated_at: 2025-02-12T16:13:33Z
url: https://github.com/astral-sh/ruff/issues/15399
synced_at: 2026-01-10T11:09:56Z
```

# A005 stdlib-module-shadowing warns that utils.logging shadows python logging module

---

_Issue opened by @dwt on 2025-01-10 12:58_

[This rule](https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/) warns for me that `utils.logging` shadows stdlib `logging`.

Is this intentional? The whole point why our logging module is in `utils` is so the names do not collide. Am I missing something?



---

_Label `question` added by @MichaReiser on 2025-01-10 13:04_

---

_Label `rule` added by @MichaReiser on 2025-01-10 13:04_

---

_Comment by @MichaReiser on 2025-01-10 13:07_

Hi. Thanks for opening the question. 

This seems to be matching the upstream behavior (more as a note to myself rather than this being a justification why):

https://github.com/gforcada/flake8-builtins/blob/5484162ace8d9546513be710d70e9c148010d9cc/flake8_builtins.py#L292

I'm not 100% sure if this is the right behavior or if the rule is too eager to warn. IMO, the rule should warn if your module is named `logging` because it then shadows the stdlib module. Naming it `utils.logging` seems less problematic to me. @AlexWaygood am I overlooking something?

---

_Comment by @mattmess1221 on 2025-01-10 15:12_

I think this rule should only trigger for modules if there isn't an `__init__.py` file in the directory OR any module in the package is executable. e.g. it has a shebang or `if __name__ == '__main__':` check.

This is the only way I can see module shadowing being possible, as doing so prepends the parent directory to `sys.path`, overriding the stdlib.

---

_Comment by @MichaReiser on 2025-01-13 12:48_

I discussed this with @AlexWaygood in our 1:1 and we think that we should change the default to only flag modules that have the exact same full qualified name as a standard library module and instead, add an option to enable the more strict linting that flags modules that only have the same name. 

The motivation for an option is that some projects might want stricter behavior to guarantee that modules don't shadow builtins when running a module from a subdirectory (e.g. `cd` into the `utils` directory and run a script from there).

---

_Label `question` removed by @MichaReiser on 2025-01-13 12:48_

---

_Label `help wanted` added by @MichaReiser on 2025-01-13 12:48_

---

_Comment by @InSyncWithFoo on 2025-01-15 11:26_

Somehow I can't replicate the problem.

1. Library layout:

	```text
	.
	├── pyproject.toml
	└── src
	    └── test
	        ├── __init__.py
	        └── utils
	            └── logging.py
	```
	
	```shell
	$ uvx ruff check --isolated --select A005 src
	All checks passed!
	```

2. App layout:

	```text
	.
	├── hello.py
	├── pyproject.toml
	└── utils
	    └── logging.py
	```

	```shell
	$ uvx ruff check --isolated --select A005
	All checks passed!
	```

This is perhaps because [`package`](https://github.com/astral-sh/ruff/blob/8331326cb6897ff5cbee19c12b719889f0303b3a/crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs#L62) is `None` is such cases.

---

One thing to note is that `package` is always `Root` and never `Nested`, even for this [straight-out-of-the-docs](https://github.com/astral-sh/ruff/blob/8331326cb6897ff5cbee19c12b719889f0303b3a/crates/ruff_linter/src/package.rs#L8-L19) case where `baz/__init__.py` is passed directly to Ruff:

```text
.
├── README.md
├── pyproject.toml
└── src
    └── foo
        ├── __init__.py
        └── bar
            └── baz
                └── __init__.py
```

Meanwhile, `test_contents` [uses a hardcoded `Root` call](https://github.com/astral-sh/ruff/blob/8331326cb6897ff5cbee19c12b719889f0303b3a/crates/ruff_linter/src/test.rs#L127).

Is this a bigger bug?

---

_Comment by @martina-oefelein on 2025-01-15 12:15_

It triggers if `util` is a regular package (contains an `__init__.py`):
```
.
├── pyproject.toml
└── src
    └── test
        ├── __init__.py
        └── utils
            ├── __init__.py
            └── logging.py
```

```bash
$ uvx ruff check --isolated --select A005 src
src/test/utils/logging.py:1:1: A005 Module `logging` shadows a Python standard-library module
```

---

_Comment by @InSyncWithFoo on 2025-01-15 12:55_

Thanks. In that case, this has something to do with the "never-`Nested`" problem above.

---

_Comment by @scott-boost on 2025-01-21 16:47_

```.
├── pyproject.toml
└── AcmeOrg
    └── http
        ├── __init__.py
        └── utils
            ├── __init__.py
            └── logging.py
```
interesting that flake8 didnt trigger A005 in this setup BUT ruff did
(notice that AcmeOrg doesnt contain a `__init__.py` file)

---

_Comment by @rh-sp on 2025-01-24 09:01_

### Opinion

I think that the current rule is too strict, but "only fully qualified names" sounds like it could be too loose, depending on how it's interpreted. In my opinion, any root-level packages that are named the same as any stdlib root-level package should be blocked, _along with all their child packages._

### Example

- ~~abc~~ ❌
- ~~collections~~ ❌
  - ~~abc~~ ❌
  - ~~foobar~~ ❌
- foobar ✅
  - abc ✅
  - collections ✅
    - abc ✅
- urlparse ✅

### Reasoning

I haven't been around then, but from what I've heard, in the olden days, it wasn't well-defined what `import abc` does when in a package. Does it refer to the root-level package `abc`, or the `abc` defined at the current package level? So in that sense, for older Pythons, it made sense to flag `foobar.abc` as potentially shadowing `abc`.

However, in modern Python, this issue has been resolved. `import abc` _always_ refers to the root-level package, _never_ `foobar.abc` (or `collections.abc`). At the same time, `.abc` (which can stand for `foobar.abc` or `collections.abc`, depending on context) can _never_ shadow `abc`. So, `foobar.abc` does not need to trigger the rule.

Additionally, `collections.foobar` should _also_ trigger the rule, even though the fully qualified name doesn't exist in `stdlib`. This is because if you `import collections.foobar`, while the import statement itself can only mean one thing, from what I understand in order for it to work, `collections` _must_ be shadowed, so the problem is still there.

Hope that makes sense :)

---

_Comment by @flying-sheep on 2025-01-27 11:45_

> `import abc` _always_ refers to the root-level package, _never_ `foobar.abc` (or `collections.abc`). At the same time, `.abc` (which can stand for `foobar.abc` or `collections.abc`, depending on context) can _never_ shadow `abc`. So, `foobar.abc` does not need to trigger the rule.

Yeah definitely. This rule has a lot of false positives.

---

_Assigned to @ntBre by @MichaReiser on 2025-02-04 10:35_

---

_Closed by @ntBre on 2025-02-10 00:33_

---

_Comment by @dwt on 2025-02-10 07:19_

🎉

---

_Comment by @glopesdev on 2025-02-12 15:56_

@ntBre We are having the opposite problem where our CI started emitting more warnings following the latest release of ruff, in places where it wasn't emitting before.

Is there documentation somewhere explaining how exactly to turn on the new "default" behavior?

---

_Comment by @glopesdev on 2025-02-12 16:00_

For those randomly checking here, the new config for `pyproject.toml` should be:

```
[tool.ruff]
lint.flake8-builtins.builtins-strict-checking = false
```

Still not clear to me why an issue which was supposed to reduce the number of default built-in warnings ended up producing more warnings under the default configuration.

---

_Comment by @ntBre on 2025-02-12 16:07_

It will be a breaking change to update the _stable_ default, so that will have to wait for the next minor version (0.10.0). In preview mode, the default should already be `lint.flake8-builtins.builtins-strict-checking = false`.

But yes, as part of this change there was also a bug fix to bring our implementation of A005 in line with the upstream, which *does* produce more warnings in some cases now.

---

_Comment by @ntBre on 2025-02-12 16:09_

Oh and yes, that was my mistake on the documentation. I have an open PR (#16097) to make that easier to find that will hopefully be merged today!

---

_Comment by @ntBre on 2025-02-12 16:13_

There's also a related breaking change for 0.10.0 to make the new option and the other flake8-builtins options less verbose in #16092. In this case, I think the new option name will be `lint.flake8-builtins.strict-checking`, but this is not currently available in preview.

---
