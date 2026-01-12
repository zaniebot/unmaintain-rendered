```yaml
number: 20482
title: Ruff 2026 Style Guide
type: issue
state: open
author: dylwil3
labels:
  - formatter
  - help wanted
  - style
assignees: []
created_at: 2025-09-19T13:37:01Z
updated_at: 2026-01-10T15:01:47Z
url: https://github.com/astral-sh/ruff/issues/20482
synced_at: 2026-01-12T15:54:57Z
```

# Ruff 2026 Style Guide

---

_@dylwil3_

From [Black's changelog](https://github.com/psf/black/blob/main/CHANGES.md) and [`Preview` enum](https://github.com/psf/black/blob/43135e9fafccbca723910a45aa62f0f182e85e5f/src/black/mode.py#L223). Last checked 19.09.2025, Last release: v25.9.0.

Goals:
* Close parity with Black when migrating from Black to Ruff
* Fix Ruff specific bugs
* Innovate on local formatting (to keep close parity with black)


## Preview Styles

### Stabilized in Black and not available in Ruff (even in preview)
* [ ] [`remove_lone_list_item_parens`](https://github.com/psf/black/pull/4312)
* [x] https://github.com/astral-sh/ruff/issues/9745  -- progress towards this here https://github.com/astral-sh/ruff/pull/21110

### Black preview styles

* [x] [`wrap_long_dict_values_in_parens`](https://github.com/psf/black/pull/3440) Non-goal, see https://github.com/psf/black/issues/4123 and https://github.com/astral-sh/ruff/issues/12856
* [ ] [`always_one_newline_after_import`](https://github.com/psf/black/pull/4489) This is enforced by [unsorted-imports (I001)](https://docs.astral.sh/ruff/rules/unsorted-imports/#unsorted-imports-i001) and configurable via [`lines-after-imports`](https://docs.astral.sh/ruff/settings/#lint_isort_lines-after-imports). There is already some small conflict with the formatter depending on how the user configures this, and this may cause a little more. Unsure about this one...
* [x] [`fix_fmt_skip_in_one_liners`](https://github.com/psf/black/pull/4552) https://github.com/astral-sh/ruff/pull/20633 https://github.com/astral-sh/ruff/pull/22119
* [x] [`wrap_comprehension_in`](https://github.com/psf/black/pull/4699)
  * https://github.com/astral-sh/ruff/pull/21005
* [x] [`remove_parens_around_except_types`](https://github.com/psf/black/pull/4720)
  * https://github.com/astral-sh/ruff/pull/20768
* [x] [`normalize_cr_newlines`](https://github.com/psf/black/pull/4710) Already handled by Ruff https://github.com/astral-sh/ruff/issues/20482#issuecomment-3314361284

### Black unstable styles

* [x] [`string_processing`](https://black.readthedocs.io/en/latest/the_black_code_style/future_style.html#labels-string-processing) Non-goal, various issues https://github.com/psf/black/issues?q=is%3Aissue%20state%3Aopen%20string_processing
* [x] [`hug_parens_with_braces_and_square_brackets`](https://github.com/psf/black/pull/3964) Note this is already implemented as a Ruff preview feature (see below). So marking this as "done" here, and deferring to the below list for whether to stabilize. See also https://github.com/astral-sh/ruff/issues/11375
* [x] [`multiline_string_handling`](https://github.com/psf/black/pull/1879) According to 2025 issue: "non goal. Maybe a simplified version of it that only joins implicitly concatenated strings"

### Ruff preview styles
Ruff specific preview styles that we may want to stabilize
* [ ] [`hug_parens_with_braces_and_square_brackets`](https://github.com/astral-sh/ruff/issues/8279)
* [ ] [`no_chaperone_for_escaped_quote_in_triple_quoted_docstring`](https://github.com/astral-sh/ruff/pull/17216)
* [ ] [`blank_line_before_decorated_class_in_stub`](https://github.com/astral-sh/ruff/issues/18865)

### Ruff improvements

## Open Bugs
Existing bugs in the ruff formatter for which no preview style exists.

### Require a new style guide
Bug fixes that change how existing code is formatted and require a breaking change.
* [x] [Unnecessary parantheses to long patterns with `as` captures](https://github.com/astral-sh/ruff/issues/17796) https://github.com/astral-sh/ruff/pull/21176
  - https://github.com/astral-sh/ruff/pull/21176
* [ ] [Dict key split to multiline (deviation from black)](https://github.com/astral-sh/ruff/issues/11857)
* [ ] [Ruff splits subscript targets instead of parenthesizing the value](https://github.com/astral-sh/ruff/issues/13761)
* [ ] [Handling of blank lines between module docstring and a comment](https://github.com/astral-sh/ruff/issues/11254) (unclear if it requires a new style guide) @konstin
* [ ] [Parenthesize assignment values that fit into the line length](https://github.com/astral-sh/ruff/issues/11820)
* [ ] [Handle over-long lambda expressions](https://github.com/astral-sh/ruff/issues/8179) (possibly requires a new style guide)

### Bug fixes that don't change existing formatting
Bug fixes that don't require a new style guide because they don't change existing code
* [ ] [Range formatting does not handle whitespace before new classes](https://github.com/astral-sh/ruff/issues/19603)
* [ ] [Formatter inserts trailing newline at the end of a file even when inside a fmt: off region](https://github.com/astral-sh/ruff/issues/19492)
* [x] [Not respecting `fmt: skip`](https://github.com/astral-sh/ruff/issues/17331) (possibly related to `fix_fmt_skip_in_one_liners` Black preview style)
* [ ] [Preserve trailing whitespace in docstring examples](https://github.com/astral-sh/ruff/issues/10275)
* [ ] [Incorrect docstring code block formatting for statement sequences](https://github.com/astral-sh/ruff/issues/11480)
* [ ] [Handling of empty lines after `;`](https://github.com/astral-sh/ruff/issues/9958)
* [x] [Suppressing statement sequences](https://github.com/astral-sh/ruff/issues/11430)
* [x] [Using `fmt:skip` to suppress compound statements](https://github.com/astral-sh/ruff/issues/11216)
  - https://github.com/astral-sh/ruff/pull/20633
* [x] [Parenthesized expression with comment on right hand side in assignment produces invalid syntax](https://github.com/astral-sh/ruff/issues/19350)
  - https://github.com/astral-sh/ruff/pull/20418
* [x] [Fuzzer: comment in middle of operand causes panic](https://github.com/astral-sh/ruff/issues/19226)
  - https://github.com/astral-sh/ruff/pull/21501
* [x] [Fuzzer: debug assert on dangling comments failed](https://github.com/astral-sh/ruff/issues/19138)
  - https://github.com/astral-sh/ruff/pull/20561



---

_Label `formatter` added by @dylwil3 on 2025-09-19 13:37_

---

_Label `help wanted` added by @dylwil3 on 2025-09-19 13:37_

---

_Label `style` added by @dylwil3 on 2025-09-19 13:37_

---

_Comment by @s-banach on 2025-09-20 00:54_

> Ruff specific preview styles that we may want to stabilize
> 
> * [ ]  [`hug_parens_with_braces_and_square_brackets`](https://github.com/astral-sh/ruff/issues/8279)

I have been using ruff format with `preview = true` since 2023, just for this one feature.
The only reason I'm reading this issue is to see whether it will be stabilized.

---

_Comment by @MeGaGiGaGon on 2025-09-20 01:24_

`ruff` already has the behavior from `normalize_cr_newlines`. I guess it was technically a formatting deviation before? But like usual no one noticed because dealing with newlines is so annoying.

<details>

<summary>Demonstrating difference</summary>

Input code powershell escaped: ``"a`rb`nc`r`n#1"``
Input code backslash escaped: `a\rb\nc\r\n#1\r\n`

|Formatter|Escaped Output|
|-----------|-----------------|
|Black 25.9.0 no preview|`a\nb\nc\n# 1\n`|
|ruff format|`a\rb\rc\r# 1\r`|
|Black 25.9.0 preview|`a\rb\rc\r# 1\r`|

Input code without visible newlines:
```py
a
b
c
#1
```

Input code with written out newlines:
```py
a<cr>
b<nl>
c<cr><nl>
#1<cr><nl>
```

Where the `#1` at the end is to cause the reformatting.

```powershell
PS ~\Desktop\New_folder>Set-Content issue.py "a`rb`nc`r`n#1"
PS ~\Desktop\New_folder>Get-Escaped-Content issue.py
a\rb\nc\r\n#1\r\n
PS ~\Desktop\New_folder>uvx black issue.py
reformatted issue.py

All done! ✨ 🍰 ✨
1 file reformatted.
PS ~\Desktop\New_folder>Get-Escaped-Content issue.py
a\nb\nc\n# 1\n
PS ~\Desktop\New_folder>Set-Content issue.py "a`rb`nc`r`n#1"
PS ~\Desktop\New_folder>uvx ruff format issue.py
1 file reformatted
PS ~\Desktop\New_folder>Get-Escaped-Content issue.py
a\rb\rc\r# 1\r
PS ~\Desktop\New_folder>Set-Content issue.py "a`rb`nc`r`n#1"
PS ~\Desktop\New_folder>uvx black issue.py --preview
reformatted issue.py

All done! ✨ 🍰 ✨
1 file reformatted.
PS ~\Desktop\New_folder>Get-Escaped-Content issue.py
a\rb\rc\r# 1\r
```

Where `Get-Escaped-Content` is a custom powershell function specifically to help deal with these annoying newlines:
```powershell
function Get-Escaped-Content {
	param (
		$Path
	)
	return Get-Content $Path -Raw | ForEach-Object { $_ -replace "\n", "\n"} | ForEach-Object { $_ -replace "\r", "\r"}
}
```

</details>

---

_Comment by @MichaReiser on 2025-09-20 11:12_

For reference the tracking issue for the [2025 style guide](https://github.com/astral-sh/ruff/issues/13371 ). 

* It was an intentional decision not to implement `https://github.com/psf/black/pull/3440` because of https://github.com/astral-sh/ruff/issues/12856. 
* The unstable styles are styles that black doesn't consider "ready" yet because of bugs. We should focus on the preview sytles first (but it's good to work on unstable styles in case black fixes all remaining issues and promotes them to preview and then stable)
* `multiline_string_handling`: Ruff now automatically joins implicitly concatenated strings. That's as far as we want to go for now.
* I'm not sure what `string_processing` is
* We should consider implementing and shipping https://github.com/astral-sh/ruff/issues/9745 (requires some design, maybe there's a middle ground that we dont' need to go as far as black)
* There are also some black 2025 style changes that we didn't implement, see https://github.com/astral-sh/ruff/issues/15927


@vlkorsakov is there a specific change that you dislike or do you dislike style changes in general?

---

_Comment by @MeGaGiGaGon on 2025-09-20 16:01_

Ruff already has part of `string_processing` in that [short string literals will be merged](https://docs.astral.sh/ruff/formatter/black/#implicit-concatenated-strings) . Also part of it is splitting long string literals, see [the docs](https://black.readthedocs.io/en/latest/the_black_code_style/future_style.html#improved-string-processing) for more info, but `string_processing` is almost certainly going to remain unstable for 2026. See the 2024 Black Stable Style issue https://github.com/psf/black/issues/4042 for more details, but there are still many outstanding serious issues https://github.com/psf/black/issues?q=is%3Aissue%20state%3Aopen%20string_processing . (But it also won't be removed since there are a ton of people that like it, so it's stuck in a difficult place)

---

_Comment by @dylwil3 on 2025-09-26 20:34_

It looks like the preview style `wrap_comprehension_in` also results in quite a few added parentheses: https://github.com/psf/black/blob/903bef54aa72e111c034ccec9997aac82bafbacc/tests/data/cases/preview_wrap_comprehension_in.py

So we may get into a similar situation as with https://github.com/astral-sh/ruff/issues/12856

---

_Comment by @MichaReiser on 2025-11-10 08:21_

Black published a [new release](https://github.com/psf/black/releases/tag/25.11.0) today and they moved multiline_string from unstable into preview. We should double check how their multiline string handling differs from our (I might have confused it with the [string_processing](https://black.readthedocs.io/en/latest/the_black_code_style/future_style.html#labels-string-processing) unstable style)

---

_Comment by @ntBre on 2025-11-10 18:54_

I tried updating our Black tests again, and the only changes to the multiline string styles were from this PR: https://github.com/psf/black/pull/4760, deleting some of the cases that I think do relate more to `string_processing`. So our [`preview_multiline_strings` snapshot](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/tests/snapshots/black_compatibility%40cases__preview_multiline_strings.py.snap) should still accurately reflect how closely we match Black.

Mostly it looks like we insert more breaks around multiline strings:

https://github.com/astral-sh/ruff/blob/deeda5690617162bf7c7364a2cb3faa69e880a19/crates/ruff_python_formatter/tests/snapshots/black_compatibility%40cases__preview_multiline_strings.py.snap#L336-L345

---

_Comment by @hauntsaninja on 2025-11-10 23:22_

Does ruff have anything planned for https://github.com/astral-sh/ruff/issues/12856 ? Especially curious about `parenthesize_conditional_expressions` , I get asked about that at work sometimes

---

_Comment by @MichaReiser on 2025-11-11 10:49_

> I tried updating our Black tests again, and the only changes to the multiline string styles were from this PR

I see. I suggest we wait then. Ruff uses some heuristics to preserve the indent for multiline strings to reduce the diff compared to the old style guide and I remember that it was a main concern for Black when they considered stabilizing in previous years. We can always follow along next year if the change works well for Black. 


> Does ruff have anything planned for https://github.com/astral-sh/ruff/issues/12856 ? Especially curious about parenthesize_conditional_expressions , I get asked about that at work sometimes

Unfortunately not. I very much hope that we'll have more dedicated time to improve Ruff's style guide next year. Shipping https://github.com/astral-sh/ruff/issues/12856 probably also requires formatter editions (that are independent of the ruff version), because I expect the diff compared to the current style guide to be rather large. You can upvote (or comment on) https://github.com/astral-sh/ruff/issues/12856 or https://github.com/astral-sh/ruff/issues/9722 specifically. This would help us prioritize the work next year.


---

_Comment by @laymonage on 2025-11-11 14:47_

I'd like to suggest #21380 to be included in the "Stabilized in Black and not available in Ruff (even in preview)" section, next to #21110 that has been released. Thanks!

---

_Comment by @dylwil3 on 2025-12-06 00:44_

Black added a new preview style on `main` (not yet released): https://github.com/psf/black/pull/4865

Removes LHS parentheses in examples such as

```python
(b) = a()[0]
(c, *_) = a()
```

At the moment we preserve the user's parentheses or lack thereof.

I'm sort of okay making this a preview style, but I would hesitate stabilizing for 2026 - it seems like not a huge improvement but would cause big diffs everywhere.

---

_Comment by @MichaReiser on 2025-12-06 12:36_

> I'm sort of okay making this a preview style, but I would hesitate stabilizing for 2026 - it seems like not a huge improvement but would cause big diffs everywhere.

I'm not sure I follow the reasoning here. The diff size doesn't depend on when we stabilize it; it's only defined by the change itself. 

Removing the parentheses around tuples requires matching on the target and call the tuple formatting with `TupleParentheses::NeverPreserve`. 

Removing unnecessary parentheses around `(b)` might be slightly trickier. I do agree that this isn't a change that we need to rush and I'd keep our existing prioritization:

* method chain formatting
* fmt:skip bug
* subscript issue

---

_Comment by @MichaReiser on 2025-12-09 08:34_

Regarding: `(c, *_) = a()`. We intentionally preserve tuple parentheses in more places than black because it can be very easy to miss that something's a tuple if the parentheses are missing. I don't remember the exact details but we should ensure we preserve parentheses in assignment targets the same as we do in assignment values.

---
