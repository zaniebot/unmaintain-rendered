```yaml
number: 228
title: "panics found by the `py-fuzzer` fuzzer"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2024-12-02T17:27:29Z
updated_at: 2025-12-07T15:22:49Z
url: https://github.com/astral-sh/ty/issues/228
synced_at: 2026-01-10T01:56:40Z
```

# panics found by the `py-fuzzer` fuzzer

---

_Issue opened by @AlexWaygood on 2024-12-02 17:27_

The following code snippets surfaced by the `py-fuzzer` script all produce red-knot panics. Some of these may be duplicates in terms of their syntax structure; I haven't tried hard to deduplicate them. I've only gone through panics found in the first 500 seeds.

To run red-knot on all failing seeds in the first 500 seeds, you can run the following command from the Ruff repository root:

```
uvx --from ./python/py-fuzzer fuzz --bin=red_knot 0 5 16 19 25 28 30 36 39 57 76 81 91 100 102 107 110 115 116 119 120 125 127 138 141 147 158 168 174 175 224 234 247 255 260 261 282 294 295 311 313 321 332 344 347 348 350 351 353 361 374 382 386 391 395 397 400 409 414 451 452 454 455 463 473 488 492 494 497
```

They are all (or at least _should_ be) valid Python syntax.

## Failing seeds

**Seeds 39, 25, 19, 107, 138, 110, 115, 120, 125, 116, 147, 174, 175, 260, 294, 313, 261, 347, 282, 350, 391, 386, 374, 414, 409, 451, 455, 497**

The panics in these seeds all boil down to recursive type aliases of the form

```py
type name = name
```

**Seed 28**

```py
class name_5[**name_4](name_5.name_5):
    pass
```

**Seed 30**

```py
async def name_5[name_2](name_5: (name_1 and name_5)(), /):
    pass
```

**Seed 0**

```py
type name_5 = name_2
type name_2 = name_5
```

**Seed 76**

```py
class name_4[**name_5](name_3, name_0=name_4.name_3):
    pass
```

**Seed 81**

```py
class name_4[**name_2](name_4.name_1):
    pass
```

**Seed 57**

```py
class name_4[name_5]({}, name_3=name_4.name_1):
    pass
```

**Seed 91**

```py
type name_2 = name_3
(name_3 := name_2)
```

**Seed 100**

```py
type name_5 = name_0
(name_0 := name_5)
```

**Seed 168**

```py
class name_0[name_5](name_0.name_5):
    pass
```

**Seed 158**

```py
async def name_4[*name_5](name_5: name_4(), /):
    pass
```

**Seed 224**

```py
class name_2[name_5](name_2.name_4):
    pass
```

**Seed 247**

```py
type name_3 = name_4
(name_0 := name_3)
(name_4 := name_0)
```

**Seed 255**

```py
class name_5[**name_3](name_5.name_3):
    pass
```

**Seed 234**

```py
def name_2[*name_0](*, name_4: name_4):
    pass
type name_4 = name_2()
```

**Seed 332**

```py
class name_1[**name_0](name_1()()):
    pass
```

**Seed 361**

```py
async def name_0[*name_5](name_4: name_0()):
    pass
```

Seed 351

```py
class name_3[*name_1](something, **name_3.name_2):
    pass
```

**Seed 344**

```py
type name_3 = name_4()

def name_4[*name_0](*, name_3: name_3):
    pass
```

**Seed 311**

```py
class name_1[**name_4](name_1, name_1=name_5):
    pass
match name_2:
    case None:
        try:
            pass
        except name_1 as name_5:
            pass
    case []:
        type name_5 = name_1
```

**Seed 400**

```py
class name_4[*name_2](name_5 if name_4() else name_4):
    pass
```

**Seed 382**

```py
class name_1[name_1](name_4, name_1=name_3.name_2):
    pass
(name_3 := name_1)
```

**Seed 395**

```py
class name_5[name_1](*name_5):
    pass
```

**Seed 397**

```py
class name_0[name_4](name_0[name_1]):
    pass
```

**Seed 488**

```py
class name_1[name_5](name_1.name_5):
    pass
```

**Seed 473**

```py
async def name_2[*name_4](*, name_5: name_3()):
    pass
(name_3 := name_2)
```

**Seed 492**

```py
class name_0[name_5](name_0.name_3):
    pass
```

**Seed 454**

```py
type name_5 = name_0
name_0 = name_5
```

**Seed 463**

```py
def name_3[**name_2](*, name_3: name_4):
    pass
type name_4 = name_3()
```

Also failing (but pysource-minimize can't minimize the panics in these for some reason, so the repros are too much text to paste into this GitHub issue) are the following seeds:
- Seed 5
- Seed 16
- Seed 36
- Seed 102
- Seed 119
- Seed 127
- Seed 141
- Seed 295
- Seed 321
- Seed 348
- Seed 353
- Seed 452
- Seed 494

---

_Label `bug` added by @AlexWaygood on 2024-12-02 17:27_

---

_Comment by @sharkdp on 2024-12-02 19:26_

Thanks @AlexWaygood. I skimmed through these examples and I think they are almost all related to known issues:

* astral-sh/ruff#14672: recursive and self-referential (circular) type aliases
* astral-sh/ruff#14333 and and the two known failures here:
  https://github.com/astral-sh/ruff/blob/5137fcc9c81610f687b6cb45413ef83c2c5eea73/crates/red_knot_workspace/tests/check.rs#L267-L270

What's new to me are some examples with self-referential/cyclic *function* definitions, but given that we have the same problem with classes and type aliases, this is not a big surprise 😄.

I think the value of the fuzzer will be much bigger after we solved those three issues (it sounds like maybe it's just one big issue with self-referential definitions). Do you think it makes sense to keep this ticket open?

---

_Comment by @AlexWaygood on 2024-12-02 22:40_

> I think the value of the fuzzer will be much bigger after we solved those three issues (it sounds like maybe it's just one big issue with self-referential definitions). Do you think it makes sense to keep this ticket open?

Agreed. I think I'd like to keep this issue open for now, though. I'd like to eventually run this fuzzer on red-knot in CI like we already do with the parser, so it's useful to have something to track that. I'm also not 100% sure if _all_ these panics are to do with self-referential definitions -- the ones that pysource-minimize is failing to minimize are somewhat mysterious (not sure what's going on there). And it's also _possible_ that on some of these seeds there are actually multiple panics, and the self-referential definitions are obscuring other issues that also exist.

---

_Assigned to @carljm by @carljm on 2025-03-01 17:26_

---

_Comment by @carljm on 2025-03-12 12:55_

After merging astral-sh/ruff#14029, we have these seeds still failing: `0 28 57 76 107 120 168 175 260 311 313 332 351 382 391 451 488 492 494` (reduction from 69 failing seeds to 18, out of the first 500). I think some of these are still cycles, where we need to add cycle handling to some more queries. Some may be other issues.

---

_Comment by @carljm on 2025-03-13 01:04_

With astral-sh/ruff#16693 we are down to these 15 failing seeds: `0 28 57 107 120 175 260 311 332 351 382 391 451 488 492`.

---

_Renamed from "red-knot panics found by the `py-fuzzer` fuzzer" to "[red-knot] panics found by the `py-fuzzer` fuzzer" by @carljm on 2025-03-27 19:03_

---

_Renamed from "[red-knot] panics found by the `py-fuzzer` fuzzer" to "panics found by the `py-fuzzer` fuzzer" by @MichaReiser on 2025-05-07 15:27_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 16:00_

---

_Label `fatal` added by @AlexWaygood on 2025-05-10 20:01_

---

_Removed from milestone `Alpha` by @MichaReiser on 2025-05-16 12:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-16 12:19_

---

_Comment by @carljm on 2025-08-15 01:13_

We are now down to just one failing seed in the 0-500 range.

---

_Comment by @AlexWaygood on 2025-10-17 11:31_

The sole remaining panic in seeds 0-500 is this:

```py
class name_0[name_5: name_0[name_5]]:
    pass
```

which is an instance of #256. Let's keep this issue open, so that we can track adding a randomised fuzzer job to CI (rather than one that just checks whether a PR introduces any _new_ panics in seeds 0-500). But I think this no longer needs to be considered a distinct beta-blocker issue to #256; I'm removing it from the Beta milestone.

---

_Removed from milestone `Beta` by @AlexWaygood on 2025-10-17 11:31_

---

_Comment by @AlexWaygood on 2025-10-18 14:48_

Note that https://github.com/astral-sh/ruff/pull/20957 changes the file contents that each seed corresponds to: running the fuzzer on Python 3.14 (as we do in CI), there are now no longer any seeds in the 0-500 range that trigger any ty panics. We still panic on the snippet in https://github.com/astral-sh/ty/issues/228#issuecomment-3415120659, however, and the fuzzer is still occasionally turning up snippets we panic on (they're just rarer than before). For example, we panic on seed 692 when the fuzzer is run with Python 3.14, which is:

```py
match 0:
    case name_1.name_1 | 0:

        class name_5[name_2](0, name_1=name_1):
            pass
for unique_name_0 in 0:
    type name_5 = 0

async def name_5():
    pass
while name_5:

    def name_1(name_2=name_5):
        pass
```

---

_Comment by @carljm on 2025-10-30 15:06_

I'm inclined to close this issue (or at least de-prioritize it and put it in the backlog). Running the fuzzer in CI is useful to avoid regressions, but fuzzer-only panics are always lower priority than panics that are actually appearing in user projects, so I don't expect we'll prioritize this anytime soon.

---

_Comment by @AlexWaygood on 2025-10-30 15:08_

I agree it should be deprioritised; that's why I already removed it from the beta milestone :-)

---

_Comment by @carljm on 2025-10-30 15:14_

Yeah, I only noticed that after I made my comment 😆 It came to my attention because it was still in the in-progress state and I hadn't noticed it was gone from the beta.

---

_Unassigned @carljm by @carljm on 2025-11-03 01:52_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 15:46_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:08_

---

_Comment by @AlexWaygood on 2025-12-07 15:22_

Since all the original panics discovered here have now been fixed, I'm going to close this as completed now and open a followup issue for randomized fuzzing in CI

---

_Closed by @AlexWaygood on 2025-12-07 15:22_

---
