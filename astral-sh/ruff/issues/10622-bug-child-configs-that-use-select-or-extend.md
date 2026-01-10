```yaml
number: 10622
title: "bug: child configs that use 'select' or 'extend-select' invalidate the parent config's 'ignore' list."
type: issue
state: open
author: Hnasar
labels:
  - documentation
  - question
assignees: []
created_at: 2024-03-26T21:01:16Z
updated_at: 2025-10-25T02:31:41Z
url: https://github.com/astral-sh/ruff/issues/10622
synced_at: 2026-01-10T11:09:52Z
```

# bug: child configs that use 'select' or 'extend-select' invalidate the parent config's 'ignore' list.

---

_Issue opened by @Hnasar on 2024-03-26 21:01_

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

## Bug/Use-case

As someone who helps maintain a monorepo with many subprojects and child configs, I want to be able to maintain a centralized configuration of ignored rules (which projects may override with `tool.ruff.extend`). There appears to be a bug where the top-level `tool.ruff.lint.ignore` configuration _isn't honored_ whenever a child config modifies either `tool.ruff.lint.select` or `tool.ruff.lint.extend-select`. This occurs all the time, even if the child config HAS NOT overridden `tool.ruff.lint.ignore` in any way.

I (think I) understand the semantics of `ruff.extend`  alongside `lint.extend-select` and `lint.select` but the semantics of the now-combined `lint.ignore` create some really confusing behaviors. As a result, I don't have a good way to fulfill my use-case mentioned above.

## Repro

Using ruff 0.3.4

```console
$ tree
.
└── parent
    ├── child
    │   ├── foo.py
    │   └── pyproject.toml
    └── pyproject.toml
```


```py
# child/foo.py
import os, re

"{}".format(1)
```

## Confusing behavior:
### 0. Parent ignores with child select

```toml
# parent/pyproject.toml
[tool.ruff]
lint.select = ["E", "UP"]
lint.ignore = ["E401", "UP032"]
```
```toml
# child/pyproject.toml
[tool.ruff]
extend = "../pyproject.toml"
lint.extend-select = ["UP"]  # also same behavior if we use lint.select ["UP"]
```

**Observed**: 
```console
$ ruff check .
parent/child/foo.py:3:1: UP032 [*] Use f-string instead of `format` call
```

**Expected**:

_**I expect that the parent `ignore` list is used in all cases except when the child has its own `ignore` list.**_

```console
$ ruff check .
All checks passed!
```

----------------

### 1. Parent ignores with child select and child ignore


```toml
# parent/pyproject.toml
[tool.ruff]
lint.select = ["E", "UP"]
lint.ignore = ["E401", "UP032"]
```
```toml
# child/pyproject.toml
[tool.ruff]
extend = "../pyproject.toml"
lint.extend-select = ["UP"]  # also same behavior if we use lint.select ["UP"]
lint.ignore = ["UP032"]
```

**Observed**: 
```console
$ ruff check .
All checks passed!
```

**Expected**:

_**I expect that if I provide a child `ignore` list then it _fully_ invalidates the parent `ignore` list.**_
```console
$ ruff check .
parent/child/foo.py:1:1: E401 [*] Multiple imports on one line
```


## Workarounds

Neither of these are ideal, but are some things I tried:

1. Copy and paste the parent's `lint.ignore` and keep it in sync in all the child configs. This is cumbersome to keep in sync and easy to miss things.
2. Instead of using `select` or `extend-select` in child configs, I can invert this and `ignore` all the top-level `selects` that a project wants to opt-out of. This is confusing, and also is cumbersome and easy to miss.

## Examples of fine behavior that makes sense
<details>
  <summary>Click to expand examples</summary>
  

### 2. No ignores

```toml
# parent/pyproject.toml
[tool.ruff]
lint.select = ["E", "UP"]
```

```toml
# child/pyproject.toml
[tool.ruff]
extend = "../pyproject.toml"
```

```console
$ ruff  check .
parent/child/foo.py:1:1: E401 [*] Multiple imports on one line
parent/child/foo.py:3:1: UP032 [*] Use f-string instead of `format` call
```

### 3. Child select
```toml
# parent/pyproject.toml
[tool.ruff]
lint.select = ["E", "UP"]
```
```toml
# child/pyproject.toml
[tool.ruff]
extend = "../pyproject.toml"
lint.select = ["UP"]
```
```console
$ ruff  check .
parent/child/foo.py:3:1: UP032 [*] Use f-string instead of `format` call
```

### 4. Child extend-select

```toml
# parent/pyproject.toml
[tool.ruff]
lint.select = ["E", "UP"]
```
```toml
# child/pyproject.toml
[tool.ruff]
extend = "../pyproject.toml"
lint.extend-select = ["UP"]
```

```console
$ ruff  check .
parent/child/foo.py:1:1: E401 [*] Multiple imports on one line
parent/child/foo.py:3:1: UP032 [*] Use f-string instead of `format` call
```


### 5. Parent ignores without any child select

```toml
# parent/pyproject.toml
[tool.ruff]
lint.select = ["E", "UP"]
lint.ignore = ["E401", "UP032"]
```
```toml
# child/pyproject.toml
[tool.ruff]
extend = "../pyproject.toml"
```
```console
ruff  check .
All checks passed!
```
</details>


---

_Comment by @charliermarsh on 2024-03-26 22:38_

So for "0. Parent ignores with child select", to the best of my recollection, I thought a child `select` was intended to "reset" the rule selection (which tends to create more intuitive behavior), but an `extend-select` was _not_ supposed to reset. So your example there _should_ work as you expected. I'll have to test it out myself and see what's up.

---

_Comment by @MichaReiser on 2024-03-27 08:32_

Here's a ZIP with the described repo. 
[I10622.zip](https://github.com/astral-sh/ruff/files/14770373/I10622.zip)


---

_Comment by @charliermarsh on 2024-03-27 16:20_

I can take a look at this tonight.

---

_Comment by @charliermarsh on 2024-03-30 17:20_

On further review this is working as expected but my previous explanation was missing one piece...

When you use `extends`, we resolve the list of resolves one configuration file at a time. So, we first resolve all of the selectors in `parent/pyproject.toml`, and come up with a list of rules. Then, we apply the selectors in `child/pyproject.toml` on top of that list. So if you have `lint.extend-select = ["UP"]` in your `child/pyproject.toml`, it will indeed then add all the `UP` rules and ignore the `UP032` ignore from `parent/pyproject.toml`. However, note that `E401` is _still_ disabled.

---

_Label `question` added by @charliermarsh on 2024-03-30 17:21_

---

_Comment by @Hnasar on 2024-04-01 15:24_

Thanks for the explanation of how it works with the file-by-file resolution, because I was really confused because I thought the 'most specific option wins' was the standard semantics.

So then for my initial user story, is there any supported way to globally ignore some rules when there's a tree of configs? If we had a `tool.ruff.lint.ignore` that overrode any inherited ignores, and a `tool.ruff.lint.extend-ignore` which _added_ to any inherited ignores, that would give me the tools I need to fix things, but otherwise I'm not sure.

---

_Comment by @MichaReiser on 2024-04-02 09:10_

I think it would be good for us to document this behavior because it wasn't clear to me. 

---

_Label `documentation` added by @MichaReiser on 2024-04-02 09:10_

---

_Comment by @Avasam on 2024-08-19 05:58_

This would be quite problematic with shared configs: https://github.com/astral-sh/ruff/issues/12352

Edit: now ignore and extend-ignore are the same.
I would expect "select" to "reset" the parent's selection. And "extend-select" to expand upon it. Which I believe is currently the case.

---

_Comment by @cscanlin on 2025-10-24 18:52_

I want to bump this one up because this did not work at all how I expect, and I'm wondering if what I want to do is even possible. I have a more detailed set of rules that I want to use (for e.g. my text editor) on all projects that is more restrictive than what each project itself enforces.

What I originally tried (simplified):
```
# project/pyproject.toml
[tool.ruff]
extend = "~/pyproject.toml"
lint.select = ["E101"]
```

```
# ~/pyproject.toml
[tool.ruff]
lint.extend-select = ["F701"]
```

When I run `ruff check . --show-settings`, I get the following rules:
```
linter.rules.enabled = [
	mixed-spaces-and-tabs (E101),
]
```
I would also expect F701 to show up there.

Overriding the extend-select does work as expected:
`ruff check . --show-settings --extend-select E401`
```
linter.rules.enabled = [
	mixed-spaces-and-tabs (E101),
	multiple-imports-on-one-line (E401),
]
```

Interestingly, if I flip this around so the select is in the parent it kind of works
```
# project/pyproject.toml
[tool.ruff]
extend = "~/pyproject.toml"
lint.extend-select = ["E101"]
```

```
# ~/pyproject.toml
[tool.ruff]
lint.select = ["F701"]
```

```
linter.rules.enabled = [
	mixed-spaces-and-tabs (E101),
	break-outside-loop (F701),
]
```

But this will affect all instances of when ruff is run in my environment, so I'd like to be able to override the parent config select in certain cases, e.g.:
`ruff check . --show-settings --select E401`

And I get the following:
```
linter.rules.enabled = [
	multiple-imports-on-one-line (E401),
]
```

I would have expected to E101 to be included here, since the extend-select is never overridden.



---

_Comment by @Avasam on 2025-10-25 02:21_

> I would also expect F701 to show up there.

@cscanlin I wouldn't, your examples seem to be working as I would expect.

`select` will make it so *only* those listed rules are selected. You want to use `extend-select` in all cases here.

---
