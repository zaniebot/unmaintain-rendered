```yaml
number: 5907
title: incorrect ranges for constants in joined strings?
type: issue
state: closed
author: davidszotten
labels:
  - bug
  - help wanted
assignees: []
created_at: 2023-07-20T07:43:01Z
updated_at: 2024-01-02T18:54:13Z
url: https://github.com/astral-sh/ruff/issues/5907
synced_at: 2026-01-10T11:09:48Z
```

# incorrect ranges for constants in joined strings?

---

_Issue opened by @davidszotten on 2023-07-20 07:43_

input
```
f"a{b}"
```

ast

```
Module(
    ModModule {
        range: 0..7,
        body: [
            Expr(
                StmtExpr {
                    range: 0..7,
                    value: JoinedStr(
                        ExprJoinedStr {
                            range: 0..7,
                            values: [
                                Constant(
                                    ExprConstant {
                                        range: 0..7,            <---- is this correct (the entire string)? or should it be 2..3 ?
                                        value: Str(
                                            "a",
                                        ),
                                        kind: None,
                                    },
                                ),
                                FormattedValue(
                                    ExprFormattedValue {
                                        range: 0..7,
                                        value: Name(
                                            ExprName {
                                                range: 4..5,
                                                id: "b",
                                                ctx: Load,
                                            },
                                        ),
                                        conversion: None,
                                        format_spec: None,
                                    },
                                ),
                            ],
                        },
                    ),
                },
            ),
        ],
        type_ignores: [],
    },
)
```

---

_Comment by @MichaReiser on 2023-07-20 07:57_

Yeah, this looks wrong. We need to fix this in our RustPython fork.

---

_Label `bug` added by @MichaReiser on 2023-07-20 07:57_

---

_Comment by @MichaReiser on 2023-07-20 08:47_

This should be the relevant code:

https://github.com/astral-sh/RustPython-Parser/blob/2d1f69cbb9e331bd7b29253e7f101b47d92bcbff/parser/src/string.rs#L678-L710

---

_Comment by @MichaReiser on 2023-07-20 15:27_

@davidszotten are you interested in implementing the necessary changes in our parser or would you prefer if I or konsti tackle this?

---

_Label `help wanted` added by @MichaReiser on 2023-07-20 15:28_

---

_Comment by @davidszotten on 2023-07-20 16:15_

i started having a look but i'll admit i'm struggling a bit. i guess i need to first fix the ranges e.g. in https://github.com/astral-sh/RustPython-Parser/blob/2d1f69cbb9e331bd7b29253e7f101b47d92bcbff/parser/src/string.rs#L480 and then extract/track those to use in `take_current`?

---

_Comment by @MichaReiser on 2023-07-21 06:57_

> i started having a look but i'll admit i'm struggling a bit. i guess i need to first fix the ranges e.g. in [astral-sh/RustPython-Parser@`2d1f69c`/parser/src/string.rs#L480](https://github.com/astral-sh/RustPython-Parser/blob/2d1f69cbb9e331bd7b29253e7f101b47d92bcbff/parser/src/string.rs#L480) and then extract/track those to use in `take_current`?

Could be. I haven't worked on this parser part before, but I took a quick look. I ended up having the same experience as you... the code is kind of hard to understand. I would need to do some debugging myself to familiarize myself with the code. Let me know if you would find this useful or if you want to spend some more time yourself figuring this out.

What I noticed is that the `range` function

https://github.com/astral-sh/RustPython-Parser/blob/2d1f69cbb9e331bd7b29253e7f101b47d92bcbff/parser/src/string.rs#L75-L77

Returns the range from `self.start..self.end`. Maybe it is sufficient to change it to `self.location..self.end`?

---

_Comment by @davidszotten on 2023-07-21 07:53_

> Maybe it is sufficient to change it to self.location..self.end?

i don't think that does the right thing


i got something reasonable working last night i think and raised a pr

---

_Comment by @MichaReiser on 2023-07-21 09:19_

https://github.com/astral-sh/RustPython-Parser/pull/33

---

_Comment by @davidszotten on 2023-07-21 10:20_

> [astral-sh/RustPython-Parser#33](https://github.com/astral-sh/RustPython-Parser/pull/33)

sorry for not explicitly posting it, thought the back references github add were enough. will be more explicit in the future

---

_Comment by @charliermarsh on 2024-01-02 18:54_

I think this is now fixed (cc @dhruvmanila).

---

_Closed by @charliermarsh on 2024-01-02 18:54_

---
