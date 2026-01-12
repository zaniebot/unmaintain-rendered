```yaml
number: 16543
title: "[syntax-errors] PEP 701 f-strings before Python 3.12"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-f-strings
created_at: 2025-03-06T20:36:02Z
updated_at: 2025-03-18T15:12:17Z
url: https://github.com/astral-sh/ruff/pull/16543
synced_at: 2026-01-12T15:55:55Z
```

# [syntax-errors] PEP 701 f-strings before Python 3.12

---

_@ntBre_

## Summary

This PR detects the use of PEP 701 f-strings before 3.12. This one sounded difficult and ended up being pretty easy, so I think there's a good chance I've over-simplified things. However, from experimenting in the Python REPL and checking with [pyright], I think this is correct. pyright actually doesn't even flag the comment case, but Python does.

I also checked pyright's implementation for [quotes](https://github.com/microsoft/pyright/blob/98dc4469cc5126bcdcbaf396a6c9d7e75dc1c4a0/packages/pyright-internal/src/analyzer/checker.ts#L1379-L1398) and [escapes](https://github.com/microsoft/pyright/blob/98dc4469cc5126bcdcbaf396a6c9d7e75dc1c4a0/packages/pyright-internal/src/analyzer/checker.ts#L1365-L1377) and think I've approximated how they do it.

Python's error messages also point to the simple approach of these characters simply not being allowed:

```pycon
Python 3.11.11 (main, Feb 12 2025, 14:51:05) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> f'''multiline {
... expression # comment
... }'''
  File "<stdin>", line 3
    }'''
        ^
SyntaxError: f-string expression part cannot include '#'
>>> f'''{not a line \
... continuation}'''
  File "<stdin>", line 2
    continuation}'''
                    ^
SyntaxError: f-string expression part cannot include a backslash
>>> f'hello {'world'}'
  File "<stdin>", line 1
    f'hello {'world'}'
              ^^^^^
SyntaxError: f-string: expecting '}'
```

And since escapes aren't allowed, I don't think there are any tricky cases where nested quotes or comments can sneak in.

It's also slightly annoying that the error is repeated for every nested quote character, but that also mirrors pyright, although they highlight the whole nested string, which is a little nicer. However, their check is in the analysis phase, so I don't think we have such easy access to the quoted range, at least without adding another mini visitor.

## Test Plan

New inline tests

[pyright]: https://pyright-play.net/?pythonVersion=3.11&strict=true&code=EYQw5gBAvBAmCWBjALgCgO4gHaygRgEoAoEaCAIgBpyiiBiCLAUwGdknYIBHAVwHt2LIgDMA5AFlwSCJhwAuCAG8IoMAG1Rs2KIC6EAL6iIxosbPmLlq5foRWiEAAcmERAAsQAJxAomnltY2wuSKogA6WKIAdABWfPBYqCAE%2BuSBVqbpWVm2iHwAtvlMWMgB2ekiolUAgq4FjgA2TAAeEMieSADWCsoV5qoaqrrGDJ5MiDz%2B8ABuLqosAIREhlXlaybrmyYMXsDw7V4AnoysyAmQ5SIhwYo3d9cheADUeKlv5O%2BpQA

---

_Label `parser` added by @ntBre on 2025-03-06 20:36_

---

_Label `preview` added by @ntBre on 2025-03-06 20:36_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-06 20:36_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-06 20:36_

---

_Comment by @github-actions[bot] on 2025-03-06 20:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @ntBre on 2025-03-07 03:00_

I thought of some additional test cases tonight:

```pycon
Python 3.11.11 (main, Feb 12 2025, 14:51:05) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> f"{"""x"""}"
  File "<stdin>", line 1
    f"{"""x"""}"
          ^
SyntaxError: f-string: expecting '}'
>>> f'{'''x'''}'
  File "<stdin>", line 1
    f'{'''x'''}'
          ^
SyntaxError: f-string: expecting '}'
>>> f"""{"x"}"""
'x'
>>> f'''{'x'}'''
'x'
```

I'm pretty sure the code here handles these but it might be nice to add them as tests. I was especially concerned about the first two but checking for the outer `quote_str` should capture the right behavior.

---

_@dhruvmanila reviewed on 2025-03-07 03:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1368 on 2025-03-07 03:17_

I think this will give you false positive when a nested valid string inside an f-string contains a `#` character:
```py
❯ uv run --python=3.9 --no-project python -q                
>>> f"outer {'# not a comment'}"
'outer # not a comment'
```

---

_@MichaReiser reviewed on 2025-03-07 08:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1378 on 2025-03-07 08:52_

This gives you false positives for triple quoted strings:

```
f"{f'''{"nested"} inner'''} outer"
```

---

_@MichaReiser reviewed on 2025-03-07 08:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1358 on 2025-03-07 08:53_

I think this also gives you false positives for

```py
>>> f"test \
... more"
'test more'
```

---

_@MichaReiser reviewed on 2025-03-07 08:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1358 on 2025-03-07 08:56_

ah no, I think this should be fine because you only iterate over expressions. It might still be worth adding a test for it as well as for 

```
f"test {a \
    } more"
```

---

_@MichaReiser reviewed on 2025-03-07 09:04_

Maybe take a look at https://github.com/astral-sh/ruff/blob/1ecb7ce6455a4a9a134fe8536625e89f74e3ec5b/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py 

and 

https://github.com/astral-sh/ruff/blob/82faa9bb62e66a562f8a7ad81a645162ca558a08/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring_preview.py

they contain a good enumeration of the tricky cases.

It does make me slightly nervous that the current approach does a lot oparations on the source text directly instead of analyzing the tokens but accessing the tokens might require making this an analyzer (linter) check

---

_@dhruvmanila reviewed on 2025-03-07 11:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1368 on 2025-03-07 11:06_

And, in the format specifier. The following is valid as per Python 3.8 parser, it's the format specifier which is invalid (unrelated to the parser) and format specifiers are part of the f-string expression node:
```py
f'outer {x:{"# not a comment"} }'
```

---

_Comment by @dhruvmanila on 2025-03-07 11:16_

> It does make me slightly nervous that the current approach does a lot oparations on the source text directly instead of analyzing the tokens but accessing the tokens might require making this an analyzer (linter) check

Yeah, and f-strings are tricky because there's a lot more involved in here.

Another approach here would be to use the tokens either in the parser or in the analyzer (which you've mentioned), more preference towards in the parser mainly because it already has the surrounding context i.e., are we in a nested f-string or are we in f-string expression? 

Maybe we could do this in the lexer itself and utilize `FStringErrorType` to emit the errors which then the parser would convert into `UnsupportedSyntaxError` but I haven't explored this option. In the lexer, it would be easier to just check for `Comment`, `Newline` tokens when in a f-string expression mode and emit the errors. My main worry regarding the lexer would be any performance implications.



---

_Comment by @ntBre on 2025-03-07 15:01_

Oof, thanks for the reviews. I had a feeling I over-simplified things, but these false positives look quite obvious in hindsight. I'll mark this as a draft for now and take a deeper look at this today.

---

_Converted to draft by @ntBre on 2025-03-07 15:01_

---

_@ntBre reviewed on 2025-03-07 15:20_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:1378 on 2025-03-07 15:20_

I think this one actually is invalid:

```pycon
Python 3.11.11 (main, Feb 12 2025, 14:51:05) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> f"{f'''{"nested"} inner'''} outer"
  File "<stdin>", line 1
    f"{f'''{"nested"} inner'''} outer"
             ^^^^^^
SyntaxError: f-string: unterminated string
```

---

_Comment by @ntBre on 2025-03-07 16:31_

I still need to look for more tricky cases in the formatter fixtures, but I checked on the suggested escape and quote test cases, and I believe those are true positives (I also added them as tests). So the main issues here are around comments, which might be quite tricky (maybe this is why pyright doesn't flag them?) and around inspecting the source text directly.

---

_Comment by @MichaReiser on 2025-03-10 11:37_

I think it would be helpful to do summarize the invalid patterns that we need to detect. It will help us decide:

* How to best detect those (tokens, AST pass, parser, lexer, all of it?)
* which patterns are easy/hard to detect

Based on this we can decide on the approach but also the  prioritisation of what the check should detect and we can even split it up into multiple PRs. 

---

_Comment by @ntBre on 2025-03-10 12:47_

> I think it would be helpful to do summarize the invalid patterns that we need to detect. It will help us decide:
> 
> * How to best detect those (tokens, AST pass, parser, lexer, all of it?)
> * which patterns are easy/hard to detect
> 
> Based on this we can decide on the approach but also the prioritisation of what the check should detect and we can even split it up into multiple PRs.

That's a good idea, thanks. The three main cases I took away from the PEP were:
1. Nested quotes
2. Escape sequences
3. Comments

Escape sequences seem to be the easiest because as far as I can tell, CPython throws an error for any `\` in an f-string expression part, whether it's part of an escape character (`\n`) or looks like a line-continuation character.

I think quotes are also easy because any nested `quote_str` (in our parlance) ends the string. That still feels oversimplified but I haven't seen any cases to the contrary. The PEP also includes this example:

> In fact, this is the most nested-fstring that can be written:
```pycon
>>> f"""{f'''{f'{f"{1+1}"}'}'''}"""
'2'
```

Comments are the hardest because you can't just check for `#` as Dhruv pointed out because that's a valid character inside of strings within the f-string.

Those are the three cases I attempted to fix here. 

I see now in [PEP 498] that "Expressions cannot contain `':'` or `'!'` outside of strings or parentheses, brackets, or braces. The exception is that the `'!='` operator is allowed as a special case." So that might be a fourth case we'd want to consider. At least initially it sounds roughly as complex as detecting comments.

[PEP 498]: https://peps.python.org/pep-0498/#specification

---

_Comment by @MichaReiser on 2025-03-12 07:27_

We discussed a possible approach in our 1:1. @ntBre let me know if that doesn't work and i can take another look

---

_@MichaReiser reviewed on 2025-03-14 07:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:17 on 2025-03-14 07:43_

I think I'd prefer an `in_range` method or similar instead of making `tokens `pub(crate)`

---

_Comment by @ntBre on 2025-03-14 22:36_

Thanks for the `in_range` suggestion! I factored out part of `Tokens::in_range` to reuse in the new `TokenSource::in_range` method, which made things much simpler.

I tried applying a similar strategy to quotes, but `FStringStart`, `FStringEnd`, and `FStringMiddle` all carry their own string flags, so it's not easy to differentiate between the inner and outer f-strings. Maybe I could bring back the stack from the previous implementation to track that, though.

I still think comparing the `quote_str` gets the correct answer because it includes triple quotes, but I'm still open to reworking that if you prefer. I could at least use `memmem` and `memchr` for the searches.

Similarly, I don't think `\` is a token, so we pretty much have to do a text search for that, as far as I can tell.

I also looked into the `:` and `!` mention from PEP 498 again, but I can't come up with anything that is valid syntax after 3.12 either. So I think it's okay not to check for those specially.

---

_Marked ready for review by @ntBre on 2025-03-14 22:37_

---

_Converted to draft by @ntBre on 2025-03-15 04:54_

---

_Comment by @MichaReiser on 2025-03-17 08:32_

> I still think comparing the quote_str gets the correct answer because it includes triple quotes, but I'm still open to reworking that if you prefer. I could at least use memmem and memchr for the searches.

Yeah, that could work. An alternative is to inspect the parsed AST. What's important is that we only run the search over expression parts (e.g. `f"test\"abcd"` is valid)

> I also looked into the : and ! mention from PEP 498 again, but I can't come up with anything that is valid syntax after 3.12 either. So I think it's okay not to check for those specially.

Do you have a reference from PEP701 that anything changed related to `:` and `!` handling?

---

_Comment by @ntBre on 2025-03-17 13:12_

> Do you have a reference from PEP701 that anything changed related to `:` and `!` handling?

No, it sounds like the same restrictions are in place in PEP 701:
- > We have decided not to lift the restriction that some expression portions need to wrap `:` and `!` in parentheses at the top level

 They were just mentioned along with comments and backslashes as receiving special treatment in PEP 498, so I was worried that they could have changed, but this sounds pretty conclusive after looking again, thanks!

I left this in draft because I wanted to run the new code on the formatter tests you linked above. I'll do that now and then open it for review again.

I also just added your `f"test\"abcd"` case as a test and will try out `memchr`.

---

_Comment by @ntBre on 2025-03-17 14:25_

I manually tested these out on the formatter test fixtures and all of the errors looked like true positives. Would it be worth adding those as permanent parser tests? It seemed weird to refer to test files in a different crate, but I could duplicate them. Hopefully I've already captured the key subset in the inline tests, though.

---

_Marked ready for review by @ntBre on 2025-03-17 15:03_

---

_@dhruvmanila reviewed on 2025-03-17 15:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:579 on 2025-03-17 15:55_

Do we need the same pattern for the `after` API? I think it's only the `in_range_impl` which is being shared?

---

_@dhruvmanila reviewed on 2025-03-17 15:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1437 on 2025-03-17 15:56_

This confused me a bit until I read the comment, maybe we should use `self.options.target_version.supports_pep_701` instead?

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:579 on 2025-03-17 16:23_

You're right that only `in_range_impl` is called elsewhere, but `in_range_impl` uses `after_impl` internally. I didn't see another clean way of doing that.

---

_@ntBre reviewed on 2025-03-17 16:23_

---

_@ntBre reviewed on 2025-03-17 16:28_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:1437 on 2025-03-17 16:28_

That's much nicer to read, thanks!

---

_@MichaReiser reviewed on 2025-03-17 16:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:579 on 2025-03-17 16:46_

I would probably not do a binary search here and instead assume that `after` is close to the end. That means, you can simply search from the back and find the last token that starts at `after`.

Edit: I see now that this in `Tokens`. I would suggest not using `Tokens` in `TokenSource` and instead handroll the implementation (it's very simple) by doing a backward search

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1442 on 2025-03-17 16:48_

It should also be okay to call `unwrap` here. The fact that we were able to parse the expression guarantees that `slash_index < u32::MAX`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1438 on 2025-03-17 16:48_

Should we highlight all backslash usages instead of just the first?

---

_@MichaReiser approved on 2025-03-17 16:49_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:579 on 2025-03-17 20:29_

Sounds good, that makes sense that `Tokens` and its binary search are overkill here. I can revert these and update the `TokenSource::in_range` implementation.

---

_@ntBre reviewed on 2025-03-17 20:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1439 on 2025-03-18 03:26_

nit: what about `slash_position`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1443 on 2025-03-18 03:27_

Is `TextSize::from(1)` to represent the size of `/` ? If so, we generally use `.text_len` instead to reflect what this size indicate:
```rs
'/'.text_len()
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:182 on 2025-03-18 03:35_

Do you plan on using this method elsewhere?

If not, we could inline the logic in `check_fstring_comments` and simplify it to avoid the iteration for the `end` variable as, I think, the parser is already at that position? So, something like what Micha suggested in https://github.com/astral-sh/ruff/pull/16543#discussion_r1999208228 i.e., just iterate over the tokens in reverse order until we reach the f-string start and report an error for all the `Comment` tokens found.

---

_@dhruvmanila approved on 2025-03-18 03:36_

Looks good! I just have a couple of minor comments, feel free to make any relevant changes if required otherwise merge it as is.

---

_@ntBre reviewed on 2025-03-18 13:08_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/token_source.rs`:182 on 2025-03-18 13:08_

I think we need/want a method of some kind because `TokenSource::tokens` is a private field. I could just add a `tokens` getter though, of course.

I also tried this without `end`, but cases like

```python
f'Magic wand: { bag['wand'] }'     # nested quotes
```

caught new errors on the trailing comment. At the point we do this processing, we've bumped past the `FStringEnd` *and* any trivia tokens after it, so I think we do need to find the `end` point as well.

Hmm, maybe a `tokens` getter would be nicest. Then I could do all of the processing on a single iterator in `check_fstring_comments` at least.

---

_@ntBre reviewed on 2025-03-18 13:28_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:1443 on 2025-03-18 13:28_

Ahh yes, I've seen that pattern before but forgot to use it. Thanks!

---

_@dhruvmanila reviewed on 2025-03-18 13:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:182 on 2025-03-18 13:32_

Can we not use the f-string range directly? Or, is there something else I'm missing? I don't think the comment is part of the f-string range.

---

_@dhruvmanila reviewed on 2025-03-18 13:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:182 on 2025-03-18 13:35_

So, the `node_range` calculation avoids any trailing trivia tokens like the one that you've mentioned in the example above. This is done by keeping track of the end of the  previous token which excludes some tokens like comment. Here, when you call `node_range`, then it will give you the range which doesn't include the trailing comment. If it wouldn't then the f-string range would be incorrect here.

---

_@dhruvmanila reviewed on 2025-03-18 13:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:182 on 2025-03-18 13:36_

Oh, shoot, I think the `tokens` field should still include the trailing comment. Happy to go with what you think is best here.

---

_@ntBre reviewed on 2025-03-18 13:50_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/token_source.rs`:182 on 2025-03-18 13:50_

Yeah I think that's a good summary. We have the exact f-string range but need to match that up with the actual `Token`s in the `tokens` field, which includes trailing comments.

I tried the `tokens` getter and moving the logic into `check_fstring_comments`, but I do aesthetically prefer how it looked with `self.tokens.in_range...` even if the `in_range` method itself looks a little weird. So I might just leave it alone for now. Thanks for double checking!

---

_Merged by @ntBre on 2025-03-18 15:12_

---

_Closed by @ntBre on 2025-03-18 15:12_

---

_Branch deleted on 2025-03-18 15:12_

---
