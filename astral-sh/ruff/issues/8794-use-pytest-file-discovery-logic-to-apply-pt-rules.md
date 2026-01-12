```yaml
number: 8794
title: "Use pytest file discovery logic to apply `PT` rules"
type: issue
state: open
author: dhruvmanila
labels:
  - linter
assignees: []
created_at: 2023-11-20T19:27:05Z
updated_at: 2024-08-02T07:08:56Z
url: https://github.com/astral-sh/ruff/issues/8794
synced_at: 2026-01-12T15:54:48Z
```

# Use pytest file discovery logic to apply `PT` rules

---

_@dhruvmanila_

For additional context, refer to https://github.com/astral-sh/ruff/issues/8145#issuecomment-1817657808 and the following comments.

> For me that would not help as I do not have a `tests/` directory. Instead, I have test files next to the files to be tested. e.g. `mypkg/util/string.py` has its test in `mypkg/util/string_test.py` and so on. I think for larger applications this is not uncommon.
> 
> I think ideally it would apply the [pytest rules for test discovery](https://docs.pytest.org/en/7.1.x/explanation/goodpractices.html#conventions-for-python-test-discovery), ideally with override settings or parsing of the [respective pytest.ini options](https://docs.pytest.org/en/7.1.x/example/pythoncollection.html#changing-naming-conventions).

> I think mirroring the pytest rules for discovery would be sensible. It would also solve #8703, and arguably solve #6474.

> However, I definitely want to avoid parsing `pytest.ini` and interpreting pytest configuration -- it's just a huge pain to implement and maintain (and also requires that we know the pytest version and keep our settings in-sync with pytest's settings). So if we _did_ do this, we'd need to stick to the defaults, _or_ add our own limited configuration.



---

_Label `linter` added by @dhruvmanila on 2023-11-20 19:27_

---

_Comment by @flying-sheep on 2024-08-02 07:08_

And the other way around, some rules never make sense for pytest,
and should always be disabled:

```toml
[tool.ruff.lint.per-file-ignores]
'tests/**/*.py' = [
    'INP001', # Test directories are not namespace packages
    'S101',   # Pytest tests should use `assert`
    'RUF018', # Assignment expressions in assert are fine here
]
```

---
