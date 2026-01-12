```yaml
number: 3946
title: Implement ssort
type: issue
state: open
author: JackAshwell11
labels:
  - plugin
  - accepted
assignees: []
created_at: 2023-04-12T10:37:41Z
updated_at: 2024-12-02T14:51:44Z
url: https://github.com/astral-sh/ruff/issues/3946
synced_at: 2026-01-12T15:54:44Z
```

# Implement ssort

---

_@JackAshwell11_

Since Ruff already implements [isort](https://beta.ruff.rs/docs/rules/#isort-i), it would be nice to see [ssort](https://github.com/bwhmather/ssort) implemented as well.

This plugin automatically reorders the contents of a python file so that statements are placed after the things they depend making navigation easier within the file. For example (from the ssort GitHub page):
Before:
```py
from module import BaseClass

def function():
    return _dependency()

def _decorator(fn):
    return fn

@_decorator
def _dependency():
    return Class()

class Class(BaseClass):
    def public_method(self):
        return self

    def __init__(self):
        pass
```
After:
```py
from module import BaseClass

class Class(BaseClass):
    def __init__(self):
        pass

    def public_method(self):
        return self

    def _decorator(fn):
        return fn

@_decorator
def _dependency():
    return Class()

def function():
    return _dependency()
```

---

_Comment by @MichaReiser on 2023-04-12 10:40_

Does ssort solve a similar problem to the newspaper rule #2436 (except that it fixes it automatically)? 

---

_Comment by @JackAshwell11 on 2023-04-12 10:50_

I haven't heard of `flake8-newspaper-style` before, but based on what I can gather, it seems so.

---

_Comment by @g-as on 2023-04-12 11:50_

It seems that they do solve a similar problem but with opposite solutions.

`ssort` does bottom-up while `flake8-newspaper-style` does top-down.

https://github.com/bwhmather/ssort#frequently-asked-questions

---

_Comment by @JackAshwell11 on 2023-04-12 11:54_

There is an active [issue](https://github.com/bwhmather/ssort/issues/18) in `ssort` asking for more sorting options. This could be part of `tool.ruff.ssort.sorting-order`. Although, I'm not sure if top-down is the correct order for a scripting language such as Python since lookups happen immediately as described [here](https://github.com/bwhmather/ssort#why-does-ssort-sort-bottom-up-rather-than-top-down).

---

_Label `plugin` added by @charliermarsh on 2023-04-13 03:28_

---

_Comment by @tjkuson on 2023-05-11 13:38_

I would add that ssort conflicts with the [Django documentation](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#model-style) and the [`DJ012`](https://beta.ruff.rs/docs/rules/django-unordered-body-content-in-model/) rule. If ssort were to be implemented in ruff, I would love it if there were a sorting option to cohere with the Django standards (beyond just top-down vs bottom-up).

---

_Comment by @JackAshwell11 on 2023-05-11 13:43_

> I would add that ssort conflicts with the [Django documentation](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#model-style) and the [`DJ012`](https://beta.ruff.rs/docs/rules/django-unordered-body-content-in-model/) rule. If ssort were to be implemented in ruff, I would love it if there were a sorting option to cohere with the Django standards (beyond just top-down vs bottom-up).

There are some issues on `ssort`'s GitHub page asking for options to change the sorting order. So that could work.

---

_Comment by @JackAshwell11 on 2023-05-11 20:05_

I would like to have a go at implementing this, not sure how successful I'll be, but I can try. Just would like to finish my exams first.

---

_Comment by @JackAshwell11 on 2023-05-18 19:56_

So after looking and understanding ssort's implementation, it definitely looks like it would be better to just re-write it entirely from the ground up. I'm gonna work on some other rules so I can get a bit more practice with Ruff's codebase first.

---

_Comment by @jgberry on 2023-06-09 02:32_

Hey @Aspect1103! I'm a ssort collaborator and have recently really taken an interest in ruff. I think there's a lot of promise here, specifically from a performance perspective. We spent considerable effort optimizing ssort's performance, but pure python can only take you so far. I could easily see a pure rust implementation having orders of magnitude better performance! Let me know if I can help out in any way!

---

_Comment by @JackAshwell11 on 2023-06-09 06:44_

> Hey @Aspect1103! I'm a ssort collaborator and have recently really taken an interest in ruff. I think there's a lot of promise here, specifically from a performance perspective. We spent considerable effort optimizing ssort's performance, but pure python can only take you so far. I could easily see a pure rust implementation having orders of magnitude better performance! Let me know if I can help out in any way!

Hi, I haven't yet had a go with making some Ruff plugins, so you could take this one on if you want.

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @jag-k on 2024-11-26 15:27_

Hey, any update here?

---

_Comment by @dhruvmanila on 2024-11-27 07:32_

@jag-k No updates yet. We're happy to accept any contributions here, there was an attempt previously (https://github.com/astral-sh/ruff/pull/5271) which can be used as a reference.

---

_Comment by @jag-k on 2024-11-27 09:00_

> @jag-k No updates yet. We're happy to accept any contributions here, there was an attempt previously (https://github.com/astral-sh/ruff/pull/5271) which can be used as a reference.

Ohh.. well, I think I need to start learning Rust for it..)) 

---

_Comment by @sanmai-NL on 2024-12-02 14:45_

See also: https://github.com/astral-sh/ruff/issues/8232

---
