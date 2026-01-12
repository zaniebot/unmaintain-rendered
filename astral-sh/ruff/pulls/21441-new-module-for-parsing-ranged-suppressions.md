```yaml
number: 21441
title: new module for parsing ranged suppressions
type: pull_request
state: merged
author: amyreese
labels:
  - internal
assignees: []
merged: true
base: main
head: suppression-parsing
created_at: 2025-11-14T00:54:56Z
updated_at: 2025-12-02T23:44:04Z
url: https://github.com/astral-sh/ruff/pull/21441
synced_at: 2026-01-12T15:57:24Z
```

# new module for parsing ranged suppressions

---

_@amyreese_

## Summary

This adds a new `suppression` module to the `ruff_linter` crate, similar to the suppression
module for ty, to parse comments for ruff suppression directives, such as `# ruff: disable[CODE]`.

This is just the piece for parsing and storing an IMR of the comments themselves. Next step
is to interpret parsed comments and map them to ranges, codes, etc that can be referenced
later when determining which diagnostics should be kept/filtered/whatever.

## Test Plan

- A bunch of new snapshot tests
- Added to the beginning of `check_path` so that the parsing runs on all checked files
  for performance regression testing.

Issue #3711 

---

_Review requested from @ntBre by @amyreese on 2025-11-14 00:55_

---

_Review requested from @MichaReiser by @amyreese on 2025-11-14 00:55_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 01:06_


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

_@MichaReiser reviewed on 2025-11-14 07:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:55 on 2025-11-14 07:53_

I assume this is something that gets clearer once we have more code but it's not clear to me how a `Suppression` can have multiple comments. 

---

_@MichaReiser reviewed on 2025-11-14 07:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:14 on 2025-11-14 07:54_

I'm leaning towards skipping `ignore` for now, given that we don't have consensus on a design yet

---

_@amyreese reviewed on 2025-11-14 19:06_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:55 on 2025-11-14 19:06_

The idea is a `Suppression` object would cover a range of the document, but for cases like `# ruff: disable` you would also need a matching `# ruff: enable` to define the range of the suppression, leading to multiple comments for a single suppression.

---

_@amyreese reviewed on 2025-11-14 19:08_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:14 on 2025-11-14 19:08_

Sure, was not planning on supporting it in the first implementation, but wanted to keep it in mind for designing how the system/structures should work. Equally applies to `noqa` suppressions I guess.

---

_@amyreese reviewed on 2025-11-14 19:10_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:55 on 2025-11-14 19:10_

The next step of the implementation would be looking through the list of discovered comments and start matching disable/enable comments and build a single `Suppression` object with the appropriate range, and containing references to both comments so that the future "unused suppression" diagnostic could easily find both comments to suggest removing them.

---

_Comment by @amyreese on 2025-11-19 01:46_

Updated with logic to match disable/enable suppression comments and transform them into ranged suppression objects. Currently just looks for matching indentation and codes, to get a simplified implementation that works, since looking at tokens is complicated by where dedent tokens appear relative to own-line comments at the end of blocks ([playground example](https://play.ruff.rs/3f5950bf-c969-4519-a207-1304b22d1c78)).

---

_Comment by @MichaReiser on 2025-11-19 07:28_

> Updated with logic to match disable/enable suppression comments and transform them into ranged suppression objects. Currently just looks for matching indentation and codes, to get a simplified implementation that works, since looking at tokens is complicated by where dedent tokens appear relative to own-line comments at the end of blocks ([playground example](https://play.ruff.rs/3f5950bf-c969-4519-a207-1304b22d1c78)).

Yeah, I think we have to "ignore" `Dedent`s if all that comes between the comment token and the `Dedent` token are trivia tokens (newline, comment,)

---

_Comment by @amyreese on 2025-11-19 16:49_

> Yeah, I think we have to "ignore" `Dedent`s if all that comes between the comment token and the `Dedent` token are trivia tokens (newline, comment,)

Plus we still need to do indentation checking to make a reasonable guess as to which scope the comment should be associated:

```py
def foo():
    if True:
        pass
        # here?
    # here?
# or here?
```

---

_@MichaReiser reviewed on 2025-11-21 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:153 on 2025-11-21 07:24_

Could we use the `indent_level` here rather than match on the exact indent (in which case a simple `usize` counter could replace our current stack? 

Python also supports mixed space-tab indents and I never remember the exact details, but using levels over the indent-string ensures that you delegate all this to the lexer/parser. 



---

_@MichaReiser reviewed on 2025-11-21 07:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:150 on 2025-11-21 07:25_

Mathing on `token` here seems weird as it isn't an enum? I think what you want to do here is to match on `token.kind()`

---

_@amyreese reviewed on 2025-11-21 16:53_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:150 on 2025-11-21 16:53_

Yes

---

_@amyreese reviewed on 2025-11-21 16:53_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:153 on 2025-11-21 16:53_

I think the problem is that if you only have an "indent level", then you can't easily match a comment to an indentation level, because indentation levels in Python can be arbitrary. This comes back to needing to determine which level a comment belongs to when it appears before a Dedent token, and I'm not aware of a way to do that other than comparing full indentation strings.  If there are helpers in ruff that can infer an indentation level to a comment, I would love to know.

> Python also supports mixed space-tab indents and I never remember the exact details

FWIW, Python does not allow mixing spaces and tabs within an indented block of code. Eg, this will produce either an `IndentationError` or a `TabError` at runtime, depending on how many spaces are used:

```
def foo():
<tab>pass
<spaces>pass
```

With modern editors and formatters, I would be surprised if mixed indents were still a problem, at least for someone at the point of using Ruff to check their code.

---

_@MichaReiser reviewed on 2025-11-21 17:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:153 on 2025-11-21 17:09_

Oh, so this is for comments at the end of a block like this:

```py
def test():
	# ruff: disable[code]
	if True:
		pass
	# ruff: enable[code] - re-enables the outer and not the inner matching ignore
```

vs 

```py

def test():
	if True:
		# ruff: disable[code]
		pass
		# ruff: enable[code] - re-enables the inner matching ignore
```

Agree, using string comparison for those is probably fine and also what we do in the formatter https://github.com/astral-sh/ruff/blob/f69f278bf12d67e17bd46b51ffcc87fb21babd3e/crates/ruff_python_formatter/src/comments/placement.rs#L603


Python does allow mixed space and tabs indent (they can even be different between statements in a block, for as long as the visual indent is the same regardless of the tab size)
https://play.ruff.rs/7a681eaf-592d-45c7-8272-1eca15252a51



---

_@amyreese reviewed on 2025-11-21 17:17_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:153 on 2025-11-21 17:17_

> Python does allow mixed space and tabs indent (they can even be different between statements in a block, for as long as the visual indent is the same regardless of the tab size)

gross 😅

---

_Closed by @amyreese on 2025-11-25 01:58_

---

_Branch deleted on 2025-11-25 01:58_

---

_Branch restored on 2025-11-25 01:59_

---

_Reopened by @amyreese on 2025-11-25 02:00_

---

_Marked ready for review by @amyreese on 2025-11-25 02:13_

---

_Renamed from "[ruff] new module for parsing suppression comments" to "[ruff] new module for parsing ranged suppressions" by @amyreese on 2025-11-25 02:13_

---

_Label `internal` added by @MichaReiser on 2025-11-25 08:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:144 on 2025-11-25 08:22_

I think we should remove this before landing as it isn't used yet (but it's great to have in place up right to landing to get the benchmark resutls)




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:42 on 2025-11-25 08:31_

Allocating a `Vec` just for comparing the code and copying over the string slices is unnecessarily expensive. 

You can do something like this


```suggestion
    fn codes_as_str<'src>(&self, source: &'src str) -> impl Iterator<Item = &'src str> {
        self.codes.iter().map(|range| source.slice(range))
    }
```

And then compare the codes using `comment.codes_as_str().eq(other.codes_as_str())`


However, this will require that the codes are in the exact same order. Is this the semantics we want?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:46 on 2025-11-25 08:34_

Can you make an example for why it's necessary to match `disable` comments to `enable` comments. I assumed that matching `enable` to `disable` comments should be sufficient because we always start with "enabled-by-default".

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:67 on 2025-11-25 08:35_

```suggestion
#[expect(unused)]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:77 on 2025-11-25 08:41_

`SmallVec` stack-allocates up to `N` items before heap allocating a regular `Vec`. This has the advantage that it reduces the pressure on heap size. However, it can have the downside that the containing type (`Suppression`) becomes larger because the `SmallVec` requires at least `N * SuppressionComment` bytes to store the inline elements. 

The main downside of small vec is that it wastes memory when the stored elements is less than `N`. It also makes copying `Suppression` more expensive because it requires writing more bytes (e.g. when adding a `Suppression` to a `Vec`)


Given that `SuppressionComment` is a fairly chunky type (72 bytes) (rust-analyzer shows you the size on hover) and the most common case is probably to suppress exactly one code, I suggest reducing `N` to 1
```suggestion
    comments: SmallVec<[SuppressionComment; 1]>,
```

After reading the entire PR, I think this shouldn't be a `SmallVec` in the first place. Instead, I think we should use two fields:

```rust
enable_comment: Option<SuppressionComment>,
disable_comment: Option<SuppressionComment>
```

This is more explicit expresses the structure found in code.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:58 on 2025-11-25 08:41_

Nit: Consider using `CompactStr`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:67 on 2025-11-25 08:43_

I suggest implementing `Copy` for `InvalidSuppressionKind`. It improves the ergonomics of this type as Rust now allows you to make copys of the type without an explicit `.clone` call. `Copy` is recommended for small types (a no-brainer for anything `<= usize`; I sometimes go as high as `2 usize`) and where identity doesn't matter

```suggestion
#[derive(Copy, Clone, Debug)]
```




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:175 on 2025-11-25 08:51_

Nit: I prefer to have the external visible API come before the implementation details. In this case, move `load_from_tokens` above `match_comments`. It has the advantage that, when reading the code, I can start getting an overview before I have to read the gnarly implementation details. 

I know that this might take some time getting used to coming from Python where you have to forward-declare functions and classes before you can use it. We don't need to do that in Rust, allowing us to lay out the code for better readability than the runtime requires.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:125 on 2025-11-25 08:57_

It's preferred to use `&str` over `&String` because you can pass a `String` reference and a static string (like `"abc"`) to a function taking a `&str`, but you can only pass a `String` reference to a function taking a `&String` argument.

```suggestion
    fn match_comments(&mut self, current_indent: &str) {
```

Overall, writing `&String` or `&Vec<T>` tends to be a code smell and you should use `&str` or `&[T]` instead 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:178 on 2025-11-25 08:58_

We can avoid copying the idents to `String` by storing `&str`
```suggestion
        let default_indent = "";
        let mut current_indent = default_indent;
        let mut indents: Vec<&str> = vec![];
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:186 on 2025-11-25 09:00_

Now that we use `&str`, which is `Copy`, we can write this as 

```suggestion
										current_indent = self.source.slice(token);
                    indents.push(current_indent);
```

Although I'd suggest removing `current_indent` altogether and instead use `indents.last().copied().unwrap_or_default()` to get the current indent. It avoids tracking the same state twice

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:268 on 2025-11-25 09:06_

You can avoid the cloning here by changing the signature of `load_from_tokens` to take a `mut self` (so that it takes an owned value, meaning, the caller won't be allowed to use `self` anymore after).

```rust
load_from_tokens(mut self, tokens: &Tokens)
```
```suggestion
        Suppressions {
            valid: self.valid,
            invalid: self.invalid,
            errors: self.errors,
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:343 on 2025-11-25 09:08_

I was a bit confused by this line. I think what you have here works because we don't need to support `# ruff:disable[RUF100] reason # fmt: off`


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:331 on 2025-11-25 09:10_

You can use `indentation` here

https://github.com/astral-sh/ruff/blob/ef8f399a65d4a6355a0140adf7ff4462e549d2ae/crates/ruff_python_ast/src/whitespace.rs#L9-L14

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:23 on 2025-11-25 09:17_

I'm inclined to remove `indent` from `SuppressionComment` as it is only information necessary for matching comments. I don't think we'll need it afterwards. 

It would also allow moving the indent handling out of the `SuppressionParser`, keeping the parser scope narrow to just parsing out the comment (and it's the builders responsiblity to do all the indent stuff). 


The way I would approach this is by adding a `PendingSuppressionComment` struct like this

```rust
struct PendingSuppressionComment<'src> {
	indent: &'src str,
	comment: SuppressionComment
}
```

It also has the advantage that we don't need to copy the indent to a `String`, instead we can just store the `str` slice (requires changing `pending` to `Vec<PendingSuppressionComment<'a>>`


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:131 on 2025-11-25 09:33_

I wonder if `load_from_tokens` should store a `Vec<(&str, Vec<SuppressionComment>)` where the first item is the indent at a given level, and the second item is the suppression comments at that level.

I think this method would greatly simplify because you could just iterate over the comments at the given level instead of having to match them by indent again. 

The downside is that we allocate a few extra `Vec`'s, one for each level with a suppression comment. I think that's fine. 


Making this change should also remove the need for `PendingSuppressionComment` because we no longer need to store the `indent` anywhere (I think)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:159 on 2025-11-25 09:41_

I don't think it's a big deal, but it's a bit unfortunate that we need to clone both comments here. Do we need all information from `SuppressionComment` or could we copy a subset of the information to `Suppression? (If not, as I said, I think this is completely fine)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:992 on 2025-11-25 09:44_

I find the snapshot tests here fairly hard to read due to all the byte offsets. You could consider adding a helper similar to what I added in ty that pretty prints suppression comments:

https://github.com/astral-sh/ruff/blob/ef8f399a65d4a6355a0140adf7ff4462e549d2ae/crates/ty_python_semantic/src/suppression.rs#L1087-L1159

Another alternative is to render the suppressions using our diagnostic system, similar to what we do for ty's ide tests 

https://github.com/astral-sh/ruff/blob/ef8f399a65d4a6355a0140adf7ff4462e549d2ae/crates/ty_ide/src/lib.rs#L392-L398


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:991 on 2025-11-25 10:03_

I think it would be great to have a few more tests here:

* What if we have to succinct `ruff: enable`
* What about multiple `ruff: disable` and only one `ruff: enable`
* What if the `ruff: disable` and `ruff: enable` match but the codes are in a different order?

---

_@MichaReiser reviewed on 2025-11-25 10:15_

This, overall looks great. I've a few inline comment for how I would restructure the code to make it more performant or more idiomatic Rust. 


My main concern is that I see a [3% performance regression](https://codspeed.io/astral-sh/ruff/branches/suppression-parsing?utm_source=github&utm_medium=check&utm_content=details) on the linter benchmarks. I think we should try the following:

1. Can we reduce the regression by integrating the extraction into [`Indexer::from_tokens`](https://github.com/astral-sh/ruff/blob/b6bd32d9dc086e81b766019b5b99e8c343de7220/crates/ruff_python_index/src/indexer.rs#L32) by having a `visit_token` method?
2. Instead of iterating over `Tokens`, iterate over `CommentRanges`, and start iterating over tokens only the comment is a `ruff: enable` / `ruff: disable` comment. A somewhat naive implementation below

```rust
let mut indents: Vec<&str> = vec![];
        let mut errors = Vec::new();

        let mut suppressions = comments
            .iter()
            .copied()
            .filter_map(|comment_range| {
                let mut parser = SuppressionParser::new(self.source, comment_range);
                match parser.parse_comment() {
                    Ok(comment) => Some(comment),
                    Err(ParseError {
                        kind: ParseErrorKind::NotASuppression,
                        ..
                    }) => None,
                    Err(error) => {
                        errors.push(error);
                        None
                    }
                }
            })
            .peekable();

        'comments: while let Some(suppression) = suppressions.peek() {
            let last_indent = tokens
                .before(suppression.range.start())
                .iter()
                .rfind(|token| token.kind() == TokenKind::Indent)
                .map(|token| self.source.slice(token))
                .unwrap_or_default();

            indents.push(last_indent);

            let tokens = tokens.after(suppression.range.start());

            // Iterate through tokens, tracking indentation, filtering trailing comments, and then
            // looking for matching comments from the previous block when reaching a dedent token.
            for (token_index, token) in tokens.iter().enumerate() {
                let current_indent = indents.last().copied().unwrap_or_default();
                match token.kind() {
                    TokenKind::Indent => {
                        indents.push(self.source.slice(token));
                    }
                    TokenKind::Dedent => {
                        self.match_comments(current_indent);

                        indents.pop();

                        if indents.is_empty() { // OR if self.pending is empty?
                            continue 'comments;
                        }
                    }
                    TokenKind::Comment => {
                        let Some(suppression) =
                            suppressions.next_if(|suppression| suppression.range == token.range())
                        else {
                            continue;
                        };

                        let Some(indent) =
                            indentation_at_offset(suppression.range.start(), self.source)
                        else {
                            // trailing suppressions are not supported
                            self.invalid.push(InvalidSuppression {
                                kind: InvalidSuppressionKind::Trailing,
                                comment: suppression,
                            });
                            continue;
                        };

                        // comment matches current block's indentation, or precedes an indent/dedent token
                        if indent == current_indent
                            || tokens[token_index..]
                                .iter()
                                .find(|t| !t.kind().is_trivia())
                                .is_some_and(|t| {
                                    t.kind() == TokenKind::Dedent || t.kind() == TokenKind::Indent
                                })
                        {
                            self.pending.push(PendingComment {
                                comment: suppression,
                                indent,
                            });
                        } else {
                            // weirdly indented? ¯\_(ツ)_/¯
                            self.invalid.push(InvalidSuppression {
                                kind: InvalidSuppressionKind::Indentation,
                                comment: suppression,
                            });
                        }
                    }
                    _ => {}
                }
            }

            self.match_comments("");
        }

        Suppressions {
            valid: self.valid,
            invalid: self.invalid,
            errors,
        }
```

I've a slight preference towards the second approach as it tries to avoid unnecessary work, but I'd also be fine going with the first approach.


---

_@amyreese reviewed on 2025-11-25 16:42_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:175 on 2025-11-25 16:42_

> I know that this might take some time getting used to coming from Python where you have to forward-declare functions and classes before you can use it. We don't need to do that in Rust, allowing us to lay out the code for better readability than the runtime requires.

That's not really true for Python. You can reference and call other functions even if they're defined later in the file, as long as at import-time you don't try to actually execute it before the runtime has gotten to the definition.

Eg, this will work just fine:

```py
def foo():
	bar()

def bar():
	...

foo()
```

It's more a style choice I picked up — for a different kind of readability — where it's easier to consume and understand a module in it's entirety if you can read it from top-to-bottom, starting with the building blocks. It helps prevent the need for jumping around in the file to understand what functions/classes are doing, because you would have read their definitions or at least their docstrings already. And if you're more interested in what the "public" API is rather than understanding the implementation, that would generally be covered by either a docstring at the top of the file, or documentation built from the source code's docstrings. 

That said, I will try to follow Ruff's/Rust's style.

---

_Comment by @MichaReiser on 2025-11-25 17:27_

As discussed in our 1:1

* I think we should move the performance optimization out of this PR. We can simply remove the call to extract the comments in this PR and address the performance concerns in a follow-up PR.
* Let's add more integration tests in https://github.com/astral-sh/ruff/pull/21623 rather than trying to improve the integration testing framework here (and extending coverage)

---

_@amyreese reviewed on 2025-11-25 17:36_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:46 on 2025-11-25 17:36_

I wrote it both ways here because I waffled a bit on where/how to do the matching, and in which direction the search for a match would be done. 

---

_@amyreese reviewed on 2025-11-25 17:55_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:42 on 2025-11-25 17:55_

> However, this will require that the codes are in the exact same order. Is this the semantics we want?

I considered using an unordered set, or just sorting the codes found, but wasn't sure what the performance semantics of that would be when trying to match comments together, especially if there was a big file with a bunch of disable/enable comments.

From the UX perspective, I feel like "must be same order" is easy to explain, and also helps enforce the idea that you can't "mix and match" codes from various disable/enable comments.

Eg, I don't think we want to support (or make users *think* we support) this:

```
# ruff: disable[foo]
...
# ruff: disable[bar]
...
# ruff: enable[foo,bar]
```

Keeping the semantics rigid around order within matched comments seems beneficial, but I'm not opposed to allowing arbitrary orders as long as the set contents match.

---

_@MichaReiser reviewed on 2025-11-25 17:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:42 on 2025-11-25 17:59_

I'm fine keeping it as is for now and change the behavior based on user feedback.

---

_@amyreese reviewed on 2025-11-25 18:10_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:77 on 2025-11-25 18:10_

The main reason I chose a vec rather than explicit fields was as consideration for  any logic around needing the comment ranges (like reporting unused suppressions/codes) could simply loop over any associated comments without having to know what kind the suppression is, or without needing to repeat logic within `if let Some(comment) = suppression.<kind>_comment` blocks. And in a hypothetical future where we added alternative suppression directives (eg, `#ruff:ignore`) or where we unified `#noqa` into this framework to deduplicate suppression handling logic, we wouldn't need to add new fields or further duplicate that logic.  But also maybe there's some rust tricks that make this nicer and I don't know what I'm talking about. 😅

---

_@MichaReiser reviewed on 2025-11-25 18:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:77 on 2025-11-25 18:24_

I'm fine leaving this as is in that case but a few thougths:

> The main reason I chose a vec rather than explicit fields was as consideration for any logic around needing the comment ranges (like reporting unused suppressions/codes) could simply loop over any associated comments without having to know what kind the suppression is, or without needing to repeat logic within if let Some(comment) = suppression.<kind>_comment blocks

`Suppression` could have a `comments()` method that returns an `Iterator` over al its comments for the cases where we really just want to iterate over them. I suspect that we still need special casing depending on the kind of comment when generating fixes. 

> or where we unified #noqa into this framework to deduplicate suppression handling logic, we wouldn't need to add new fields or further duplicate that logic. 

I wouldn't worry too much about that. This seems far out and there are a few options as well, E.g. we could have an `enum` depending on the comment kind that stores one or two comments (I don't think there's any case where we want to store 3?)

---

_@amyreese reviewed on 2025-11-25 19:32_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:23 on 2025-11-25 19:32_

I'm definitely not used to thinking in terms of composing types like this. That will take more practice to reach for it more regularly. 

---

_@amyreese reviewed on 2025-11-25 19:47_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:992 on 2025-11-25 19:47_

I tried doing this earlier on and got so lost in rust compiler errors that I decided to defer it for later once I actually had the logic working 😅

---

_@amyreese reviewed on 2025-11-25 20:29_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:77 on 2025-11-25 20:29_

> Given that SuppressionComment is a fairly chunky type (72 bytes) (rust-analyzer shows you the size on hover) and the most common case is probably to suppress exactly one code, I suggest reducing N to 1

Also I think you misunderstand the current use here. In the expected case, this vec would contain a reference to both the disable and enable `SuppressionComment` objects. Unmatched disable/enable would end up in the `invalid` pile rather than getting a `Suppression` object created.

---

_@amyreese reviewed on 2025-11-25 20:50_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:159 on 2025-11-25 20:50_

Other than the indents — which I will move out somewhere else — the rest of the data in the comment object was intended for use by diagnostics like unused suppressions/codes. 

---

_Comment by @astral-sh-bot[bot] on 2025-11-25 22:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-25 22:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 42 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ruff] new module for parsing ranged suppressions" to "new module for parsing ranged suppressions" by @MichaReiser on 2025-11-26 07:01_

---

_@MichaReiser reviewed on 2025-11-26 07:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:77 on 2025-11-26 07:04_

> Unmatched disable/enable would end up in the invalid pile rather than getting a Suppression object created.

I thought we decided to allow implicit *enables* at the end of the block. 

---

_@amyreese reviewed on 2025-12-01 17:36_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:77 on 2025-12-01 17:36_

Looking back at the notion doc, the summary says we decided on requiring a matching enable comment, I believe coming back to the idea of wanting to make sure that refactors didn't accidentally remove a `#ruff:enable` line and unknowingly suppress more lines than originally intended.

---

_Review requested from @MichaReiser by @amyreese on 2025-12-02 00:04_

---

_Comment by @amyreese on 2025-12-02 00:08_

Updated with most of the suggestions, including use of `&str`, better testing snapshots, etc. Performance work to bridge comment ranges and tokens was left for a future PR. I think this should be ready to land assuming I haven't introduced other nits.

---

_@MichaReiser reviewed on 2025-12-02 08:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:77 on 2025-12-02 08:18_

I'm leaning towards making this a lint rule, which we can enable by default, but giving users a way to opt out if they want to.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:12 on 2025-12-02 08:20_

Use `expect` over `allow`. This way, rustc will remind you to remove the unused warning once the struct is used

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:177 on 2025-12-02 08:22_

Nit
```suggestion
                                        matches!(t.kind(), TokenKind::Dedent | TokenKind::Indent)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:131 on 2025-12-02 08:23_

I still think that this might be a nice simplification but up to you

---

_@MichaReiser approved on 2025-12-02 08:25_

Thanks, this looks good. 

I suggest keeping `disable` comments without an explicit comment as valid suppressions and instead creating a new ruff dedicated lint rule that warns about this. This way, it gives users a way to opt-out from this behavior. 

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:12 on 2025-12-02 16:05_

I tried doing that, but then I get `unfulfilled` issues because they are used by tests. 🙃  Is there a correct way to account for that?

---

_@amyreese reviewed on 2025-12-02 16:05_

---

_Merged by @amyreese on 2025-12-02 23:39_

---

_Closed by @amyreese on 2025-12-02 23:39_

---

_Branch deleted on 2025-12-02 23:40_

---
