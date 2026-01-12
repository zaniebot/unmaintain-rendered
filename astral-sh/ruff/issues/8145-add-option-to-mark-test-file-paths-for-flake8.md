```yaml
number: 8145
title: "Add option to mark test file paths for `flake8-pytest-style`"
type: issue
state: closed
author: a-recknagel
labels:
  - question
assignees: []
created_at: 2023-10-23T12:42:45Z
updated_at: 2024-12-27T20:54:50Z
url: https://github.com/astral-sh/ruff/issues/8145
synced_at: 2026-01-12T15:54:47Z
```

# Add option to mark test file paths for `flake8-pytest-style`

---

_@a-recknagel_

I don't want to run any linters for my test code, except for `flake8-pytest-style`. It would be nice if I could give it the path(s) to the test files somehow, so that I don't need to have two distinct configurations for running `ruff`.

workaround:
```shell
$ ruff src  # uses my config, checks my source code
$ ruff --isolated --select PT tests  # only uses PT, checks my test code
```

And the second line would also need to include any other `flake8-pytest-style` or top-level options that it needs. Being able to set something like 

```toml
[tool.ruff.flake8-pytest-style]
test-linting-only-path = ["tests"]
```
which would exclude these files/folders from any other linters given a call `ruff src tests` would help.

---

_Renamed from "Add option to set path for `flake8-pytest-style`" to "Add option to mark test file paths for `flake8-pytest-style`" by @a-recknagel on 2023-10-23 12:43_

---

_Comment by @dhruvmanila on 2023-10-30 02:58_

Do you think using [`extend`](https://docs.astral.sh/ruff/settings/#extend) would help in your scenario? So, you would add a `pyproject.toml` in the `tests/` directory which would `extend` from the root `pyproject.toml` file and add a [`extend-select = ["PT"]`](https://docs.astral.sh/ruff/settings/#extend-select) to it.

---

_Label `question` added by @dhruvmanila on 2023-10-30 02:58_

---

_Comment by @ThiefMaster on 2023-11-18 21:48_

For me that would not help as I do not have a `tests/` directory. Instead, I have test files next to the files to be tested. e.g. `mypkg/util/string.py` has its test in `mypkg/util/string_test.py` and so on. I think for larger applications this is not uncommon.

I think ideally it would apply the [pytest rules for test discovery](https://docs.pytest.org/en/7.1.x/explanation/goodpractices.html#conventions-for-python-test-discovery), ideally with override settings or parsing of the [respective pytest.ini options](https://docs.pytest.org/en/7.1.x/example/pythoncollection.html#changing-naming-conventions).

---

_Comment by @charliermarsh on 2023-11-18 23:10_

I think mirroring the pytest rules for discovery would be sensible. It would also solve https://github.com/astral-sh/ruff/issues/8703, and arguably solve https://github.com/astral-sh/ruff/issues/6474.

---

_Comment by @charliermarsh on 2023-11-18 23:12_

However, I definitely want to avoid parsing `pytest.ini` and interpreting pytest configuration -- it's just a huge pain to implement and maintain (and also requires that we know the pytest version and keep our settings in-sync with pytest's settings). So if we _did_ do this, we'd need to stick to the defaults, _or_ add our own limited configuration.

---

_Comment by @dhruvmanila on 2023-11-20 19:27_

Thanks! I think we can close this one out as the suggested solution should work for @a-recknagel case but happy to re-open if it doesn't.

As for the @ThiefMaster case, I'll open a new issue to track the change (#8794).


---

_Closed by @dhruvmanila on 2023-11-20 19:27_

---

_Comment by @a-recknagel on 2023-11-21 09:29_

No need to re-open, `extend` works for me. Thanks!

---

_Comment by @a-recknagel on 2023-11-21 09:46_

just to spell it out once in case someone has the same issue, this is what I ended up doing which I'm happy enough with:

**project structure**
```
.
├── src
│   └── my_package
│       ├── __init__.py
│       └── foo.py
├── tests
│   ├── ruff.toml
│   └── my_package
│       └── test_foo.py
└── pyproject.toml
```

**pyproject.toml**
```toml
["..."]  # other settings that don't matter here
[tool.ruff]
# settings I want to share between both src and test linting
src = ["src"]
isort = { combine-as-imports = true, force-wrap-aliases = true }
# src-specific rule set
select = ["E", "F", "W", "N", "I", "UP", "..." , "D", "RUF"]
```

**tests/ruff.toml**
```toml
# the magic word I was missing
extend = "../pyproject.toml"
# test-specific rule set
select = ["F", "N", "I", "PT", "RUF"]
```

**what I run**
```shell
$ ruff src
$ ruff tests --config tests/ruff.toml
```

---

_Comment by @sjdemartini on 2024-12-27 20:54_

For what it's worth, I found the following to be an easier option for keeping these rules specific to test files in my `tests/` directory, which is to use `per-file-ignores` in my single top-level ruff `pyproject.toml` config file:

```toml
[tool.ruff.lint.per-file-ignores]
# Ignore pytest rules outside tests directory
"!tests/**/*" = ["PT"]
```

---
