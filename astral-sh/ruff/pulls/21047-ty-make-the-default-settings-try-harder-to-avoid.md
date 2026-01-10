```yaml
number: 21047
title: "[ty] Make the default settings try harder to avoid crashes"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/sfinae
created_at: 2025-10-23T14:46:03Z
updated_at: 2025-10-30T15:30:03Z
url: https://github.com/astral-sh/ruff/pull/21047
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Make the default settings try harder to avoid crashes

---

_Pull request opened by @Gankra on 2025-10-23 14:46_

This introduces a safe_mode to our settings type that tells our initialization code to more aggressively believe Settings Failure Is Not An Error. This allows the full settings initialization logic to be used as much as possible for the default database while trying to keep any discarded results as narrow as possible for graceful degradation.

Finally, Python has SFINAE.

Fixes https://github.com/astral-sh/ty/issues/859

---

_Label `server` added by @Gankra on 2025-10-23 14:46_

---

_Label `ty` added by @Gankra on 2025-10-23 14:46_

---

_Comment by @github-actions[bot] on 2025-10-23 14:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-23 14:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @Gankra on 2025-10-23 15:37_

Ah, rude. The exact case of `VIRTUAL_ENV=gibberish` actually blows up in asserts in `dynamic_resolution_paths`. This is a big ol' game of whackamole.

---

_Comment by @Gankra on 2025-10-23 16:03_

The failure with a gibberish `VIRTUAL_ENV` is a fascinating maybe-independent bug in our handling of symlinked python environments (homebrew).

We actually handle the gibberish VIRTUAL_ENV value gracefully but then we fallback to grabbing a reference to some homebrew site-packages and then blowup because symlinks are happening and the same dir looks like two different dirs.

(just on main, not with my patches, although my patches don't affect this):

```
No root found for path '/opt/homebrew/Cellar/python@3.13/3.13.5/lib/python3.13/site-packages'. Known roots: FileRoots(
...
path: "/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages",
```

Here we have a symlink:

```
/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages 
-> 
../../../../../../lib/python3.13/site-packages
```

---

_Marked ready for review by @sharkdp on 2025-10-23 19:06_

---

_Review requested from @carljm by @sharkdp on 2025-10-23 19:06_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-23 19:06_

---

_Review requested from @sharkdp by @sharkdp on 2025-10-23 19:06_

---

_Review requested from @dcreager by @sharkdp on 2025-10-23 19:06_

---

_Review requested from @MichaReiser by @sharkdp on 2025-10-23 19:06_

---

_Comment by @MichaReiser on 2025-10-23 19:08_

@sharkdp did you intentionally mark this pr as ready for review?

---

_Comment by @sharkdp on 2025-10-23 19:11_

No, sorry. I was reading through it on mobile and must have hit something accidentally. And I can't even find the button to change it back 🙃

By the way, love the SFINAE branch name.

---

_Converted to draft by @AlexWaygood on 2025-10-23 19:24_

---

_Comment by @MichaReiser on 2025-10-25 21:37_

You mentioned over chat if we should return a Result with the errors instead of a safe flag. I'm starting to see the appeal of this, if we try to get as much right as possible even in safe mode

The resolution methods could return a result where the error type wraps the fallback settings and the first (or all errors). A caller could then decide to build the program with the fallback settings (or error). 

I'm not sure how much harder that is

---

_Comment by @Gankra on 2025-10-30 15:30_

We no longer have an input that crashes the default server so I'm going to close this as WONTFIX until someone reports a new one.

---

_Closed by @Gankra on 2025-10-30 15:30_

---
