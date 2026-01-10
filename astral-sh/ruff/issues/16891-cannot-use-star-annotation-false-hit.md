```yaml
number: 16891
title: Cannot use star annotation false hit
type: issue
state: closed
author: nijel
labels:
  - question
assignees: []
created_at: 2025-03-21T09:31:45Z
updated_at: 2025-03-21T09:51:49Z
url: https://github.com/astral-sh/ruff/issues/16891
synced_at: 2026-01-10T11:09:58Z
```

# Cannot use star annotation false hit

---

_Issue opened by @nijel on 2025-03-21 09:31_

### Summary

ruff 0.11.1 started to fail with:

> Error: translate/storage/fluent.py:1221:38: SyntaxError: Cannot use star annotation on Python 3.9 (syntax was added in Python 3.11)

I don't think this is a syntax error when the affected code is passing CI tests on Python 3.9 since its introduction.

Problematic code with the error:

https://github.com/translate/translate/pull/5548/files#file-translate-storage-fluent-py-L1221

Reproducer:

```py
def _combine_comments(*comments: str) -> str:
    return ",".join(comments)
```

This works just fine with Python 3.9.21. It can also be properly type checked with mypy on Python 3.9.21.


### Version

 0.11.1

---

_Comment by @dhruvmanila on 2025-03-21 09:37_

Thank you for reporting! This is fixed on `main`, we will do a release today to get it out.

---

_Closed by @dhruvmanila on 2025-03-21 09:37_

---

_Comment by @dhruvmanila on 2025-03-21 09:40_

Hmm, actually I might've mis-understood the issue.

---

_Reopened by @dhruvmanila on 2025-03-21 09:42_

---

_Label `question` added by @dhruvmanila on 2025-03-21 09:42_

---

_Comment by @dhruvmanila on 2025-03-21 09:43_

So quickly looking at the `pyproject.toml` in the project, you've configured Ruff's [target-version to be 3.9](https://github.com/translate/translate/blob/3c9628723899acaee2cd504e6c7dbb16a8f19b77/pyproject.toml#L235), with preview mode enabled.

This syntax was actually added in Python 3.11 with [PEP 646](https://peps.python.org/pep-0646/).

In preview mode, Ruff will be raising syntax errors by checking whether the syntax actually exists on the configured target version.

---

_Comment by @dhruvmanila on 2025-03-21 09:45_

> I don't think this is a syntax error when the affected code is passing CI tests on Python 3.9 since its introduction.

I do get an error when running it under 3.9:

```console
$ pbpaste | uv run --python=3.9 --no-project python -m ast -
Traceback (most recent call last):
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/runpy.py", line 197, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/ast.py", line 1600, in <module>
    main()
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/ast.py", line 1596, in main
    tree = parse(source, args.infile.name, args.mode, type_comments=args.no_type_comments)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/ast.py", line 50, in parse
    return compile(source, filename, mode, flags,
  File "<stdin>", line 1
    def _combine_comments(*comments: *str) -> str:
                                     ^
SyntaxError: invalid syntax
```

---

_Comment by @nijel on 2025-03-21 09:46_

https://github.com/astral-sh/ruff/pull/16878 seems to address the issue, if I understand it correctly. I just made the report confusing, removed the confusing part now, sorry.

---

_Comment by @dhruvmanila on 2025-03-21 09:51_

No problem! Yeah, that does fix your issue :)

---

_Closed by @dhruvmanila on 2025-03-21 09:51_

---
