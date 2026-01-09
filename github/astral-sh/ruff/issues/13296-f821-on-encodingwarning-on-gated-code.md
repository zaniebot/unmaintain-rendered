---
number: 13296
title: F821 on EncodingWarning on gated code
type: issue
state: open
author: jaraco
labels:
  - type-inference
assignees: []
created_at: 2024-09-09T21:41:12Z
updated_at: 2024-10-28T07:55:15Z
url: https://github.com/astral-sh/ruff/issues/13296
synced_at: 2026-01-07T13:12:15-06:00
---

# F821 on EncodingWarning on gated code

---

_Issue opened by @jaraco on 2024-09-09 21:41_

As [reported in discord](https://discord.com/channels/1039017663004942429/1070132471699607623/1282814518828990526), related to #13287, recent ruff versions (0.6.4 or so) are failing checks on [EncodingWarnings](https://github.com/jaraco/zipp/blob/5d2fa666ffae2e89a6e4ccbc5ed9b3a5b8d64fc0/tests/test_path.py#L191-L195).

```
tests/test_path.py:191:31: F821 Undefined name `EncodingWarning`
    |
189 |         assert sys.flags.warn_default_encoding
190 |         root = zipfile.Path(alpharep)
191 |         with self.assertWarns(EncodingWarning) as wc:
    |                               ^^^^^^^^^^^^^^^ F821
192 |             root.joinpath("a.txt").read_text()
193 |         assert __file__ == wc.filename
    |

tests/test_path.py:194:31: F821 Undefined name `EncodingWarning`
    |
192 |             root.joinpath("a.txt").read_text()
193 |         assert __file__ == wc.filename
194 |         with self.assertWarns(EncodingWarning) as wc:
    |                               ^^^^^^^^^^^^^^^ F821
195 |             root.joinpath("a.txt").open("r").close()
196 |         assert __file__ == wc.filename
    |
```

This test is [gated on `sys.flags.warn_default_encoding`](https://github.com/jaraco/zipp/blob/5d2fa666ffae2e89a6e4ccbc5ed9b3a5b8d64fc0/tests/test_path.py#L182-L185), so is unreachable on Pythons where `EncodingWarning` won't be present. The F821 failure occurs even when running ruff under Python 3.12, so it's ruff's multi-python support that's implicated (an ambitious endeavor, to be sure!).

Given arbitrary sophistication, I'd expect `ruff` to detect the conditional that blocks this code on unsupported Pythons, infer which Pythons, then only perform the check relevant to those Pythons.

---

_Label `type-inference` added by @MichaReiser on 2024-09-10 17:14_

---

_Comment by @MichaReiser on 2024-09-10 17:26_

Thanks for opening this. 

The specific example requires type-inference to get right. Even then, it might be fairly difficult for a static analysis to figure that the code only runs in Python 3.12 because the constraint isn't that obvious (it's not an explicit `sys.version_info` check`):

* It requires understanding the `unittest.skipIf` decorator
* It requires understanding that `EncodingWarning` is always defined when `sys.warn_default_encoding` is defined



---

_Comment by @Avasam on 2024-09-15 16:33_

Isn't there a way in Ruff to specify extra globals/builtins ? I can't find it in docs, so I may be misremembering. Something like https://eslint.org/docs/latest/use/configure/language-options#using-configuration-files

This would also be useful for projects run from special contexts/interpreters (like an application's scripting engine using Python, or a non-CPython implementation with extra builtins, or projects "polluting" the builtins)

---

_Comment by @AlexWaygood on 2024-09-15 17:22_

> Isn't there a way in Ruff to specify extra globals/builtins ?

https://docs.astral.sh/ruff/settings/#builtins ?

---

_Comment by @eli-schwartz on 2024-10-27 20:56_

> The specific example requires type-inference to get right. Even then, it might be fairly difficult for a static analysis to figure that the code only runs in Python 3.12 because the constraint isn't that obvious (it's not an explicit `sys.version_info` check`):

I notice the same issue in my codebase, where it is very explicit and obvious:

```python
    if os.environ.get('MESON_SHOW_DEPRECATIONS'):
        # workaround for https://bugs.python.org/issue34624
        import warnings
        for typ in [DeprecationWarning, SyntaxWarning, FutureWarning, PendingDeprecationWarning]:
            warnings.filterwarnings('error', category=typ, module='mesonbuild')
        warnings.filterwarnings('ignore', message=".*importlib-resources.*")

    if sys.version_info >= (3, 10): # and os.environ.get('MESON_RUNNING_IN_PROJECT_TESTS'):
        # workaround for https://bugs.python.org/issue34624
        import warnings
        warnings.filterwarnings('error', category=EncodingWarning, module='mesonbuild')
        # python 3.11 adds a warning that in 3.15, UTF-8 mode will be default.
        # This is fantastic news, we'd love that. Less fantastic: this warning is silly,
        # we *want* these checks to be affected. Plus, the recommended alternative API
        # would (in addition to warning people when UTF-8 mode removed the problem) also
        # require using a minimum python version of 3.11 (in which the warning was added)
        # or add verbose if/else soup.
        warnings.filterwarnings('ignore', message="UTF-8 Mode affects .*getpreferredencoding", category=EncodingWarning)
```

```
mesonbuild/mesonmain.py:251:51: F821 Undefined name `EncodingWarning`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
mesonbuild/mesonmain.py:258:105: F821 Undefined name `EncodingWarning`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
```

The sys.version_info `and` condition is commented out to demonstrate that the issue is freestanding, although I am of the opinion it shouldn't matter -- when semantic narrowing can be done based on a single condition and that condition appears inside an `if ... and` then it is always correct to do semantic narrowing there as well. But I recall that some tools fail to do this (mypy) and couldn't recall whether that is the case for ruff.

---

_Comment by @MichaReiser on 2024-10-28 07:55_

@eli-schwartz This is expected. 

Ruff generally doesn't support any form of semantic narrowing. A few exceptions exist (`if typing.TYPE_CHECKING`) but `sys.version_info` checks are not. We're working on a new semantic model called red knot that does support sys version (and other) narrowing, which is why this issue is marked as type-inference.

---

_Referenced in [astral-sh/ruff#15916](../../astral-sh/ruff/issues/15916.md) on 2025-02-03 19:26_

---
