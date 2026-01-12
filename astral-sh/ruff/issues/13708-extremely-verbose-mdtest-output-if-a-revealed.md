```yaml
number: 13708
title: "Extremely verbose `mdtest` output if a revealed type is not what is expected"
type: issue
state: closed
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
created_at: 2024-10-10T21:16:19Z
updated_at: 2024-10-11T00:33:54Z
url: https://github.com/astral-sh/ruff/issues/13708
synced_at: 2026-01-12T15:54:53Z
```

# Extremely verbose `mdtest` output if a revealed type is not what is expected

---

_@AlexWaygood_

If you have a `# revealed` assertion on a line in an `mdtest`, and the revealed type is not what you are asserting, the output is something like this:

```
  /src/test.py
    line 16: unmatched assertion: revealed: tuple[Literal[A], Literal[B], Literal[C], Literal[A], Literal[object]]
    line 16: unexpected error: [undefined-reveal] "`reveal_type` used without importing it; this is allowed for debugging convenience but will fail at runtime"
    line 16: unexpected error: [revealed-type] "Revealed type is `tuple[Literal[A], Literal[X], Literal[Y], Literal[O], Literal[object]]`"
```

Really I only made a single mistake here: the revealed type is not what I asserted it would be. However, for this single mistake, I have to pay the "fine" of three (long) lines of `mdtest` output!


---

_Label `testing` added by @AlexWaygood on 2024-10-10 21:16_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-10 21:16_

---

_Comment by @AlexWaygood on 2024-10-10 21:16_

(cc. @carljm)

---

_Comment by @carljm on 2024-10-10 21:32_

It is generally the case that if you make an assertion that doesn't match a diagnostic on that line, you will get two "failures" on the test: an unmatched assertion and an unmatched diagnostic. I don't really see a way around this without hiding useful info. To debug a mismatch you likely want to see both things that failed to match each other.

In the case of `revealed: ` we currently only suppress the undefined-reveal diagnostic if we actually match a revealed type. We could relax this and say that any `revealed: ` assertion on a line already shows your intent to use `reveal_type` and should always silence `undefined-reveal` on that line, whether the revealed-type actually matches or no. That seems reasonable to me and would reduce the three lines of output to two lines. 

---

_Comment by @AlexWaygood on 2024-10-10 21:47_

That sounds reasonable to me, too... though I'm still not sure I _fully_ understand why we don't simply suppress all the `unimported-reveal` diagnostics in `mdtest`s. If a contributor writes this in a test:

```py
reveal_type(2)
```

And the test comes back at them with a message telling them

```
    line 16: unexpected error: [undefined-reveal] "`reveal_type` used without importing it; this is allowed for debugging convenience but will fail at runtime"
```

they'll surely think that they need to import `reveal_type` in order to make the test to pass, but that's not true (the error will go away as soon as they add a `# revealed` assertion).

In https://github.com/astral-sh/ruff/pull/13695#discussion_r1795629361 you mentioned that you wanted to emit these diagnostics to be emitted in `mdtest` so that you'd be able to test the diagnostics emitted by `reveal_type` itself, but I'm not sure I see why the tests for `reveal_type` itself _have_ to use `mdtest`. ISTM that a simpler way of doing it would be to have it so that the `unimported-reveal` diagnostics are never emitted in the context of `mdtest`, and the (small number of) tests for `reveal_type` itself are not written using `mdtest`.

---

_Comment by @carljm on 2024-10-10 22:01_

Yes, we could do it that way. The general principle that pushes me away from doing that is to avoid modifying the behavior of the system under test as much as possible, since that leads to misleading test results and can also lead to confusion. The `undefined-reveal` diagnostic is a real diagnostic that red-knot really emits; always silencing it in tests means that the diagnostics you see from red-knot on a snippet of code may be different from the diagnostics you would see from red-knot for that exact same code outside of the test context. That's something to avoid, if possible. Inability to write tests for the `undefined-reveal` diagnostic in mdtest is just a symptom of that problem.

You may be right that contributors will be confused by the `undefined-reveal` diagnostic, and if that does turn out to be a problem, we can revisit. Maybe the specifics of this case outweigh the general principle.

---

_Comment by @AlexWaygood on 2024-10-10 22:06_

Makes sense. I do also find it confusing for me personally, as well as contributors, that I have to remember to ignore these messages telling me to import `reveal_type` ;) But we can leave it for now and wait to see if I'm the only one. This idea of yours sounds great as a first step:

> In the case of `revealed: ` we currently only suppress the undefined-reveal diagnostic if we actually match a revealed type. We could relax this and say that any `revealed: ` assertion on a line already shows your intent to use `reveal_type` and should always silence `undefined-reveal` on that line, whether the revealed-type actually matches or no. That seems reasonable to me and would reduce the three lines of output to two lines.

---

_Comment by @carljm on 2024-10-10 23:41_

Pushed a PR that implements the first step (silencing `undefined-reveal` on any line including a `# revealed: ` assertion), and also tries to balance giving some more useful direction for undefined-reveal in mdtest context, without concealing red-knot's normal diagnostic for undefined-reveal.

---

_Closed by @carljm on 2024-10-11 00:33_

---

_Closed by @carljm on 2024-10-11 00:33_

---
