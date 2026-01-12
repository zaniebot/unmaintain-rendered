```yaml
number: 3921
title: "Implement `flake8_todos`"
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: flake8_todo
created_at: 2023-04-09T02:28:07Z
updated_at: 2023-05-13T16:05:11Z
url: https://github.com/astral-sh/ruff/pull/3921
synced_at: 2026-01-12T03:50:02Z
```

# Implement `flake8_todos`

---

_Pull request opened by @evanrittenhouse on 2023-04-09 02:28_

Completes #3870

Will be able to easily clear #3859 when this lands as well.

---

_Comment by @github-actions[bot] on 2023-04-09 02:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.7±0.39ms     2.4 MB/sec    1.00     16.5±0.27ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.03ms     4.1 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.6±5.34µs     6.0 MB/sec    1.00    489.5±3.05µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.05ms     5.1 MB/sec    1.00      8.0±0.09ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1730.9±42.19µs     9.6 MB/sec    1.00  1704.6±15.47µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.02   201.4±16.07µs    14.7 MB/sec    1.00    198.3±6.31µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.4±0.03ms     6.3 MB/sec    1.00      6.4±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1247.8±11.91µs    13.3 MB/sec    1.01   1256.3±4.22µs    13.3 MB/sec
parser/numpy/globals.py                    1.05    132.1±3.26µs    22.3 MB/sec    1.00    125.5±1.78µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.02ms     9.3 MB/sec    1.00      2.7±0.02ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.23ms     2.4 MB/sec    1.00     17.0±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.07ms     3.9 MB/sec    1.00      4.3±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.7±8.84µs     5.9 MB/sec    1.00   502.2±17.98µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.10ms     3.6 MB/sec    1.02      7.2±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.08ms     4.9 MB/sec    1.01      8.4±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1782.8±30.13µs     9.3 MB/sec    1.00  1779.4±39.27µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.3±3.56µs    15.1 MB/sec    1.04    203.2±9.25µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.7±0.05ms     6.0 MB/sec    1.00      6.7±0.06ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1291.7±25.15µs    12.9 MB/sec    1.00  1285.8±27.40µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    130.9±2.78µs    22.5 MB/sec    1.00    130.5±2.24µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.8 MB/sec    1.00      2.9±0.06ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:17 on 2023-04-09 03:01_

Do we actually want to do this? If not, I'll remove this comment when I implement T006.

---

_@evanrittenhouse reviewed on 2023-04-09 03:01_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/checkers/tokens.rs`:69 on 2023-04-09 03:01_

I'll start working on these once this PR is merged

---

_@evanrittenhouse reviewed on 2023-04-09 03:01_

---

_Marked ready for review by @evanrittenhouse on 2023-04-09 03:40_

---

_Review comment by @dhruvmanila on `crates/ruff/src/checkers/tokens.rs`:70 on 2023-04-09 05:43_

As per the [naming convention](https://beta.ruff.rs/docs/contributing/#rule-naming-convention), I think these should be named `Missing[X]InTODO` (_allow missing author in TODO_) where `X` is the missing part.

---

_Review comment by @dhruvmanila on `crates/ruff/src/registry.rs`:802 on 2023-04-09 05:46_

I think it's better to avoid single letter prefix for posterity, maybe `TOD`, `TDO`, `TD`?

---

_@dhruvmanila reviewed on 2023-04-09 05:54_

---

_@evanrittenhouse reviewed on 2023-04-09 16:09_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/registry.rs`:802 on 2023-04-09 16:09_

Do we use our own error codes? If so, I'm happy to change it - I only used `T` because [flake8_todos](https://github.com/orsinium-labs/flake8-todos) uses it.

---

_Review comment by @evanrittenhouse on `crates/ruff/src/checkers/tokens.rs`:70 on 2023-04-09 16:22_

Changed!

---

_@evanrittenhouse reviewed on 2023-04-09 16:22_

---

_Review comment by @dhruvmanila on `crates/ruff/src/registry.rs`:802 on 2023-04-09 17:43_

> Do we use our own error codes?

We do to avoid collisions as there are multiple error groups in `ruff` as compared to a single group in individual plugins. Example: [flake8-tidy-imports (TID)](https://beta.ruff.rs/docs/rules/#flake8-tidy-imports-tid) vs [Plugin repo (I)](https://github.com/adamchainz/flake8-tidy-imports#rules). But, let @charliermarsh's take the final call on this.

---

_@dhruvmanila reviewed on 2023-04-09 17:43_

---

_@charliermarsh reviewed on 2023-04-09 22:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:802 on 2023-04-09 22:30_

Yeah that's right -- we typically use three-letter prefixes to avoid collisions, even if it deviates from upstream. I guess `TOD` or `TDO` are equally reasonable here.

---

_@charliermarsh reviewed on 2023-04-09 22:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todo/rules.rs`:14 on 2023-04-09 22:31_

The [Rust naming convention](https://rust-lang.github.io/api-guidelines/naming.html) suggests treating acronyms as one word, so all these should be e.g. `InvalidTodoTag`.

---

_@evanrittenhouse reviewed on 2023-04-09 23:57_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:14 on 2023-04-09 23:57_

Done

---

_Review comment by @evanrittenhouse on `crates/ruff/src/registry.rs`:802 on 2023-04-10 02:03_

Done

---

_@evanrittenhouse reviewed on 2023-04-10 02:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:331 on 2023-04-11 07:06_

Using `capture` groups is kind of expensive. Is it necessary to get the captures or is it sufficient to only test if the regex matches?

---

_@MichaReiser reviewed on 2023-04-11 07:07_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/mod.rs`:27 on 2023-04-11 09:04_

You'll have to change this to `assert_messages` after #3906 lands. It pretty prints the diagnostics instead of serializing them to yaml. 

```suggestion
        assert_messages!(snapshot, diagnostics);
```

---

_@MichaReiser reviewed on 2023-04-11 09:05_

---

_@evanrittenhouse reviewed on 2023-04-11 12:07_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:331 on 2023-04-11 12:07_

The only two alternatives I could think of were:
1. Write different regexes for each combination of successful/failed rules.
2. Just capture a TODO tag and perform string manipulations on the line (for example, split at a colon and ensure that the next character is a space). This _could _ involve additional regexes, though I'd have to think about it more. 

Neither option felt sufficient to warrant using it over capture groups to me, but happy to change approaches if you'd like. 

Out of curiosity, how do you know that regex captures are expensive?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:331 on 2023-04-11 15:21_

The regex crate documentation has some notes on performance and recommend to only [use what you need](https://github.com/rust-lang/regex/blob/master/PERFORMANCE.md#only-ask-for-what-you-need). That's why I asked if we "need" it, because, if not, it would be an easy performance win. But it's okay to use it if we need it. 

You can see some real-world wins in #3735

 

---

_@MichaReiser reviewed on 2023-04-11 15:21_

---

_@evanrittenhouse reviewed on 2023-04-11 15:29_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/mod.rs`:27 on 2023-04-11 15:29_

Sounds good!

---

_@evanrittenhouse reviewed on 2023-04-11 15:34_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:331 on 2023-04-11 15:34_

Oh cool, thanks!

 I think that captures are definitely the easiest solution. Matching is very tough since there could be missing pieces at any point in the comment. I checked the benchmarks above and they seem alright. 

If it's cool with you, I'm okay to move forward with this approach - if there are any slowdowns in the future, we could revisit this using `find`? Only issue I see with that for now is that the implementation could be relatively difficult, I think.

---

_@MichaReiser reviewed on 2023-04-12 07:21_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:331 on 2023-04-12 07:21_

Sounds good to me!

---

_Renamed from "Implement T001, T002, T004, T005, and T007" to "Implement TDO001, TDO002, TDO004, TDO005, TDO006, and TDO007" by @evanrittenhouse on 2023-04-13 17:52_

---

_Comment by @evanrittenhouse on 2023-04-14 02:40_

@charliermarsh Would you prefer I implement TDO003 in this PR as well, or do we allow partial implementation of plugins?

---

_Comment by @charliermarsh on 2023-04-14 02:48_

Partial is cool, I just haven't had time to sit down and dig in on this one yet.

---

_Comment by @evanrittenhouse on 2023-04-14 02:50_

Sorry, didn't mean to rush you at all! Just the first time I've tried implementing a plugin, so wasn't sure how we handle that. Please take your time

---

_@evanrittenhouse reviewed on 2023-04-15 15:50_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:121 on 2023-04-15 15:50_

`get_captured_matches()` and `get_tag_regex_errors()` both call `TODO_REGEX.captures_iter()`, an expensive operation. I'll need to refactor that to only call it once and pass the capture groups around before this gets merged.

---

_Comment by @evanrittenhouse on 2023-04-15 19:47_

No idea why these errors are happening - will take a closer look in a bit

E: had to bump the ruleset

---

_Renamed from "Implement TDO001, TDO002, TDO004, TDO005, TDO006, and TDO007" to "Implement flake8_todo" by @evanrittenhouse on 2023-04-17 00:56_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:247 on 2023-04-23 13:40_

Nit: It could be worth using [`RegexSet`](https://docs.rs/regex/latest/regex/struct.RegexSet.html) to avoid scanning the same text multiple times

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:294 on 2023-04-23 13:41_

Should we use an `if-else` here to avoid two diagnostics at the same location?

```suggestion
			if tag.to_uppercase() == "TODO" {
                    diagnostics_ref.push(Diagnostic::new(
                        InvalidCapitalizationInTodo {
                            tag: String::from(tag),
                        },
                        range,
                    ));
                } else {
	                diagnostics_ref.push(Diagnostic::new(
	                    InvalidTodoTag {
	                        tag: String::from(tag),
	                    },
	                    range,
	                ));
			}   
```

---

_@MichaReiser reviewed on 2023-04-23 13:42_

---

_@evanrittenhouse reviewed on 2023-04-23 13:53_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:294 on 2023-04-23 13:53_

I'm not sure how heavily we frown upon multiple diagnostics in one spot - I thought it'd be fine in this instance since a "toDo", for example, also isn't the same as "TODO", so it's technically both invalid and capitalized incorrectly.

Definitely open to making them exclusive though!

---

_@evanrittenhouse reviewed on 2023-04-23 13:54_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:247 on 2023-04-23 13:54_

Great point! Didn't realize that exits - I'll change it in a couple
hours, out right now  

---

_@evanrittenhouse reviewed on 2023-04-23 14:15_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:294 on 2023-04-23 14:15_

On that note, @MichaReiser is it worth implementing autofixes for these rules? I think we could do it for all of them except "missing author" and "missing link".

E: Going to implement an autofix on TDO006 for now. If there's interest I can check into how we'd do the others

---

_Comment by @evanrittenhouse on 2023-04-25 02:00_

Re-marking as draft to fix an edge case and clean up the code a little bit

---

_Converted to draft by @evanrittenhouse on 2023-04-25 02:00_

---

_Renamed from "Implement flake8_todo" to "Implement `flake8_todo`" by @evanrittenhouse on 2023-04-25 03:27_

---

_@evanrittenhouse reviewed on 2023-04-25 15:52_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:250 on 2023-04-25 15:52_

This won't lead to false positives since we update the variable on each TODO. Each link check (the only place this matters) will therefore have an updated value.

---

_Comment by @evanrittenhouse on 2023-04-25 16:25_

Sorry for the false starts. I believe this is good to go now

---

_Marked ready for review by @evanrittenhouse on 2023-04-25 16:25_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:245 on 2023-05-01 08:32_

We can speed this up using `Indexer.comment_ranges()`. It gives you the ranges of all comments and you can slice into the text by using `Locator.slice`

```
for range in indexer.comment_ranges() {
	let comment = locator.slice(*range);
	...
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:83 on 2023-05-01 08:35_

Should we document the allowed issue-links (including ids `TODO-1234`)?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:274 on 2023-05-01 08:39_

Do we ned to add a check for this at the end of the file. E.g. 

```python
# TODO
```

No link at the end. 

An alternative would be to use a peekable iterator and perform a lookahead to see if the next comment is only separated by whitespace from this comment (or a single line break even and whitespace), and contains an issue link

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:227 on 2023-05-01 08:42_

Can we use a case-insensitive match instead of writing out all the different combinations of TODO? Should we also match on `fixme` or `FixMe`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:330 on 2023-05-01 08:47_

I wonder if it would improve performance if we avoid capture groups altogether by:

* Using a Regex to find any occurrences of `TODO`, `FIXME` or `BUG` (`regex.find(text)`)
* Then use `str` methods to test for author, colon, space, etc). 

I might be wrong but my impression is that it shouldn't be too hard to write and it has the benefit that we have fewer regex (that I personally find hard to understand and maintain) and less capture group matching. What do you think?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:383 on 2023-05-01 08:49_

You can use `comment.find(['t', 'T'])`. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__invalid-capitalization-in-todo_TDO006.py.snap`:9 on 2023-05-01 08:50_

Highlighting the whole comment can become very noisy for long comments. I think it would be better only to highlight the problematic section

```suggestion
6 | # ToDo(evanrittenhouse): invalid capitalization
  |      ^^^^  TDO006
```

This applies to all diagnostics in this PR.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-author-in-todo_TDO002.py.snap`:9 on 2023-05-01 08:52_

```suggestion
7 | # TODO: this has no author
  |                ^ TDO002
```

Carret should be at the `:` to highlight where the author name was expected.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-author-in-todo_TDO002.py.snap`:4 on 2023-05-01 08:53_

Should we extend the message to explain where the author is expected or give an example?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-colon-in-todo_TDO004.py.snap`:9 on 2023-05-01 08:53_

```suggestion
6 | # TODO(evanrittenhouse) this has no colon
  |                                                ^ TDO004
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-link-in-todo_TDO003.py.snap`:8 on 2023-05-01 08:55_

Is the link expected to always appear at the end or on the next line? 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-space-after-colon-in-todo_TDO007.py.snap`:9 on 2023-05-01 08:55_

```suggestion
6 | # TODO(evanrittenhouse):this has no space after a colon
  |                                                 ^ TDO007
```

---

_@MichaReiser approved on 2023-05-01 08:55_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_todo/rules.rs`:175 on 2023-05-01 11:29_

Autofix titles gives suggestion on what to do, so I think this might be more helpful and inline with other titles:
```suggestion
        let InvalidCapitalizationInTodo { tag } = self;
        "Replace `{tag}` with `TODO`".to_string()
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_todo/rules.rs`:310 on 2023-05-01 11:46_

nit: Should we use the same pattern as used at other places? Define a mutable diagnostic, check if autofix is enabled and then update the diagnostic with the edits.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_todo/rules.rs`:253 on 2023-05-01 11:52_

Just curious to know if this is beneficial. If it is, shouldn't it be _outside_ the loop?

---

_@dhruvmanila reviewed on 2023-05-01 11:52_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:330 on 2023-05-01 15:13_

I can definitely look into it. My worry with `str` methods is that the rules may not all be applied together, so testing for stuff may get tricky. I'm sure there's a way, but I'll have to check it out. 

For example, testing for a colon in `# TODO: test`, `# TODO we need to fix a few things: a, b, and c` and `# TODO (evanrittenhouse) test` is tough because you don't know where the colon should be. Should it be after the closing parenthesis of an author tag - that may not exist. We can't map it off of an offset/column since we don't know how long the author/tag is. We also can't just test if a colon is in the string since it could be in the TODO's text. 

Again, I'm sure there's a way, and I agree that it'd be easier to reason about than regex capture groups. Just haven't had a chance to sit down and think about what the detection code would look like :)

---

_@evanrittenhouse reviewed on 2023-05-01 15:13_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:274 on 2023-05-01 15:14_

I like the idea of a peekable iterator. I'll rework the code to perform a lookahead on one of those rather than the current lookbehind - thanks for the idea!

---

_@evanrittenhouse reviewed on 2023-05-01 15:14_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:330 on 2023-05-01 15:24_

> For example, testing for a colon in # TODO: test, # TODO we need to fix a few things: a, b, and c and # TODO (evanrittenhouse) test 

My understanding is that the colon should either come directly after the `TODO` or after the author. Is this correct? If so, then we would need to skip the "author" part even if the author rule is disabled. This isn't any different than what the `Regex` is doing today, it just happens implicitly. 

I'm also OK merging as is and playing around with string methods as a separate PR.

---

_@MichaReiser reviewed on 2023-05-01 15:24_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:330 on 2023-05-01 15:47_

Yeah, it'd just be copying the `Regex` into code. If it's cool with you, I'll probably make the speed adjustments/`Peekable` changes as mentioned above, then try to merge?

I've noted down the string method stuff in my to-do list, so I'll check that out when I get some time!

---

_@evanrittenhouse reviewed on 2023-05-01 15:47_

---

_Converted to draft by @evanrittenhouse on 2023-05-02 02:53_

---

_@MichaReiser reviewed on 2023-05-02 07:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:330 on 2023-05-02 07:08_

Sounds good to me. Thank you

---

_@evanrittenhouse reviewed on 2023-05-03 01:25_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-link-in-todo_TDO003.py.snap`:8 on 2023-05-03 01:25_

On the next line. I'm thinking of flagging the line with the `TODO` and saying along the lines of "the next line should have an issue link". I think it would break if we tried throwing the diagnostic on the line after the `TODO` in the case where the `TODO` is the last line of the file.

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:376 on 2023-05-04 01:52_

@MichaReiser Apologies if this is a basic question. Is there a better way to handle the second `unwrap()` call? This is my first PR really handling the new `TextRange/TextSize` structs, so not sure if there's a more idiomatic way.

I'm also curious - why do those two structs use `u32` over `usize` as their underlying offset measure?

---

_@evanrittenhouse reviewed on 2023-05-04 01:52_

---

_@evanrittenhouse reviewed on 2023-05-04 01:57_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:245 on 2023-05-04 01:57_

Can look into this in another PR as it'll involve some larger changes

---

_@MichaReiser reviewed on 2023-05-04 06:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:376 on 2023-05-04 06:34_

> Apologies if this is a basic question. Is there a better way to handle the second unwrap() call?

You can do:

```
comment.find(['t', 'T']).and_then(|pos| TextSize::try_from(pos).ok()).unwrap()
```

I recommend changing the function's return type to `TextSize`. Using `TextSize` communicates that this is a byte offset position and not any other index (or integer). 



> I'm also curious - why do those two structs use u32 over usize as their underlying offset measure?

The motivation is that `u32` requires half the space compared to an `usize` on 64 bit systems and that should be more than sufficient for any reasonable program (you can represent files up to 4 GB). The result is that ruff uses less memory and is faster because it reads and writes less data, and more data fits into a single L1 cache line. 



---

_@evanrittenhouse reviewed on 2023-05-04 13:29_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:376 on 2023-05-04 13:29_

Ah makes sense. Thanks for the tip and detailed explanation!

---

_Marked ready for review by @evanrittenhouse on 2023-05-09 15:12_

---

_Review requested from @MichaReiser by @evanrittenhouse on 2023-05-09 15:12_

---

_Comment by @evanrittenhouse on 2023-05-09 15:13_

@MichaReiser Re-requesting review since I ended up getting rid of the regex capture groups to better place diagnostics; quite a bit has changed.

---

_Comment by @charliermarsh on 2023-05-09 18:53_

I'm happy to review.

---

_Comment by @evanrittenhouse on 2023-05-09 19:02_

@charliermarsh Go for it!

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:177 on 2023-05-10 07:27_

Nit:

```suggestion
        "Replace `{tag}` with `TODO`".to_string()
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:233 on 2023-05-10 07:30_

Nit: You can use a fixed array here instead since the indices all start at 0
```suggestion

static PATTERN_TAG_LENGTH: &'static [usize] = [
    "TODO".len(),
    "BUG".len(),
    "FIXME".len(),
    "XXX".len(),
];
```

And you can access the length using `PATTERN_TAG_LENGTH.get(index)` (or even use `PATTERN_TAG_LENGTH[index]` because the index being out of bounds is a programming error and not something that we need to handle gracefully)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:245 on 2023-05-10 07:33_

Nit: Can we use `Indexer.comment_ranges()` with `locator.slice(range)` It would avoid iterating over all comments.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:310 on 2023-05-10 07:38_

```suggestion
        content: &comment[tag_start_offset..tag_start_offset + tag_length]
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:334 on 2023-05-10 07:42_

Will this generate two diagnostics, `TDO-001` and `TDO-006`, for `Todo`? I think we should generally avoid emitting multiple diagnostics for the same range when possible if both have the same (manual or automated) fix.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:348 on 2023-05-10 07:43_

Nit:
```suggestion
                invalid_capitalization.set_fix(Fix::unspecified(Edit::range_replacement(
                    "TODO".to_string(),
                    tag.range
                )));
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:367 on 2023-05-10 07:44_

I believe `From` should work here

```suggestion
    let mut relative_offset: usize = usize::from(tag.range.end() - comment_range.start())
```

... or is this the same as using `tag.range.len()` (you want the length of the tag?)

But I would even recommend to use `TextSize` to make it clear that this is a byte offset (and not a character index)



---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:368 on 2023-05-10 07:47_

My understanding is that `relative_offset` is a byte offset. That's why using `chars().skip(offset)` will give you an incorrect result if the text contains any non-ascii characters because they are 2-4 bytes long. 

```suggestion
    let mut comment_rest = &comment[usize::from(relative_offset)..];
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:398 on 2023-05-10 08:12_

Nit: I think you can simplify this somewhat by using `trim_start` and `starts_with`

```rust
let trimmed = comment_rest.trim_start();
let content_offset = comment_rest.text_len() = trimmed.text_len(); // whitespace length

let author_end = content_offset + if trimmed.starts_with('(') {
	if let Some(end_index) = trimmed.find(')') {
		// Test if it is not empty? 
		TextSize::try_from(end_index + 1).unwrap()
	} else {
		// Missing closing ')'
		// Create a diagnostic?
		trimmed.text_len()
	}
} else {
	diagnostics.push(Diagnostic::new(
          MissingAuthorInTodo,
          TextRange::at(content_offset, TextSize::new(1)),
      ));

	TextSize::new(0)
};

let after_author = &comment_rest[usize::from(author_end)..];

let after_colon = if let Some((_colon, after_colon)) = after_author.split_once(':') {
	if after_colon.starts_with(' ') {
		// all good
		&after_colon[1..]
	} else {
		diagnostics.push(Diagnostic::new(
              MissingSpaceAfterColonInTodo,
              TextRange::at(range, TextSize::new(1)),
          ));
    }
} else {
	// No colon
	diagnostics.push(Diagnostic::new(
          MissingColonInTodo,
          TextRange::at(adjusted_colon_position, TextSize::new(1)),
      ));
};

if after_colon.is_empty() {
	diagnostics.push(Diagnostic::new(
          MissingTextInTodo,
          TextRange::at(comment_range.end(), TextSize::new(1)),
      )),
}
```

I don't know if this is really better, but working with strings rather than a char iterator can sometimes be easier.

I haven't updated all ranges. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todo/rules.rs`:415 on 2023-05-10 08:16_

Using `TextSize::new(1)` as length here is unsafe in case the first character after the TODO is a non-ascii character, because it then only includes the first byte of the multi-byte character. 

What you can do is either compute the size of the next character `text_after_range_end.chars().next().map(TextLen::text_len).unwrap_or_default()` or we change the range to highlight the tag (I know, that's the opposite of what I said last time, I'm sorry). Highlighting the tag might be easier. 

This is also true for other places where you use `TextSize::new(1)` as the range's length

---

_@MichaReiser approved on 2023-05-10 08:18_

Amazing work! 🎸🎸🎸

I have a few NIT comments where I let you decide whether you want to address them or keep the code as is. 

The one issue that we need to fix before merging is that you use `TextSize::new(1)` as the range's length where we don't know what the character in that range is. This can panic if the character in that range is a non-ascii character (multi-byte character) because we then slice of the first byte of the character. 

---

_@evanrittenhouse reviewed on 2023-05-10 13:53_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:367 on 2023-05-10 13:53_

So it's technically the distance of the `#` denoting the start of the comment and the last character of the tag. For example, if you had: `# TODO`, `tag.range.len()` would be 4 but `relative_offset` would be 6. 

This is because `flake8_todo` matches [all whitepsace](https://github.com/orsinium-labs/flake8-todos/blob/master/flake8_todos/_rules.py#L12) before the tag. Something like `#    TODO` would still match our regex, where `relative offset` would be 8, while `tag.range.len()` would still be 4

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:334 on 2023-05-10 13:58_

Sounds good to me, I'll keep that in mind for future changes as well

---

_@evanrittenhouse reviewed on 2023-05-10 13:58_

---

_@evanrittenhouse reviewed on 2023-05-10 17:50_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:415 on 2023-05-10 17:50_

Ah I see what you mean - okay, yeah. I'll just stick to pointing the new Diagnostic to the `tag` for now, that seems much easier

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todo/rules.rs`:93 on 2023-05-10 18:20_

I'm curious about this rule. It feels much more opinionated than the other rules and perhaps even unconventional (I haven't worked on a project with this requirement before). What are your thoughts on its relevance? I know it's part of the original implementation, but I wonder if this will be a rule that is `ignore`d more often than is healthy.

---

_@charliermarsh reviewed on 2023-05-10 18:20_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:93 on 2023-05-10 18:26_

It seems a bit different from the others, to be sure. I'd be okay with removing it. Seems like fixing it could take a decent chunk of work (i.e. making a new Github issue or something). Pretty much only included it since it was mentioned in the original `flake8-todo`.

Please let me know what you think! I think there'll be some additional work here because of the byte offset issue that Micha mentioned above, so happy to work these changes in too.

---

_@evanrittenhouse reviewed on 2023-05-10 18:26_

---

_Comment by @charliermarsh on 2023-05-10 19:28_

One common false positive I'm seeing here is around comments that include the word "bug" in them:

```py
# We need to do the same for ArchivedAttachment to avoid
# bugs if deleted attachments are later restored.
ArchivedAttachment.objects.filter(messages__recipient_id=stream.recipient_id).update(
    is_realm_public=None
)
```

---

_Comment by @evanrittenhouse on 2023-05-10 19:35_

Hmm, good point. 

The regex will only match tags which have whitespace in front of them, so in this case it's catching "bug" because of the space before it (e.g. `# some bug here` wouldn't match our regex since there's non-whitespace before it). I wonder if we could ignore the tag if the line preceding it is a comment? That would solve this case, but on the flip side we'd ignore cases like this: 
```
# implement flake8-todo
# BUG: not catching tags properly
```

Any thoughts/alternatives are welcome!

---

_@charliermarsh reviewed on 2023-05-10 20:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todo/rules.rs`:93 on 2023-05-10 20:17_

I looked through a couple repos that have this rule enabled. Of the ~ten I checked, three had this disabled ([1](https://github.com/project-march/ProjectMarch/blob/1747-ros-implementation-e-stop-and-pdb/.flake8), [2](https://github.com/aws-samples/aws-security-reference-architecture-examples/blob/main/.flake8#L19), [3](https://github.com/interactions-py/interactions.py/blob/stable/setup.cfg#L59)), although one had the author rules disabled and kept this one. So, maybe we just leave it, and expect people to configure based on their project preferences.

---

_Comment by @charliermarsh on 2023-05-10 20:19_

Is the issue that we can't rely on capitalization, or parentheses, or the presence of a colon because we're linting against the absence of those things? I'm wondering if we just remove BUG since it's so generic. ESLint's [`no-warning-comments`](https://eslint.org/docs/latest/rules/no-warning-comments) rule defaults to TODO, FIXME, and XXX.

---

_Comment by @evanrittenhouse on 2023-05-10 20:51_

Yep, exactly - the more traditional measures of a TODO tag could be missing. I'm happy to remove `BUG`. I'm going to implement some of the above changes tonight, so I'll do that then.

E: on second thought, we could make `BUG`, `FIXME`, and `XXX` case-insensitive as well, and only catch the all-uppercase case. That would also effectively force precedence of `InvalidTodoTag` over `InvalidCapitalizationInTodo`, which seems like it makes more sense IMHO; if you have `# fixme: `, we should prompt the user to change the tag to `# TODO` vs. `# FIXME`, since the latter would just cause `InvalidTodoTag` to throw on Ruff's next pass. Feels like that may be the best middle ground?

---

_Comment by @charliermarsh on 2023-05-12 01:02_

@evanrittenhouse - Do you mind just re-requesting me when the above is reflected, and I'll give it a last look + merge?

---

_Comment by @evanrittenhouse on 2023-05-12 01:04_

@charliermarsh sounds good. Do we want to go with the uppercase-only regex or do we want to remove BUG entirely?

---

_Comment by @charliermarsh on 2023-05-12 01:08_

Let's just remove BUG, I think.

Also -- what do you think about using `TD` instead of `TDO`? I find the `O` and the `0` a bit visually confusing.

---

_@evanrittenhouse reviewed on 2023-05-12 04:06_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todo/rules.rs`:398 on 2023-05-12 04:06_

Really appreciate this detailed suggestion. Makes it much easier to reason about than the `char` iterator approach I was using

---

_Review requested from @charliermarsh by @evanrittenhouse on 2023-05-12 04:31_

---

_Comment by @evanrittenhouse on 2023-05-12 11:39_

I'm not super sure why those tests are failing - it seems like they're referencing the old snapshots which no longer exist, but I already removed them so I'm not even really sure why they're being referenced

---

_Comment by @JonathanPlasse on 2023-05-12 11:59_

These snapshots must be deleted.
```
/home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__invalid-capitalization-in-todo_TDO006.py.snap
  /home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-colon-in-todo_TDO004.py.snap
  /home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-author-in-todo_TDO002.py.snap
  /home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-text-in-todo_TDO005.py.snap
  /home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__invalid-todo-tag_TDO001.py.snap
  /home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-space-after-colon-in-todo_TDO007.py.snap
  /home/runner/work/ruff/ruff/crates/ruff/src/rules/flake8_todo/snapshots/ruff__rules__flake8_todo__tests__missing-link-in-todo_TDO003.py.snap
```

---

_Comment by @JonathanPlasse on 2023-05-12 12:09_

You can do this automatically by running
```console
cargo insta test --all --all-features --delete-unreferenced-snapshots
```

---

_Comment by @charliermarsh on 2023-05-13 13:10_

Gonna do one pass over the docs then merge.

---

_Renamed from "Implement `flake8_todo`" to "Implement `flake8_todos`" by @charliermarsh on 2023-05-13 14:01_

---

_Comment by @charliermarsh on 2023-05-13 14:02_

There is one bug here, whereby links aren't detected in multiline comments, i.e., this is a false positive:

```py
# TODO: here's a TODO on the last line with no link
# Here's more content.
# TDO-3870
```

---

_Merged by @charliermarsh on 2023-05-13 14:19_

---

_Closed by @charliermarsh on 2023-05-13 14:19_

---

_Branch deleted on 2023-05-13 16:05_

---
