```yaml
number: 14686
title: "Formatter: Raw strings containing double quotes"
type: issue
state: closed
author: ferdnyc
labels:
  - formatter
  - style
assignees: []
created_at: 2024-11-29T23:37:00Z
updated_at: 2024-12-02T08:51:55Z
url: https://github.com/astral-sh/ruff/issues/14686
synced_at: 2026-01-12T15:54:54Z
```

# Formatter: Raw strings containing double quotes

---

_@ferdnyc_

I'm not sure how Black handles this case, but Ruff wants to force this:

```python
r'\"'
```

to instead be this:

```python
r"\""
```

...Which just feels like a completely unnecessary drop in clarity.

Tested/confirmed with the following code, run through  Ruff 0.6.7 via `ruff format --diff`:

```python
#!/usr/bin/env python3

input = '"Some string"'

output = input.translate({ord('"'): r'\"'})
print(f"{output=}")
```

Result:
```diff
$ ruff format --diff ./ruff_test.py 
--- ruff_test.py
+++ ruff_test.py
@@ -2,6 +2,5 @@
 
 input = '"Some string"'
 
-output = input.translate({ord('"'): r'\"'})
+output = input.translate({ord('"'): r"\""})
 print(f"{output=}")
-

1 file would be reformatted
```


---

_Label `formatter` added by @AlexWaygood on 2024-11-30 00:55_

---

_Label `style` added by @MichaReiser on 2024-11-30 10:34_

---

_Comment by @MichaReiser on 2024-11-30 10:43_

> I'm not sure how Black handles this case, but Ruff wants to force this:

I tried it in Black's playground and Ruff's behavior matches Black's. 

Pasting the different strings into the REPL produces the following output

```
>>> print(r"\"")
\"
>>> print(r'\"')
\"
>>> print(r'"')
"
```

What's important to note is that the `\` is necessary to escape the `"`, but it also changes the string's value (the backslash is preserved, unlike in regular strings). Ruff can't remove the `\` because of that, even when the string uses single quotes. 

Based on this, I think Ruff's/Black's quote choice is consistent: They use the preferred quote style (most commonly `"` unless configured otherwise) except when using the preferred quote style requires more escapes. In this case, the `\` isn't just an escape, it's semantically important. Because of that, using `'` or `"`requires the exact same amount of quotes, in which case both formatters use the preferred quote style.



---

_Comment by @ferdnyc on 2024-12-01 18:07_

Thanks for looking at this, @MichaReiser.

> What's important to note is that the `\` is necessary to escape the `"`, but it also changes the string's value (the backslash is preserved, unlike in regular strings). Ruff can't remove the `\` because of that, even when the string uses single quotes.

Correct. Actually, if we're being technical the `\` **doesn't** escape the `"`, because there are no escapes in raw strings. While in a non-raw string, `\"` is an escaped double quote, in a raw string it's simply a backslash followed by a double quote. The non-raw equivalent of that string is `'\\"'` or `"\\\""`.

> Based on this, I think Ruff's/Black's quote choice is consistent:

I don't entirely _disagree_, but it's consistent in the context of what I would argue are an incomplete set of criteria.

> They use the preferred quote style (most commonly `"` unless configured otherwise) except when using the preferred quote style requires more escapes.

See, the way I look at it, the reason to use single quotes around a string containing double quotes **isn't** primarily to avoid extra escapes... 

> In this case, the `\` isn't just an escape,

Not an escape at all.

> it's semantically important. Because of that, using `'` or `"`requires the exact same amount of quotes, in which case both formatters use the preferred quote style.

The main reason to use different quote styles around a string containing quotes, to me, is because it makes it easier to differentiate the outer quotes from the internal ones. It's the same reason we switch from double to single quotes in English, "When we're embedding 'quotations inside quotations'."

Which is why I think Ruff (and probably Black) is also wrong to want to turn this:

```python
print('"He\'s in \'ere."')
```
into this:
```python
print("\"He's in 'ere.\"")
```

Exact same **number** of escapes either way, but having escaped double quotes immediately adjacent to outer double quotes is much less easily parseable (for humans) than having escaped single quotes deep inside the string, but unescaped double quotes directly adjacent to unescaped single quotes at the outer extremes.


---

_Comment by @ferdnyc on 2024-12-01 18:37_

> > In this case, the `\` isn't just an escape,
> 
> Not an escape at all.

Actually, I take that back. It _is_ an escape, but only because the raw string rules make a special exception for escaped quotes. From the STRINGS help:

> Even in a raw literal, quotes can be escaped with a backslash, but the backslash remains in the result; for example, `r"\""` is a valid string literal consisting of two characters: a backslash and a double quote; `r"\"` is not a valid string literal (even a raw string cannot end in an odd number of backslashes).  Specifically, *a raw literal cannot end in a single backslash* (since the backslash would escape the following quote character).

The fact that, unlike in non-raw strings, `r'\"'` and `r"\""` happen to be equivalent, is something they don't address.

---

_Comment by @MichaReiser on 2024-12-02 07:48_

Thanks for the detailed explanation. 

> The main reason to use different quote styles around a string containing quotes, to me, is because it makes it easier to differentiate the outer quotes from the internal ones. It's the same reason we switch from double to single quotes in English, "When we're embedding 'quotations inside quotations'."

I can see the reasoning behind this. I worry that it's too complicated for an auto-formatter. 

* It's important for auto-formatter that the result isn't surprising for users. Users should understand the heuristics. Too clever heuristics and users won't understand why the formatter formats their code in a specific way, which can be frustrating
* Implementing your proposal would require coming up with a heuristic to a) match quotes: There's no formal grammar in strings that require that quotes are balanced and b) decide what are outer and what are inner quotes. 
* The style applied by Ruff/Black aligns with auto formatter in other ecosystems (e.g. Prettier). Consistent cross-ecosystem behavior has the advantage that the formatter output is less surprising to users and matches the code that most users read, making it easier for them to read the code. 

That's why I'd like to keep the formatting as is for now.

---

_Closed by @MichaReiser on 2024-12-02 07:48_

---
