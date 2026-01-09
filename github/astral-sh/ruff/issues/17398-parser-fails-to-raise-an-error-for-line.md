---
number: 17398
title: Parser fails to raise an error for line continuation at EOF
type: issue
state: closed
author: ntBre
labels:
  - bug
  - parser
assignees: []
created_at: 2025-04-14T20:31:31Z
updated_at: 2025-04-15T15:56:13Z
url: https://github.com/astral-sh/ruff/issues/17398
synced_at: 2026-01-07T13:12:16-06:00
---

# Parser fails to raise an error for line continuation at EOF

---

_Issue opened by @ntBre on 2025-04-14 20:31_

### Summary

While going through the remaining errors we don't detect in #7633, I noticed a surprising `unexpected EOF while parsing` error caused by a case like this:

```python
class C:
    def f():
        return 1, \
```

I can't reproduce this in the playground, but this is the output locally with this saved as `try.py`:

```shell
$ python -m ast try.py
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/usr/lib/python3.13/ast.py", line 1871, in <module>
    main()
    ~~~~^^
  File "/usr/lib/python3.13/ast.py", line 1867, in main
    tree = parse(source, name, args.mode, type_comments=args.no_type_comments)
  File "/usr/lib/python3.13/ast.py", line 54, in parse
    return compile(source, filename, mode, flags,
                   _feature_version=feature_version, optimize=optimize)
  File "try.py", line 3
    return 1, \
               ^
SyntaxError: unexpected EOF while parsing
$ ruff check try.py
All checks passed!
```

And I see the same thing for a single `\` as input:

```shell
$ python -m ast <<<'\'
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/usr/lib/python3.13/ast.py", line 1871, in <module>
    main()
    ~~~~^^
  File "/usr/lib/python3.13/ast.py", line 1867, in main
    tree = parse(source, name, args.mode, type_comments=args.no_type_comments)
  File "/usr/lib/python3.13/ast.py", line 54, in parse
    return compile(source, filename, mode, flags,
                   _feature_version=feature_version, optimize=optimize)
  File "<stdin>", line 1
    \
     ^
SyntaxError: unexpected EOF while parsing
$ ruff check - <<<'\'
All checks passed!
```

I'm not sure what's different in the playground, but it detects both of these cases, and it reports a single `Unknown` token in the case of the single `\`.

### Version

ruff 0.11.5 (7186d5e9a 2025-04-10)

---

_Label `bug` added by @ntBre on 2025-04-14 20:31_

---

_Label `parser` added by @ntBre on 2025-04-14 20:31_

---

_Comment by @dhruvmanila on 2025-04-14 21:58_

It might be worth checking whether this discrepancy exists when running the playground locally. Otherwise, we could just check what the `contents` parameter here is which might be different:

https://github.com/astral-sh/ruff/blob/72a3bc810c7f497a2fd3e730b402beadc4080dbd/crates/ruff_wasm/src/lib.rs#L276-L280

Or,

https://github.com/astral-sh/ruff/blob/72a3bc810c7f497a2fd3e730b402beadc4080dbd/crates/ruff_wasm/src/lib.rs#L159-L159

---

_Comment by @ntBre on 2025-04-15 13:14_

I can reproduce the same thing in the local playground. I'm not sure if there's a better way to debug print in WASM, but I just returned errors containing `contents` when I typed a single backslash, and they both showed `\`.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-04-15 14:54_

---

_Referenced in [astral-sh/ruff#17409](../../astral-sh/ruff/pulls/17409.md) on 2025-04-15 14:55_

---

_Comment by @dhruvmanila on 2025-04-15 14:56_

Thanks for confirming. I think I know where the issue is. I've raised a draft PR, will write a couple of test cases and make it ready for review.

---

_Comment by @dhruvmanila on 2025-04-15 15:56_

> I'm not sure what's different in the playground, but it detects both of these cases, and it reports a single `Unknown` token in the case of the single `\`.

For context, the reason for this is because the playground requires us to explicitly add the final newline character in the source: https://play.ruff.rs/e6418bb3-ddf0-4143-9e82-b1c117472348

---

_Closed by @dhruvmanila on 2025-04-15 15:56_

---

_Closed by @dhruvmanila on 2025-04-15 15:56_

---
