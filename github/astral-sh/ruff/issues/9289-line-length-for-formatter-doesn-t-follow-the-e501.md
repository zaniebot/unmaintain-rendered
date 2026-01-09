---
number: 9289
title: "line-length for formatter doesn't follow the E501 lint rules."
type: issue
state: closed
author: jungwookim
labels:
  - question
assignees: []
created_at: 2023-12-27T03:15:36Z
updated_at: 2023-12-27T22:32:10Z
url: https://github.com/astral-sh/ruff/issues/9289
synced_at: 2026-01-07T13:12:15-06:00
---

# line-length for formatter doesn't follow the E501 lint rules.

---

_Issue opened by @jungwookim on 2023-12-27 03:15_

I want to set the proper line length but its behaviour is pretty weird. I've attached the reproducible example code and my configurations below.

```bash
$ ruff --version
ruff 0.1.8

$ ruff check a.py
a.py:5:81: E501 Line too long (108 > 80)
a.py:9:81: E501 Line too long (96 > 80)
```

After I fix the E501 above with new line, I format it with `ruff format a.py`. It produces line too long again.

In other words, ruff format doesn't do anything with wrap lines even they violate their own rule (i set line-length = 80 but it rolls back)

## pyproject.toml
I skipped some unrelated configurations.

```toml
[tool.ruff]
line-length = 80
indent-width = 4

# Assume Python 3.10
# NOTE: Need to be same as DEFAULT_PYTHON_TAG in build_tools/w4.bzl.
target-version = "py310"

[tool.ruff.lint]
select = ["E4", "E7", "E9", "F", "E501"]
ignore = []

[tool.ruff.lint.pycodestyle]
max-line-length = 80
```

## example code

```python
# a.py (origin)
def line_too_long_test_fn():
    config_enums = {'DataType': {'TEST_DATA_1': 0, 'TEST_DATA_2': 1}}
    test_data_1 = {
        'LessThan80': 'This is a test string that is less than 80 characters',
        'MoreThan80CharactersMoreThan80Characters': 'This is a test string that is more than 80 characters',
    }
    test_data = {
        config_enums.DataType.TEST_DATA_1: test_data_1.LessThan80,
        config_enums.DataType.TEST_DATA_2: test_data_1.MoreThan80CharactersMoreThan80Characters,
    }

    return test_data
```

```python
# a.py (I fixed)
def line_too_long_test_fn():
    config_enums = {'DataType': {'TEST_DATA_1': 0, 'TEST_DATA_2': 1}}
    test_data_1 = {
        'MoreThan80CharactersMoreThan80Characters':
            'This is a test string that is more than 80 characters',
    }
    test_data = {
        config_enums.DataType.TEST_DATA_2:
            test_data_1.MoreThan80CharactersMoreThan80Characters,
    }

    return test_data
```


```python
# a.py (after ruff format)
def line_too_long_test_fn():
    config_enums = {'DataType': {'TEST_DATA_1': 0, 'TEST_DATA_2': 1}}
    test_data_1 = {
        'MoreThan80CharactersMoreThan80Characters': 'This is a test string that is more than 80 characters',
    }
    test_data = {
        config_enums.DataType.TEST_DATA_2: test_data_1.MoreThan80CharactersMoreThan80Characters,
    }

    return test_data
```

I guess it happens on dictionary data or edge cases or a bug?..?

---

_Comment by @charliermarsh on 2023-12-27 13:53_

Thanks! I believe this is only handled as part of Black's preview style (#8437) -- note that Black does not reformat in this case: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AJ8AQ5dAD2IimZxl1N_WlbvK5V-T0Ww3PRf-11njtYzMrWsaS3TRmKAkAKHBe-gctPasA09OYB17rb6BAdKGuTIrNpp8UTVoUaMUhqWdUHOHIacwiygEsEI1DsgOkazDIUZxfybAW9FQZlu8DJTBJ5q6Gdk_jG_sPsKaU-TIdllvTjRRqobO4UXHxnKlBsfWgUtFURbh_dDhtxESCpTFoJMyFu2DICb__XKdsd372sWNpwwXBjuY43Hyang3a-xjZR517pphtuOiOt9p_UzNn-G8u-vuJ6ZYL-dp1dpEMI_CC2A937-CbK_SCaTw8ugV5IJWHYCQuE-lMbmvJzVn19GFL3jRSf3xLdpFQ0NgDEBDN2gvwAAABOMzzQ8w--hAAGqAv0EAACT0xdyscRn-wIAAAAABFla -- so I consider it a duplicate of that issue.

Note that this is (to some degree) expected... The formatter uses the line length as a guideline, but no formatter can guarantee that _all_ content will be reformatted within that line length. (We recommend disabling E501 when using the formatter, though strictly speaking it's not considered "incompatible".)


---

_Closed by @charliermarsh on 2023-12-27 13:53_

---

_Label `question` added by @zanieb on 2023-12-27 22:32_

---
