```yaml
number: 15794
title: Preserve quotes in generated f-strings
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/f-string-quotes
created_at: 2025-01-28T21:11:11Z
updated_at: 2025-01-29T18:28:24Z
url: https://github.com/astral-sh/ruff/pull/15794
synced_at: 2026-01-12T15:55:52Z
```

# Preserve quotes in generated f-strings

---

_@ntBre_

## Summary

This is another follow-up to #15726 and #15778, extending the quote-preserving behavior to f-strings and deleting the now-unused `Generator::quote` field.

## Details
I also made one unrelated change to `rules/flynt/helpers.rs` to remove a `to_string` call for making a `Box<str>` and tweaked some arguments to some of the `Generator::unparse_f_string` methods to make the code easier to follow, in my opinion. Happy to revert especially the latter of these if needed.

Unfortunately this still does not fix the issue in #9660, which appears to be more of an escaping issue than a quote-preservation issue. After #15726, the result is now `a = f'# {"".join([])}' if 1 else ""` instead of `a = f"# {''.join([])}" if 1 else ""` (single quotes on the outside now), but we still don't have the desired behavior of double quotes everywhere on Python 3.12+. I added a test for this but split it off into another branch since it ended up being unaddressed here, but my `dbg!` statements showed the correct preferred quotes going into [`UnicodeEscape::with_preferred_quote`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_literal/src/escape.rs#L54).

## Test Plan

Existing rule and `Generator` tests.


---

_Label `bug` added by @ntBre on 2025-01-28 21:11_

---

_Label `fixes` added by @ntBre on 2025-01-28 21:11_

---

_Review requested from @MichaReiser by @ntBre on 2025-01-28 21:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flynt/helpers.rs`:18 on 2025-01-28 21:13_

nit: explicit `from` calls are often more readable than `.into()` calls, as you can easily see what type the object is being converted into

```suggestion
        value: Box::from(s),
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:72 on 2025-01-28 21:15_

🥳

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1405 on 2025-01-28 21:17_

(we'll also probably want to preserve whether an f-string or bytestring is raw or not, like you did with regular strings. But that's for another PR!)

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1771 on 2025-01-28 21:18_

maybe rename the test function?

```suggestion
    fn quotes_are_preserved() {
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1782 on 2025-01-28 21:19_

I think this comment won't make sense to a future reader of the code, who might not know that there ever was a `Generator::quote` field. Can we rephrase it to say something like "We preserve the quote style of the original source"?

---

_Comment by @github-actions[bot] on 2025-01-28 21:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/50ff629ac5e2efb69afd75378f44a02ea6003ffc/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f"{shard}.{external_host}"` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/50ff629ac5e2efb69afd75378f44a02ea6003ffc/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/50ff629ac5e2efb69afd75378f44a02ea6003ffc/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f"{shard}.{external_host}"` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/50ff629ac5e2efb69afd75378f44a02ea6003ffc/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood approved on 2025-01-28 21:21_

Nice! Would it be possible to add some new lint rule test fixtures that show a behaviour change from this PR? Maybe a new snippet or two for one of the `flynt` rules?

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1405 on 2025-01-28 21:26_

Oh good call, I'll add it to my todo list next to preserving triple quotes!

---

_@ntBre reviewed on 2025-01-28 21:26_

---

_@ntBre reviewed on 2025-01-28 21:28_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1771 on 2025-01-28 21:28_

I also thought about moving these to the existing `quote` test. Would that be better, or is it worth keeping them separate?

`quote` uses `round_trip` instead of `round_trip_with`, but we're not really utilizing `round_trip_with` now either.

---

_@ntBre reviewed on 2025-01-28 21:31_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1782 on 2025-01-28 21:31_

Will do if we don't move these into `quote` as I mentioned above. I think using `assert_round_trip` in `quote` (or here I guess) might make the comment less important?

---

_Comment by @ntBre on 2025-01-28 22:32_

> Maybe a new snippet or two for one of the `flynt` rules?

This is trickier than I expected. For both of these rules, I just used `Checker::default_fstring_flags` instead of passing them along from an existing string. For the `flynt` rule that's reasonable since it's creating an f-string where one didn't exist before, but the RUF030 rule should actually preserve them. I think I need to restructure the rule code a bit to make this possible, though.

---

_Comment by @dhruvmanila on 2025-01-29 03:45_

> but we still don't have the desired behavior of double quotes everywhere on Python 3.12+. I added a test for this but split it off into another branch since it ended up being unaddressed here, but my `dbg!` statements showed the correct preferred quotes going into [`UnicodeEscape::with_preferred_quote`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_literal/src/escape.rs#L54).

We should make sure that this doesn't introduce any incompatibility with the f-string style in the formatter (I think it shouldn't) as the formatter prefers alternating quote style instead of using the same quotes in Python 3.12+ (https://docs.astral.sh/ruff/formatter/#f-string-formatting).

---

_Comment by @dhruvmanila on 2025-01-29 03:52_

> Unfortunately this still does not fix the issue in #9660, which appears to be more of an escaping issue than a quote-preservation issue.

Sorry, I think I missed this issue before but I was wondering whether that should actually be considered a bug or not because the formatter prefers using alternating quote style. I think this should not be a bug because if the issue author is running the formatter after the linter, then the quotes will be changed back which might seem confusing.

---

_Comment by @AlexWaygood on 2025-01-29 10:59_

Hmm, @dhruvmanila I still would consider #9660 a bug I think. I agree that if a lint autofix is generating a new f-string completely from scratch, it should prefer to use alternating quotes like the formatter does. But that's not what is happening in #9660 -- in #9660, it's replacing an existing f-string with a new f-string, but the new f-string has a different quoting style to the old f-string. That seems like an unnecessary stylistic change for the autofix to make, considering that the user might not even have any autoformatter enabled on their code. If they're using the Ruff formatter, then the issue doesn't arise in the first place, because their existing f-string would probably already have alternating quotes. (Or, if it's an f-string that they've just added and haven't yet formatted, they can run the formatter immediately after they run the linter in order to fix it up.)

Basically, my opinion is that where possible lint autofixes should aim to have no opinion at all on quoting styles, and should leave that to the formatter. Instead, they should as much as possible try to preserve quoting styles when doing autofixes.

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1771 on 2025-01-29 11:00_

Yeah, I think we can merge the tests now (or delete this function entirely if it is now completely redundant). As we discussed previously, the main purpose of this function was to test that setting the `quote` field worked correctly. But the `quote` field has now gone! :-D

---

_@AlexWaygood reviewed on 2025-01-29 11:00_

---

_Comment by @AlexWaygood on 2025-01-29 11:02_

> This is trickier than I expected. For both of these rules, I just used `Checker::default_fstring_flags` instead of passing them along from an existing string. For the `flynt` rule that's reasonable since it's creating an f-string where one didn't exist before, but the RUF030 rule should actually preserve them. I think I need to restructure the rule code a bit to make this possible, though.

From the ecosystem report in https://github.com/astral-sh/ruff/pull/15794#issuecomment-2620078880, it looks like there's some changes to SIM108 suggested autofixes -- you could maybe add some f-string snippets to the SIM108 fixtures, if the flynt rules are hard to add tests for?

---

_Comment by @ntBre on 2025-01-29 15:00_

@AlexWaygood Thanks for the SIM108 suggestion, I added two cases for that: one with double quotes and one with single quotes. I was looking for rules that explicitly interacted with f-strings, but I see now that just about any rule that uses the `Generator` and can apply to an expression with f-strings would have worked, which is exciting on its own!

@dhruvmanila What do you think about Alex's comments? I tend to agree with preserving whatever the user had and then letting the formatter clean it up afterward. We still have some time to decide either way, though. I was going to work on preserving prefixes and triple quotes before taking a stab at that issue anyway.

---

_@AlexWaygood approved on 2025-01-29 18:16_

🎉

---

_Merged by @ntBre on 2025-01-29 18:28_

---

_Closed by @ntBre on 2025-01-29 18:28_

---

_Branch deleted on 2025-01-29 18:28_

---
