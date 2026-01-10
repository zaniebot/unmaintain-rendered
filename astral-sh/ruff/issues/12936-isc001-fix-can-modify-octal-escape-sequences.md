```yaml
number: 12936
title: ISC001 fix can modify octal escape sequences
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-08-16T15:44:47Z
updated_at: 2024-08-27T17:53:28Z
url: https://github.com/astral-sh/ruff/issues/12936
synced_at: 2026-01-10T11:09:54Z
```

# ISC001 fix can modify octal escape sequences

---

_Issue opened by @dscorbett on 2024-08-16 15:44_

The fix for ISC001 changes behavior when the first string ends with a one- or two-digit octal escape sequence and the second begins with an octal digit.

```console
$ ruff --version
ruff 0.6.0
$ cat isc001.py
print("\12""0")
$ python isc001.py

0
$ ruff check --isolated --select ISC001 isc001.py --fix
Found 1 error (1 fixed, 0 remaining).
$ python isc001.py
P
```

The least disruptive fix would be to detect this specific situation and pad the escape sequence to three digits. In the above example, that would result in `"\0120"`.

---

_Comment by @AlexWaygood on 2024-08-17 15:01_

The reason for this seems to be that CPython seems to process the octal escape _before_ it concatenates the two strings when it parses the source to create the AST:

```pycon
>>> import ast
>>> print(ast.dump(ast.parse(r'"\12" "0"')))
Module(body=[Expr(value=Constant(value='\n0'))], type_ignores=[])
>>> print(ast.dump(ast.parse(r'"\120"')))
Module(body=[Expr(value=Constant(value='P'))], type_ignores=[])
```

Our AST [also understands this](https://play.ruff.rs/e13f740a-be5f-443c-8ac4-d48c8142f47c), but it seems like the fix for this rule doesn't.

---

_Label `bug` added by @AlexWaygood on 2024-08-17 15:02_

---

_Label `fixes` added by @AlexWaygood on 2024-08-17 15:02_

---

_Label `help wanted` added by @AlexWaygood on 2024-08-17 15:02_

---

_Comment by @dhruvmanila on 2024-08-20 15:11_

Related https://github.com/astral-sh/ruff/issues/12753

---

_Comment by @dylwil3 on 2024-08-22 13:27_

> Related #12753

As you suggest there, a quick fix would be to move this rule and `UP012` to an ast-based check instead of a token-based one. However, since the parser evaluates some escape sequences, this would result in replacing 

```
"\12" "0"
```
with
```
"\n0"
```
for example.

If we instead want to preserve the escape sequence and suggest the fix `"\0120"`, then we may have to re-implement 'half' of the parsing logic inside this rule (which is fine).

Is there a preference?

---

_Comment by @MichaReiser on 2024-08-22 13:30_

I'm slightly leaning towards omitting the fix if there's an octal escape (or other escapes). Or is detecting the octal escape equally hard to emitting the right fix?

---

_Comment by @dylwil3 on 2024-08-23 02:46_

My guess is that all three options

1. detecting octals at the end of the first string and skipping the fix
2. having the fix be ast-based (and therefore evaluating the octals prior to concatenating)
3. detecting octals at the end of the first string and normalizing the octal before concatenating

 are about the same level of difficulty - none seem too difficult - with option (3) being a tinge fussier than (1) and (2).

---

_Comment by @dhruvmanila on 2024-08-23 04:45_

At least for https://github.com/astral-sh/ruff/issues/12753, I think we should avoid omitting the fix if there's an escape sequence.

I remember talking with @MichaReiser about adding a token flag for strings which suggests that this string / f-string token contains an escape sequence. This flag would be set by the lexer as it's already looking at each character. But, I think it might be a bit difficult because the lexer would directly skip to the ending quote character using `memchr`

https://github.com/astral-sh/ruff/blob/028cb68cd1131089b620a31ed25a0ab5e1c400b6/crates/ruff_python_parser/src/lexer.rs#L945-L946

Another option would be to set the flag only on the AST node which should be easier to do because of the `StringParser`:

https://github.com/astral-sh/ruff/blob/028cb68cd1131089b620a31ed25a0ab5e1c400b6/crates/ruff_python_ast/src/nodes.rs#L1832-L1834

But, I would prefer if the flag could be added to the `TokenFlags` instead.

Another solution might be to move the rule to use the AST where we can access the concatenated value which would make it easier to detect the escape sequence. Once we detect the escape sequence, we can avoid emitting a fix for that string expression.

https://github.com/astral-sh/ruff/blob/028cb68cd1131089b620a31ed25a0ab5e1c400b6/crates/ruff_python_ast/src/nodes.rs#L1766-L1769

Curious to hear @MichaReiser's opinion.

---

_Comment by @MichaReiser on 2024-08-24 12:38_

I'm leaning towards a local fix in ISC001. It seems simple enough to look at the individual literal elements and search backward. Flagging whether the string contains any escape seems overly aggressive and would remove the fix even when the escape isn't at the end of the string. Migrating to AST rules seems fine to me, but only if we are okay with the fix replacing other escapes (which I think it should not). 

We only need to be careful about that `\\01` is not an octal escape but `\\\01` is. 

---

_Comment by @dhruvmanila on 2024-08-26 04:57_

Yeah, I think implementing a local fix seems reasonable to me. What do you think? @dylwil3 

---

_Comment by @dylwil3 on 2024-08-26 05:08_

Sounds good, I'll give it a shot!

---

_Closed by @AlexWaygood on 2024-08-27 17:53_

---
