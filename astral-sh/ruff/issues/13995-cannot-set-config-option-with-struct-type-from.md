```yaml
number: 13995
title: Cannot set config option with struct type from command-line
type: issue
state: closed
author: LordAro
labels:
  - cli
  - help wanted
assignees: []
created_at: 2024-10-30T12:20:10Z
updated_at: 2025-01-06T13:20:29Z
url: https://github.com/astral-sh/ruff/issues/13995
synced_at: 2026-01-10T11:09:55Z
```

# Cannot set config option with struct type from command-line

---

_Issue opened by @LordAro on 2024-10-30 12:20_

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

While trying out some adhoc testing, I ran the following:
```
ruff check --extend-select=PT --config 'lint.flake8-pytest-style="csv"'
```

which resulted in the error:

```
error: invalid value 'lint.flake8-pytest-style="csv"' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

Could not parse the supplied argument as a `ruff.toml` configuration option:

invalid type: string "csv", expected struct Flake8PytestStyleOptions
in `lint`

For more information, try '--help'
```

according to the tip (and the help text) it *should* work, but... doesn't. I'm not sure if there is a format where it does work, but if nothing else it's unclear.

I can imagine this applies to other options that use this form, but this is the only one I've found.

Ruff: 0.7.1

---

_Comment by @MichaReiser on 2024-10-30 12:40_

I think  you're missing the last name of the setting: `lint` and `flake8-pytest-style` are only the setting group:

```
ruff check --extend-select=PT --config lint.flake8-pytest-style.parametrize-names-type="csv"
```

---

_Label `question` added by @MichaReiser on 2024-10-30 12:40_

---

_Comment by @MichaReiser on 2024-10-30 12:44_

Okay, it's slightly more complicated because the value must be a string but this should work

```bash
ruff check --extend-select=PT --config "lint.flake8-pytest-style.parametrize-names-type='csv'"
```

---

_Comment by @LordAro on 2024-10-30 14:29_

Oh you're quite right, I'm a fool and managed to only copy the section name rather than the rule itself.

Not that I want to be that guy, but maybe the error message could be improved somehow? Mentioning a struct seems unhelpful...

---

_Comment by @MichaReiser on 2024-10-30 15:02_

> Not that I want to be that guy, but maybe the error message could be improved somehow? Mentioning a struct seems unhelpful...

That makes a lot of sense to me. I also had to play with the CLI to determine the right syntax (see my first answer). I'm not sure how much we can do about the struct part without writing our own deserialization framework but we should take a look if there are any low hanging improvements

---

_Label `question` removed by @MichaReiser on 2024-10-30 15:02_

---

_Label `cli` added by @MichaReiser on 2024-10-30 15:02_

---

_Label `help wanted` added by @MichaReiser on 2024-10-30 15:02_

---

_Closed by @AlexWaygood on 2025-01-06 13:20_

---
