```yaml
number: 1836
title: Reconsider imports as type declarations
type: issue
state: open
author: zsol
labels: []
assignees: []
created_at: 2025-12-10T10:34:31Z
updated_at: 2026-01-04T20:10:58Z
url: https://github.com/astral-sh/ty/issues/1836
synced_at: 2026-01-12T15:54:25Z
```

# Reconsider imports as type declarations

---

_@zsol_

### Summary

When a `with` statement binds to a name that has already been declared, an `invalid-assignment` error is raised (as expected?), but future references to the name are associated with the declared type, not the type that the context manager yields.

A simple repro: https://play.ty.dev/6a08fc89-50e6-4de9-bd6d-b9f9e52412dd

### Version

0.0.1-alpha.27

---

_Comment by @carljm on 2025-12-10 17:51_

Hmm, I think our choice to privilege already-declared types over invalid assignments matches all existing type checkers ([pyright](https://pyright-play.net/?code=B4LgBAzgLgTgsAKGGAvGAjImBTAbtgQwBsB9KATwAdsAKYASiA), [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=49aea6c60cb5a0f4616c8905e3d1cbcd), [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSI%2BiABHAC4BOAOuvtQLzUCMLDMAbjFRQA%2BnVLEYACnwBKFiAA0IAK51ocEuUQgAxNQCq6qBHHUwK9AGN1udHBYtMMMOdwMAtqjoj0Kj9gwDDI0EOh0stQAtAB8tIyILNTJ1Hx0Kgzo5kwgAHL%2BgQw0wPgAvjkKymR8YFCkhHS4HlAU%2BgAKpDV1tBg4BNRWdpAA5hneEHaELPoAyjAw1AAWdHTEcIgA9BvVLnWE7sMbMOgbmLhWcBuD6CNjtiduDNSo-KjQqNiwA0MQowzjdmouGI900LDIdEWdiiggYcAmWU4OQAzIQuAAmCroEClZSoGwQQQAMWgMAoaCweCIZBxQA)), including in the case of a context manager specifically ([pyright](https://pyright-play.net/?code=B4LgBAzgLgTmC8YBEAzA9mpBYAULgxgDYCGEEYAwlKLmHWACYCmKYA%2Bm0wHZRMwcAKCE0IoAlGAC0APjABLHiFr0VMJlACuMLmACMyus1YcmwOVEHDRAGjAAqYjADmEW3bsBrAO6OXEmWAARhiESjgqqupaOgAqMBpMuLhe5gAWlNQCEqRgNOH0agBuTMSEbFAAngAOTALAYkA), [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=734a49f521469f8c8f39472ffa26aed3), [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSI%2BiABHAC4BO1AvNQDri64dvq8DGUVHDjUAwnSq9qM6phhhqAfSUx0dGAxUAKODChgAlNQC0APmoR1iabLsMYdAK4N01AIy2Z8xSpj4IOh09AwAaagAqVAYAczhwiIiAawB3aLjjc2o8XCgbNztZB2dXagAVBicYXl4UwIALcUltY2FqKQKZBwA3GFQoJTpSYhhtfENeEFCQJzpoOBJyRBAAYmoAVTmoQNJqMCd0fjncdDgarAU93AYAW1Qg9Ccb7E0xmis6TIt6Bnz7RxcbjAHAAck8Xr9qMB8ABfHjoKYgMgOMBQUiEOi4G5QChrAAKpBRaNoGBwBGo-BOkBiLnuEBOhF4awAyjAYNR6nQ6MQ4IgAPR85EKNGEa4xPlqPmYXD8OB8ynoam047oPlXJiobqoaCobCwClUiA0hh0k7UXDEFULXhkOj1E4mXoMOD0tysDgAZkI7gATPCQDDpqgjhBegAxaAwChoLB4IhkANAA)).

What we probably do differently, that caused us to behave differently in your example, is specifically that we consider an import to "declare" a type; I think other type checkers don't do that. So if anything, that might be the thing we want to re-evaluate.

I think this is a questionable decision on our part -- an import is not really a declaration. I think we did it to solve some issues with re-exports, but we could check what the impact would be of reversing that decision.

---

_Renamed from "Name shadowing due to context manager assignment misbehaves" to "Reconsider imports as type declarations?" by @carljm on 2025-12-10 17:51_

---

_Added to milestone `Stable` by @carljm on 2025-12-10 17:51_

---

_Renamed from "Reconsider imports as type declarations?" to "Reconsider imports as type declarations" by @carljm on 2026-01-04 20:10_

---
