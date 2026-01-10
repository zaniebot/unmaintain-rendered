```yaml
number: 2418
title: "Implement `flake8-cognitive-complexity`"
type: issue
state: open
author: ngnpope
labels:
  - plugin
  - needs-decision
  - needs-design
assignees: []
created_at: 2023-01-31T21:28:06Z
updated_at: 2026-01-07T12:17:16Z
url: https://github.com/astral-sh/ruff/issues/2418
synced_at: 2026-01-10T11:09:45Z
```

# Implement `flake8-cognitive-complexity`

---

_Issue opened by @ngnpope on 2023-01-31 21:28_

[GitHub](https://github.com/Melevir/flake8-cognitive-complexity), [PyPI](https://pypi.org/project/flake8-cognitive-complexity/).

- [ ] [`CCR001`](): Cognitive complexity is too high (X > Y)

Configuration options: `max-cognitive-complexity`

This plugin is focused on [cognitive complexity](https://docs.codeclimate.com/docs/cognitive-complexity) vs [cyclomatic complexity](https://docs.codeclimate.com/docs/cyclomatic-complexity) which we have via `mccabe`. Apparently this is the algorithm used by SonarSource, CodeClimate, etc. according to the README. See [here](https://github.com/Melevir/cognitive_complexity/blob/master/README.md#what-is-cognitive-complexity) for more details.

---

_Label `plugin` added by @charliermarsh on 2023-01-31 23:05_

---

_Comment by @sbrugman on 2023-02-01 11:56_

Cool plugin! `ruff` would be a nice opportunity to spread this to a wider audience.

It passes over de function body once to computer a complexity score >= 0. Should not be too hard or expensive to add. 

---

_Comment by @KerberosMorphy on 2023-02-21 22:03_

I was looking to replace my `flake8` plugins list with `ruff` and there's 2 plugins I couldn't replace yet:

- `flake8-cognitive-complexity`
- `flake8-expression-complexity`

Hope to see them implemented in ruff.

---

_Comment by @mariosker on 2023-04-27 08:40_

Also in the same terms it would be nice to implement [`flake8-cohesion`](https://github.com/sasanjac/flake8-cohesion)

---

_Comment by @KerberosMorphy on 2023-04-27 15:00_

> Also in the same terms it would be nice to implement [`flake8-cohesion`](https://github.com/sasanjac/flake8-cohesion)

Didn't know that one, thanks for sharing ;)

---

_Comment by @webknjaz on 2023-05-18 14:13_

It'd be also nice to implement https://github.com/Miserlou/JonesComplexity that WPS uses, for example..

---

_Comment by @MichaReiser on 2023-05-19 07:40_

Are you planning to use these rules alongside the mccabe complexity rule or instead of it? 

---

_Comment by @webknjaz on 2023-05-19 12:46_

I usually have all enabled, unless they are exact duplicates, of course..

---

_Comment by @KerberosMorphy on 2023-05-24 17:26_

> Are you planning to use these rules alongside the McCabe complexity rule or instead of it?

I always include cyclomatic + expression + cognitive.

Here a quick example which demonstrate each complexity:

### `foo`

Would be the best option if we only looks for Cyclomatic complexity.

But the expression is complex, especially for a junior developer or someone who just start with Python.

The Cognitive represent, as I interpret it myself, the amount of context you have to store in your brain.

```python
def foo(**kwargs):  # Cognitive: 3, Cyclomatic: 1, Max Expression: 5
    return [  # Expression: 5
        key if key == value else value if value in kwargs else f"{key}+{values}"
        for key, value, in kwargs
    ]
```

### `bar`

Would be the worst option if we only looks for Cyclomatic complexity.

But the expression is simple, each line is easy to understand.

You could need a higher level of context storage in your brain as you analyse this code, which is why it give a higher Cognitive complexity.

```python
def bar(**kwargs):  # Cognitive: 5, Cyclomatic: 4, Max Expression: 2
    res = []  # Expression: 2

    for key, value, in kwargs.items():]  # Expression: 1
        if key == value:  # Expression: 1
            res.append(key)  # Expression: 1
            continue

        if value in kwargs:  # Expression: 1
            res.append(value)  # Expression: 1
            continue

        res.append(f"{key}+{values}")  # Expression: 2

    return res
```

### `boo` using `subboo`

Cyclomatic complexity is reduce compare to `bar` but still higher than `foo`.

Max Expression is the same than `bar`.

The Cognitive complexity is at 2, each function is pretty simple to analyze without the need to keep too much context in your brain.

```python
def subboo(key, value, keys):  # Cognitive: 2, Cyclomatic: 3, Max Expression: 2
    if key == value:  # Expression: 1
        return key

    if value in keys:  # Expression: 1
        return value

    return f"{key}+{values}"  # Expression: 2

def boo(**kwargs):  # Cognitive: 0, Cyclomatic: 1, Max Expression: 2
    return [subbar(key, value, kwargs) for key, value, in kwargs.items()]  # Expression: 2
```


---

_Comment by @tylerlaprade on 2023-08-08 23:57_

FWIW, `flake8-cognitive-complexity` and `flake8-cohesion` are the only two rules I still run. If those were added to Ruff, I could fully remove `flake8` from my project.

I do run McCabe in Ruff as well, but they serve three different purposes, IMHO.

---

_Comment by @Anders-Steen on 2024-02-21 14:17_

Anyone want to look at my PR? #9812 

---

_Label `needs-decision` added by @MichaReiser on 2024-03-28 16:14_

---

_Label `needs-design` added by @MichaReiser on 2024-03-28 16:14_

---

_Comment by @MichaReiser on 2024-03-28 16:14_

> I do run McCabe in Ruff as well, but they serve three different purposes, IMHO.

Could you explain how the purpose between Mc Cabe and this rule differ in your view?

---

_Comment by @tylerlaprade on 2024-03-28 16:20_

> Could you explain how the purpose between Mc Cabe and this rule differ in your view?
-@MichaReiser

Cognitive complexity is how hard the code is to understand, while cyclomatic complexity is the number of independent branches. For example, a switch case that handles every letter of the alphabet might be very easy to understand, but it would have 26 separate branches.

---

_Comment by @MichaReiser on 2024-03-28 16:54_

Thanks, that makes sense. What's the motivation for catching the switch if it is easy to understand? 

I'm asking because we're currently leaning towards having one single complexity rule with a configuration option for the metric to avoid overlap (and conflicts). However, that wouldn't support your use case.

---

_Comment by @tylerlaprade on 2024-03-28 18:48_

Ah, I see your point! The main reason I run McCabe is because I don't want to run flake8 in Github Actions for just a single rule. I personally wouldn't mind replacing it.

However, I do find unique benefit from the related flake8-cohesion, which looks at a class as a whole.

---

_Comment by @cameronbrill on 2024-11-26 21:48_

Hi @MichaReiser! I'm a big fan of the rule proposed in this issue, I'm curious have you/the astral team done any of [the design work here](https://github.com/astral-sh/ruff/pull/9812#issuecomment-2025605055)/is this included in a product roadmap in any way? 

---

_Comment by @MarcBresson on 2025-03-04 13:41_

Hello! Is there any traction to have this implemented in ruff?

---

_Comment by @Tishka17 on 2025-05-30 15:53_

Hi. Is there any plan for this feature?

---

_Comment by @cameronbrill on 2025-08-29 19:27_

Hi @MichaReiser coming back to this one, if the team is open to it, I'd be interested in trying to implement the cognitive complexity rule to replace McCabe cyclomatic complexity. Any tips beyond the contribution guide would be appreciated 🙂 

---

_Comment by @MichaReiser on 2025-09-15 09:14_

Hi @cameronbrill Thanks for your interest. 

I don't think we're currently in a position where we can accept a PR that replaces the existing rule AND we also don't want to add a new complexity rule before we figure out https://github.com/astral-sh/ruff/issues/1774. I'm sorry, this has to wait a little longer

---

_Comment by @cameronbrill on 2025-09-18 21:54_

no worries, thanks for getting back to me!

---

_Comment by @ashrub-holvi on 2026-01-07 12:17_

Just mentioning [radon](https://github.com/rubik/radon) as someone make search by that name and now it's only mentioned in [closed as duplicate ticket](https://github.com/astral-sh/ruff/issues/20552).

---
