```yaml
number: 8470
title: "Warnings about incompatible rules with `--select ALL`"
type: issue
state: open
author: DimitriPapadopoulos
labels:
  - question
assignees: []
created_at: 2023-11-03T13:08:28Z
updated_at: 2025-08-01T10:01:00Z
url: https://github.com/astral-sh/ruff/issues/8470
synced_at: 2026-01-12T15:54:48Z
```

# Warnings about incompatible rules with `--select ALL`

---

_@DimitriPapadopoulos_

I'd rather not see warnings about incompatible rules with `--select ALL`. Do the following warnings about incompatible rules appear by default on purpose with `--select ALL`? 
* ```python
  """Documentation."""
  ```
* ```
  $ ruff --isolated --select ALL file.py
  warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
  warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
  $ 
  ```
* ```
  $ ruff --version
  ruff 0.1.3
  ```

---

_Renamed from "warning about incompatible rules with `--select ALL`" to "warnings about incompatible rules with `--select ALL`" by @DimitriPapadopoulos on 2023-11-03 13:09_

---

_Renamed from "warnings about incompatible rules with `--select ALL`" to "Warnings about incompatible rules with `--select ALL`" by @DimitriPapadopoulos on 2023-11-03 13:44_

---

_Comment by @zanieb on 2023-11-03 13:45_

If you don't want the warnings, I'd recommend explicitly ignoring two of those rules (whichever you do not want). In general, we do not recommend usage of `--select ALL`.

---

_Label `question` added by @zanieb on 2023-11-03 13:45_

---

_Comment by @charliermarsh on 2023-11-03 13:47_

Yeah, these are intended, because the rules are legitimately in conflict. Ideally, we will resolve this when do recategorize the rules in the future. At present, the only other option I see is to straight-up omit those two specific rules (`D203` and `D213`) from `ALL`, which could also cause confusion in the other direction (though would be much more rare).

---

_Comment by @charliermarsh on 2023-11-03 13:47_

(We could also consider removing those rules.)

---

_Comment by @DimitriPapadopoulos on 2023-11-03 13:48_

It makes sense. However, newcomers who start with `ruff check --help` see:
```
$ ruff check -h
Run Ruff on the given files or directories (default)
[...]
Rule selection:
      --select <RULE_CODE>
          Comma-separated list of rule codes to enable (or ALL, to enable all rules)
      --ignore <RULE_CODE>
          Comma-separated list of rule codes to disable
[...]
```

---

_Comment by @DimitriPapadopoulos on 2023-11-03 13:58_

They might be tempted to use option `--select ALL`. The result is that they see these warnings, which do make sense but might be disturbing for first time users.

---

_Comment by @MichaReiser on 2023-11-03 14:48_

It could make sense to remove `ALL` from the CLI help. It isn't our recommended way of using ruff and is overused. 

---

_Comment by @zanieb on 2023-11-03 14:55_

@MichaReiser I think that may make sense after recategorization

---

_Comment by @MichaReiser on 2023-11-03 15:02_

> @MichaReiser I think that may make sense after recategorization

What's your motivation for waiting until after recatogorization? I'm only proposing to remove it from the help text to promote it less. It may also be the case that we won't have a way to run all rules after recategorization (without listing all categories)

---

_Comment by @zanieb on 2023-11-03 15:03_

@MichaReiser I see the presence of sensible categories as essential to replace `ALL`. Until then, I worry we'll just get more questions if we remove the hint.

---

_Comment by @charliermarsh on 2023-11-03 16:33_

I don't think we should remove it from help, etc., until we have a reasonable alternative.

---

_Comment by @charliermarsh on 2023-11-03 16:33_

I _do_ think we could consider removing these rules from `ALL`.

---

_Comment by @zanieb on 2023-11-03 16:57_

I don't have a strong preference on removal from `ALL` but find excluding rules from `ALL` a weird pattern.

I'd rather close in favor of https://github.com/astral-sh/ruff/issues/1774

---

_Comment by @charliermarsh on 2023-11-03 16:58_

It's weird, but so is the behavior here, and I do dislike that it's _so_ common to run into these warnings when the rules themselves are _so_ uncommon.

---

_Comment by @tmeiczin on 2023-11-09 16:59_

We have over a million lines of code and currently use `ALL`  and disable about 40 rules (0.1.5 without preview). Without the `ALL` would we need to explicitly enable all 700 something rules? Enable all the categories, then specify individual ones we want disabled? That seems like it would get awkward? Or maybe I'm just selecting them wrong as it is?

I think many of the formatters and linters (maybe to a lesser extent) are going to be somewhat opinionated. I'm ok with that provided there is a reasonable amount of flexibility to alter the behavior to suit my projects. Maybe in addition to `ALL` add a `DEFAULT` which is a curated selection and chooses or disables rules with conflicts. Or possibly something like a `--select-conflict-defaults` which allows to specify which rules are enabled in case of conflicts.

---

_Comment by @MichaReiser on 2023-11-27 23:33_

> We have over a million lines of code and currently use ALL and disable about 40 rules (0.1.5 without preview). Without the ALL would we need to explicitly enable all 700 something rules? Enable all the categories, then specify individual ones we want disabled? That seems like it would get awkward? Or maybe I'm just selecting them wrong as it is?

You would have to enable all categories that aren't enabled by default, which should only be a few (<10). The categories we're thinking about are similar to [clippy's](https://doc.rust-lang.org/clippy/) but with a few additional ones (e.g. `format` for formatting related lints)



---

_Comment by @Avasam on 2024-12-27 01:35_

Without enabling `ALL`, these still come in conflict if you enable the `D` category. I'd rather fully opposite rules like these be configurable instead. (combine the codes and use a config option to decide on the behaviour).

That would also make more sense in the context of shared configs https://github.com/astral-sh/ruff/issues/12352



---

_Comment by @nunokaeru on 2025-04-08 09:08_

> If you don't want the warnings, I'd recommend explicitly ignoring two of those rules (whichever you do not want). In general, we do not recommend usage of `--select ALL`.

In a codebase I work on we have this configuration (partial), however I still get the warning even when I ignore all documentation:

```toml
[tool.ruff]
line-length = 120

# Supported rules https://github.com/charliermarsh/ruff/#supported-rules
lint.select = ["ALL"]

lint.ignore = [
    "ANN",    # flake8-annotations: Allow untyped code
    "E501",   # Line lenght higher than 88 characters
    "D",      # pydocstyle: Allow undocummented code
```

```log
[nuno@fedora xochitl]$ uvx ruff check .
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
All checks passed!
```

---

_Comment by @MichaReiser on 2025-04-08 09:18_

@nm-remarkable 

I just tried 

```
[tool.ruff]
line-length = 120

# Supported rules https://github.com/charliermarsh/ruff/#supported-rules
lint.select = ["ALL"]

lint.ignore = [
    "ANN",    # flake8-annotations: Allow untyped code
    "E501",   # Line lenght higher than 88 characters
    "D",      # pydocstyle: Allow undocummented code
]

lint.pydocstyle.convention = "google"
```

(with or without the `pydocstyle.convention` line) and I'm unable to reproduce. Could you open a new issue and provide more details (full ruff configuration, what ruff version are you using)

---

_Comment by @czechnology on 2025-08-01 10:01_

I wish that Ruff would include some default all-covering family of rules without the conflicting rules.
I do like to take advantage of all the advice that the rules give me and only explicitly disable the ones which don't work for me.

However, right now if I `select = ["ALL"]`, I need to exclude quite many individual conflicting rules and repeat this in all our projects.

Please add something like `select = ["ALL_COMPAT"]` or `select = ["STRICT"]` to easily enable all rules without any conflicts. If there is something to pick (like `D203` _xor_ `D211`, or `D212` _xor_ `D213`), just pick one! Let us have a default default to follow.

---
