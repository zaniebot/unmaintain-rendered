```yaml
number: 12854
title: Indent multi-line expressions in function arguments
type: issue
state: closed
author: edaqa-uncountable
labels:
  - formatter
  - needs-decision
  - style
assignees: []
created_at: 2024-08-13T08:48:10Z
updated_at: 2024-08-13T10:58:36Z
url: https://github.com/astral-sh/ruff/issues/12854
synced_at: 2026-01-12T15:54:52Z
```

# Indent multi-line expressions in function arguments

---

_@edaqa-uncountable_

Would it be possible to add an option to allow indentation of multi-line expressions in function arguments?  Currently it aligns everything to the left with the other arguments.

For example, this is the formatting I get:

```
some_func(
   param_one=value,
   param_two="hi"
   if some_cond
   else "bye",
   param_three=1
   if some_cond
   else 2,
)
```

That alignment is hard to read. I want something like this instead:

```
some_func(
   param_one=value,
   param_two="hi"
       if some_cond
       else "bye",
   param_three=1
       if some_cond
       else 2,
)
```

I've shortened the lines to focus on the alignment (though I realize this short they might end up on one-line)

I admit I don't know what the ideal formatting here is, but the first one is definitely not it.  Perhaps it should always rewrite as a paren expression, which has nicer results.

```
some_func(
   param_one=value,
   param_two=(
       "hi"
       if some_cond
       else "bye"
   ),
   param_three=(
       1
       if some_cond
       else 2
   )
)
```

Keywords: indent function argument parameter

---

_Label `formatter` added by @dhruvmanila on 2024-08-13 08:56_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-13 08:56_

---

_Label `style` added by @dhruvmanila on 2024-08-13 08:56_

---

_Comment by @MichaReiser on 2024-08-13 10:58_

I agree, that Ruff's current nested expression formatting isn't ideal (for example, the formatting of the `if else` expression) and I explained my thinking around it in https://github.com/psf/black/issues/4123 (and why we deviate from black).


We plan to revisit sub-expression formatting. I just created https://github.com/astral-sh/ruff/issues/12856 to make this more visible. 

Therefore, yes we want to improve nested if expression formatting although I'm not sure if it will use the exact style that you're proposing. 

---

_Closed by @MichaReiser on 2024-08-13 10:58_

---
