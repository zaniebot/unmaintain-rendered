```yaml
number: 160
title: Multi-platform checking
type: issue
state: open
author: MichaReiser
labels:
  - wish
assignees: []
created_at: 2025-03-26T13:31:28Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/160
synced_at: 2026-01-12T15:54:22Z
```

# Multi-platform checking

---

_@MichaReiser_

Calling a platform-specific method with no `python-platform` set currently always emits a `possibly-unbound` warning ([playground](https://playknot.ruff.rs/baf5d9d3-f4e7-4c87-ab8a-8302d2add03d)), even if the branch is guarded by an explicit `sys.platform` check. 

The only way around the `possibly-unbound` warning seems to be to 

1. Use a suppression comment
1. Only check for a specific platform, but that's not an option for everyone

Ideally, the `sys.platform` narrowing would be sufficient but that's probably non-trivial. A more short-term solution would be to explore ways to avoid the `possibly-unbound` false positive. 

---

_Label `bug` added by @MichaReiser on 2025-03-26 13:31_

---

_Comment by @carljm on 2025-03-26 13:37_

Effectively, fixing this properly means reviving the work we'd planned to do for multi-version-checking, but applying it only to multi-platform-checking for now. Because what we are wanting here is the ability to check for multiple platforms at once, and validate that you aren't using a platform-specific API without checking the platform first.

I think that might not actually be too hard? It just means introducing a `ExistsOnPlatform` type and having `sys.platform` checks do narrowing and visibility constraints as intersections with that type.

But there might also be a shorter-term fix that silences the false positives without actually giving us the ability to error if you use a platform-specific API without checking the platform.

---

_Comment by @AlexWaygood on 2025-04-03 17:26_

the _easiest_ fix here might be to just default to `platform = "linux"`, which I believe is what all other type checkers do. Then we'd always understand `sys.platform`-guarded branches as either definitely-reachable or definitely-unreachable, the same as we do for `sys.version_info` guarded branches.

I know this is not the behaviour we'd like in the long term, but I think making this change would be pretty easy; it might be the best course of action for the alpha.

---

_Comment by @MichaReiser on 2025-04-03 17:53_

This sounds like a reasonable solution for the alpha. We could consider defaulting to the current platform which gives better editor experience but might not necessarily be the right choice for the cli

---

_Comment by @AlexWaygood on 2025-04-03 17:55_

Oh, sorry, I think that's what most other type checkers do: default to the current platform, not default to `"linux"`. So I don't think that behaviour would come as a surprise to users

---

_Comment by @MichaReiser on 2025-04-03 20:44_

Slightly different but related (but might be worth its own issue):

Something that https://github.com/astral-sh/ruff/pull/17183 showed is that some projects have platform specific files. E.g. rich has a file specific to windows. I'm not sure how best to support this use case because it would require overriding platform at a per file level


Pyright supports this by allowing multiple execution environments https://microsoft.github.io/pyright/#/configuration?id=sample-config-file

---

_Comment by @carljm on 2025-04-03 22:37_

I agree that in the short term we should just always have a single configured (or defaulted) platform and not attempt multi-platform checking. Ignore the rest of this comment for short-term :)

If/when we do aim for multi-platform (and possibly multi-version?) checking, after chatting with @sharkdp today I have an updated understanding of how that could work. I don't think we need the `PlatformIs` style types to intersect with, now that we have visibility constraints. Instead we can record a visibility constraint (as we already do) for a test like `sys.platform == "win32"`, and when evaluating visibility constraints we can pass in a context which contains a potentially-updated value for `sys.platform`, which would come from narrowing constraints at the usage site. So for example, given

```py
if sys.platform == "win32":
    def win_only_function(): ...

# later, maybe even in a different file...

if sys.platform == "win32":
    win_only_function()
```

The definition of `win_only_function` would come with a visibility constraint of `sys.platform == "win32"`. This already happens today, we just aren't able to evaluate that constraint to a known truthiness if we don't have a single platform configured. But what we can do in future is record a narrowing on the second `if sys.platform == "win32"` check so that within that block we understand that `sys.platform` is now `"win32"`. And we can include that information in the context for evaluating the visibility constraints on the definition of `win_only_function()`, so that we are able to evaluate it as definitely-truthy (but only within that context.)

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:20_

---

_Renamed from "[red-knot] Calling system specific methods and possibly unbound" to "Calling system specific methods and possibly unbound" by @MichaReiser on 2025-05-07 15:26_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 00:57_

---

_Renamed from "Calling system specific methods and possibly unbound" to "Multi-platform checking" by @carljm on 2025-06-11 00:57_

---

_Label `bug` removed by @carljm on 2025-07-01 06:39_

---

_Label `feature` added by @carljm on 2025-07-01 06:39_

---

_Comment by @andythomas on 2025-08-17 12:36_

I am not sure if this is related enough to the `possibly-unbound` warning, but this case is quite similar:

```
if os.name == "nt":
    try:
        from ctypes import windll  # ty: ignore[unresolved-import] -> Only exists on Windows.
```

---

_Comment by @AlexWaygood on 2025-08-17 12:42_

@andythomas that's a slightly different issue. The typing spec only specifies that type checkers must understand simple tests using `sys.version_info` and `sys.platform`, and so type checkers generally do not understand conditions using other constants/functions using `os.name` or `platform.system()`. We could _consider_ adding support for that, but it would add a fair amount of complexity and it might be hard to get it fully accurate. (You can see that typeshed's stubs for `ctypes`, which we use as the source of truth for the standard library, guard the definition of this symbol inside a `sys.platform == "win32"` branch but don't tell us anything about whether the symbol is available when `os.name == "nt"`: https://github.com/python/typeshed/blob/85a787bba3057f1df44188b4c815069d2d931557/stdlib/ctypes/__init__.pyi#L111-L112.)

---

_Comment by @andythomas on 2025-08-17 12:56_

That is very interesting, thank you for the quick answer. So, is it save to say (also in other contexts) that `sys.platform` is the preferred check?

---

_Comment by @AlexWaygood on 2025-08-17 13:06_

> So, is it save to say (also in other contexts) that `sys.platform` is the preferred check?

`sys.platform` will almost always be better understood by type checkers than `os.name`. At runtime, [`sys.platform`](https://docs.python.org/3/library/sys.html#sys.platform) has finer granularity than [`os.name`](https://docs.python.org/3/library/os.html#os.name), but I _believe_ checking `if os.name == "nt"` should almost always produce the same results as checking `if sys.platform == "win32"`

---

_Label `wish` added by @AlexWaygood on 2025-10-05 10:56_

---

_Label `feature` removed by @AlexWaygood on 2025-10-05 10:56_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
