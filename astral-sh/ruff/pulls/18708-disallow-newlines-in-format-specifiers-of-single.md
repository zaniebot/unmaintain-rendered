```yaml
number: 18708
title: Disallow newlines in format specifiers of single quoted f- or t-strings
type: pull_request
state: merged
author: MichaReiser
labels:
  - parser
assignees: []
merged: true
base: main
head: micha/f-string-format-spec-parsing
created_at: 2025-06-16T13:59:11Z
updated_at: 2025-06-18T12:56:18Z
url: https://github.com/astral-sh/ruff/pull/18708
synced_at: 2026-01-10T18:39:08Z
```

# Disallow newlines in format specifiers of single quoted f- or t-strings

---

_Pull request opened by @MichaReiser on 2025-06-16 13:59_

## Summary

This PR disallows newlines in format specifiers of single-quoted f- and t-strings. This aligns ruff's behavior with CPython's upstream behavior. 

I think we should wait with merging this PR until after the next release so that the formatter can fix-up invalid format specifiers. 

Part of https://github.com/astral-sh/ruff/issues/18632

Fixes https://github.com/astral-sh/ruff/issues/18730


---

_Label `parser` added by @MichaReiser on 2025-06-16 13:59_

---

_Comment by @github-actions[bot] on 2025-06-16 14:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @MichaReiser on 2025-06-16 14:12_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-06-16 14:12_

---

_Renamed from "Disallow newlines in format specifiers of sinlge quoted f- or t-strings" to "Disallow newlines in format specifiers of single quoted f- or t-strings" by @AlexWaygood on 2025-06-16 14:41_

---

_@dylwil3 reviewed on 2025-06-16 14:48_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/error.rs`:456 on 2025-06-16 14:48_

To match CPython, should we make this an error for interpolated strings that just says:

```
newlines are not allowed in format specifiers
```

When we then use `LexicalErrorType::FString(...)` etc. it will prepend `f-string:` and `t-string:` appropriately.

source: https://github.com/python/cpython/blob/f0799795994bfd9ab0740c4d70ac54270991ba47/Parser/lexer/lexer.c#L1424

---

_@MichaReiser reviewed on 2025-06-16 15:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:456 on 2025-06-16 15:00_

Hmm, I didn't notice the `f-string` prefix that CPYthon adds. Funnily enough, CPython contains the string type twice (which is the reason why I changed the implementation to use a `LexicalErrorType`

```
SyntaxError: f-string: newlines are not allowed in format specifiers for single quoted f-strings`
```

---

_@dylwil3 reviewed on 2025-06-16 15:06_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/error.rs`:456 on 2025-06-16 15:06_

I'm submitting a patch to CPython for this actually... I think they forgot to account for t-strings here. The other syntax errors in the lexer do something like this:

```c
                _PyTokenizer_syntaxerror(tok,
                                    "unterminated triple-quoted %c-string literal"
                                    " (detected at line %d)",
                                    TOK_GET_STRING_PREFIX(tok), start);
```


---

_@dylwil3 reviewed on 2025-06-16 15:08_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/error.rs`:456 on 2025-06-16 15:08_

But yeah I'm not sure how we would implement this in our setup since, as you point out, they use the interpolated string type also in the body of the message 😦 

---

_@MichaReiser reviewed on 2025-06-16 16:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:456 on 2025-06-16 16:10_

> string type also in the body of the message

I really don't like this 😅  I'll think about how I can word the message and otherwise leave it as is (because I find repeating worse than not having the `f-string:` prefix which I find slightly weird anwyay because of the double colon)

---

_@dylwil3 approved on 2025-06-16 17:06_

---

_Label `do-not-merge` added by @MichaReiser on 2025-06-16 17:11_

---

_Review requested from @carljm by @MichaReiser on 2025-06-17 05:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-17 05:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-17 05:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-17 05:39_

---

_Review request for @dcreager removed by @AlexWaygood on 2025-06-17 06:43_

---

_Review request for @carljm removed by @AlexWaygood on 2025-06-17 06:43_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-06-17 06:43_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-17 06:43_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/error.rs`:90 on 2025-06-17 12:27_

optional nit:

```suggestion
                    "newlines are not allowed in format specifiers when using single quotes"
```

---

_@dylwil3 approved on 2025-06-17 12:29_

Looks good to me!

---

_@dhruvmanila reviewed on 2025-06-17 16:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:90 on 2025-06-17 16:10_

I think this might be a bit misleading since it's not the entire format spec where it isn't allowed, it's mainly the literal part of the format spec where it isn't allowed. So, the following is valid

```py
# Newline is inside an expression element of the format spec
f"hello {x:.{y
             }f}"
```

And, running it on Python 3.13.5:
```console
$ uv run --python 3.13 --no-project python -m ast parser/_.py
Module(
   body=[
      Expr(
         value=JoinedStr(
            values=[
               Constant(value='hello '),
               FormattedValue(
                  value=Name(id='x', ctx=Load()),
                  conversion=-1,
                  format_spec=JoinedStr(
                     values=[
                        Constant(value='.'),
                        FormattedValue(
                           value=Name(id='y', ctx=Load()),
                           conversion=-1),
                        Constant(value='f')]))]))])
```

While, the following is invalid:

```py
# Newline is inside the literal element of the format spec
f"hello {x:.{y}f
         }"
```

Running it on Python 3.13.5:

```console
$ uv run --python 3.13 --no-project python -m ast parser/_.py       
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
  File "parser/_.py", line 1
    f"hello {x:.{y}f
    ^
SyntaxError: unterminated f-string literal (detected at line 1)
```

---

_@dhruvmanila reviewed on 2025-06-17 16:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:90 on 2025-06-17 16:14_

I think this is actually just an unterminated f-string literal error and it's not really that the newline isn't allowed.

I don't think we would raise a similar (newline specific) error for something like:
```py
f"hello
world"
```
which is the same situation except it's the outer literal element.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/fixtures.rs`:329 on 2025-06-17 16:19_

```suggestion
                "Formatting `{input_path}` was expected to succeed but it failed: {err}",
```

---

_@dhruvmanila approved on 2025-06-17 16:20_

Looks good!

I'd probably keep the new error as `UnterminatedString`, refer to my inline comment.

---

_@MichaReiser reviewed on 2025-06-17 16:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:90 on 2025-06-17 16:20_

I'm not sure I follow. The error message is the same as CPython uses. We could clarify it to `format spec literal` but I'm not sure if that's not more confusing for users (I doubt many know that they even can use interpolations in format specs)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:90 on 2025-06-17 16:22_

Oh, I didn't realize that CPython raised a similar error message.

Feel free to ignore my message, it seems I shouldn't be doing late night reviews!

---

_@dhruvmanila reviewed on 2025-06-17 16:22_

---

_@MichaReiser reviewed on 2025-06-18 12:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:90 on 2025-06-18 12:28_

But your comment is 100% correct and matches the implementation. I'm just not sure if using the more precise language here helps users or would be more confusing.

---

_Merged by @MichaReiser on 2025-06-18 12:56_

---

_Closed by @MichaReiser on 2025-06-18 12:56_

---

_Branch deleted on 2025-06-18 12:56_

---

_Label `do-not-merge` removed by @MichaReiser on 2025-06-18 12:56_

---
