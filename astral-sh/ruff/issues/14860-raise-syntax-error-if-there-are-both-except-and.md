```yaml
number: 14860
title: "Raise syntax error if there are both `except` and `except*` in the same `try` block"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - help wanted
  - parser
assignees: []
created_at: 2024-12-09T05:50:56Z
updated_at: 2024-12-10T23:50:56Z
url: https://github.com/astral-sh/ruff/issues/14860
synced_at: 2026-01-12T15:54:54Z
```

# Raise syntax error if there are both `except` and `except*` in the same `try` block

---

_@dhruvmanila_

The task is to fix this TODO:

https://github.com/astral-sh/ruff/blob/1bd8fbb6e862259a4515f180c2b2ab119d3ffc23/crates/ruff_python_parser/src/parser/statement.rs#L1341-L1355

I'd consider this as a bug because the AST marks the entire `try` block as containing an `except*` and because the parser doesn't raise a syntax error, the formatter will add the `*` to all `except` block (https://play.ruff.rs/059d8f35-5f66-4be5-887e-9f38fa75b40e):

```diff
  try:
      pass
- except:
+ except*:
      pass
  except* ExceptionGroup:
      pass
```

CPython parser (not the compiler) also raises a syntax error:

```console
$ pbpaste | python -m ast -
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/dhruv/.pyenv/versions/3.12.1/lib/python3.12/ast.py", line 1834, in <module>
    main()
  File "/Users/dhruv/.pyenv/versions/3.12.1/lib/python3.12/ast.py", line 1830, in main
    tree = parse(source, args.infile.name, args.mode, type_comments=args.no_type_comments)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/dhruv/.pyenv/versions/3.12.1/lib/python3.12/ast.py", line 52, in parse
    return compile(source, filename, mode, flags,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<stdin>", line 5
    except* ExceptionGroup:
    ^^^^^^^
SyntaxError: cannot have both 'except' and 'except*' on the same 'try'
```

---

_Label `bug` added by @dhruvmanila on 2024-12-09 05:50_

---

_Label `parser` added by @dhruvmanila on 2024-12-09 05:50_

---

_Label `help wanted` added by @dhruvmanila on 2024-12-09 05:51_

---

_Closed by @dylwil3 on 2024-12-10 23:50_

---

_Closed by @dylwil3 on 2024-12-10 23:50_

---
