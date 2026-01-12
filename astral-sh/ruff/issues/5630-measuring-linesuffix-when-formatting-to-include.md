```yaml
number: 5630
title: "Measuring `LineSuffix` when formatting to include trailing comments"
type: issue
state: closed
author: cnpryer
labels:
  - formatter
assignees: []
created_at: 2023-07-09T16:32:56Z
updated_at: 2023-08-31T14:37:45Z
url: https://github.com/astral-sh/ruff/issues/5630
synced_at: 2026-01-12T15:54:45Z
```

# Measuring `LineSuffix` when formatting to include trailing comments

---

_@cnpryer_

In #5169 I'm playing with test cases and noticed `tuple`s with trailing comments exceeding line-length don't get formatted.

I added this to the `ruff` fixtures for `tuple` formatting:
```py
# Trailing comment should force format
i1 = ("aasdsdasd", "aasdsdasd", "aasdsdasd", "aasdsdasd", "aasdsdasd", "aasdsdasd")  # Trailing
```

With `black` you'll get:
```py
# Trailing comment should force format
i1 = (
    "aasdsdasd",
    "aasdsdasd",
    "aasdsdasd",
    "aasdsdasd",
    "aasdsdasd",
    "aasdsdasd",
)  # Trailing
```

Here's the modified snapshot results:
```
Snapshot file: crates/ruff_python_formatter/tests/snapshots/format@expression__tuple.py.snap
Snapshot: format@expression/tuple.py
Source: crates/ruff_python_formatter/tests/fixtures.rs:151
Input file: crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/tuple.py
────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────
  258   258 │     "qweiurpoiqwurepqiurpqirpuqoiwrupqoirupqoirupqoiurpqiorupwqiourpqurpqurpqurpqurpqurpqurüqurqpuriq",
  259   259 │ )
  260   260 │ 
  261   261 │ # Trailing comment should force format
  262       │-i1 = (
  263       │-    "aasdsdasd",
  264       │-    "aasdsdasd",
  265       │-    "aasdsdasd",
  266       │-    "aasdsdasd",
  267       │-    "aasdsdasd",
  268       │-    "aasdsdasd",
  269       │-)  # Trailing
        262 │+i1 = ("aasdsdasd", "aasdsdasd", "aasdsdasd", "aasdsdasd", "aasdsdasd", "aasdsdasd")  # Trailing
  270   263 │ ```
```

If I can scoop this up in #5169 I'll submit a separate PR, but I figured I could start by documenting this here.

<br>

**UPDATE**: Looking into it more this looks like a general trailing comment issue rather than specifically a `tuple` issue. You can repro with `dict` using:

```py
d2 = {"a": 1000, "b": 1000, "c": 1000, "d": 1000, "e": 1000, "f": 1000, "g": 1000}  # Trailing
```

---

_Label `formatter` added by @MichaReiser on 2023-07-09 18:09_

---

_Comment by @MichaReiser on 2023-07-09 18:14_

Yes, this is a generic problem applying to all trailing comments. The reason is that the `Printer` doesn't measure the content of a `LineSuffix`. This is mainly to keep the implementation simpler because it would otherwise be necessary to not only track the current position (needed for source maps), but also the line width with trailing comments included. 

I don't mind changing the implementation, depending on its performance impact. But I think that this can also be a reasonable deviation. The relevant line where we skip over line suffixes is here:

https://github.com/astral-sh/ruff/blob/56bae34f09acec3963503955693cd393d6555359/crates/ruff_formatter/src/printer/mod.rs#L218-L220

I would probably add a new `line_suffix_width` state-variable and compute the width of the content on the above-mentioned lines. We can then create a new `width()` method on the `Printer` that returns the width (current line position + line suffix width).

The main challenge is how to compute the width without actually printing the content. I guess, we could call into fits and then take the width from there but that feels like a hack. 


---

_Renamed from "Formatter fails to format `tuple`s with trailing comments exceeding line-length" to "Measuring `LineSuffix` when formatting to include trailing comments" by @cnpryer on 2023-07-16 02:11_

---

_Comment by @charliermarsh on 2023-08-06 16:30_

Does it simplify the problem at all if we constrain the kinds of nodes that can be printed as line suffixes? (Do we ever need line suffix apart from for comments?)

---

_Comment by @charliermarsh on 2023-08-06 16:35_

I can answer those questions myself. The bigger question is probably whether we want to deviate from Black here or fix this.

---

_Comment by @cnpryer on 2023-08-06 17:09_

> The bigger question is probably whether we want to deviate from Black here or fix this.

I think linting influences some of my formatting decisions. So if my understanding is that line-length violations *include* trailing comments as part of the line suffix calculation, then naturally I'll probably expect the formatting to align with that constraint.

In other words, I currently lean towards how `black` handles it.

---

_Comment by @MichaReiser on 2023-08-07 06:47_

> I think linting influences some of my formatting decisions. So if my understanding is that line-length violations include trailing comments as part of the line suffix calculation, then naturally I'll probably expect the formatting to align with that constraint.

That makes sense to me. But this raises the question of whether the formatting must be consistent with all lint rules Ruff supports. I don't think this is a desired property of the formatter because it would limit our options when choosing a formatting, because we could only design for the least common denominator. 

I'm leaning towards automatically disabling rules that we know conflict with the formatter (you shouldn't need formatting-related rules if you use our formatter). 


> Does it simplify the problem at all if we constrain the kinds of nodes that can be printed as line suffixes? (Do we ever need line suffix apart from for comments?)

The question is, what are comments ;) We need to support text and hard line breaks at least, possibly labels too. I don't see this as a high risk issue. I'm confident that we can support it and would prefer to delay working on it until we're further along with the formatter. However, we'll have to be extra careful to:

* ideally: Only pay the additional overhead for line suffixes (don't introduce a new code path that must be executed for each FormatElement)
* The change is in line with the semantics of all other IR elements (My main concern is `MeasureMode::AllLines`



---

_Comment by @MichaReiser on 2023-08-17 10:21_

Related black issues

* https://github.com/psf/black/issues/1713
* https://github.com/psf/black/issues/1125
* https://github.com/psf/black/issues/3450
* https://github.com/psf/black/issues/3329

Prettier issue:
* https://github.com/prettier/prettier/issues/4754

---

_Comment by @MichaReiser on 2023-08-17 13:26_

Regarding the consistency with the lint rule issue. I would recommend disabling the rule if someone uses our formatter, similar to what Prettier's eslint plugin does. There are cases where it is known that Black doesn't break your line (it may even not be allowed to break in these positions) and requiring a suppression comment in that case only makes things worse (to add on top of that, the formatter might move your comment, because the line is too long :laughing:)

---

_Comment by @MichaReiser on 2023-08-17 14:36_

One good counter example of why Ruff should respect the line width that @zanieb made:

```python
if (
    first_test() 
    and not settings.TEST_SUITE
):  # nocoverage: We can't verify the signature in test suite since we fetch the events
    pass
```

If I deliberately formatted the comment on its own line for better readability, then the formatter shouldn't collapse the line and make the comment exceed the line width.

---

_Comment by @MichaReiser on 2023-08-17 14:39_

One "simple" implementation idea: Add a `reserved_width` field to `LineSuffix` and add that width to `printer.state.line_width`. This means its up to the line suffix user to decide whether the line suffix should count to the line width or not. The downside to this is that the client code needs to count the characters and use `options.tab_width()` (doesn't exist today) for the tab space.

@cnpryer would you be interested in implementing this?

---

_Comment by @MichaReiser on 2023-08-17 15:20_

I promise I'll stop my spam after this.

After discussing this internally. We want to align with black because we believe that respecting the line width improves the readability of code, this includes comments. 

There are examples of where Black/Ruff split the line weirdly if it has a long trailing comments. Comments may more likely trigger these problems but aren't limited to them. Let's take the subscript example:

```python
a[0] # a very long comment that exceeds the line width and makes the subscript split eventually

# Black
a[
    0
]  # a very long comment that exceeds the line width and makes the subscript split eventually
```

This is worse formatting, in my view, but it isn't specific to comments:

```python
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[4] 

# Ruff
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[
    4
]
```

It's less likely that you trigger the second case, but the proper solution IMO is to **never** break subscripts if indexing by a number literal. This solves the odd formatting in both cases. 



---

_Comment by @cnpryer on 2023-08-17 16:25_

> @cnpryer would you be interested in implementing this?

I would, but I want to spend some time today looking at this again (and thinking about it) before I claim it. I've got several things I'm trying to juggle right now, so my main concern is that causing any delay on the formatter's progress.

Off the bat I'm struggling to wrap my head around the client code impact (needing `options.tab_width()`), but I'm sure it'll make more sense after digging around.

> After discussing this internally. We want to align with black because we believe that respecting the line width improves the readability of code, this includes comments.

Are you thinking about tossing this in the Alpha bucket? Or would it be Beta territory?

---

_Comment by @cnpryer on 2023-08-17 16:27_

Ah I see it's in Alpha tracked by #6069

---

_Comment by @MichaReiser on 2023-08-17 16:30_

> Are you thinking about tossing this in the Alpha bucket? Or would it be Beta territory?

Don't feel pressured by it. It's ready when it's ready. 

> Off the bat I'm struggling to wrap my head around the client code impact (needing options.tab_width()), but I'm sure it'll make more sense after digging around.

The client code will need to compute the width of a comment. This means we'll have to duplicate some (or all of) 

https://github.com/astral-sh/ruff/blob/910dbbd9b605eb98a27b06611544fa2de11a1256/crates/ruff_formatter/src/printer/mod.rs#L1278-L1298

Specifically, the width of a tab is configurable (is a tab 2 or 4 spaces)?. 

> I would, but I want to spend some time today looking at this again (and thinking about it) before I claim it. I've got several things I'm trying to juggle right now, so my main concern is that causing any delay on the formatter's progress.

Totally up to you. I tagged you because you showed interest in working on the Printer, but I also don't want to pressure you. 

---

_Comment by @MichaReiser on 2023-08-17 18:48_

Okay, I lied... I have one other observation to share. [Pyink](https://github.com/google/pyink) excludes trailing pragma comments from the computed width. What's nice about the IR change we've been discussing is that we could support this as well, by simply setting the width to 0 if it is a pragma comment.

---

_Comment by @cnpryer on 2023-08-18 11:55_

Started looking into it this morning. I'd say leave this as unassigned in case someone wants to leapfrog me. At some point I'll have more time, and when that happens I'd be more comfortable claiming issues.

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:13_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:32_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:32_

---

_Comment by @cnpryer on 2023-08-23 17:19_

Just wanted to provide some kind of update. Been working on this in small bursts unfortunately (caught up with some other stuff). [Here](https://github.com/cnpryer/ruff/tree/line-suffix) is my branch so far. I noticed some pragma activity so I wanted to provide whatever I can to help those efforts.

Don't let me hold you up though. I'll open up a PR if I feel like I can finish this.

---

_Closed by @MichaReiser on 2023-08-28 05:46_

---

_Comment by @davidszotten on 2023-08-31 07:40_

i was expecting this to also fix (example from django)

(`-ruff +black`)

```diff
-UNUSABLE_PASSWORD_SUFFIX_LENGTH = 40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
+UNUSABLE_PASSWORD_SUFFIX_LENGTH = (
+    40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
+)
```

did i misunderstand something?

---

_Comment by @MichaReiser on 2023-08-31 07:45_

> i was expecting this to also fix (example from django)
> 
> (`-ruff +black`)
> 
> ```diff
> -UNUSABLE_PASSWORD_SUFFIX_LENGTH = 40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
> +UNUSABLE_PASSWORD_SUFFIX_LENGTH = (
> +    40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
> +)
> ```
> 
> did i misunderstand something?


Yes and no. This is related to https://github.com/astral-sh/ruff/pull/6872 The current implementation uses an heuristic of when to use this layout because it is expensive. 

https://github.com/astral-sh/ruff/blob/fc89976c24d1c8b9d13d88ef70b619e8a2ac393a/crates/ruff_python_formatter/src/expression/parentheses.rs#L38-L50

We could extend the heuristic to take trailing end of line comments into consideration (use it if the node has any trailing end of line comments).

---

_Comment by @davidszotten on 2023-08-31 12:49_

thanks for the explanation. your suggestion seems worth a try so started having a look. however, e.g. in the example above we call `should_use_best_fit` with the `ExprConstant` but the comments is attached to the `StmtAssign`. is there some nice way to find the comment from the constant?

---

_Comment by @MichaReiser on 2023-08-31 12:56_

Hmm good point. You get the parent in `NeedsParentheses`, but not sure if that will work reliably. 

---

_Comment by @davidszotten on 2023-08-31 13:22_

hm. parent improves the django example but doesn't quite fix it

```diff
 UNUSABLE_PASSWORD_SUFFIX_LENGTH = (
-    40
-)  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
+    40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
+)
```

we now end up moving the comment

---

_Comment by @MichaReiser on 2023-08-31 13:36_

Do I understand correctly that it now gets moved inside of the parentheses? That's rather unexpected.

---

_Comment by @davidszotten on 2023-08-31 14:24_

sorry for being unclear. it's the other way around

input:

```
UNUSABLE_PASSWORD_SUFFIX_LENGTH = 40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
```

black

```
UNUSABLE_PASSWORD_SUFFIX_LENGTH = (
    40  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
)
```

ruff:

```
UNUSABLE_PASSWORD_SUFFIX_LENGTH = (
    40
)  # number of random chars to add after UNUSABLE_PASSWORD_PREFIX
```


oh, and starting with the black version as input, my ruff impl is unstable :(

---

_Comment by @davidszotten on 2023-08-31 14:37_

made a pr so we have code to discuss and a better place for this discussion https://github.com/astral-sh/ruff/pull/7023

---
