---
number: 1003
title: "Decide on \"enabled-by-default\" inlays"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-08-15T13:46:51Z
updated_at: 2025-12-31T22:14:31Z
url: https://github.com/astral-sh/ty/issues/1003
synced_at: 2026-01-10T01:51:14Z
---

# Decide on "enabled-by-default" inlays

---

_Issue opened by @MichaReiser on 2025-08-15 13:46_

Decide which inlays should be enabled by default.

https://github.com/astral-sh/ruff/pull/19910#discussion_r2275980197

---

_Label `server` added by @MichaReiser on 2025-08-15 13:46_

---

_Comment by @AlexWaygood on 2025-08-19 09:55_

I would prefer for these to be off by default. I think this is what most Python IDE users coming from Pylance will expect, since they are off by default in Pylance.

In cases like this, I find the inlay hints telling me what the argument names are very distracting, and it annoys me that they significantly increase the amount of space on my screen taken up by my code: 

<img width="1722" height="760" alt="Image" src="https://github.com/user-attachments/assets/b85441ed-a65c-417f-9490-0ba9e6115c45" />

<br><br>

The parameter names aren't particularly useful in this funciton call; if they were, I would have used keyword arguments in the function call. (The fact that Python has keyword arguments makes the situation quite different from Rust, in my opinion -- I often find reading Rust outside of an IDE quite hard due to the lack of keyword arguments, but inlay hints supplied by rust-analyzer or other language servers can definitely help mitigate that. The prevalence of keyword arguments in Python means that I generally find Python code much easier to read outside of an IDE.)

I think in general, it's quite important to have defaults that match users' expectations with regards to the language server. It would take me some time to figure out how to change the appropriate setting for this feature in VSCode (I can never find my way to the right setting on the rare occasion I need to change something).

---

_Comment by @MichaReiser on 2025-08-19 10:22_

This sounds as if it is mainly about the argument inlay hints and not necessarily about all inlay hints.

---

_Comment by @AlexWaygood on 2025-08-19 10:26_

I'm finding the argument inlay hints especially noisy in the playground right now, for sure. I do like that some of our other inlay hints provide an easy way to tell what type ty infers a variable as having, without even having to hover over a symbol.

---

_Comment by @MichaReiser on 2025-08-19 11:06_

Disabling the argument inlays by default seems very reasonable to me and I agree that they're more noisy (and probably too noisy to have them enabled by default). 

---

_Comment by @sharkdp on 2025-08-19 12:17_

For argument inlays, I'm wondering if there is a reasonable default somewhere in between "off by default" and "on by default": For calls with a single positional parameter, it's almost certainly never going to be useful to show an inlay hint. However, for calls that take three or more *positional* arguments, it might be reasonable to show them by default. For two arguments, there are probably arguments in both directions, but I would argue that the meaning is clear for the majority of functions. So could there be a setting where we show inlays for (d), but not for (a-c)?
```py
# (a) no inlay hint
print("Hello")

# (b) no inlay hint
max(1, 3)

# (c) also no inlay hint, only one positional argument
subprocess.run(["program", "arg"], check=True, text=True)  

# (d) 3+ positional args => add hints: randrange(start=0, stop=10, step=2)
randrange(0, 10, 2)
```

---

_Comment by @AlexWaygood on 2025-08-19 12:22_

> However, for calls that take three or more _positional_ arguments, it might be reasonable to show them by default.

and especially if:
- all arguments passed in are the same type (like your `randrange` function). In that kind of situation it can become hard to tell which argument corresponds to which parameter
- _and_ the parameter names are longer than a single letter (e.g. I wouldn't particularly appreciate `x` and `y` showing up as inlay hints for a call to a function like [this](https://github.com/python/typeshed/blob/d270bb0dc1df3fcfae2808c9a5a0f7ec802e1fb2/stdlib/statistics.pyi#L115-L117), even if it had >=3 positional arguments)

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-06 16:50_

---

_Comment by @Gankra on 2025-12-04 17:53_

Since this was last discussed we've landed a ton of inlay hint improvements, making them both more useful and less annoying.

Type inlay hints:
* are supressed in many cases where they're trivial/unhelpful
* are truncated when too long
* can be hovered to get type info / docs
* can be cmd+clicked to goto-definition
* can be double-clicked to insert them (helping you add types to your code)

Function argument inlay hints:
* are supressed in many cases where they're trivial/unhelpful
* can be cmd+clicked to goto-definition

I find both really helpful now, helping me understand python types and the libraries I'm using. But I am a big inlay-hint fangirl.

---

_Comment by @AlexWaygood on 2025-12-04 17:59_

I would personally still prefer them suppressed (by default) for function arguments in call expressions. They're still too noisy and distracting for me :-)

Most of the time if it's ambiguous which argument is being passed to which parameter, you just use a keyword argument in Python. I just don't think they provide the same degree of benefits as they do with Rust.

---

_Comment by @MichaReiser on 2025-12-04 18:01_

I would just wait for more user feedback rather than taking any one's preference here :)

---

_Comment by @MeGaGiGaGon on 2025-12-31 22:14_

My two cents on this as you may have gathered from my other issues, I'm a massive fan of inlay hints and would love if everything was on by default.

---
