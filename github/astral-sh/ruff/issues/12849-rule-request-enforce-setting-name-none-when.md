---
number: 12849
title: "Rule request: enforce setting name=None when raising AttributeError/NameError with a custom error message on py310+"
type: issue
state: open
author: arnaud-ma
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-12T18:32:32Z
updated_at: 2024-08-14T15:23:05Z
url: https://github.com/astral-sh/ruff/issues/12849
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule request: enforce setting name=None when raising AttributeError/NameError with a custom error message on py310+

---

_Issue opened by @arnaud-ma on 2024-08-12 18:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Given this code where the 'bar' attribute has been deprecated for 'new_bar':

```python3
class Foo:
    def __init__(self):
        self._bar = "other_bar"
        self.new_bar = "new_bar"

    def __getattr__(self, name):
        if name == "bar":
            msg = "Use 'new_bar' instead."
            raise AttributeError(msg)
        return super().__getattribute__(name)
```

In Python >= 3.10, the error message is:

```python3
AttributeError: Use 'new_bar' instead.. Did you mean: '_bar'?
```

The "Did you mean" part (called name suggestions) is not revelant, or even misleading.
The workaround is simply to specify the 'name' parameter to None, e.g. `AttributeError(msg, name=None)` (see the [python doc](https://docs.python.org/3/library/exceptions.html#AttributeError)). 

What I'm still not sure about is: are there times when it is really relevant when writing our own AttributeError or NameError, in addition of the message ?

---

_Label `rule` added by @dhruvmanila on 2024-08-13 08:47_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-13 08:47_

---

_Comment by @MichaReiser on 2024-08-14 07:27_

@AlexWaygood any thoughts on this? Are there legitimate cases where not specifying the name on an `AttributeError` is correct?

---

_Renamed from "Rule request: Parameter name=None on AttribureError and NameError" to "Rule request: Parameter name=None on AttributeError and NameError" by @AlexWaygood on 2024-08-14 07:33_

---

_Comment by @AlexWaygood on 2024-08-14 11:53_

This is clever! I haven't seen this technique before for suppressing the "Did you mean...?" suggestions in the error messages for `AttributeError`s and `NameError`s.

As to whether it's a good idea: the `name` attribute on `AttributeError` instances is documented, so it's guaranteed not to be removed (unless there's a `DeprecationWarning` for 2 years, which seems unlikely). However, I can't see anything in the docs that says anything about using this as a technique to suppress the "Did you mean...?" error messages with `AttributeError`.

@pablogsal, sorry to ping you -- would you be able to provide any context here, as the author of this CPython feature? Do you think this a dependable way to suppress these suggestion messages on `AttributeError`s, that's guaranteed not to break in future Python versions?

---

_Comment by @AlexWaygood on 2024-08-14 12:11_

I guess I'm not actually sure what the specific rule request is, re-reading this issue. @arnaud-ma -- are you asking for a rule that complains if you set `name=None`? Or are you asking for a rule that complains if you _don't_ set `name=None` when you're manually raising `AttributeError` with a custom error message?

---

_Comment by @arnaud-ma on 2024-08-14 12:24_

A rule that complains if we don't set. Sorry I may have expressed myself badly. But my doubt in my last sentence was to know if there were cases where the name suggestions were relevant. Of course for an `AttributeError` thrown by Python itself it is, since it has no other information. But otherwise in most cases, I think the error should be fully explained in the error message. And the author of the error message probably does not know that the "Did you mean...?" could be added at the end of the message.

---

_Comment by @AlexWaygood on 2024-08-14 12:26_

Great, thanks @arnaud-ma. That's what I thought, but it's good to check that we're on the same page :-)

---

_Renamed from "Rule request: Parameter name=None on AttributeError and NameError" to "Rule request: enforce setting name=None when raising AttributeError/NameError with a custom error message on py310+" by @AlexWaygood on 2024-08-14 12:42_

---

_Comment by @pablogsal on 2024-08-14 13:29_

> @pablogsal, sorry to ping you -- would you be able to provide any context here, as the author of this CPython feature? Do you think this a dependable way to suppress these suggestion messages on `AttributeError`s, that's guaranteed not to break in future Python versions?

As you said the attribute itself won't disappear, but how it's used by the suggestions machinery is an implementation detail and I would not recommend depending on that if we ever need to change. We are quite constrained upstream to emmit this messages in ways that won't hurt performance for the happy case so I don't want to shoot ourselves in the foot with backwards compatibility promises. 

Pragmatically speaking I don't think this will change at all, so it's not crazy to depend on that.

The other thing I notice is that this is suggesting a name that starts with an underscore and maybe we should change this upstream to skip these.... 🤔 

---

_Comment by @AlexWaygood on 2024-08-14 13:38_

> The other thing I notice is that this is suggesting a name that starts with an underscore and maybe we should change this upstream to skip these.... 🤔

Oh, I think that's already been improved in Python 3.13+, via https://github.com/python/cpython/issues/116871 / https://github.com/python/cpython/pull/116930!

---

_Comment by @arnaud-ma on 2024-08-14 13:49_

It ignores but only when the error is not raised manually (tried my example in Python 3.14 with and without the `__getattr__`).

---

_Comment by @pablogsal on 2024-08-14 15:23_

> > The other thing I notice is that this is suggesting a name that starts with an underscore and maybe we should change this upstream to skip these.... 🤔
> 
> Oh, I think that's already been improved in Python 3.13+, via [python/cpython#116871](https://github.com/python/cpython/issues/116871) / [python/cpython#116930](https://github.com/python/cpython/pull/116930)!

Ah thanks for the heads up! Somehow I missed Serhi’s PR

---
