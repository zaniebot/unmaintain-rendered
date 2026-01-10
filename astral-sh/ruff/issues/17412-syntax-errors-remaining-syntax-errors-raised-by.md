---
number: 17412
title: "[syntax-errors] Remaining syntax errors raised by the compiler"
type: issue
state: open
author: ntBre
labels:
  - help wanted
  - tracking
assignees: []
created_at: 2025-04-15T18:34:46Z
updated_at: 2025-12-04T14:39:08Z
url: https://github.com/astral-sh/ruff/issues/17412
synced_at: 2026-01-10T01:22:58Z
---

# [syntax-errors] Remaining syntax errors raised by the compiler

---

_Issue opened by @ntBre on 2025-04-15 18:34_

## Summary

A number of semantic or compile-time syntax errors have already been implemented
in #11934, but there are still many remaining ones. The purpose of this issue is
to track progress on this remainder. The initial source for most of these is
[this comment], which reports fuzzer results comparing Python's compiler and
ruff, but others may be included over time.

The next section lists the syntax errors that still need to be implemented, and
the sections after that include some more information on getting started with an
implementation.

## Syntax Errors

### Errors mapping to current rules

These might be the easiest to start with because the existing rules have working
implementations and corresponding tests. However, they could possibly require
new context methods (see below), which could complicate things.

- [x] `return` with value in async generator (B901)
  - https://github.com/astral-sh/ruff/pull/21200
  - partial overlap with B901, which also applies to sync generators. This is
    only a syntax error in the async case
- [x] no binding for nonlocal (PLE0117)
  - https://github.com/astral-sh/ruff/pull/21032
- [x] `break` outside loop (F701)
  - https://github.com/astral-sh/ruff/pull/20556
  - https://github.com/astral-sh/ruff/pull/20944
- [x] `continue` not properly in loop (F702)
  - https://github.com/astral-sh/ruff/pull/20869
  - https://github.com/astral-sh/ruff/pull/20944
- [x] `yield from` in async function (PLE1700)
  - https://github.com/astral-sh/ruff/pull/20051
- [x] future feature is not defined (F407)
  - https://github.com/astral-sh/ruff/pull/20554
- [x] import `*` only allowed at module level (F406)
  - https://github.com/astral-sh/ruff/pull/20166
- [x] multiple starred expressions in assignment (F622)
  - https://github.com/astral-sh/ruff/pull/20243

### Errors mapping to current parse errors

These are also straightforward and will be similar to #17131.

- duplicate keyword argument (https://github.com/astral-sh/ruff/pull/17804)
  ```python
  def foo(x): ...
  
  foo(x=1, x=2)  # SyntaxError: keyword argument repeated: x
  ```

### New errors

- [x] name is parameter and global (https://github.com/astral-sh/ruff/pull/20426)
  ``` python
  def f(a): global a
  ```
- [ ] name is parameter and nonlocal
  ``` python
  def f(a): nonlocal a
  ```
- [ ] annotated name can't be global (https://github.com/astral-sh/ruff/pull/20868)
  ``` python
  x: int = 1

  def f():
      global x
      x: str = "foo"  # SyntaxError: annotated name 'x' can't be global
  ```
- [x] alternative patterns bind different names (https://github.com/astral-sh/ruff/pull/20682)
    ```python
    match 42:
        case [x] | [y]: ...
    ```
    This can probably be handled in the `MatchPatternVisitor` along with the other `match`-related errors.

- [x] can't use starred expression here (https://github.com/astral-sh/ruff/pull/17624)
  ``` python
  x = *value
  ```
- [x] nonlocal declaration not allowed at module level (https://github.com/astral-sh/ruff/pull/17559)
  ``` python
  nonlocal x
  ```

### Extensions to existing syntax errors

- Assigning to and deleting attributes named `__debug__` is also a syntax error
  ```python
  __debug__ = False  # Currently caught by ruff
  x.__debug__ = False  # Not currently caught, but also a syntax error
  ```

### Encoding issues

These syntax errors are all related to #6791 and should only be implemented if
we decide to respect these alternate encodings in general.

These can also be a bit tricky to put into a file, so most of these have shell
code snippets for generating the input rather than Python code directly.

- ascii codec can't decode byte `...` in position `...`: ordinal not in `range(128)`

  ```shell
  printf '# -*- coding: ascii -*-\n\xc2' > example.py
  ```

- charmap codec can't decode byte ...

  ```shell
  printf '# -*- coding: cp1252 -*-\n\x8d' > example.py
  ```

- utf7 codec can't decode byte 0xc3

  ```shell
  printf '# -*- coding: utf-7 -*-\n\xc3' > example.py
  ```

  This one reports an `E902` error, but only if it's enabled. Otherwise we only print a warning about invalid UTF-8.

- encoding problem: `...` with BOM

  ``` shell
  printf '\ufeff# -*- coding: ascii -*-' > example.py
  ```

  I think any encoding other than UTF-8 with a BOM (`\uFEFF`) will cause this issue.

- invalid character
- invalid non-printable character
  - both this and `invalid character` mostly seem to happen with the
    `iso-8859-1` encoding and a unicode character

- unknown encoding

  ```python
  # coding: not-a-real-encoding
  ```
- source code string cannot contain null bytes
  ```shell
  printf '# \x00' > example.py
  ```
  This mostly seems like an issue in comments. This also isn't exactly an encoding error, but it's more closely related to these. It's also raised by the CPython parser, not the compiler.

## Implementation

Issue #11934 contains links to each PR implementing a new semantic syntax error.
These can be used as examples of what a new implementation looks like, but this
section has a few more general notes.

All of the semantic syntax errors currently live in [`semantic_errors.rs`] and
correspond to a variant of the [`SemanticSyntaxErrorKind`] enum. The
[`SemanticSyntaxChecker`] is responsible for emitting these errors, but unlike
other AST visitors, it does not generally traverse the AST on its owns[^1]. Instead,
it relies on a parent visitor to call its `visit_stmt` and `visit_expr` methods
in the parent visitor's `visit_stmt` or `visit_expr` methods.

The [`SemanticSyntaxChecker`] also tracks very little of its own state. Instead,
it again defers to the parent visitor via the [`SemanticSyntaxContext`] trait.
Examples of methods on this trait include:
* `python_version` -- returns the target Python version for linting
* `in_async_context` -- returns whether or not async code like `await` or `async
  for` loops are allowed in the current scope

Typically the parent visitor, like the [`Checker`] from the `ruff_linter` crate,
will implement [`SemanticSyntaxContext`] and pass itself to any
[`SemanticSyntaxChecker`] method requiring a context.

Another important context method is `report_semantic_error`, which may require
special handling of new [`SemanticSyntaxErrorKind`] variants. For example, some
syntax errors emitted by CPython overlap with existing ruff rules (e.g.
[`await-outside-async`
(PLE1142)](https://docs.astral.sh/ruff/rules/await-outside-async/)). These need
to be transformed into normal ruff [`Diagnostic`]s instead of being reported
directly as syntax errors. This happens within the
[`Checker::report_semantic_error`] method.

## Testing

There are two main ways of testing new semantic errors. The first, and easier,
method is to write inline parser tests using `test_ok` and `test_err` comments,
for example:

https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/src/semantic_errors.rs#L80-L95

These are automatically extracted from the source code by the parser's
[`generate_inline_tests`] function and then run like normal snapshot tests.
These work very well for simple errors that don't require detailed information
from the [`SemanticSyntaxContext`].

For errors that do require such contextual information tracked by the parent
visitor, a better alternative is to use integration tests, such as those found
in `linter.rs` in the `ruff_linter` crate:

https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_linter/src/linter.rs#L1015-L1025

https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_linter/src/linter.rs#L1064-L1066

The second of these can be reused for other semantic errors that correspond to
ruff rules, while something like the first example can be used for new
syntax-error-specific tests. In both cases, these integration tests will take
advantage of the existing context tracking done by the real [`Checker`] instead
of trying to duplicate that tracking in the parser's test
[`SemanticSyntaxCheckerVisitor`].

## Comparing to the fuzzer results

Again, the initial errors in this issue were extracted from [this comment] in
#7633 by following this procedure:
1. Download and unzip the fuzzer results from [here](https://github.com/qarmin/Automated-Fuzzer/releases/download/test/Cpython.no.parsable.parsable.by.ruff.zip)
2. Run the Python script below to get a partially-deduplicated list of errors
3. Double check these errors against ruff and CPython's parser and compiler

<details>
<summary>Python script for processing fuzzer results</summary>

```python
import argparse
import compileall
import contextlib
import dataclasses
import io
import os
import re
import subprocess
from collections import Counter

FILE = re.compile(r'File "([^"]+)", line (\d+)')
MESSAGE = re.compile(r"SyntaxError: (.*)")


@dataclasses.dataclass
class Error:
    file: str
    line_number: int
    message: str


# Ruff rules already re-implemented as semantic syntax errors, not just rules
# that match CPython syntax errors.
RULES = [
    "F404",
    "F704",
    "F706",
    "PLE0118",
    "PLE1142",
]


def run_ruff(ruff_bin, path):
    return subprocess.run(
        [
            ruff_bin,
            "check",
            "--no-cache",
            "--silent",
            "--preview",
            "--config",
            f"lint.select={RULES}",
            path,
        ]
    ).returncode


def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--ruff-bin", default="ruff")

    args = parser.parse_args()

    with (
        contextlib.redirect_stdout(io.StringIO()) as out,
        contextlib.redirect_stderr(os.devnull),
    ):
        compileall.compile_dir(".", force=True, quiet=1)

    seen_messages = Counter()
    errors = list()

    file, line_number = None, None
    for line in out.getvalue().splitlines():
        if m := FILE.search(line):
            file, line_number = m.groups()
        if m := MESSAGE.search(line):
            message = m[1]
            # combine repetitive messages into single entries
            for prefix in [
                "unknown encoding",
                "invalid escape sequence",
                "'ascii' codec can't decode",
                "future feature",
                "invalid character",
                "invalid non-printable character",
                "no binding for nonlocal",
            ]:
                if message.startswith(prefix):
                    message = prefix
                    break
            if (
                message not in seen_messages
                and run_ruff(args.ruff_bin, file) == 0
            ):
                errors.append(Error(file, line_number, message))

            seen_messages[message] += 1

    for error in sorted(errors, key=lambda e: e.message):
        count = seen_messages[error.message]
        print(f"{error.file}:{error.line_number}: {error.message} ({count})")


if __name__ == "__main__":
    main()
```
</p>
</details>

[^1]: There are some local violations of this principle. For example, the
    [`ReboundComprehensionVisitor`] recursively visits the expressions in a
    comprehension looking for any expressions rebinding one of the
    comprehension's iteration variables. However, this is a highly-localized AST
    traversal and won't have the same kind of performance implications as an
    additional full AST traversal.

[this comment]: https://github.com/astral-sh/ruff/issues/7633#issuecomment-1740424031
[`semantic_errors.rs`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/src/semantic_errors.rs
[`SemanticSyntaxErrorKind`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/src/semantic_errors.rs#L891
[`SemanticSyntaxChecker`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/src/semantic_errors.rs#L19
[`ReboundComprehensionVisitor`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/src/semantic_errors.rs#L1291
[`SemanticSyntaxContext`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/src/semantic_errors.rs#L1586C11-L1586C32
[`Checker`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_linter/src/checkers/ast/mod.rs#L182
[`Diagnostic`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_diagnostics/src/diagnostic.rs#L22
[`Checker::report_semantic_error`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_linter/src/checkers/ast/mod.rs#L572
[`generate_inline_tests`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/tests/generate_inline_tests.rs#L24
[`SemanticSyntaxCheckerVisitor`]: https://github.com/astral-sh/ruff/blob/014bb526f45afd654e5f42db92310e98b5068a54/crates/ruff_python_parser/tests/fixtures.rs#L470



---

_Label `help wanted` added by @ntBre on 2025-04-15 18:34_

---

_Label `tracking` added by @ntBre on 2025-04-15 18:34_

---

_Renamed from "[syntax-errors] Remaining errors raised by the compiler" to "[syntax-errors] Remaining syntax errors raised by the compiler" by @ntBre on 2025-04-15 18:38_

---

_Referenced in [astral-sh/ruff#15506](../../astral-sh/ruff/issues/15506.md) on 2025-04-17 20:46_

---

_Referenced in [astral-sh/ruff#17492](../../astral-sh/ruff/pulls/17492.md) on 2025-04-20 08:42_

---

_Referenced in [astral-sh/ty#120](../../astral-sh/ty/issues/120.md) on 2025-04-21 13:08_

---

_Referenced in [astral-sh/ruff#17463](../../astral-sh/ruff/pulls/17463.md) on 2025-04-21 13:14_

---

_Referenced in [astral-sh/ruff#7633](../../astral-sh/ruff/issues/7633.md) on 2025-04-22 13:43_

---

_Referenced in [astral-sh/ruff#17559](../../astral-sh/ruff/pulls/17559.md) on 2025-04-22 17:54_

---

_Referenced in [astral-sh/ruff#17131](../../astral-sh/ruff/pulls/17131.md) on 2025-04-23 15:56_

---

_Referenced in [astral-sh/ruff#17624](../../astral-sh/ruff/pulls/17624.md) on 2025-04-25 06:07_

---

_Referenced in [astral-sh/ruff#17804](../../astral-sh/ruff/pulls/17804.md) on 2025-05-03 01:28_

---

_Comment by @abhijeetbodas2001 on 2025-05-05 18:26_

> generator expression must be parenthesized 

Regardless of whether there is a trailing comma or not, the exact same AST is generated by ruff for the code (only the ranges are different).
Given this, how do we detect the trailing comma during the semantic syntax error phase?
Should this rather be detected during parsing?
For example, the `ast` module does not give an AST at all for this:

```py
>>> import ast; ast.parse("max(i for i in range(3),)")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.11/ast.py", line 50, in parse
    return compile(source, filename, mode, flags,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<unknown>", line 1
    max(i for i in range(3),)
        ^^^^^^^^^^^^^^^^^^^
SyntaxError: Generator expression must be parenthesized
```

---

_Comment by @ntBre on 2025-05-05 18:44_

Oh you're exactly right, thanks! The fact that `ast.parse` fails means this is not a semantic syntax error and should instead be caught by our parser. I'll open a separate issue for that.

---

_Referenced in [astral-sh/ruff#17867](../../astral-sh/ruff/issues/17867.md) on 2025-05-05 18:48_

---

_Referenced in [astral-sh/ruff#17637](../../astral-sh/ruff/pulls/17637.md) on 2025-05-08 13:32_

---

_Referenced in [astral-sh/ruff#17988](../../astral-sh/ruff/pulls/17988.md) on 2025-05-09 17:07_

---

_Referenced in [astral-sh/ruff#18671](../../astral-sh/ruff/issues/18671.md) on 2025-06-14 15:09_

---

_Referenced in [astral-sh/ruff#19112](../../astral-sh/ruff/pulls/19112.md) on 2025-07-08 02:23_

---

_Referenced in [astral-sh/ty#823](../../astral-sh/ty/issues/823.md) on 2025-07-14 19:33_

---

_Referenced in [astral-sh/ty#1026](../../astral-sh/ty/issues/1026.md) on 2025-08-17 13:40_

---

_Referenced in [astral-sh/ruff#20051](../../astral-sh/ruff/pulls/20051.md) on 2025-08-22 19:40_

---

_Comment by @11happy on 2025-08-22 19:44_

Hii , I am new to the astral community , I have tried to implement syntax error for `yield from` in async functions. 
Looking forward to learn & contribute.

Thank you

---

_Referenced in [astral-sh/ruff#20166](../../astral-sh/ruff/pulls/20166.md) on 2025-08-30 12:23_

---

_Comment by @11happy on 2025-08-30 12:26_

I have also tried to implement `F406` (mport * only allowed at module leve) in PR linked above.
Thank you

---

_Referenced in [astral-sh/ruff#20243](../../astral-sh/ruff/pulls/20243.md) on 2025-09-04 16:36_

---

_Comment by @11happy on 2025-09-04 16:39_

Hii @ntBre , I have tried to implement  `F622` multiple starred expression (linked above) , can you please take a look whenever you get a chance.
Thank you

---

_Referenced in [astral-sh/ruff#20426](../../astral-sh/ruff/pulls/20426.md) on 2025-09-15 22:47_

---

_Comment by @11happy on 2025-09-15 22:52_

Hi @ntBre , I’ve implemented the new error case where a name is both a parameter and global. I think implementation is similar for the case where a name is both a parameter and nonlocal. Do you recommend adding that in the same PR, or should I open a separate one?
Whenever you get a chance, I’d appreciate your review. Thanks a lot for your time and guidance!

---

_Comment by @ntBre on 2025-09-15 23:17_

Thank you! Let's start with `global` and save `nonlocal` for a separate PR. Both of these are a bit trickier than they look initially because `global` handling in ty is quite different from Ruff, and Ruff doesn't have much of the `nonlocal` support that ty has, so we may not even want to tackle that one immediately.

---

_Comment by @11happy on 2025-09-15 23:20_

Sure thing : ) , I understand.

---

_Referenced in [astral-sh/ruff#20554](../../astral-sh/ruff/pulls/20554.md) on 2025-09-24 16:47_

---

_Comment by @11happy on 2025-09-24 16:48_

I have tried to implement F407 , have linked the PR above.

Thank you : )

---

_Comment by @11happy on 2025-09-24 16:55_

Hii @ntBre , I am also working on this break outside loop F701 , this would require some changes to the context, I have added a new `in_loop_context` , could you please take a loop & let me know if I am headed in right direction. https://github.com/astral-sh/ruff/pull/20556

Thank you :)

---

_Referenced in [astral-sh/ruff#20682](../../astral-sh/ruff/pulls/20682.md) on 2025-10-02 16:06_

---

_Comment by @11happy on 2025-10-02 16:07_

Hii @ntBre , I have tried to implement the semantic syntax error where alternative patterns bind different names (linked above) . Whenever you get a chance, I’d appreciate your review. 

Thank you : )

---

_Referenced in [astral-sh/ruff#20868](../../astral-sh/ruff/pulls/20868.md) on 2025-10-14 16:57_

---

_Comment by @11happy on 2025-10-14 17:12_

Hii @ntBre , now as we have `in_loop_context` method I have implemented(ported) `F702` as semantic syntax error linking PR (https://github.com/astral-sh/ruff/pull/20869) & have also worked on a new semantic syntax error linked above where annotated name cannot be global. Would appreciate your review .

Thank you : )

---

_Comment by @11happy on 2025-10-22 17:11_

Linking PR for PLE0117 https://github.com/astral-sh/ruff/pull/21032

Thank you : )

---

_Referenced in [astral-sh/ruff#21200](../../astral-sh/ruff/pulls/21200.md) on 2025-11-02 09:35_

---

_Comment by @11happy on 2025-11-07 06:11_

Linking the follow up PR for name is parameter and global for ty https://github.com/astral-sh/ruff/pull/21312

Thank  you : )

---

_Referenced in [astral-sh/ruff#21495](../../astral-sh/ruff/issues/21495.md) on 2025-11-17 07:13_

---

_Comment by @dhruvmanila on 2025-11-27 03:44_

This is another one that's easy enough to do:

```python
In [7]: class Foo2[T3 = int, T4]: ...
  Cell In[7], line 1
    class Foo2[T3 = int, T4]: ...
                         ^
SyntaxError: non-default type parameter 'T4' follows default type parameter
```

It's interesting that this isn't raised by the parser while a similar error involving function parameters is being raised by the parser:

```shell
❯ echo "class Foo[T1 = int, T2]: ..." | python -m ast -                                      
Module(
   body=[
      ClassDef(
         name='Foo',
         body=[
            Expr(
               value=Constant(value=Ellipsis))],
         type_params=[
            TypeVar(
               name='T1',
               default_value=Name(id='int', ctx=Load())),
            TypeVar(name='T2')])])

❯ echo "def foo(t1 = int, t2): ..." | python -m ast -
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 1865, in <module>
    main()
    ~~~~^^
  File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 1861, in main
    tree = parse(source, name, args.mode, type_comments=args.no_type_comments)
  File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 50, in parse
    return compile(source, filename, mode, flags,
                   _feature_version=feature_version, optimize=optimize)
  File "<stdin>", line 1
    def foo(t1 = int, t2): ...
                      ^^
SyntaxError: parameter without a default follows parameter with a default
```

---

_Referenced in [astral-sh/ty#1651](../../astral-sh/ty/issues/1651.md) on 2025-11-27 03:45_

---

_Referenced in [astral-sh/ruff#21635](../../astral-sh/ruff/pulls/21635.md) on 2025-11-27 03:46_

---

_Referenced in [astral-sh/ruff#21657](../../astral-sh/ruff/pulls/21657.md) on 2025-11-27 09:40_

---

_Comment by @11happy on 2025-11-27 09:41_

> This is another one that's easy enough to do:
> 
> In [7]: class Foo2[T3 = int, T4]: ...
>   Cell In[7], line 1
>     class Foo2[T3 = int, T4]: ...
>                          ^
> SyntaxError: non-default type parameter 'T4' follows default type parameter
> It's interesting that this isn't raised by the parser while a similar error involving function parameters is being raised by the parser:
> 
> ❯ echo "class Foo[T1 = int, T2]: ..." | python -m ast -                                      
> Module(
>    body=[
>       ClassDef(
>          name='Foo',
>          body=[
>             Expr(
>                value=Constant(value=Ellipsis))],
>          type_params=[
>             TypeVar(
>                name='T1',
>                default_value=Name(id='int', ctx=Load())),
>             TypeVar(name='T2')])])
> 
> ❯ echo "def foo(t1 = int, t2): ..." | python -m ast -
> Traceback (most recent call last):
>   File "<frozen runpy>", line 198, in _run_module_as_main
>   File "<frozen runpy>", line 88, in _run_code
>   File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 1865, in <module>
>     main()
>     ~~~~^^
>   File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 1861, in main
>     tree = parse(source, name, args.mode, type_comments=args.no_type_comments)
>   File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 50, in parse
>     return compile(source, filename, mode, flags,
>                    _feature_version=feature_version, optimize=optimize)
>   File "<stdin>", line 1
>     def foo(t1 = int, t2): ...
>                       ^^
> SyntaxError: parameter without a default follows parameter with a default

I have linked the PR.

Thank you : )

---

_Comment by @11happy on 2025-12-04 10:20_

> This is another one that's easy enough to do:
> 
> In [7]: class Foo2[T3 = int, T4]: ...
>   Cell In[7], line 1
>     class Foo2[T3 = int, T4]: ...
>                          ^
> SyntaxError: non-default type parameter 'T4' follows default type parameter
> It's interesting that this isn't raised by the parser while a similar error involving function parameters is being raised by the parser:
> 
> ❯ echo "class Foo[T1 = int, T2]: ..." | python -m ast -                                      
> Module(
>    body=[
>       ClassDef(
>          name='Foo',
>          body=[
>             Expr(
>                value=Constant(value=Ellipsis))],
>          type_params=[
>             TypeVar(
>                name='T1',
>                default_value=Name(id='int', ctx=Load())),
>             TypeVar(name='T2')])])
> 
> ❯ echo "def foo(t1 = int, t2): ..." | python -m ast -
> Traceback (most recent call last):
>   File "<frozen runpy>", line 198, in _run_module_as_main
>   File "<frozen runpy>", line 88, in _run_code
>   File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 1865, in <module>
>     main()
>     ~~~~^^
>   File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 1861, in main
>     tree = parse(source, name, args.mode, type_comments=args.no_type_comments)
>   File "/Users/dhruv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/lib/python3.13/ast.py", line 50, in parse
>     return compile(source, filename, mode, flags,
>                    _feature_version=feature_version, optimize=optimize)
>   File "<stdin>", line 1
>     def foo(t1 = int, t2): ...
>                       ^^
> SyntaxError: parameter without a default follows parameter with a default

I have linked the PR for ty one as well.
Thank you : )


---
