---
number: 13371
title: Ruff 2025 style guide
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
  - style
assignees: []
created_at: 2024-09-16T16:19:01Z
updated_at: 2025-01-13T15:25:04Z
url: https://github.com/astral-sh/ruff/issues/13371
synced_at: 2026-01-10T01:22:53Z
---

# Ruff 2025 style guide

---

_Issue opened by @MichaReiser on 2024-09-16 16:19_

From [Black's changelog](https://github.com/psf/black/blob/main/CHANGES.md). Last checked 16.09.2024, Last release: v24.8.0.

Goals: 
* Close parity with Black when migrating from Black to Ruff
* Fix Ruff specific bugs
* Innovate on local formatting (to keep close parity with black)


## Preview Styles

### Black preview styles
* [x] [`remove_redundant_guard_parens`](https://github.com/psf/black/pull/4214) https://github.com/astral-sh/ruff/pull/13513
* [x] [`parens_for_long_if_clauses_in_case_block`](https://github.com/psf/black/pull/4269) (related to `remove_redundant_guard_parens`): See [#10969](https://github.com/astral-sh/ruff/pull/10969) https://github.com/astral-sh/ruff/pull/13513
* [ ] [`no_normalize_fmt_skip_whitespace`](https://github.com/psf/black/pull/4146/files): Don't normalize leading whitespace before `fmt: skip` comments.

### Black unstable styles
* [ ] [`hug_parens_with_braces_and_square_brackets`]
  * https://github.com/astral-sh/ruff/issues/11375
* [x]  [~~`wrap_long_dict_values_in_parens`~~](https://github.com/astral-sh/ruff/issues/12856): We decided not to support this style for now because it introduces new parentheses. 
* [x] [~~`multiline_string_handling`~~](https://github.com/astral-sh/ruff/issues/8896) non goal. Maybe a simplified version of it that only joins implicitly concatenated strings

### Ruff preview styles
Ruff specific preview styles that we may want to stabilize 
* [x] [`f_string_formatting`](https://github.com/astral-sh/ruff/issues/7594) (style improvement)
	* [x] https://github.com/astral-sh/ruff/issues/11056
	* [x] https://github.com/astral-sh/ruff/issues/13237
	* [x] https://github.com/astral-sh/ruff/issues/13813
	* [x] https://github.com/astral-sh/ruff/issues/14608
	* [x] fixes https://github.com/astral-sh/ruff/issues/14766
* [x] [`comprehension_leading_expression_comments_same_line`](https://github.com/astral-sh/ruff/pull/12282) (bugfix)
* [x] [`with_single_item_pre_39_enabled`](https://github.com/astral-sh/ruff/pull/10276) (style improvement)
* [x] [`f_string_implicit_concatenated_string_literal_quotes`](https://github.com/astral-sh/ruff/pull/13539) (bugfix)

### Ruff improvements
* [x] [Unnecessary parentheses around return-type annotations](https://github.com/astral-sh/ruff/issues/9447) https://github.com/astral-sh/ruff/pull/13381
* [x] [Implicit string formatting](https://github.com/astral-sh/ruff/issues/9457)
* [x] [Can omit parentheses layout for patterns](https://github.com/astral-sh/ruff/issues/6933): Related to `remove_redundant_guard_parens` and `parens_for_long_if_clauses_in_case_block`


## Open Bugs
Existing bugs in the ruff formatter for which no preview style exists.

### Require a new style guide
Bug fixes that change how existing code is formatted and require a 
* [x] [line length calculation for docstring code block is incorrect when using `dynamic`](https://github.com/astral-sh/ruff/issues/13358)
* [ ] [Handling of blank lines between module docstring and a comment](https://github.com/astral-sh/ruff/issues/11254) (unclear if it requires a new style guide) @konstin 
* [ ] [Parenthesize assignment values that fit into the line length](https://github.com/astral-sh/ruff/issues/11820)
* [ ] [Handle over-long lambda expressions](https://github.com/astral-sh/ruff/issues/8179) (possibly requires a new style guide)

### Bug fixes that don't change existing formatting
Bug fixes that don't require a new style guide because they don't change existing cod
* [ ] [Preserve trailing whitespace in docstring examples](https://github.com/astral-sh/ruff/issues/10275)
* [ ] [Incorrect docstring code block formatting for statement sequences](https://github.com/astral-sh/ruff/issues/11480)
* [x] [Trailing parenthesized function return type comment becomes leading comment](https://github.com/astral-sh/ruff/issues/13369)
* [ ] [Handling of empty lines after `;`](https://github.com/astral-sh/ruff/issues/9958)
* [ ] [Suppressing statement sequences](https://github.com/astral-sh/ruff/issues/11430)
* [ ] [Using `fmt:skip` to suppress compound statements](https://github.com/astral-sh/ruff/issues/11216)

## Black Bug fixes
Black preview styles that are related to Black-specific bug fixes. Ruff already handles those cases correctly
*  [`pep646_typed_star_arg_type_var_tuple`](https://github.com/psf/black/pull/4440)
*  [`unify_docstring_detection`](https://github.com/psf/black/pull/4095): Format module and single quoted docstrings
* [`typed_params_trailing_comma`](https://github.com/psf/black/pull/4164)
*  [`is_simple_lookup_for_doublestar_expression`](https://github.com/psf/black/pull/4154)
*  [`docstring_check_for_newline`](https://github.com/psf/black/pull/4185)
*  [`pep646_typed_star_arg_type_var_tuple`](https://github.com/psf/black/pull/4440)


### Needs fixing

* [x] https://github.com/astral-sh/ruff/issues/14778

---

_Label `formatter` added by @MichaReiser on 2024-09-16 16:19_

---

_Label `style` added by @MichaReiser on 2024-09-16 16:19_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-09-16 16:24_

---

_Label `help wanted` added by @MichaReiser on 2024-09-17 10:44_

---

_Label `help wanted` added by @MichaReiser on 2024-09-17 10:44_

---

_Comment by @MichaReiser on 2024-09-17 10:44_

If anyone's interested in picking up one item on this list, let me know :) Happy to help

---

_Comment by @MichaReiser on 2024-09-17 12:38_

One open question is whether we want to support multiple formatter edition (or at least one old edition). It took the community a long time to upgrade to Ruff 0.2

---

_Comment by @bnkc on 2024-09-18 05:37_

Hey @MichaReiser I'd be interested in picking up https://github.com/astral-sh/ruff/issues/11216. Let me know if this is alright. Thanks!

---

_Comment by @MichaReiser on 2024-09-18 06:02_

> Hey @MichaReiser I'd be interested in picking up #11216. Let me know if this is alright. Thanks!

Sure. Feel free to give it a go. I don't know what changes are necessary for it but feel free to comment on that issue with ideas or go ahead with a PR and we can discuss there.

---

_Comment by @MichaReiser on 2024-09-25 16:43_

@bnkc how is it going with the docstring bug? Do you need help or did you decide not to work on it (which is fine)? I'm asking because I'm considering to look into it myself because it is probably the most "sever" bug.

---

_Comment by @MichaReiser on 2024-09-27 08:04_

@dhruvmanila, do you remember what the open decisions were on the f-string formatting and what the remaining work is for it to be stabilized? Do you know if black implemented a similar formatting style?

---

_Comment by @dhruvmanila on 2024-10-01 13:33_

> @dhruvmanila, do you remember what the open decisions were on the f-string formatting and what the remaining work is for it to be stabilized? Do you know if black implemented a similar formatting style?

* https://github.com/astral-sh/ruff/issues/13237
* Style decision regarding indentation: https://github.com/astral-sh/ruff/discussions/9785#discussioncomment-8470590
* https://github.com/astral-sh/ruff/issues/11056 which depends on https://github.com/astral-sh/ruff/issues/6591
* https://github.com/astral-sh/ruff/issues/13813

---

_Comment by @MichaReiser on 2024-10-16 14:01_

Regarding f-string formatting. We should look into whether it plays nicely with https://github.com/astral-sh/ruff/blob/ae9c6a9255294cb5cc884726fcbfbcd441f2f76c/crates/ruff_python_formatter/src/other/arguments.rs#L221

From looking at the `is_multiline` implementation I get the impression that it is an over-approximation and a string that was a multiline string might collapse (but maybe that's fine?)

---

_Comment by @MichaReiser on 2024-10-24 09:50_

Black's plan is to stabilize all preview styles, but none of the unstable features which aligns with what I outlined in the summary. https://github.com/psf/black/issues/4501

---

_Referenced in [astral-sh/ruff#13906](../../astral-sh/ruff/pulls/13906.md) on 2024-10-24 13:37_

---

_Referenced in [astral-sh/ruff#14001](../../astral-sh/ruff/issues/14001.md) on 2024-11-04 19:57_

---

_Referenced in [astral-sh/ruff#14103](../../astral-sh/ruff/issues/14103.md) on 2024-11-05 11:02_

---

_Referenced in [astral-sh/ruff#14329](../../astral-sh/ruff/pulls/14329.md) on 2024-11-17 19:18_

---

_Referenced in [astral-sh/ruff#14513](../../astral-sh/ruff/issues/14513.md) on 2024-11-21 12:47_

---

_Comment by @alechouse97 on 2024-11-25 16:47_

Are there any plans to incorporate allowing empty new lines after certain code blocks from #9745 ?

---

_Comment by @MichaReiser on 2024-11-25 17:22_

> Are there any plans to incorporate allowing empty new lines after certain code blocks from #9745 ?

No, allowing empty lines after clause header is not planned for the upcoming style guide

---

_Referenced in [astral-sh/ruff#14638](../../astral-sh/ruff/issues/14638.md) on 2024-11-27 17:32_

---

_Referenced in [psf/black#4522](../../psf/black/issues/4522.md) on 2024-12-04 07:22_

---

_Referenced in [astral-sh/ruff#14984](../../astral-sh/ruff/issues/14984.md) on 2024-12-15 16:11_

---

_Referenced in [astral-sh/ruff#15131](../../astral-sh/ruff/issues/15131.md) on 2024-12-24 13:56_

---

_Referenced in [astral-sh/ruff#15238](../../astral-sh/ruff/pulls/15238.md) on 2025-01-03 14:13_

---

_Closed by @MichaReiser on 2025-01-09 09:20_

---

_Referenced in [Homebrew/homebrew-core#203745](../../Homebrew/homebrew-core/pulls/203745.md) on 2025-01-09 17:12_

---

_Referenced in [ConsenSysDiligence/mythril#1892](../../ConsenSysDiligence/mythril/pulls/1892.md) on 2025-01-13 11:01_

---

_Comment by @trim21 on 2025-01-13 15:19_

I'm late for this issue, can we have a stable output of https://docs.astral.sh/ruff/formatter/black/#call-expressions-with-a-single-multiline-string-argument ?

If it's too late for stable 2025 style, is it possible to be a preview style?

---

_Comment by @MichaReiser on 2025-01-13 15:25_

> I'm late for this issue, can we have a stable output of [docs.astral.sh/ruff/formatter/black#call-expressions-with-a-single-multiline-string-argument](https://docs.astral.sh/ruff/formatter/black/#call-expressions-with-a-single-multiline-string-argument) ?
> 
> If it's too late for stable 2025 style, is it possible to be a preview style?

I'm not sure I understand what you mean. The feature itself is stable since last year. Please open a new issue if it isn't working for you together with an example.

---

_Referenced in [astral-sh/ruff#9745](../../astral-sh/ruff/issues/9745.md) on 2025-02-17 11:17_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-20 11:12_

---
