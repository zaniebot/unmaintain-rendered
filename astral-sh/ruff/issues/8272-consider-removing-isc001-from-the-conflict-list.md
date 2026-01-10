```yaml
number: 8272
title: "Consider removing `ISC001` from the conflict list with ruff format"
type: issue
state: closed
author: yakMM
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-10-27T07:20:44Z
updated_at: 2025-01-09T17:10:09Z
url: https://github.com/astral-sh/ruff/issues/8272
synced_at: 2026-01-10T11:09:50Z
```

# Consider removing `ISC001` from the conflict list with ruff format

---

_Issue opened by @yakMM on 2023-10-27 07:20_

Reminder on the rule: [Checks for implicitly concatenated strings on a single line.](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/)

Why I believe there is actually no conflict with ruff format on default settings:

Imagine a situation where the formatter puts two strings on the same line:
`foo = "foo" "bar"`

If the rule is activated (and a fix available), the line will become:
`foo = "foobar"`

Which will not be further modified by ruff format.

However, if the rule is not activated, it will stay like:
`foo = "foo" "bar"`

I assume some people will want to be warned about this appearing because of the formatter and will want to fix it, hence `ISC001` being useful alongside ruff format.

Am I missing something on the incompatibility between this rule and ruff format?

---

_Label `formatter` added by @MichaReiser on 2023-10-27 07:47_

---

_Label `needs-decision` added by @MichaReiser on 2023-10-27 07:47_

---

_Comment by @MichaReiser on 2023-10-27 07:58_

Thanks for reporting and opening a new issue. 

I think we didn't make an explicit decision, but we merged the improvements we had to include them in 0.1.3

Rules and options that are compatible with the formatter should have the following properties:

1. Formatting the code doesn't introduce new violations; to prevent that, it is necessary to run `check --fix`, `format`, `check --fix`, ... until you get a stable result. This especially becomes important when we integrate `format` into `check` (and `check --fix`)
1. Formatting doesn't undo an intent that you explicitly expressed. For example, `split-on-trailing-comma=true`conflicts with `format.skip-magic-trailing-comma=false` because the formatter would remove the trailing comma, undoing your expressed intent that the aliases should be wrapped. This is an extension of 1, but I like to list it separately because it is not just incompatible with the rule, it does not uphold the expressed intent of users.


I do see how `ISC001` is useful in combination with the formatter, and it doesn't violate the 2nd property, but it does violate the first property (so does `E501`, so maybe I shouldn't have removed that one). 

I don't have a good answer yet about what we should do about these rules. Warning seems appropriate because you can run into situations where you must run `ruff check --fix` a second time after running `ruff format`. 



---

_Comment by @yakMM on 2023-10-27 09:15_

I fully agree with the intent `2.`, no doubt about it.

I'm mixed about `1.` It is easily fixed if you run the formatter before the linter, thing that was mandatory at the time of `black` and `flake8`: the formatter would preemptively fix some code that would otherwise be a violation for the linter (lines too long for example, would be fixed by the formatter).

I get that this way of thinking is a bit changed because of the `--fix` flag, but still I believe it's more appropriate to have a pipeline with formatting first and linter as second, as you don't expect `check --fix` to "break the format" of some code, unless you have actual contradictions within your rules (breaking point `2.`)

---

_Comment by @yakMM on 2023-10-27 09:18_

If we imagine a world in which `format` and `check` are combined, I believe we can expect `format + fix` according to the rules, and the the remaining violations are displayed to the user. In this context, having `ISC001` on would not bring any contradiction, it would just specify "format my code and make sure strings are concatenated", which is exactly what I want to express in my pipeline, by running `format` and then `check` with ` ISC001`

---

_Comment by @yakMM on 2023-10-27 09:19_

Anyways, thank you very much for the openness and allowing discussions about all this, as well as the quick iterative fixes 

---

_Comment by @MichaReiser on 2023-10-27 09:19_

> I get that this way of thinking is a bit changed because of the --fix flag, but still I believe it's more appropriate to have a pipeline with formatting first and linter as second, as you don't expect check --fix to "break the format" of some code, unless you have actual contradictions within your rules (breaking point 2.)

Unfortunately, this is not guaranteed. Applying an autofixes may produce code that now exceeds the preferred line width or the formatter can now collapse multiple lines. That means, it is generally necessary to run the formatter after linting. 

---

_Comment by @yakMM on 2023-10-27 09:28_

> > I get that this way of thinking is a bit changed because of the --fix flag, but still I believe it's more appropriate to have a pipeline with formatting first and linter as second, as you don't expect check --fix to "break the format" of some code, unless you have actual contradictions within your rules (breaking point 2.)
> 
> Unfortunately, this is not guaranteed. Applying an autofixes may produce code that now exceeds the preferred line width or the formatter can now collapse multiple lines. That means, it is generally necessary to run the formatter after linting.

Oh really? I'm surprised as rules like [SIM108](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/) do not trigger if there is no place for the fix within the specified line-length. Maybe it depends on the rules then

---

_Comment by @MichaReiser on 2023-10-27 09:32_

Yeah, some rules have this check, but not all of them. However, all fixes that remove (or change code) may reduce the length of lines, possibly causing reformats.

---

_Comment by @yakMM on 2023-10-29 08:32_

> Yeah, some rules have this check, but not all of them. However, all fixes that remove (or change code) may reduce the length of lines, possibly causing reformats.

Ah okay I get it. It would be best if the fixes of `ruff check --fix` would be compliant with the formatter settings imo, but I see the ruff approach is more to run `format` after `check`.

In that case should I go back advocating for a way to suppress warning about rules breaking point 1?

My use case is to be warned about `implicitly concatenated strings on a single line` without warnings ideally.

---

_Comment by @tylerlaprade on 2023-12-12 15:45_

I really don't want warnings every time I run `ruff format`. I might personally know that it's safe to ignore, but this is bound to confuse more junior members of my team. For reference, when I previously pre-emptively included `FURB` in our config so it would be enabled as soon as it was out of preview, they saw the warnings about `--preview` when running `ruff format`, thought they had to run `ruff format --preview` instead, and ended up with >80 changed files in an otherwise tiny PR.

This rule is not incompatible with the formatter in the same way as `ISC003` or `E501`. Moreover, disabling this rule would easily allow bugs to creep in, because it's never the case that someone intentionally concatenates two strings in a line without an f-string or `+` sign.

---

_Comment by @MichaReiser on 2023-12-14 04:57_

I hear your feedback and I see how `ISC001` is useful even when used with the formatter. The issue is on my to do list I just haven't gotten around to look at it in more detail. 

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-12-14 04:57_

---

_Comment by @arthur-st on 2023-12-22 10:39_

@tylerlaprade What do you mean under `ISC003` incompatibility with the formatter? Doesn't seem to be incompatible as per the docs or the formatter behaviour, at least.

---

_Comment by @MichaReiser on 2024-01-10 14:36_

One option for addressing this issue is to change our formatter, see https://github.com/astral-sh/ruff/issues/9457. However, there's a caveat. The new formatting would be non-reversible and I'm undecided if we should accept that or wait until we implement Black's string processing preview style.

Please comment on the linked issue if you would object to the formatter making this change automatically for you.

---

_Comment by @steve-mavens on 2024-04-25 10:18_

I don't know if this is useful to anyone, but my current local lint script runs:

```
ruff . --fix
ruff format .
ruff . --extend-select ISC001
```

If the final check fails then I assume manual intervention will be required to make sure my check/format config is compatible, or  perhaps to manually reformat some bit of code that's triggering incompatibilities. In some cases re-runs without intervention could reach a stable state.

Is this the expected usage of ruff? If so then it does feel like ruff itself could provide that workflow in a combined "fix my code using all my config" command. Currently it feels like `ruff check` and `ruff format` are two separate tools that happen to be in the same executable, with a bit of work left to do meet the potential goal of bringing black formatting into ruff "proper", in the way that (for example) isort formatting already has been. Which is fine, because both ruff commands work really well, it just requires a bit of research to figure out how to run ruff at the moment.


---

_Comment by @mscheifer on 2024-05-07 01:57_

Another option could be a way to disable the warning per rule like:
```toml
[tool.ruff.format]
disable-rule-conflict-warning = ["ICS001"]
```
That way I'm explicitly stating that I understand that I might have to run the formatter and `check --fix` back and forth, and maybe do some manual intervention even, to get it to all pass, for this rule specifically.

---

_Comment by @VeckoTheGecko on 2024-08-15 09:47_

Not sure if this is the right thread (maybe this should be a new issue), but there is a conflict between ISC001 and Ruff format if the string has a method associated with it.

```
parcels/tools/exampledata_utils.py:135:13: ISC001 Implicitly concatenated string literals on one line
    |
133 |     if dataset not in example_data_files:
134 |         raise ValueError(
135 |             f"Dataset {dataset!r} not found. Available datasets are: " ", ".join(example_data_files.keys())
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ ISC001
136 |         )
```

Of course, those string literals can't be combined because of the join.

---

_Comment by @MichaReiser on 2024-10-25 11:17_

Preview mode now comes with https://github.com/astral-sh/ruff/pull/13663. This isn't enough to fix the incompatibility because the preview style doesn't support triple quoted and raw strings. 

A simple way of solving the incompatibility is to always format raw and triple quoted implicitly concatenated strings over multiple lines (except when in unparenthesized statement positions). This fixes the incompatibility and, arguably, also makes the implicit concat more visible. 

```python
a = r"test" "other"
```

would be formatted as 

```python
a = (
	r"test"
	"other"
)
```

Any thoughts on this?

---

_Closed by @MichaReiser on 2024-11-03 11:49_

---

_Comment by @johnthagen on 2025-01-09 15:39_

Looks like this was released as part of 0.9.0

- https://github.com/astral-sh/ruff/pull/15238
- https://astral.sh/blog/ruff-v0.9.0#fewer-single-line-implicitly-concatenated-strings

---
