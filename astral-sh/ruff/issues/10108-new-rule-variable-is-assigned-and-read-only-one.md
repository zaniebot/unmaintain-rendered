```yaml
number: 10108
title: "new rule: variable is assigned and read only one time"
type: issue
state: open
author: Tatsh
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-02-24T05:49:17Z
updated_at: 2025-01-29T18:15:11Z
url: https://github.com/astral-sh/ruff/issues/10108
synced_at: 2026-01-12T15:54:49Z
```

# new rule: variable is assigned and read only one time

---

_@Tatsh_

Example:

```python
a = heavy_func() # error: Variable is assigned and read only one time
x(a)
```

Fixer would replace `x(a)` with `x(heavy_func())`

This would not trigger:

```python
a = heavy_func()
x(a)
y(a)
```

---

_Label `rule` added by @MichaReiser on 2024-02-24 13:26_

---

_Comment by @MichaReiser on 2024-02-24 13:30_

Hy @Tatsh 

Would you mind explaining the motivation for the new rule or why you think using a variable only once is "bad"? 

I suspect you prefer inlining simple expressions over having many short-lived variables. This is reasonable to me. I worry that such a lint rule is too aggressive and motivates people to inline even complex expressions that lead to less readable code overall or that the lines become very long, exceeding the configured line width. To avoid this, the rule would need to be able to distinguish between simple and complex expressions, which is in itself a hard problem and it's very likely that each of us has a different understanding of what makes a complex expression. 



---

_Comment by @Tatsh on 2024-02-24 17:17_

It's definitely a personal preference. This is more complex than I thought if we want to figure out the level of complexity. 

One approach would be to limit this to assignment and then used on the very next line (not counting comments).

Another approach would be to limit the rule to numbers, strings and function calls with no arguments. Explanation: export the constant or rename the function to something more clear.

Also, I would not want this rule to flag f-strings because prior to 3.12 there are quoting rules that are kind of painful.

---

_Comment by @MichaReiser on 2024-02-24 17:34_

Thanks for the additional explanation. That helps a lot. 

At this point, I fear that the rule is very specific and probably difficult to explain (in which case, users will be confused about why or why not a certain usage is flagged). I'll leave the issue open to see if this is something that more people in the community need. 

---

_Label `needs-decision` added by @MichaReiser on 2024-02-24 17:34_

---

_Comment by @eli-schwartz on 2024-02-26 02:48_

> One approach would be to limit this to assignment and then used on the very next line (not counting comments).
> 
> Another approach would be to limit the rule to numbers, strings and function calls with no arguments. Explanation: export the constant or rename the function to something more clear.

The use case of:

```python
    msg = 'This is a text message describing the error that has just occurred'
    raise utils.AppnameParserException(msg, location=self.current_node) from exc
```

to avoid long lines like:
```python
    raise utils.AppnameParserException('This is a text message describing the error that has just occurred', location=self.current_node) from exc
```

violates both your heuristics at the same time. I would consider it factually incorrect of a linter to suggest I shouldn't be using variables for single-use simple strings like this. Even without the location kwarg, or `from exc`.

On the other hand, I suppose I don't have to enable the rule. :)

---

_Comment by @Tatsh on 2024-02-26 04:12_

> > One approach would be to limit this to assignment and then used on the very next line (not counting comments).
> > 
> > Another approach would be to limit the rule to numbers, strings and function calls with no arguments. Explanation: export the constant or rename the function to something more clear.
> 
> The use case of:
> 
> ```python
>     msg = 'This is a text message describing the error that has just occurred'
>     raise utils.AppnameParserException(msg, location=self.current_node) from exc
> ```
> 
> to avoid long lines like:
> ```python
>     raise utils.AppnameParserException('This is a text message describing the error that has just occurred', location=self.current_node) from exc
> ```
> 
> violates both your heuristics at the same time. I would consider it factually incorrect of a linter to suggest I shouldn't be using variables for single-use simple strings like this. Even without the location kwarg, or `from exc`.
> 
> On the other hand, I suppose I don't have to enable the rule. :)

I would love if Ruff could auto break strings, with parentheses as necessary. And there should be an option if there is not one already to ignore long strings for the line too long rule (really to ignore `["'][,\)$`). Markdownlint has this rule for links that go over the limit.

---

_Comment by @nickdrozd on 2025-01-29 18:15_

I would also like this check. Rationale is that assignment statements are dead weight, don't do anything, just clutter.

Say I am reading new function, want to know what it does in broad strokes. Function has twenty statements. Turns out five statements are single-use assignments. So, 25% of statements are junk. Hey function author, get to the point!

Of course, this is little bit opinionated. Would be good candidate for "restriction" class lints.

Also, should be restricted to functions with only single arg, or something like that. Because of possible order of side effects. Say we have:

```python
a = f()

b = g()

h(a, b)
```

Call `f` before `g`. But if we inline:

```python
h(f(), g())
```

Still call `f` before `g`? Depends on how Python order of argument evaluation, which is ???. So there are some subtleties to consider.

---
