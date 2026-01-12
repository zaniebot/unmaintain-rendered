```yaml
number: 19792
title: "`UP032` fix fails when braces are created via escapes"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-08-06T20:21:10Z
updated_at: 2025-08-11T20:04:41Z
url: https://github.com/astral-sh/ruff/issues/19792
synced_at: 2026-01-12T15:54:57Z
```

# `UP032` fix fails when braces are created via escapes

---

_@MeGaGiGaGon_

### Summary

The fix for [f-string (UP032)](https://docs.astral.sh/ruff/rules/f-string/#f-string-up032) will fail if the braces (`{` and `}`) are created via escapes instead of literal characters: [Example](https://play.ruff.rs/2a2c4778-a9ff-4499-b9cf-b89c38a25d4b)
```powershell
PS ~>echo '"\x7btest\x7d".format(test=1)' | uvx ruff check --select UP032 --fix -
"\x7btest\x7d"
Found 1 error (1 fixed, 0 remaining).
```
This does work properly with `.format` and could be fixed the same way as the non-cursed version:
```pycon
>>> "\x7btest\x7d".format(test=1)
'1'
>>> f"{1}"
'1'
```
This also applies to all the other ways of writing `{` and `}` non-literally (`\x`, `\u`, `\U`, `\N`)

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Comment by @MeGaGiGaGon on 2025-08-06 20:33_

This same thing also applies to [printf-string-formatting (UP031)](https://docs.astral.sh/ruff/rules/printf-string-formatting/#printf-string-formatting-up031) example [playground](https://play.ruff.rs/d6f0f412-a887-482b-a337-3a52a10472a1)
```powershell
PS ~>echo '"\x25s" % 1' | uvx ruff check --select UP031 --fix --unsafe-fixes -
"\x25s".format(1)
Found 1 error (1 fixed, 0 remaining).
```

---

_Label `bug` added by @ntBre on 2025-08-07 11:54_

---

_Label `fixes` added by @ntBre on 2025-08-07 11:54_

---

_Comment by @IDrokin117 on 2025-08-08 12:27_

I am going to work on this

---

_Assigned to @IDrokin117 by @ntBre on 2025-08-08 16:22_

---

_Comment by @IDrokin117 on 2025-08-09 19:45_

# Investigation results
## Context

It looks like it's tough to deal with Unicode escape sequences in both cases for now. But before short explanation how it works:
1. The string parsed into `FormatPart` by `FormatString` [here](https://github.com/astral-sh/ruff/blob/0.12.7/crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs#L273). For example:
```
"text {}".format(1) # -> [Literal("test "), Field { field_name: "", conversion_spec: None, format_spec: "" }]
```
where `Field` is a place for `{}` to be replaced.
2. Next maps positional and keyword arguments to their values (e.g. `{}: 1`) and substitutes them.

The problem in the issue occurred due to 1) iterated through [raw contents](https://github.com/astral-sh/ruff/blob/0.12.7/crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs#L246)(not parsed) 2) unicode sequences treated as literals
```
print("\x7btest\x7d {test}".format(test=1)) # -> [Literal("\\x7btest\\x7d "), Field { field_name: "test", conversion_spec: None, format_spec: "" }]
```

## Question
Are Unicode escape sequences should be decoded and replaced? I mean
```
"\x7btest\x7d".format(test=1)
```
to
```
"{1}" # (1)
```
Or we can try to keep it
```
"\x7b1\x7d" # (2)
```
I think the second one is correct, because that was the intention.

## First case
Back to the first (1) case. It is possible to solve the problem by iterating through parsed content instead of raw (e.g. parse whole expression `"\x7btest\x7d {test}"` -> `"{test} {test}"`). It is already implemented func [`parse_expression`](https://github.com/astral-sh/ruff/blob/8230b79829db0148afeefa634f3dde00b3a52410/crates/ruff_python_parser/src/lib.rs#L139). But it means other escape sequences will parse as well (e.g. `"\N{snowman} \x7btest\x7d {test}"` -> "☃ {test} {test}"). In my view, the rule shouldn't substitute Unicode sequences with literal characters in the source code.

## Second case
The second option (2) requires handling Unicode sequences in `FormatString`. This will force a fully/partial rework of `FormatString`. We need to at least parse or check Unicode sequence brackets and retain them for substitution. 

# Conclusion
To resolve this issue, we need to identify Unicode sequence brackets when parsing source content into FormatPart objects and classify them as Field rather than Literal. The possible solutions require a decision, so I'm asking more experienced contributors for help. There may already be a sophisticated solution in the repository.


---

_Comment by @MeGaGiGaGon on 2025-08-09 21:10_

Sadly keeping the escapes as escapes for `.format` -> f-string won't work, since f-strings only work with literal `{}`s:
```pycon
>>> "\x7btest\x7d".format(test=1)
'1'
>>> f"{1}" # works
'1'
>>> f"\x7b1\x7d" # doesnt work
'{1}'
```

---

_Comment by @IDrokin117 on 2025-08-09 21:24_

Should we make it unsafe fix in such case?

---

_Comment by @ntBre on 2025-08-11 19:56_

Thank you for looking into this!

I actually misinterpreted the initial report. I thought the rule knew that the escapes were braces and was still trying to perform the substitution, but the `test` in the input string is actually irrelevant. UP032 still flags this [case](https://play.ruff.rs/2fb58f99-8885-4a19-b83a-81012d750265):

```py
"\x7b\x7d".format(test=1)
```

and just drops the `format` call. I think that would be another way around this: don't offer a fix if there's a mismatch between the number of placeholders and arguments to `format`. That kind of seems like a bug in its own right anyway.

It would be cool to replace the escapes with real braces, but I think it would be totally fine just to avoid offering an incorrect fix. I expect this kind of thing to be rare anyway.

---

_Comment by @ntBre on 2025-08-11 20:04_

Even more directly, UP032 flags this case: `"".format(test=1)`, definitely showing that it's not interpreting the escapes as I thought.

Hmm, another interesting data point is that we do flag F524 in the [playground](https://play.ruff.rs/07806812-ce88-4760-a13f-151a78bbee83) case I linked above. So we may be able to look at its implementation if we do want to replace the braces. I don't immediately see the difference between the two because they both use `FormatString::from_str`, but I guess there's something different in there.

---
