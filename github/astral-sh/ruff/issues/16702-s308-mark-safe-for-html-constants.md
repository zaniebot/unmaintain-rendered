---
number: 16702
title: "[S308] `mark_safe` for HTML constants"
type: issue
state: closed
author: mfontana-elem
labels:
  - rule
assignees: []
created_at: 2025-03-13T09:57:53Z
updated_at: 2025-03-17T10:29:09Z
url: https://github.com/astral-sh/ruff/issues/16702
synced_at: 2026-01-07T13:12:16-06:00
---

# [S308] `mark_safe` for HTML constants

---

_Issue opened by @mfontana-elem on 2025-03-13 09:57_

### Question

I have a question regarding this linting rule (imported from `flake8-bandit`).

I **think** I understand the problems with using `django.utils.safestring.mark_safe`. But know that `format_html` is being deprecated for being used without arguments¹, I struggle to find what would be the correct way to handle something as simple as creating a filter that performs: 
```python
def myfilter(case, ...):
  if case == "hello":
    return "<i>Hello world!</i>"
  elif case == "bye":
    return "<b>Bye world!</b>"
  else:
    ...
```
(This is not an accurate filter API, but illustrates the purpose).

In this case I **know** the HTML is safe, but I don't understand how to create a safestring from it without getting the `DeprecationWarningError` from Django (`format_html`) or noqa'ing the S308 rule (`suspicious-mark-safe-usage`).

I fail to see how a string constant could introduce a XSS unless there is programmer negligence, in which case all bets are off. Having said this, I might be the negligent here! 😅 

¹: And for a good reason. When coupled with f-strings interpolation happens before Django gets to escape inputs.

### Version

ruff 0.8.0

---

_Label `question` added by @mfontana-elem on 2025-03-13 09:57_

---

_Comment by @MichaReiser on 2025-03-13 11:40_

Thanks for the nice write up!

> I fail to see how a string constant could introduce a XSS unless there is programmer negligence, in which case all bets are off. Having said this, I might be the negligent here! 😅

Ruff's implementation (which I think matches bandit's) isn't very sophisticated. It only searches for calls to `mark_safe` without performing any analysis of where the content might be coming from.

Allowing calls to `mark_safe` where the argument is a string literal without format or f-string placeholders does sound reasonable to me. 

For now, the spirit of the rule is that you use `mark_safe` with an explicit noqa comment, acknowledging that I've thought about this. But that's obviously not as good as not requiring a noqa comment because it then doesn't catch if someone adds placeholders in the future.

So what I think should be accepted is:

```py
def myfilter(case, ...):
  if case == "hello":
    return mark_safe("<i>Hello world!</i>")
  elif case == "bye":
    return mark_safe("<b>Bye world!</b>")
  else:
    ...
```

---

_Label `question` removed by @MichaReiser on 2025-03-13 11:40_

---

_Label `rule` added by @MichaReiser on 2025-03-13 11:40_

---

_Comment by @mfontana-elem on 2025-03-13 11:42_

If it does indeed sound reasonable, I could try to have a look into submitting a PR in the upcoming weeks. I'm pretty much of a newbie when it comes to Rust, but don't care to spend the time off the clock.

---

_Comment by @MichaReiser on 2025-03-13 11:45_

It does sound reasonable to me, unless I'm overlooking something :)

You can find the code here 

https://github.com/astral-sh/ruff/blob/46fe17767d3d5cf7b1f7729b024fca7821d19264/crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs#L1054

and you can see an example that inspects the arguments here

https://github.com/astral-sh/ruff/blob/46fe17767d3d5cf7b1f7729b024fca7821d19264/crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs#L1058-L1074

---

_Comment by @Daverball on 2025-03-13 13:56_

You can also take a look at S704 and potentially reuse some of its logic, since it's trying to accomplish the same thing for a different API.

https://github.com/astral-sh/ruff/blob/851427a33043f56cccfa2e1e2c27c852f074b75b/crates/ruff_linter/src/rules/flake8_bandit/rules/unsafe_markup_use.rs#L135-L160

---

_Referenced in [astral-sh/ruff#16770](../../astral-sh/ruff/pulls/16770.md) on 2025-03-15 20:26_

---

_Comment by @mfontanaar on 2025-03-15 20:34_

Thanks a lot for the pointers. With your help a new version of the rule worked on the first `cargo run`!

I opened https://github.com/astral-sh/ruff/pull/16770 with the proposed change.

- - - 

Relatedly, it would be great if the `@mark_safe` decorator could be used for functions which can only return a string literal. For instance, following the example of the title, it would be nice if
```python
@mark_safe
def my_filter(case, ...):
  if case == "hello":
    return "<i>Hello world!</i>"
  else:
    return "<b>Bye world!</b>"
```

However, this would increase complexity. Whether the symbol is being used as function or as a decorator should be detected and branched accordingly. For the second case, the decorated function should be inspected and all return statements analyzed for "string-literal'ness.

---

_Closed by @MichaReiser on 2025-03-17 10:29_

---

_Closed by @MichaReiser on 2025-03-17 10:29_

---
