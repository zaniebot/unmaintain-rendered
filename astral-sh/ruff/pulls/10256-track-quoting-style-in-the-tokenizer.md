```yaml
number: 10256
title: Track quoting style in the tokenizer
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: quotestyle-tokenizer
created_at: 2024-03-06T19:39:57Z
updated_at: 2024-03-08T23:13:25Z
url: https://github.com/astral-sh/ruff/pull/10256
synced_at: 2026-01-12T15:55:31Z
```

# Track quoting style in the tokenizer

---

_@AlexWaygood_

## Summary

This PR changes the tokenizer so that all information about quoting style and prefixes is captured in a bitflag; this bitflag is then stored as a field on `String`, `FStringStart`, `FStringMiddle` and `FStringEnd` tokens.

By itself, this change does not fix any bugs. However, it's a necessary first step if we want to start tracking this information in the AST, which is necessary if we want to solve #7799 in a principled and universal way.

It should be easiest to review this PR one commit at a time:
1. The first commit adds the bitflag and starts storing it on `String` tokens.
2. The second commit also changes f-string tokens so that they store the same information
3. The third commit applies various cleanups and simplifications to various linter rules that are now possible with this refactor.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-06 19:39_

---

_Comment by @AlexWaygood on 2024-03-06 19:51_

It looks like some of the benchmarks are showing a 2-3% slowdown for `lexer::Lexer::next_token()`: https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:quotestyle-tokenizer. I'll see tomorrow if there's anything I can do to ameliorate that.

---

_Comment by @github-actions[bot] on 2024-03-06 19:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/e312ac2483317ceec62cf62ff5c58bbcef8dcffe/rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py#L9'>rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py:9:36:</a> Q004 [*] Unnecessary escape on inner quote character
+ <a href='https://github.com/rotki/rotki/blob/e312ac2483317ceec62cf62ff5c58bbcef8dcffe/rotkehlchen/chain/evm/decoding/cowswap/decoder.py#L41'>rotkehlchen/chain/evm/decoding/cowswap/decoder.py:41:45:</a> Q004 [*] Unnecessary escape on inner quote character
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| Q004 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/e312ac2483317ceec62cf62ff5c58bbcef8dcffe/rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py#L9'>rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py:9:36:</a> Q004 [*] Unnecessary escape on inner quote character
+ <a href='https://github.com/rotki/rotki/blob/e312ac2483317ceec62cf62ff5c58bbcef8dcffe/rotkehlchen/chain/evm/decoding/cowswap/decoder.py#L41'>rotkehlchen/chain/evm/decoding/cowswap/decoder.py:41:45:</a> Q004 [*] Unnecessary escape on inner quote character
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| Q004 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @AlexWaygood on 2024-03-06 20:13_

(I'll also look through the ecosystem results tomorrow.)

---

_Comment by @charliermarsh on 2024-03-06 20:14_

@AlexWaygood - It's _possible_ that you have a slightly different version of LALRPOP than whatever was used most recently to generate the parser, hence the thousands of lines of changes in the generated `python.rs`. Can you check if you're using `// auto-generated: "lalrpop 0.20.0"`?

---

_Comment by @AlexWaygood on 2024-03-06 20:16_

> Can you check if you're using `// auto-generated: "lalrpop 0.20.0"`?

Ah, I'm using 0.20.2 locally :(

I did wonder...

---

_Comment by @charliermarsh on 2024-03-06 20:22_

It's one way to buff up your contribution stats.

---

_Comment by @AlexWaygood on 2024-03-06 21:50_

I fixed up the huge changes from using the wrong lalrpop version, with some help from @BurntSushi on the necessary `git`-fu to get there 🥳

---

_@charliermarsh reviewed on 2024-03-06 22:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:210 on 2024-03-06 22:09_

I tend to use: `flags.is_some_and(...)`

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string_token_flags.rs`:43 on 2024-03-06 22:13_

This may not be necessary, since `char` is `Clone` (IIRC). Could we omit it?

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string_token_flags.rs`:68 on 2024-03-06 22:14_

Nit: I'd expect these to be cased as `DISALLOWED_U_STRING_PREFIXES`, to match `Self::U_PREFIX` and (e.g.) "f-string".

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string_token_flags.rs`:171 on 2024-03-06 22:15_

Could perhaps just be `self. prefix_len().len()`?

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/lexer.rs`:555 on 2024-03-06 22:17_

Should this error be propagated? Or is it handled elsewhere / upstream?

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/lexer.rs`:182 on 2024-03-06 22:18_

Do we need to set `flags = flags.with_double_quotes();` here?

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/lexer.rs`:200 on 2024-03-06 22:19_

Should this happen in `lex_string`, instead? `lex_string` currently sets the triple-quoting, for example.

---

_@charliermarsh reviewed on 2024-03-06 22:19_

I think this looks great!

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:171 on 2024-03-06 22:30_

Something like this?

```rs
    pub fn prefix_len(self) -> TextSize {
        let len: u32 = self.prefix_str().len().try_into().unwrap();
        TextSize::from(len)
    }
```

---

_@AlexWaygood reviewed on 2024-03-06 22:30_

---

_@AlexWaygood reviewed on 2024-03-06 22:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lexer.rs`:555 on 2024-03-06 22:46_

This _shouldn't_ ever error -- the only reason this would error out is if it would be invalid to combine the `f` and `r` prefixes. But _we_ know that it's always valid to combine the `f` and `r` prefixes. So calling `unwrap()` here should be safe.

But I should probably change it to an `expect()` call, so that it's more obvious why it's safe.

---

_@charliermarsh reviewed on 2024-03-06 22:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string_token_flags.rs`:171 on 2024-03-06 22:59_

I thought there was `self.prefix_str().text_len()`, does that work?

---

_@AlexWaygood reviewed on 2024-03-06 23:02_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:171 on 2024-03-06 23:02_

Didn't know about that -- that does work, thanks!

---

_Comment by @charliermarsh on 2024-03-07 04:18_

Can you check if the ecosystem changes are intended?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:178 on 2024-03-07 08:36_

I think we can do better here than searching the strings: `inline_quotes` is an enum. So we should be able to simply check whether the corresponding bit is set depending on the enum variant of `flake8_quotes.inline_quotes`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:171 on 2024-03-07 08:37_

`flags` is a very generic term but I can't think of a much better terminology other than potentially `StringKind` (defined by quote style, quote numbers, and the set prefixes)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:77 on 2024-03-07 08:43_

Nit: I think it's safe to return `Some` (or just `flags`) because it's guaranteed that f-strings never have the `unicode` flag set (because they can't). That would simplify the downstream code because you don't need to check if `flags` is `Some``

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_character.rs`:116 on 2024-03-07 08:45_

Should we introduce a method `flags.text_len()` that returns the entire length of the flags? That would simplify the computation here

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:387 on 2024-03-07 08:46_

Yeah I think we should have a `text_len` method. This seems a very common pattern.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:414 on 2024-03-07 08:47_

I wonder if we should implement `Dipslay` for `flags`, because formatting the prefix, followed by the quotes also seems to be a very common pattern.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:59 on 2024-03-07 08:51_

What's the motivation for adding the `StringFlags` to `FStringMiddle` and `FStringEnd`? Can't we assume that they always match the flags of `FStringStart` (at least the quotes)?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:3 on 2024-03-07 08:52_

Nit: I would probably move this into `string.rs`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:171 on 2024-03-07 08:55_

The naming here feels inconsistent to me, because it's `is_raw` and not `is_rawstring`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:241 on 2024-03-07 08:56_

Nit
```suggestion
    pub const fn quote_len(self) -> TextSize {
        if self.is_triple_quoted() {
            TextSize::new(3)
        } else {
            TextSize::new(1)
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:209 on 2024-03-07 08:58_

Todo ;) 

yeah, that might be nice but maybe as a separate PR. Althougth the naming could get very confusing because we suddenly have two `QuoteStyle` types ;)

```
    #[must_use]
    pub const fn from_style(style: QuoteStyle) -> Option<QuoteChar> {
        match style {
            QuoteStyle::Single => Some(QuoteChar::Single),
            QuoteStyle::Double => Some(QuoteChar::Double),
            QuoteStyle::Preserve => None,
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:83 on 2024-03-07 09:07_

I like that you abstracted away the fact that we use bitflags internall. I wonder if we should take this one step further by:

* Introducing a new `StringPrefix` enum that's `FString`, `RawBytes`, `Raw`, `UString` (similar to the existing `StringKind` enum)
* Reducing the methods on this type. E.g. instead of `string_flags.quote_char()` to `string_flags.quote_style().as_char()` or implement `Display` for `QuoteStyle` and `StringPrefix` (and `StringFlags`?) so that `format!("{prefix}{quotes}", string_flags.prefix(), string_flags.quote_style())` works.

The advantage of exposing the `StringPrefix` is that we can keep the guarantee that you can't construct incorrect invariants without having to use `try` methods but use the more efficient bitflag storage internally. 

That means, the mutation methods would change to `StringFlags::with_prefix(self, prefix: StringPrefix)` and they're guaranteed to never fail. 

Doing this could also help with the lexer because we could then keep the lexer largely unchanged because it would call `StringPrefix::try_from` (or `parse`?) as it used to before and can construct the `StringFlags` by calling `StringFlags::new(prefix, quotes)`

---

_@MichaReiser reviewed on 2024-03-07 09:10_

This looks good. Nice work. I really like how you abstracted away the bit representation and I think it allows us to abstract away even more.

I recommend changing the API of `StringFlags` by introducing a new `StringPrefix` enum (similar to the existing `StringKind` enum) that guarantees that constructing invalid prefixes is impossible. This should reduce the changes necessary in the lexer and remove the unwrap/except calls (that could be the cause of the perf regression). 

I further recommend removing `StringFlags` from `FStringMiddle` and `FStringEnd` except if we have a very specific use case where knowing the flags is required.

---

_Label `internal` added by @MichaReiser on 2024-03-07 09:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer/fstring.rs`:6 on 2024-03-07 09:25_

I think I would prefer to keep this private even at the crate level and provide a method like `flags` or `into_flags` depending on the use-case. The main reason would be to hide the implementation details.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token.rs`:59 on 2024-03-07 09:32_

The information would be required at least for `FStringMiddle` as that contains the string content and is being used by multiple rules. As to `FStringStart` and `FStringEnd`, I think we can get away with not including the information in at least one of them, possibly both. I would check the usages of `FStringStart` / `FStringEnd` in the codebase and see if there are any and check if we can use `FStringMiddle` instead.

---

_@dhruvmanila reviewed on 2024-03-07 09:33_

---

_@MichaReiser reviewed on 2024-03-07 09:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__fstring_with_format_spec.snap`:214 on 2024-03-07 09:57_

Nit: It might be worth implementing `Debug` for `StringFlags` to get a nicer representation (I think that should be straightforward after introducing `StringPrefix` because you can than do:

```rust
f.debug_struct("StringFlags").field("prefix", &self.prefix()).field("quote_style", &self.quote_style()).finish()
```

---

_@MichaReiser reviewed on 2024-03-07 10:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:59 on 2024-03-07 10:00_

Hmm, `FStringMiddle` tracking it makes sense when `FStringStart` didn't track it. But it seems redundant now that `FStringStart` exposes the quote information because the prefix (kind) always matches the prefix of the closest `FStringStart` preceding the `FStringMiddle` (that wasn't closed with `FStringEnd`)

---

_@AlexWaygood reviewed on 2024-03-07 10:23_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:209 on 2024-03-07 10:23_

Next time I'll just `@` you on the PR 😆 I mainly just wanted to have the discussion about whether it's worth "unifying" the various enums we have scattered across the codebase for describing the two quote styles. Maybe it's not -- maybe it's small and obvious enough that it's okay to just duplicate it across the various crates. If it _is_ worth having a single enum somewhere, I'm not really sure where it should go. It feels odd to me to have it live in the parser crate, since -- while it's a useful enumeration for the parser -- it's not really anything parser-specific. It's really something quite abstract and intrinsic to the language (which is why we have similar enums in the linter, the stylist and the formatter already).

FTR, here's the very-similar enums we already have in various places:

https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_python_formatter/src/string/mod.rs#L234-L242

https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_python_codegen/src/stylist.rs#L113-L119

https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_linter/src/rules/flake8_quotes/settings.rs#L9-L17

https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_python_literal/src/escape.rs#L1-L5

That last one is already quite nice and self-contained, so possibly I could use that one here -- but the `ruff_python_parser` crate doesn't currently depend on the `ruff_python_literal` crate. Is it worth adding a dependency between the two crates just for this? (Genuine question -- I'm curious for your opinion!)

---

_@AlexWaygood reviewed on 2024-03-07 10:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:387 on 2024-03-07 10:29_

I agree that it's a common pattern, but the reason I held back from doing this was because it felt potentially quite confusing to me. `text_len()` would return `x + y` where `x` is the length of the prefixes and `y` is the length of the opening quotes -- right? But I feel like that's potentially a footgun, since you always want to use that for the opening of the string, and never for the closing of the string. If the API forces you to explicitly calculate it, then there's no footgun. Also:
- I wasn't sure what to call it -- `text_len()` to me implies that it would calculate the length of the contents of the string
- I'm not sure it would actually simplify the code much here, since we still need to do the conversion to `usize`

Writing this out, though, makes me think that maybe it would be an okay API if we gave it a clear enough name -- maybe something like `string_opener_len()`. WDYT?

---

_@MichaReiser reviewed on 2024-03-07 10:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:209 on 2024-03-07 10:30_

We probably need to move `string_token_flags` to the AST once you want to expose it on string nodes ;)

I would leave this for now and probably even remove the TODO. It's a nice cleanup we could do but having separate types also has its advantages.

---

_@AlexWaygood reviewed on 2024-03-07 10:32_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:209 on 2024-03-07 10:32_

I definitely want to leave it for now! I tried, when preparing this PR, to see if I could unify this enum with the one in `stylist.rs`. It quickly unravelled into a "now you need to rewrite nearly every line in this crate" kind of thing, and I ran away 😆

I'll remove the TODO 👍

---

_@AlexWaygood reviewed on 2024-03-07 10:38_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:59 on 2024-03-07 10:38_

The issue with f-strings is with empty f-strings. Given an f-string `f''`, here's what the tokenizer currently emits:

```
[
    Ok(
        (
            FStringStart,
            0..2,
        ),
    ),
    Ok(
        (
            FStringEnd,
            2..3,
        ),
    ),
]
```

https://play.ruff.rs/dfe9a07c-8082-4ac8-a199-acf2ea354c92

If we only track quoting style in `FStringMiddle` tokens, that means we won't be able to distinguish between single/double quotes for empty f-strings. Maybe that's an edge case, but the aim of this PR is to make sure that we can always easily retrieve quoting-style information from the tokenizer, and to prepare the ground so that in a future PR we can easily retrieve that information from the AST as well.

I can see a few ways of solving this:
1. Track quoting style in all three f-string tokens: `FStringStart`, `FStringMiddle` and `FStringEnd`. (What this PR currently does.)
2. Only track quoting style in `FStringStart` tokens. That feels like it makes some sense, given that all f-strings currently have a start, but not all of them currently have a middle (in our representation). It would be quite an invasive change relative to what I'm currently doing, however.
3. Start emitting `FStringMiddle` tokens for empty f-strings



---

_@AlexWaygood reviewed on 2024-03-07 13:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:414 on 2024-03-07 13:05_

I didn't implement `Display` but I added a `format_string_contents()` method

---

_@AlexWaygood reviewed on 2024-03-07 13:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:171 on 2024-03-07 13:05_

I've renamed it to `kind`

---

_@AlexWaygood reviewed on 2024-03-07 13:13_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:63 on 2024-03-07 13:13_

@MichaReiser: I made the possible prefixes into an enum, like you suggested. I agree that it makes it much better.

Were you suggesting to incorporate the value of the enum into the bitflag somehow? How would that work exactly?

---

_Comment by @AlexWaygood on 2024-03-07 13:29_

> + [rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py:9:36:](https://github.com/rotki/rotki/blob/f6a86c7a1961f9518a28dd5ced1e9748bb216130/rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py#L9) Q004 [*] Unnecessary escape on inner quote character
> + [rotkehlchen/chain/evm/decoding/cowswap/decoder.py:41:45:](https://github.com/rotki/rotki/blob/f6a86c7a1961f9518a28dd5ced1e9748bb216130/rotkehlchen/chain/evm/decoding/cowswap/decoder.py#L41) Q004 [*] Unnecessary escape on inner quote character

I wish I could say I knew _why_ these changes causes these two lines to be flagged when previously they weren't. But the good news is... I think these are true positives! The `\'` escape inside both strings does seem to be unnecessary, since the string uses double quotes:

```pycon
>>> x = b"\xe7\xeb\x0c\xa1\x1b\x83tN\xce=x\xe9\xbe\x01\xb9\x13B_\xba\xe7\x0c2\xce\'rm\x0e\xcd\xe9.\xf8\xd2"
>>> y = b"\xe7\xeb\x0c\xa1\x1b\x83tN\xce=x\xe9\xbe\x01\xb9\x13B_\xba\xe7\x0c2\xce'rm\x0e\xcd\xe9.\xf8\xd2"
>>> x == y
True
```

I'll add some tests to make sure these don't regress in the future.

---

_Comment by @AlexWaygood on 2024-03-07 13:35_

The codspeed benchmarks are still showing some regressions in the lexer, but are also now showing some speedups in the linter.

---

_@AlexWaygood reviewed on 2024-03-07 14:14_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:3 on 2024-03-07 14:14_

I wanted to put it in its own module because:
- It felt (just) big enough to justify it
- I wanted to make sure it was isolated and didn't depend on any other things in this crate (including things in `string.rs`)
- I wanted to make sure no implementation details of `StringKind` could be used/accessed by other parts of `string.rs`.

But maybe it could live in a separate module inside `string.rs`?

---

_Comment by @AlexWaygood on 2024-03-07 14:19_

Reviews have mostly been addressed -- thanks! The outstanding questions are:
1. What to do about f-string tokens: https://github.com/astral-sh/ruff/pull/10256#discussion_r1515773636
2. Whether `StringKind` et al deserve to be in their own module/file, or whether they should be moved to `string.rs`: https://github.com/astral-sh/ruff/pull/10256#discussion_r1515775011
3. The way `StringKind` stores its data: https://github.com/astral-sh/ruff/pull/10256#discussion_r1516128233
4. Getting a better `Debug` implementation for `StringKind` (depends on resolving (3) first): https://github.com/astral-sh/ruff/pull/10256#discussion_r1515874783

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-03-07 14:19_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-07 14:19_

---

_@MichaReiser reviewed on 2024-03-07 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:3 on 2024-03-07 15:08_

It's fine. We have to move it to the AST crate anyway when we add the flags to the AST nodes

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:26 on 2024-03-07 15:09_

I think I would prefer more explicit names than just the characters. Like `Unicode` or `Raw`

---

_@MichaReiser reviewed on 2024-03-07 15:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:63 on 2024-03-07 15:13_

Hmm yeah. The idea was that you would match on `StringPrefix` and set the corresponding bitflags

```rust
impl StringKind {
    const fn from_prefix(prefix: StringPrefix,..) -> Self {
        let mut flags = StringFlags::empty();
        
        match prefix {
            StringPrefix::Unicode => flags |= StringFlags::Unicode,
            StringPrefix::Raw => flags |= StringFlags::Raw,
            ...
        }
    }

    const fn prefix() -> StringPrefix {
        // not correct, only to show the idea
        if self.is_uniocde() {
            StringPrefix::Unicode
        } else if self.is_raw() {
            StringPrefix::Raw
        } else if {
            ...
        }
    }
}
```

---

_@MichaReiser reviewed on 2024-03-07 15:13_

---

_@AlexWaygood reviewed on 2024-03-07 15:16_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:63 on 2024-03-07 15:16_

Ahh, I see. I'll make the change!

---

_@dhruvmanila reviewed on 2024-03-07 15:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token.rs`:59 on 2024-03-07 15:34_

> Hmm, `FStringMiddle` tracking it makes sense when `FStringStart` didn't track it. But it seems redundant now that `FStringStart` exposes the quote information because the prefix (kind) always matches the prefix of the closest `FStringStart` preceding the `FStringMiddle` (that wasn't closed with `FStringEnd`)

The reason `FStringMiddle` is important is because that's the token which contains the string value of f-strings which gives us access to the value itself and the range. This information is used in multiple places ([here](https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs#L70-L78) for text value, [here](https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_linter/src/rules/pylint/rules/invalid_string_characters.rs#L182-L184) for the range, [here](https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_python_index/src/multiline_ranges.rs#L49-L54)). If we remove this information from `FStringMiddle`, then we need to add some logic to query this based on the given `FStringMiddle`.

I have a hunch that (3) would create a lot of complexities and might require special handling which I want to avoid. It's also not according to the spec.

(1) seems like the simplest to have for now but I think we can at least not have the information in `FStringEnd` which, I think, is never required.

---

_@AlexWaygood reviewed on 2024-03-07 15:35_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:59 on 2024-03-07 15:35_

> (1) seems like the simplest to have for now but I think we can at least not have the information in `FStringEnd` which, I think, is never required.

Good point, I think there are indeed no use cases for carrying the information in `FStringEnd`

---

_@dhruvmanila reviewed on 2024-03-07 15:48_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:178 on 2024-03-07 15:48_

I might be misreading this but I don't think this change is equivalent. We were previously checking whether the string value _contains_ the quotes. So, whether `'` is in `"hello ' world"`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:209 on 2024-03-07 15:49_

Same query here. Is the change equivalent to the previous logic?

---

_@dhruvmanila reviewed on 2024-03-07 15:49_

---

_Comment by @dhruvmanila on 2024-03-07 15:54_

> 1. What to do about f-string tokens: [Track quoting style in the tokenizer #10256 (comment)](https://github.com/astral-sh/ruff/pull/10256#discussion_r1515773636)

I would recommend to go with `FStringStart` and `FStringMiddle`. I think the next steps, as mentioned in the PR description, is to encode the information in the AST. The parser would use the `FStringStart` token to get the information. Later, we could potentially move some of the rules from token-based to AST-based which would then make having this information in `FStringMiddle` not required.

We could also have a subset of information stored in the `FStringMiddle` but I think that's what we're trying to avoid here.

---

_@AlexWaygood reviewed on 2024-03-07 16:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:178 on 2024-03-07 16:50_

I don't think so. The previous condition is:

```rs
                if !leading_quote(locator.slice(tok_range)).is_some_and(|text| {
                    contains_quote(text, quotes_settings.inline_quotes.as_char())
                })
```

So that's:
1. Grab the source-code for the string-token's range
2. Call `leading_quote()` on the result of (1) to get the first quote mark that appears in that source-code string. The result of this call will be `Option<&str>`. Store this result in a variable named `text`.
3. If `text` is `Some(&str)`, iterate through it to see whether this string contains a character representing the user's preferred quotation style

So we're not iterating through the string token's value to see whether it contains the quote, I don't think -- we're only iterating through a string that represents the opening quote in the string token's source-code range

---

_@dhruvmanila reviewed on 2024-03-07 16:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:178 on 2024-03-07 16:58_

Oh I see. I mis-understood that. Thanks for clarifying :)

---

_@AlexWaygood reviewed on 2024-03-07 16:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:178 on 2024-03-07 16:59_

No problem!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-07 16:59_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-03-07 16:59_

---

_Comment by @AlexWaygood on 2024-03-07 17:46_

The performance regressions in the lexer seem to have disappeared from the benchmarks. Some of [the benchmarks on codspeed](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:quotestyle-tokenizer) are still showing regressions of around 1%, but I can't really make much sense of them -- I _think_ they're just noise (correct me if I'm wrong!). There's also some nice speedups on some of the lint rules that have been reworked as part of this PR to make use of the information that's now tracked in the tokens:

<details>
<summary>Performance improvements on some lint rules</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/899be95e-b488-4016-ab21-b02d116204fd)
![image](https://github.com/astral-sh/ruff/assets/66076021/3f6fa695-5307-4a87-bc3c-9492a2a9548d)

</details>

Overall, codspeed measures this PR as performance-neutral.

---

_Comment by @charliermarsh on 2024-03-07 22:15_

(Consider me signed off, though I'll leave it to Micha / Dhruv to give the green light!)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string_token_flags.rs`:47 on 2024-03-08 03:20_

nit: we can have the same names as in `StringPrefix` for consistency

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string_token_flags.rs`:170 on 2024-03-08 03:23_

nit: I'd prefer to have the `string` prefix separate for readability

```suggestion
    /// Does the string have a `u` or `U` prefix?
    pub const fn is_u_string(self) -> bool {
        self.0.contains(StringFlags::U_PREFIX)
    }

    /// Does the string have an `r` or `R` prefix?
    pub const fn is_raw_string(self) -> bool {
        self.0.contains(StringFlags::R_PREFIX)
    }

    /// Does the string have an `f` or `F` prefix?
    pub const fn is_f_string(self) -> bool {
        self.0.contains(StringFlags::F_PREFIX)
    }

    /// Does the string have a `b` or `B` prefix?
    pub const fn is_byte_string(self) -> bool {
        self.0.contains(StringFlags::B_PREFIX)
    }
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__triple_quoted_windows_eol.snap`:13 on 2024-03-08 03:28_

I like this!

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer/fstring.rs`:23 on 2024-03-08 03:32_

Can we add a `debug_assert` here as a sanity check to make sure that the `kind` is only a f-string?
```rs
    pub(crate) const fn new(kind: StringKind, nesting: u32) -> Self {
		debug_assert!(kind.is_f_string());
		// ...
	}
```

---

_@dhruvmanila approved on 2024-03-08 03:40_

Great work here! Small nits but otherwise good to go from my side.

---

_@AlexWaygood reviewed on 2024-03-08 07:17_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__triple_quoted_windows_eol.snap`:13 on 2024-03-08 07:17_

Credit to @MichaReiser for the idea!

---

_@AlexWaygood reviewed on 2024-03-08 08:22_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:47 on 2024-03-08 08:22_

I have a weak preference for keeping them "inconsistent", as I think it emphasises the fact that just because something has the `R_PREFIX` flag set doesn't necessarily mean that's the only prefix it has -- it might have other prefixes as well. In that way, it's semantically different to `StringPrefix` in an important way, since `StringPrefix` enumerates all the ways in which the prefixes can be validly _combined_, whereas the bitflag does not.

---

_Comment by @AlexWaygood on 2024-03-08 08:34_

Thanks, all! On to the AST 🚀

---

_Merged by @AlexWaygood on 2024-03-08 08:40_

---

_Closed by @AlexWaygood on 2024-03-08 08:40_

---

_Branch deleted on 2024-03-08 23:13_

---
