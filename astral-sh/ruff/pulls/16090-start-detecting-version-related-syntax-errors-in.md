```yaml
number: 16090
title: Start detecting version-related syntax errors in the parser
type: pull_request
state: merged
author: ntBre
labels:
  - preview
assignees: []
merged: true
base: main
head: brent/syntax-errors-parser
created_at: 2025-02-11T00:19:08Z
updated_at: 2025-02-26T04:03:51Z
url: https://github.com/astral-sh/ruff/pull/16090
synced_at: 2026-01-10T19:49:01Z
```

# Start detecting version-related syntax errors in the parser

---

_Pull request opened by @ntBre on 2025-02-11 00:19_

## Summary

This PR builds on the changes in #16220 to pass a target Python version to the parser. It also adds the `Parser::syntax_errors` field, which collects version-related syntax errors while parsing. These syntax errors are then turned into `Message`s in ruff (in preview mode) or `SyntaxDiagnostic`s in red-knot.

This PR only detects one syntax error (`match` statement before Python 3.10), but it has been pretty quick to extend to several other simple errors (see #16308 for example).

## Test Plan

The current tests are CLI tests in the linter crate, but these could be supplemented with inline parser tests after #16357.

I also tested the display of these syntax errors in VS Code:

![image](https://github.com/user-attachments/assets/062b4441-740e-46c3-887c-a954049ef26e)
![image](https://github.com/user-attachments/assets/101f55b8-146c-4d59-b6b0-922f19bcd0fa)


---

_Comment by @github-actions[bot] on 2025-02-11 00:35_

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

_@MichaReiser reviewed on 2025-02-11 21:44_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-11 21:44_

We shouldn't use `LintName` here. The idea of the other error codes is to be mainly static. 

---

_@MichaReiser reviewed on 2025-02-11 21:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:68 on 2025-02-11 21:46_

I guess we could really just move `PythonVersion` to the parser and use that type everywhere. But that's a good separate refactor

---

_@MichaReiser reviewed on 2025-02-11 21:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:332 on 2025-02-11 21:47_

I think we should instead parametrize the parser to take a `PythonVersion` argument and only emit relevant errors because creating errors is sort of expensive. 

---

_@MichaReiser reviewed on 2025-02-11 21:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:48 on 2025-02-11 21:48_

My suggestion here is to only handle version specific syntax errors in the parser. I'd still recommend handling semantic (compiler errors) in a separate pass outside the parser. 



---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:332 on 2025-02-12 14:44_

Oh yes, I should have noted that in the summary. I have a stashed version locally where I passed a `target_version` to all of the parser functions, and it became a bit of a mess and a huge diff. This is definitely a hack I would not want to preserve in a real implementation.

---

_@ntBre reviewed on 2025-02-12 14:44_

---

_@MichaReiser reviewed on 2025-02-12 14:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:332 on 2025-02-12 14:49_

You can store the version on the `Parser` struct. We may also need to pass it to the `Lexer` (and store it there?) to warn about unsupported fstring syntax.

---

_@ntBre reviewed on 2025-02-12 14:56_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:332 on 2025-02-12 14:56_

I'm probably missing something here, but the problem I was having is that the `Parser` is only `pub(crate)` and the public interface is through the `parse_module`, `parse_expression`, etc functions. So to store it in the `Parser`, I had to modify the signature of all those functions instead of adding something like `Parser::with_python_version`, which would be closer to ideal I think.

What I thought of later, but haven't tried, is instead adding a new `parse_module_with_version` function, for example, that I can call in the few places I want it to at least avoid having to change every call site immediately. The existing functions would just set a default version on the `Parser` in this case.

---

_@MichaReiser reviewed on 2025-02-12 14:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:332 on 2025-02-12 14:59_

We probably want to introduce some `ParserOptions` struct that stores the version and could also store the parse mode and possibly offset. I'd have to go through all the signatures to see what makes the most sense.



---

_@dhruvmanila reviewed on 2025-02-12 17:12_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:332 on 2025-02-12 17:12_

Should we add something like `JsFileSource` in Biome (https://github.com/biomejs/biome/blob/bca10f5198a6afaeffdcf7116f65444f9e11f674/crates/biome_js_syntax/src/file_source.rs#L142-L153) which will allow us to include the `PySourceType` and possibly `Mode` value as well?

I think we talked about this earlier but decided not to introduce anything new just for a single parameter `PySourceType`. The APIs would then be simplified to take `source: &str, source_type: PyFileSource` and sometimes a `TextRange`

---

_@ntBre reviewed on 2025-02-13 14:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:68 on 2025-02-13 14:51_

Do you think it fits better in the parser or in `ruff_db`? I think that could be a good separate refactor like you said.

---

_@ntBre reviewed on 2025-02-13 14:56_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-13 14:56_

Do you mean this should be `Option<&'static str>`, for example, or that I should use a new enum variant entirely?

---

_@ntBre reviewed on 2025-02-13 16:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:68 on 2025-02-13 16:44_

Ah, I guess it has to go in the parser since `ruff_db` depends on the parser, or we'd get a circular import. I'll push the PR I was working on then.

---

_@MichaReiser reviewed on 2025-02-13 17:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:68 on 2025-02-13 17:05_

Yes, hmm. I thought I replied but it seems I didn't. whoops. Yes, that's exactly the reason why the parser is probably the best case (the parser is also the minimum denominator for what Python versions we can support ;))

---

_@MichaReiser reviewed on 2025-02-13 17:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:68 on 2025-02-13 17:06_

The only downside of this is that crates that don't call into parsing themself will need to depend on the parser. So yet another possibility is the AST crate. But moving it should be easy once we have a PR that unifies them :)

---

_Comment by @ntBre on 2025-02-14 23:54_

Just rebased onto main to get the `PythonVersion` and `Span` changes. I still need to work in the `PyFileSource` or `ParserOptions` idea ~~and remove the semantic error detection~~, but my hope is that this branch will eventually be the usable prototype for version-specific errors.

---

_@MichaReiser reviewed on 2025-02-21 18:00_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-21 18:00_

That's probably no longer relevant. But the idea is that the `ErrorCode`s struct explicitly lists all error codes. The one exception to this are lint rules, because we want to define them in different crates. 

So what I'm suggesting is that you change this to `ParenthesizedWithItemsPre310` (or whatever the error code should be). But I think this is no longer necessary, now that we map all version related syntax error to syntax error.

---

_Label `preview` added by @ntBre on 2025-02-24 14:55_

---

_Renamed from "Detect more syntax errors in the parser" to "Start detecting version-related syntax errors in the parser" by @ntBre on 2025-02-24 14:56_

---

_@ntBre reviewed on 2025-02-24 17:53_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:435 on 2025-02-24 17:53_

`VersionError` or something similar might be a better name for this.

---

_Marked ready for review by @ntBre on 2025-02-24 19:06_

---

_Review requested from @carljm by @ntBre on 2025-02-24 19:06_

---

_Review requested from @AlexWaygood by @ntBre on 2025-02-24 19:06_

---

_Review requested from @sharkdp by @ntBre on 2025-02-24 19:06_

---

_Comment by @carljm on 2025-02-24 20:11_

This is awesome!

I would love to have red-knot mdtests demonstrating whatever syntax errors we have support for here (using the mdtest support for setting the python version to show that the error is python version dependent), so as to verify the red-knot-specific support added here (and that we don't break it with future changes.) I would want this, to verify the support in red-knot, even if the main tests for this are moved into the parser crate.

I realize this is maybe a bit painful as it means double-testing everything you add in this area. If necessary maybe someone from red-knot team could take it on as a separate follow-up PR.

---

_Comment by @ntBre on 2025-02-24 20:25_

Thank you!

I think you're right that it's good to have the ruff and red-knot integration tests even with inline parser tests. I'll try writing my first mdtests. I've been curious to learn more about them anyway!

---

_@ntBre reviewed on 2025-02-24 21:32_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/version_related_syntax_errors.md`:19 on 2025-02-24 21:32_

I think this relates to @MichaReiser's comment about `LintName` (or I'm doing something dumb). I tried using `# error: [match-before-python-310]` but kept getting this very confusing error:

```
  crates/red_knot_python_semantic/resources/mdtest/diagnostics/version_related_syntax_errors.md:19 unmatched assertion: error: [match-before-python-310]
  crates/red_knot_python_semantic/resources/mdtest/diagnostics/version_related_syntax_errors.md:19 unexpected error: 1 [match-before-python-310] "Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)"
```

until I switched to the contains-text assertion.

---

_@MichaReiser reviewed on 2025-02-24 21:48_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-24 21:48_

I haven't reviewed the rest of the PR but I'd be interested to understand what the motivation is for `InvalidSyntax(Option<LintName>)` over just using `SyntaxError`.

---

_@MichaReiser reviewed on 2025-02-24 21:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/version_related_syntax_errors.md`:19 on 2025-02-24 21:49_

Yes, it's probably related. Let's revisit the error codes first.

---

_@ntBre reviewed on 2025-02-24 22:29_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-24 22:29_

I don't really remember the motivation, which is probably not a good sign :sweat_smile: I think I just needed a `DiagnosticId` to implement `Diagnostic` for `SyntaxDiagnostic` and this looked like the best way to go. I think I was still picturing giving each one a name/rule code when I wrote this originally like in some of the other prototypes.

Should I just reuse the original, unit `InvalidSyntax` variant? Or did you mean something else by "over just using `SyntaxError`?"

This has been confusing to me for some reason, sorry about that. I guess I don't fully understand what these are used for, or at least I certainly didn't understand when I wrote this initially. Seeing the mdtest errors helped to clear that up a bit, I think.

---

_@MichaReiser reviewed on 2025-02-25 08:00_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 08:00_

I would just use `SyntaxError` unless we have reasons not to (e.g., we want to use a dedicated error code for each diagnostic). Reusing `SyntaxError` has the advantage that you get the correct handling in the extension for free (Ruff VS code has a setting to disable syntax errors). I think we also have some other places with extra handling for syntax errors. 

The alternative is (but I'm not convinced we should do this) is to add a `parenthesized-with-items-pre-310` variant to `ErrorCode` (not using `LintName`, just add the variant ;)).

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:24 on 2025-02-25 08:02_

We should avoid adding new arguments to `parsed_module`. Salsa uses an optimized storage for queries that have exactly one argument other than `db`. It's probably not that big of a deal here because we only call this query once per file (it's not a hot query like` infer_expression_types`). However, it does make it more cumbersome to use the method and I don't see a use case today for parsing the same file with different python versions (we don't call it `target-version` in red knot).

Ideally, you'd use `ProgramSettings` right in `parsed_module` but you can't because it's defined in `red_knot_python_semantic`. What I'd do for now is add a new `python_version` method to the `ruff_db::Db` trait. All DBs above `red_knot_pyton_semantic` will return `ProgramSettings::get(db).python_version(db)`. Other `DBs can use a hard-coded value.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:397 on 2025-02-25 08:03_

I suspect we're now resolving the `target_version` multiple times. Once here and then somewhere inside `check_path`? I guess that's probably fine but I wonder how much work it would be to avoid it.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:512 on 2025-02-25 08:04_

The `target_version` shouldn't change between loops. We can move that out

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:461 on 2025-02-25 08:07_

Should this use `minimum_version` or should we remove that method?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:478 on 2025-02-25 08:07_

What do we use the `as_str` implementation for?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:435 on 2025-02-25 08:08_

Maybe `UnsupportedSyntaxError`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:310 on 2025-02-25 08:09_

We should rename this to `unsupported_syntax_errors` or similar. All errors in this struct are syntax errors.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2271 on 2025-02-25 08:10_

I'm unsure if highlighting the entire match statement here is the best choice. Although it's not wrong. I'm just worried that it could lead to very long code frames

---

_@MichaReiser requested changes on 2025-02-25 08:13_

Thanks. I suggest splitting the Red Knot integration out of this PR. There are some bigger design questions that should be discussed separately (e.g., how to pass the target version to `parsed_module`, what error code to use). Having a separate PR unblocks your work on Ruff and should also reduce the need for stacking new-syntax-error PRs because you can test them on the Ruff version.


This otherwise looks good to me, except that using the term `SyntaxError` for version-related syntax errors is confusing. All parse errors are syntax errors. I suggest we use something like `UnsupportedSyntax` and update all structs/fields accordingly.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:310 on 2025-02-25 09:20_

Yeah, I think we should rename this to something else, I like Micha's suggestion. I also want to point out that the `syntax_errors` method should be renamed as well otherwise it could be confused with `errors` method that currently exists on `Parsed`.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2271 on 2025-02-25 09:23_

Maybe we could highlight the `match` token instead? That's what Pyright do as well: https://pyright-play.net/?pythonVersion=3.8&strict=true&code=LYQwLgxgFgBAjALgFA1TCIDOBTey0EwB0JQA

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:330 on 2025-02-25 09:27_

There's a `is_valid` method down below which checks whether the `Parsed` has any `ParseError`.

I don't think it should account for the `SyntaxError`s because otherwise it would change the behavior of downstream tools but I would update the documentation of `is_valid` to add this.

---

_Review comment by @dhruvmanila on `crates/red_knot_project/src/lib.rs`:349 on 2025-02-25 09:45_

What's the motivation for extending the syntax errors only if there were no parse errors? Shouldn't we extend it unconditionally?

This might not require to be handled in this PR if we decide to move out red-knot related changes in a separate PR.

---

_@dhruvmanila reviewed on 2025-02-25 09:47_

---

_Comment by @dhruvmanila on 2025-02-25 09:53_

@ntBre Can you update the test plan by testing this out in an editor context (VS Code extension) along with setting [`showSyntaxErrors`](https://docs.astral.sh/ruff/editors/settings/#showsyntaxerrors) as `on` and `off`? Feel free to message me on Discord if you need help in setting up a local development version of VS Code. The [contributing guide](https://github.com/astral-sh/ruff-vscode/blob/main/CONTRIBUTING.md) will be useful here.

---

_@MichaReiser reviewed on 2025-02-25 10:29_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 10:29_

Looking through the other changes, I understand that you use the `syntax-error` code for Ruff. It's unclear to me why we'd then use dedicated error codes in Red Knot over also just using `invalid-syntax`.

---

_@ntBre reviewed on 2025-02-25 13:28_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 13:28_

That makes sense, thank you! I put it back to `InvalidSyntax` for the sake of the red-knot follow-up PR.

---

_@MichaReiser reviewed on 2025-02-25 13:31_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 13:31_

We could also have one `UnsupportedSyntax` error code. It's a bit inconsistent with Ruff but I do find it somewhat appealing.

---

_@ntBre reviewed on 2025-02-25 13:33_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:435 on 2025-02-25 13:33_

I like that, thanks!

---

_@ntBre reviewed on 2025-02-25 13:39_

---

_Review comment by @ntBre on `crates/ruff_db/src/parsed.rs`:24 on 2025-02-25 13:39_

Ah that makes sense. I definitely wanted to call `Program::get(db).python_version(db)` inside of `parsed_module` until I realized that `Program` wasn't available. I will keep this in mind for the separate PR.

---

_@ntBre reviewed on 2025-02-25 13:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:512 on 2025-02-25 13:42_

Oops, good catch.

---

_@AlexWaygood reviewed on 2025-02-25 13:43_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 13:43_

One reason why we might want to have different error codes for different syntax errors is so that we can provide more detailed documentation for some syntax errors. This isn't _much_ of a concern with the specific syntax error that @ntBre is adding here, as there's not much more to say than "you can't use the `match` statement on Python <3.10".[^1]. However, as we discussed on Brent's design proposal, it will be a concern with other syntax errors that we'll want to detect in the future -- for example, we have very nice docs for [`F404`](https://docs.astral.sh/ruff/rules/late-future-import/#late-future-import-f404) currently in Ruff, and it would be a shame to provide worse docs for red-knot users when we start detecting that syntax error in our brand new tool.

Some errors that we may want to provide per-error docs for are errors that only appear on older or newer Python versions. For example, the details around when you're able to parenthesize context managers on Python <3.9 are pretty subtle. So are the details on when `match` patterns are considered [irrefutable](https://peps.python.org/pep-0634/#irrefutable-case-blocks) (it's a syntax error to have more than one irrefutable pattern in a `match` statement on Python 3.10+).

@MichaReiser also pointed out in the comments on Brent's design doc that there are existing syntax errors the parser detects that probably might also benefit from better docs.

(To be clear, I'm not saying that we _have_ to have multiple error codes for these syntax errors. And there are obviously costs as well as benefits, which Micha outlined well. But this is one possible motivation for why it might help to have different error codes for different syntax errors: it would allow users to easily look them up in our documentation.)

[^1]: That said, I actually don't think it would _hurt_ to have a documentation page that links to the relevant PEPs that introduced the `match` statement, and/or the Python docs for the `match` statement.

---

_@ntBre reviewed on 2025-02-25 13:46_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:461 on 2025-02-25 13:46_

I made this use `minimum_version` for now. We could still probably just remove the method, though. I started generating these methods in a macro in #16308 anyway.

---

_@ntBre reviewed on 2025-02-25 13:47_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:478 on 2025-02-25 13:47_

It was used in the `LintName` construction, but I removed it now!

---

_@ntBre reviewed on 2025-02-25 13:52_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2271 on 2025-02-25 13:52_

Good idea, it did look pretty bad in the red-knot snapshot.

---

_@ntBre reviewed on 2025-02-25 13:58_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:330 on 2025-02-25 13:58_

Good catch! I also updated the docs for `Parsed::as_result` and `Parsed::into_result`.

---

_@ntBre reviewed on 2025-02-25 13:59_

---

_Review comment by @ntBre on `crates/red_knot_project/src/lib.rs`:349 on 2025-02-25 13:59_

I was trying to mirror the type errors, but I guess it does make more sense to treat them more like the other parse errors. I'll still make the change so that the follow-up is easier!

---

_@MichaReiser reviewed on 2025-02-25 14:05_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 14:05_

yeah, that's one reason I'd see benefits in using different codes. But I think we should do so consistently between Ruff and Red Knot. 

The nice thing about the errors not being suppressable or configurable is that splitting syntax error into more codes in the future isn't a breaking change. I'm leaning towards using a single (or two) error codes for a first version, considering that Ruff doesn't support documenting error codes other than rules (and Red Knot doesn't have the infrastructure but it's at least designed so that we could)

---

_Comment by @ntBre on 2025-02-25 14:09_

Good call @dhruvmanila. I think I must have missed wiring this up in the LSP because I'm not getting any errors with a `match` with a target version of 3.8 in my `ruff.toml`. I'll take a look at that now.

---

_@AlexWaygood reviewed on 2025-02-25 14:09_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 14:09_

Providing good documentation for the more complex syntax errors (for both Ruff and red-knot) is quite important to me. I'm happy to leave it out of this first PR, but I do think we should make sure that we have a solution for this before stabilising these rules.

---

_@MichaReiser reviewed on 2025-02-25 14:20_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 14:20_

Makes sense. It would probably be good to create an issue for this so that we can explore different options on how we can accomplish this (e.g. a non-error-code specific solution could be to maintain the documentation on our website and link to them from the diagnostic)

---

_@ntBre reviewed on 2025-02-25 14:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:397 on 2025-02-25 14:41_

Ooh good catch. I was able to remove the resolution in `check_path` by passing the version to it everywhere. The other three resolutions in the linter are in `add_noqa_to_path`, `lint_only`, and `lint_fix` and right before `parse_unchecked` calls, so I think those are needed.

This is probably a separate issue, but do we need to re-parse (and re-resolve) in `add_noqa_to_path`? I guess it gets called before we know what else we're doing with the files.

---

_Comment by @ntBre on 2025-02-25 16:13_

I think I've addressed all of the feedback here, but one thing Micha and I discussed in our 1:1 is checking on resetting diagnostics on speculative parsing. I have not done that yet.

---

_@MichaReiser approved on 2025-02-25 16:23_

Thanks for addressing the feedback. 

One last thing that I missed in my last review. We need to ensure that we handle version related syntax errors correctly when doing speculative parsing -- that means, we have to drop them if it turns out that the parser took the wrong branch. 

You can do this by capturing the length of the unsupported syntax errors vec in the `checkpoint` method and truncate the errors in the `rewind` method

https://github.com/astral-sh/ruff/blob/97d0659ce3e3977245ca202770078a9df60849dd/crates/ruff_python_parser/src/parser/mod.rs#L657-L682 

---

_Comment by @MichaReiser on 2025-02-25 16:56_

I hope the checkpointing doesn't regress performance... No, it all looks green. Nice

---

_@ntBre reviewed on 2025-02-25 17:29_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic.rs`:76 on 2025-02-25 17:29_

I opened an issue with Alex's comment above: https://github.com/astral-sh/ruff/issues/16377

I don't have strong feelings either way right now, but I think it's a good idea to keep this in mind, especially as we get to the more complicated errors.

---

_Comment by @ntBre on 2025-02-25 17:31_

Thanks again everyone for the reviews! I'll leave this open until tomorrow or so in case @dhruvmanila wants to have another look, but as far as I know this is ready to merge!

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:205 on 2025-02-26 03:01_

nit: I think we can merge this into the above chain of `parsed.errors()` as we've already checked the `show_syntax_errors` flag. Like:
```rs
parsed.errors().iter().map(...).chain(parsed.unsupported_syntax_errors().iter().map(...))
```

---

_@dhruvmanila approved on 2025-02-26 03:02_

This is great. Thank you for waiting on me and I like the updated test plan with the editor screenshots. Just a minor nit, feel free to ignore it.

---

_@ntBre reviewed on 2025-02-26 03:25_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:205 on 2025-02-26 03:25_

Nice catch!

---

_Merged by @ntBre on 2025-02-26 04:03_

---

_Closed by @ntBre on 2025-02-26 04:03_

---

_Branch deleted on 2025-02-26 04:03_

---
