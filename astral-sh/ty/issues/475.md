```yaml
number: 475
title: Is there some way we can support setuptools-style editable installs?
type: issue
state: open
author: pythonweb2
labels:
  - imports
assignees: []
created_at: 2025-05-21T18:15:12Z
updated_at: 2025-11-24T08:19:29Z
url: https://github.com/astral-sh/ty/issues/475
synced_at: 2026-01-10T01:58:59Z
```

# Is there some way we can support setuptools-style editable installs?

---

_Issue opened by @pythonweb2 on 2025-05-21 18:15_

### Question

This issues exists in other type checkers (e.g. https://github.com/python/mypy/issues/13392), and I can see it exists here as well.

When I install an editable dependency, in this case using uv as my package manager, I get a whole bunch of `unresolved-import` issues. What I had to do with mypy was this:

```
[[tool.mypy.overrides]]
module = [
    "editable_1.*",
    etc...
]
ignore_missing_imports = true
```

Mainly I'm just wondering if you are planning on supporting resolving editable dependencies, or at least being able to ignore these errors in a targeted manner, as opposed to disabling the check entirely.

Thanks for the work on the new exciting project!

### Version

ty 0.0.1-alpha.6 (f11c6012d 2025-05-20)

---

_Label `question` added by @pythonweb2 on 2025-05-21 18:15_

---

_Label `imports` added by @AlexWaygood on 2025-05-21 18:26_

---

_Comment by @AlexWaygood on 2025-05-21 18:29_

We support editable installs that statically write the path to the editable install to a `.pth` file in `site-packages`. However, just like every other type checker, we unfortunately cannot support editable installs that use import hooks. This would require us to dynamically execute Python code, which is not something a static type checker can realistically support. 

---

_Closed by @AlexWaygood on 2025-05-21 18:29_

---

_Comment by @carljm on 2025-05-21 19:34_

Can I confirm that you are using the setuptools build backend with uv? Because uv's default build backend is hatchling, and my understanding is hatchling should do static editable installs via `.pth` files, which work with static analysis. Only setuptools (AFAIK) defaults to the incompatible import-hook style of editable install.

---

_Comment by @pythonweb2 on 2025-05-21 22:00_

Oh, yeah it is a setuptools package, I was using uv to install it.

---

_Comment by @carljm on 2025-11-13 06:43_

This came up from a partner, with a suggestion that basedpyright may have a technique for handling these editable installs? Need to verify that, but if it's true we should consider if we can support these somehow.

---

_Reopened by @carljm on 2025-11-13 06:43_

---

_Renamed from "Unresolved imports from an editable dependency" to "Is there some way we can support setuptools-style editable installs?" by @carljm on 2025-11-13 06:44_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:44_

---

_Comment by @MichaReiser on 2025-11-13 07:40_

Hmm, I'm not sure there's any special handling. I only did a quick search for `pth` and their logic looks very much the same as ours.

https://github.com/DetachHead/basedpyright/blob/acfea41d0026bf99fa9084f101776ff9bdf5ca70/packages/pyright-internal/src/analyzer/pythonPathUtils.ts#L179-L194

@DetachHead do you know if basedpyright does anything special for editable installs created by setuptools?

---

_Comment by @DetachHead on 2025-11-13 12:41_

basedpyright's behavior is unchanged from pyright, which supports `.pth` files, but does not support whatever setuptools does by default.

i did however [tweak the documentation a bit](https://docs.basedpyright.com/latest/usage/import-resolution/#setuptools) in response to https://github.com/detachHead/basedpyright/issues/731 to make it a bit more clear that this is controlled by the build backend rather than the frontend, because pyright's documentation didn't really make that clear



---

_Comment by @carljm on 2025-11-13 15:27_

Thanks! Ok I'm going to leave this out of ty Stable milestone -- it doesn't seem like anyone has any good short-term ideas for actually supporting these setuptools editables.

I'll leave this open as a long-term issue -- what really needs to happen is someone pushing forward a PEP for a metadata standard for editable installs to fix this mess. And in theory we could do that.

---

_Removed from milestone `Stable` by @carljm on 2025-11-13 15:28_

---

_Added to milestone `Post-stable` by @carljm on 2025-11-13 15:28_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Label `question` removed by @MichaReiser on 2025-11-24 08:19_

---
