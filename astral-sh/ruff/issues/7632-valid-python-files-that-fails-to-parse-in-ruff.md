```yaml
number: 7632
title: Valid python files, that fails to parse in ruff
type: issue
state: closed
author: qarmin
labels:
  - bug
  - parser
assignees: []
created_at: 2023-09-24T18:18:05Z
updated_at: 2023-10-01T02:28:22Z
url: https://github.com/astral-sh/ruff/issues/7632
synced_at: 2026-01-10T11:09:49Z
```

# Valid python files, that fails to parse in ruff

---

_Issue opened by @qarmin on 2023-09-24 18:18_

Ruff 0.0.291
Cpython 3.11.4

Tested with 
```
ruff format . --check
```
and cpython part
```
python -m compileall .
```

[RuffBroken.zip](https://github.com/astral-sh/ruff/files/12709323/RuffBroken.zip)

Example of file
```
from ..const import d#ta_source_const

def validate_incming_payload(baseballdc_request):

    requested_data_source_trimmed = baseballdc_request['data_source'].strip().upper()

    if(requested_data_source_trimmed not in data_source_const.DATA_SOURCE_LIST):
        raise ValueError(generate_data_source_error_message(baseballdc_request))

def generate_data_source_error_message(baseballdc_request): 

    requested_data_source = baseballdc_request['data_source']
    valid_data_sources = data_source_cons.DATA_SOURCE_LIST

    error_message_text = 'Value error in the incoming request payload:'
    data_sourcÛ_error_text = f'The data source requested ("{requested_data_source}") was not valid.'
    valid_data_source_text = f'The current list of valid datasources to choose from for basblldc is: {valid_data_sources}'

    return f'{error_message_text}\n\n{data_source_erro%_text}\{valid_data_source_text}\n';
```
error
```
error: Failed to format 48_IDX_0_RAND_14765242697460627399.py: source contains syntax errors: ParseError { error: Lexical(FStringError(SingleRbrace)), offset: 908, source_path: "<filename>" }
```

---

_Label `bug` added by @charliermarsh on 2023-09-24 18:48_

---

_Label `parser` added by @charliermarsh on 2023-09-24 18:48_

---

_Comment by @dhruvmanila on 2023-09-25 04:38_

The following files contains f-string errors which will be fixed by #7376. I've verified that those files are parsed without any errors on that branch:
* `335_IDX_0_RAND_13905148532772824398.py`
* `48_IDX_0_RAND_14765242697460627399.py`
* `586_IDX_0_RAND_36061242038301280.py`
* `855_IDX_0_RAND_15689123331827052431.py`

Other files contains identifier starting with unicode character which we're unable to identify which then produces a syntax error.

---

_Comment by @qarmin on 2023-09-29 07:02_

Looks that #7376 introduced regression, and now some valid python files shows errors:
```
error: Failed to format 549_IDX_0_RAND_5726799750657298695.py: source contains syntax errors: LexicalError { error: FStringError(UnterminatedString), location: 3282 }
error: Failed to format 230_IDX_0_RAND_14394415519784108053.py: source contains syntax errors: ParseError { error: UnrecognizedToken(For, None), offset: 1799, source_path: "<filename>" }
```

[RuffBroken.zip](https://github.com/astral-sh/ruff/files/12761815/RuffBroken.zip)


---

_Comment by @MichaReiser on 2023-09-29 08:33_

> Looks that #7376 introduced regression, and now some valid python files shows errors:
> 
> ```
> error: Failed to format 549_IDX_0_RAND_5726799750657298695.py: source contains syntax errors: LexicalError { error: FStringError(UnterminatedString), location: 3282 }
> error: Failed to format 230_IDX_0_RAND_14394415519784108053.py: source contains syntax errors: ParseError { error: UnrecognizedToken(For, None), offset: 1799, source_path: "<filename>" }
> ```
> 
> [RuffBroken.zip](https://github.com/astral-sh/ruff/files/12761815/RuffBroken.zip)

CC: @dhruvmanila 

---

_Comment by @charliermarsh on 2023-09-30 17:15_

@dhruvmanila - We should probably try to fix these regressions before the next release. I can also help out if needed.

---

_Comment by @dhruvmanila on 2023-09-30 18:31_

`230_IDX_0_RAND_14394415519784108053.py`: This fails on 3.12.0rc3 as well because the generator is missing the parentheses. I'm surprised that it parses on 3.11. Maybe it's because the parentheses were added manually? Atleast in our case we did the expression parsing by adding the surrounding parentheses manually. So,

```diff
- f"', '.join({ckt.name for ckt in self._ckts_with_errors})"
+ f"', '.join({(ckt.name for ckt in self._ckts_with_errors)})"
```

It fails in the same way the below code fails:
```python
foo = ckt.name for ckt in self._ckts_with_errors
```

---

`549_IDX_0_RAND_5726799750657298695.py`: This is failing because of Windows newline. A minimal repro:

```python
f'text \\r\n
more text'
```



---

_Closed by @dhruvmanila on 2023-10-01 02:28_

---
