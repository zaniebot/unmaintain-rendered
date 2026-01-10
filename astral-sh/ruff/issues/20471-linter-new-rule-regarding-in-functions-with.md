```yaml
number: 20471
title: "Linter: new rule regarding `=` in functions with complex expressions"
type: issue
state: closed
author: adamsol
labels: []
assignees: []
created_at: 2025-09-18T17:03:24Z
updated_at: 2025-10-06T19:53:26Z
url: https://github.com/astral-sh/ruff/issues/20471
synced_at: 2026-01-10T11:09:59Z
```

# Linter: new rule regarding `=` in functions with complex expressions

---

_Issue opened by @adamsol on 2025-09-18 17:03_

### Summary

It is generally agreed that formatting like `foo(x: int=42)` or `foo[T: int | str=int]` (example from #13699) looks strange, as the equals sign visually contradicts operator precedence. That's why [PEP 8](https://peps.python.org/pep-0008/#other-recommendations) makes an exception from its own rule and dictates putting spaces around `=` in such situations, which is properly implemented in Ruff.

What about formatting like `foo(x=a + b)`? This is a symmetrical situation, yet there exists no exception for it, and no separate rule to flag such cases.

Or a more practical example from Django:

```py
Order.objects.filter(timestamp__gt=timezone.now() - timedelta(minutes=30))
```

where it's quite difficult to even notice the equals sign at the first glance.

Some time ago I reported this in https://github.com/PyCQA/pycodestyle/issues/1240, but the idea was rejected. Is there any interest in Ruff to implement such a linter rule that would report this as bad formatting?

---

PS. Personally I'd love to simplify this and just always put spaces around `=` (and I'm not the only one - see [this SO thread](https://stackoverflow.com/questions/8853063/pep-8-why-no-spaces-around-in-keyword-argument-or-a-default-parameter-value) or [this discussion](https://github.com/astral-sh/ruff/discussions/20034)), but I know the general stance against violating PEP 8 is strong, so I'm not asking about this here (although it's already "implemented" in Ruff for type variable defaults: #18746).

---

_Comment by @MichaReiser on 2025-09-19 06:54_

I come from the JS ecosystem and I was also suprised by this formatting, but Ruff follows PEP8:

> Don’t use spaces around the = sign when used to indicate a keyword argument, or when used to indicate a default value for an unannotated function parameter:

We currently don't accept any lint rules that aren't in line with PEP8. 


---

_Closed by @MichaReiser on 2025-09-19 06:54_

---

_Comment by @adamsol on 2025-09-19 08:01_

This rule would not violate PEP8. As I already wrote in https://github.com/PyCQA/pycodestyle/issues/1240, there are at least 3 different ways the snippet's readability could be improved without putting spaces around `=`:

* Factor out the expression to a separate variable.
* Put parentheses around the expression.
* Remove spaces around the binary operator.

Even though PEP 8 doesn't mention that formatting as bad style, I'm sure there are many style rules that PEP8 doesn't mention, but are still implemented in Ruff. The point is to have these cases flagged in a repo, so we can then manually make a decision how to improve the formatting.

Edit: in case anyone is interested, here's my Flake8 plugin that implements this: https://github.com/adamsol/flake8-arg-spacing (since it seems Ruff doesn't currently support plugins: #283).

---
