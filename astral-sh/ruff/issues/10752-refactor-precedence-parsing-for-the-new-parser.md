```yaml
number: 10752
title: Refactor precedence parsing for the new parser
type: issue
state: closed
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
created_at: 2024-04-03T09:15:53Z
updated_at: 2024-04-23T04:55:04Z
url: https://github.com/astral-sh/ruff/issues/10752
synced_at: 2026-01-10T11:09:53Z
```

# Refactor precedence parsing for the new parser

---

_Issue opened by @dhruvmanila on 2024-04-03 09:15_

As suggested:
- [x] Compare expressions: https://github.com/astral-sh/ruff/pull/10730#discussion_r1547594779
- [x] Unary expressions: https://github.com/astral-sh/ruff/pull/10726#discussion_r154789344(https://github.com/astral-sh/ruff/pull/10726#discussion_r1547893446)
- [x] Boolean expressions: https://github.com/astral-sh/ruff/pull/10727#discussion_r1547623778
- [x] Binary expressions: https://github.com/astral-sh/ruff/pull/10731#discussion_r1547851504


---

_Label `internal` added by @dhruvmanila on 2024-04-03 09:15_

---

_Label `parser` added by @dhruvmanila on 2024-04-03 09:15_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-03 10:02_

---

_Comment by @dhruvmanila on 2024-04-21 10:27_

I'll probably scratch the PRs as I've a better idea. Something like
```rs
enum OperatorType {
    Boolean(BoolOp),
    Comparison(CmpOp),
    Binary(Operator),
}

impl OperatorType {
    fn try_from_tokens(tokens: [TokenKind; 2]) -> Option<OperatorType> {}
    fn precedence(&self) -> OperatorPrecedence {}
}
```

---

_Closed by @dhruvmanila on 2024-04-23 04:55_

---
