```yaml
number: 20930
title: Avoid reusing nested, interpolated quotes before Python 3.12
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: brent/f-string-fix
created_at: 2025-10-16T20:33:45Z
updated_at: 2025-10-17T12:49:18Z
url: https://github.com/astral-sh/ruff/pull/20930
synced_at: 2026-01-10T17:34:34Z
```

# Avoid reusing nested, interpolated quotes before Python 3.12

---

_Pull request opened by @ntBre on 2025-10-16 20:33_

## Summary

Fixes #20774 by tracking whether an `InterpolatedStringState` element is nested inside of another interpolated element. This feels like kind of a naive fix, so I'm welcome to other ideas. But it resolves the problem in the issue and clears up the syntax error in the black compatibility test, without affecting many other cases.

The other affected case is actually interesting too because the [input](https://github.com/astral-sh/ruff/blob/96b156303b81c5114e8375a6ffd467fb638c3963/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py#L707) is invalid, but the previous quote selection fixed the invalid syntax:

```pycon
Python 3.11.13 (main, Sep  2 2025, 14:20:25) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> f'{1: abcd "{'aa'}" }'  # input
  File "<stdin>", line 1
    f'{1: abcd "{'aa'}" }'
                  ^^
SyntaxError: f-string: expecting '}'
>>> f'{1: abcd "{"aa"}" }'  # old output
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Invalid format specifier ' abcd "aa" ' for object of type 'int'
>>> f'{1: abcd "{'aa'}" }'  # new output
  File "<stdin>", line 1
    f'{1: abcd "{'aa'}" }'
                  ^^
SyntaxError: f-string: expecting '}'
```

We now preserve the invalid syntax in the input.

Unfortunately, this also seems to be another edge case I didn't consider in https://github.com/astral-sh/ruff/pull/20867 because we don't flag this as a syntax error after 0.14.1:

<details><summary>Shell output</summary>
<p>

```
> uvx ruff@0.14.0 check --ignore ALL --target-version py311 - <<EOF
f'{1: abcd "{'aa'}" }'
EOF
invalid-syntax: Cannot reuse outer quote character in f-strings on Python 3.11 (syntax was added in Python 3.12)
 --> -:1:14
  |
1 | f'{1: abcd "{'aa'}" }'
  |              ^
  |

Found 1 error.
> uvx ruff@0.14.1 check --ignore ALL --target-version py311 - <<EOF
f'{1: abcd "{'aa'}" }'
EOF
All checks passed!
> uvx python@3.11 -m ast <<EOF
f'{1: abcd "{'aa'}" }'
EOF
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/brent/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/lib/python3.11/ast.py", line 1752, in <module>
    main()
  File "/home/brent/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/lib/python3.11/ast.py", line 1748, in main
    tree = parse(source, args.infile.name, args.mode, type_comments=args.no_type_comments)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/brent/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/lib/python3.11/ast.py", line 50, in parse
    return compile(source, filename, mode, flags,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<stdin>", line 1
    f'{1: abcd "{'aa'}" }'
                  ^^
SyntaxError: f-string: expecting '}'
```

</p>
</details> 


I assumed that was the same `ParseError` as the one caused by `f"{1:""}"`, but this is a nested interpolation inside of the format spec.

## Test Plan

New test copied from the black compatibility test. I guess this is a duplicate now, I started working on this branch before the new black tests were imported, so I could delete the separate test in our fixtures if that's preferable.

---

_Label `bug` added by @ntBre on 2025-10-16 20:34_

---

_Label `formatter` added by @ntBre on 2025-10-16 20:34_

---

_Renamed from "Brent/f string fix" to "Avoid reusing nested, interpolated quotes before Python 3.12" by @ntBre on 2025-10-16 20:35_

---

_Comment by @github-actions[bot] on 2025-10-16 20:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-10-16 20:46_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-16 20:46_

---

_Comment by @MichaReiser on 2025-10-17 07:42_

I like the solution. It's very non-invasive, comes with little code and runtime complexity and it preserves the syntax error as is (which I think is better than trying to fixing syntax errors and than creating an even bigger mess)

---

_@MichaReiser approved on 2025-10-17 07:42_

---

_Merged by @ntBre on 2025-10-17 12:49_

---

_Closed by @ntBre on 2025-10-17 12:49_

---

_Branch deleted on 2025-10-17 12:49_

---
