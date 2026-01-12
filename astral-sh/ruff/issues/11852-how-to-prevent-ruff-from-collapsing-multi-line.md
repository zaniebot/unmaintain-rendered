```yaml
number: 11852
title: How to prevent ruff from collapsing multi-line items into a single line
type: issue
state: closed
author: AntiSol
labels: []
assignees: []
created_at: 2024-06-13T08:19:54Z
updated_at: 2024-06-13T10:20:55Z
url: https://github.com/astral-sh/ruff/issues/11852
synced_at: 2026-01-12T15:54:51Z
```

# How to prevent ruff from collapsing multi-line items into a single line

---

_@AntiSol_

Hi There,

I had a bit of a look around but didn't find the answer I was looking for. it's very possible I just wasn't searching for the right thing - apologies if this is something that already has an answer.

If I write code like:
```python

def some_func(
    arg: str,
    arg2: int = 42
):
    return do_something(arg,arg2)
```

How can I stop ruff from collapsing this into a single line?

This also goes for things like dicts, e.g:
```python
stay_on_multiple_lines_dammit = {
    'foo': bar,
    'baz': 'bork'
}
```

 In most cases, if I've taken the time to split it into multiple lines, it's for readability, and I want to keep it that way.

I'm aware of the ability to turn off formatting for a block using `#fmt: off` and `#fmt: on`, but I find that most of the time ruff collapses these things, it's not what I want, so I'd like to prevent this collapsing from happening globally via some setting or by turning off some rule.

Note however that I do _not_ have a problem with it doing the reverse, i.e if I declare
```python
def some_func(arg: str, arg2: int = 42):
    return do_something(arg,arg2)
```

I don't have a problem with it breaking that into multiple lines similar to my first example if it's longer than my limit, and I want to keep that behaviour.

Thanks!

---

_Comment by @tmke8 on 2024-06-13 10:18_

Add trailing commas, like this:

```python

def some_func(
    arg: str,
    arg2: int = 42,
):
    return do_something(arg,arg2)
```
and
```python
stay_on_multiple_lines_dammit = {
    'foo': bar,
    'baz': 'bork',
}
```

---

_Comment by @AntiSol on 2024-06-13 10:20_

ooh cool, easy! thanks!

---

_Closed by @AntiSol on 2024-06-13 10:20_

---
