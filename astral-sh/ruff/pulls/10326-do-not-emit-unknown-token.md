```yaml
number: 10326
title: "Do not emit `Unknown` token"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - parser
assignees: []
base: dhruv/parser
head: dhruv/token-source
created_at: 2024-03-11T06:18:11Z
updated_at: 2024-03-11T09:31:41Z
url: https://github.com/astral-sh/ruff/pull/10326
synced_at: 2026-01-10T22:47:01Z
```

# Do not emit `Unknown` token

---

_Pull request opened by @dhruvmanila on 2024-03-11 06:18_

## Summary

This PR updates the `TokenSource` to not emit the `Unknown` token if there's a lexical error.

To understand the reasoning behind this, we'll use the following example:

```python
f'hello {x!

z = 1
```

Here, the lexer will emit two errors at the _same_ location as show in the following output:

```
...
13 Name {
    name: "z",
} 14
15 Equal 16
17 Int {
    value: 1,
} 18
18 NonLogicalNewline 19
Error: 19:19 unexpected EOF while parsing
Error: 19:19 f-string: unterminated string
19 Newline 19
```

1. The parser is trying to parse a list of module statements
2. Starts parsing f-string
3. The parser is trying to parse a list of f-string elements
4. It encounters `z` after `!` which is an invalid conversion flag.
5. Exit the list parsing logic as the recovery can be handled by the outer context (module statement)
6. ... _some more error handling_
7. Current token is `Unknown` because of the lexical error (unexpected EOF)
8. F-string parsing logic skips this
9. Current token is `Unknown` again because of the "unterminated string" error
10. Both the previous and current token are the same and at the same location which means that the parser didn't progress. Panic!

The parser will then emit 2 `Unknown` tokens in total for the errors. This creates a problem in the logic to make sure that the parser is progressing because the tokens are at the same location which will seem like the parser is stuck. The program then panics.

This token is only used as a placeholder when there's a lexical error. This means that the parser need to skip this token when `next_token` is called or it needs to be handled by the parsing logic whichever matches against the current token kind. The latter was done by the f-string parsing logic. But this seems redundant as we can just avoid emitting it which is what this PR does.


---

_Label `parser` added by @dhruvmanila on 2024-03-11 06:18_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-11 06:18_

---

_Comment by @dhruvmanila on 2024-03-11 09:31_

(There was an internal discussion for keeping this the way it is for now.)

---

_Closed by @dhruvmanila on 2024-03-11 09:31_

---
