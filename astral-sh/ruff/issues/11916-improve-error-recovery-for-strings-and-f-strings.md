```yaml
number: 11916
title: Improve error recovery for strings and f-strings
type: issue
state: closed
author: dhruvmanila
labels:
  - parser
assignees: []
created_at: 2024-06-18T04:50:46Z
updated_at: 2025-10-21T06:57:30Z
url: https://github.com/astral-sh/ruff/issues/11916
synced_at: 2026-01-10T11:09:54Z
```

# Improve error recovery for strings and f-strings

---

_Issue opened by @dhruvmanila on 2024-06-18 04:50_

Currently, the parser drops all unterminated strings or more precisely the lexer doesn't emit the string token for unterminated strings. This is for both normal and formatted string literals.

This can be solved by always emitting the string / f-string tokens even if it's unterminated. We would need to introduce an `Unterminated` token flag which will signal the parser about the same.

For example:
```py
"hello world

def foo():
    pass
```

Even though the parser recovers from the above unterminated string, the AST doesn't include the string itself. Here, the new tokens would be:
```
TokenKind::String (with TokenFlags::Unterminated)
TokenKind::Newline
TokenKind::NonLogicalNewline
TokenKind::Def
...
```
This will help the parser create the correct AST. We'd also need to move the error handling from the lexer to the parser.

Note that this will only work for single-quoted strings as triple-quoted strings will just consume the remaining source code completely if there's no closing quote.

### F-strings

There's more to f-strings than a normal string literal because it can contain an expression element. This means that there could be unterminated f-string or the closing curly brace could be missing or an error in the expression itself.

For example:
```py
f"hello {foo
# or, f"hello {foo}
# or, f"hello {foo} world
# or, f"hello

def bar():
    pass
```
Here, there's a `NonLogicalNewline` token after the "foo" variable because of an unclosed `{`. This would require a modified re-lexing logic to (1) recover from an unclosed `{` and (2) recover from an unterminated f-string.

This also requires moving the error handling from the lexer to the parser.

But, here's another problem: the parser doesn't use existing list parsing logic for parsing comma-separated expressions aka tuple expression which means it doesn't benefit from any error recovery logic. Thus, if there's an error in the f-string expression itself, there's a high change it'll not recovery correctly.
```py
f'hello {foo bar

def func():
    pass
```
For the above code snippet, should "foo bar" be parsed as a tuple expression with a missing comma (?) because currently the "bar" would be parsed as a name node in the global scope.

### `Unknown` token

If we were to move the error handling for strings to the parser, it would mean that there won't be any `Unknown` token. This means that the change cannot be made without support for https://github.com/astral-sh/ruff/issues/11915. The main reason is that these rules rely on the fact that there'll be an `Unknown` token in the token stream where there's a lexial error. Maybe there's a way around that but I'm not exactly sure now.

---

_Label `parser` added by @dhruvmanila on 2024-06-18 04:50_

---

_Comment by @dhruvmanila on 2024-06-19 04:36_

After #11915, if we do start emitting the tokens for unterminated strings, we should avoid running any token-based rules on them. This could be done by checking whether the `Unterminated` token flag is set or not.

---

_Comment by @MichaReiser on 2025-10-21 06:57_

I think we can close this now that https://github.com/astral-sh/ruff/pull/20848 landed

---

_Closed by @MichaReiser on 2025-10-21 06:57_

---
