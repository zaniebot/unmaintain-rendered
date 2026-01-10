```yaml
number: 13695
title: "[red-knot] document test framework"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/test-framework-docs
created_at: 2024-10-09T23:04:12Z
updated_at: 2024-10-10T19:02:03Z
url: https://github.com/astral-sh/ruff/pull/13695
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] document test framework

---

_Pull request opened by @carljm on 2024-10-09 23:04_

This adds documentation for the new test framework.

I also added documentation for the planned design of features we haven't built yet (clearly marked as such), so that this doc can become the sole source of truth for the test framework design (we don't need to refer back to the original internal design document.)

Also fixes a few issues in the test framework implementation that were discovered in writing up the docs.


---

_Label `red-knot` added by @carljm on 2024-10-09 23:04_

---

_Review requested from @MichaReiser by @carljm on 2024-10-09 23:04_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-09 23:04_

---

_Comment by @github-actions[bot] on 2024-10-09 23:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @T-256 on `crates/red_knot_test/README.md`:151 on 2024-10-10 00:01_

```suggestion
x: int = y  # error: [invalid-assignment]
```

---

_Review comment by @T-256 on `crates/red_knot_test/README.md`:168 on 2024-10-10 00:03_

it's not clarified that will there any plan for future to bypass this limitation.

---

_Review comment by @T-256 on `crates/red_knot_test/README.md`:253 on 2024-10-10 00:22_

I think mdtest used to improve readability of tests, I don't think embedding json content of Jupyter Notebook could help to keep it readable. how about make a link feature to external file?
`````markdown
````markdown
# test jpyter notebook
```ipynb
<attach>path="./embeddeds/test1.ipynb"</attach>
```
````
`````

---

_@T-256 reviewed on 2024-10-10 00:29_

There is circular imports concept in Python, do you plan support it into mdtests?

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:70 on 2024-10-10 04:20_

Assuming that the quotes are required, can we clarify that part somewhere here?

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:91 on 2024-10-10 04:21_

I find "non-assertion-comment" a bit confusing because at first I thought it meant a non-assertion _comment_, so maybe the next statement / code line?

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:113 on 2024-10-10 04:23_

Should it be "only one file can use..." instead of "only zero or one files can use..."?

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:119 on 2024-10-10 04:24_

Did you mean?
```suggestion
reveal_type(C)  # revealed: Literal[C]
```

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:253 on 2024-10-10 04:30_

Or, maybe some kind of start and end indicator that signals that any code blocks between them should be in a single Jupyter Notebook file.

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:269 on 2024-10-10 04:32_

````suggestion
```toml
[tool.knot]
warn-on-any = true
```

````

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:326 on 2024-10-10 04:33_

````suggestion
```py
reveal_type(I_AM_THE_ONLY_BUILTIN)  # revealed: Literal[1]
```

````

---

_@dhruvmanila reviewed on 2024-10-10 04:35_

---

_Comment by @carljm on 2024-10-10 04:37_

> There is circular imports concept in Python, do you plan support it into mdtests?

I'm not sure what extra support mdtests would need for circular imports? It allows you to create whatever Python files you like with whatever contents you like, so you can create and test any circular import scenario you would want. 

---

_@carljm reviewed on 2024-10-10 04:42_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:168 on 2024-10-10 04:42_

It's an inherent limitation of `cargo test` that there is no way to dynamically create tests. I think to directly address this limitation we'd have to either write our own test runner, or use a build script to code-generate tests. Neither of those are planned. 

What I think is more likely to indirectly address this problem is that we could add some kind of "focus" indicator that's easy to add and remove from a test file that will cause it to only run that test from that file. Or we could do something similar that works via env var. 

---

_@carljm reviewed on 2024-10-10 04:57_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:253 on 2024-10-10 04:57_

I guess it depends what we need to test about notebooks. If all we care about is the Python contents of cells, then something like @dhruvmanila suggests makes sense. If we want to write tests in this format that actually test other characteristics of the JSON structure of a notebook, then I think we should embed the JSON directly. It doesn't help readability to have key aspects of the test hidden in a different file. 

---

_@dhruvmanila reviewed on 2024-10-10 05:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_test/README.md`:253 on 2024-10-10 05:04_

> If we want to write tests in this format that actually test other characteristics of the JSON structure of a notebook

I'm not sure if Red Knot should be testing the notebook JSON structure, I think it should be done as an integration tests which currently there are :)

But yeah, what you said makes sense and we can think about it when it's required.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:30 on 2024-10-10 08:16_

```suggestion
Two kinds of assertions are supported: `# revealed:` (shown above) and `# error:`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:43 on 2024-10-10 09:30_

I'd move everything after the em-dash here into a footnote. It's a useful clarification for what you mean by "pseudo-standard-library module", but it's also not directly relevant to the thing you're explaining.

Should we open this paragraph by talking about `typing_extensions`, and then after that discuss that it can also be imported from `typing` on newer versions of Python? If it's being imported in tests, I'd wager that 99% of the time you'll want to import it from `typing_extensions` rather than `typing`, since most tests are not going to be Python-version-specific.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:45 on 2024-10-10 09:46_

```suggestion
from typing_extensions import reveal_type
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:58 on 2024-10-10 09:47_

Is this documentation for people writing tests using the test framework, or for people developing the test framework? I assumed the former, in which case I think this delves too far into implementation details of exactly how the `unimported-reveal` diagnostics are suppressed by the test framework. (Well, in our current implementation, they're not exactly "suppressed" at all, but they're never visible to people writing or running the tests, so it comes to the same thing!)

```suggestion
For convenience, type checkers also pretend that `reveal_type` is a built-in, so that this import is
not required. Style preference is to usually not import it in tests, unless specifically testing
something about the behavior of importing it.

(Using `reveal_type` without importing it generally causes red-knot to issue a diagnostic warning
that it was used without importing it, to guard against errors at runtime. However, in the specific
context of tests using mdtest, this diagnostic is never emitted.)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:63 on 2024-10-10 09:50_

(not sure I love this. I feel like we could easily end up with tests silently and incorrectly passing because an unintended kind of error is being matched.)

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:74 on 2024-10-10 10:00_

```suggestion
Any combination of some or all of these can be used in a single assertion, but they must come in
order: first column, if present; then rule code, if present; then contains-text, if present. For
example, an assertion using all three would look like
`# error: 8 [invalid-assignment] "Some text"`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:76 on 2024-10-10 10:04_

```suggestion
Error assertions in tests intended to test type checker semantics should primarily use rule-code
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:81 on 2024-10-10 10:05_

this feels sorta tautological, and kind of obvious. I think I'd remove it.

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:91 on 2024-10-10 10:24_

Or "applies to the next line that does not contain an assertion comment"?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:108 on 2024-10-10 10:25_

```suggestion
assertion as the line of source code on which the matched diagnostics are emitted.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:112 on 2024-10-10 10:26_

```suggestion
Some tests require multiple files, with imports from one file into another. Multiple fenced code
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:113 on 2024-10-10 10:26_

or "at most one file can use..."?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:119 on 2024-10-10 10:27_

maybe we should doctest our documentation for mdtest 😆

(This is a joke, I don't know how we'd actually do that.)

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:127 on 2024-10-10 10:30_

I don't know that new contributors will necessarily know what "import root" means here, though I don't immediately have a better suggestion

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:137 on 2024-10-10 10:37_

"Markdown", "markdown" or "MarkDown"? You have two spellings in the same sentence here ;)

I don't mind too much, but we should at least be consistent!

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:163 on 2024-10-10 10:38_

```suggestion
The tests are run independently, in independent in-memory file systems and with new red-knot [Salsa](https://github.com/salsa-rs/salsa)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:165 on 2024-10-10 10:38_

```suggestion
databases. This means that each is a from-scratch run of the type checker, with no data persisting from any
previous test.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:167 on 2024-10-10 10:39_

```suggestion
Due to limitations of Rust's leading test runners, an entire test suite (Markdown file) is run as a single Rust
```

Or "commonly used", etc. I doubt it's an inherent limitation of all test runners written in Rust :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:201 on 2024-10-10 10:41_

Beware the comma splice!

```suggestion
A header-demarcated section must either be a test or a grouping header; it cannot be both. That is,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:208 on 2024-10-10 10:43_

```suggestion
the test framework) between fenced code blocks. This permits natural documentation of
why a test exists, and what it intends to assert:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:225 on 2024-10-10 10:45_

```suggestion
We may want to be able to assert that a diagnostic spans multiple lines, and to assert the columns it
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:226 on 2024-10-10 10:45_

```suggestion
begins and/or ends on. The planned syntax for this will use `<<<` and `>>>` to mark the start and end lines for
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:237 on 2024-10-10 10:48_

maybe this?

```suggestion
In cases of overlapping such assertions, resolve ambiguity using more chevrons: `<<<<` begins an
```

strictly speaking `<` is an "angle bracket", but in the realm of software I'd generally think of "bracket" as referring to one of the `()[]{}` characters.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:258 on 2024-10-10 10:49_

```suggestion
A fenced code block with no language will always be an error.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:255 on 2024-10-10 10:51_

```suggestion
Of course, red-knot is only run directly on `py` and `pyi` files, and assertion comments are only
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:245 on 2024-10-10 10:51_

```suggestion
We will allow specifying any of these using the `text` language in the code block tag string:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:263 on 2024-10-10 10:52_

```suggestion
We will add the ability to specify non-default red-knot configurations to use in tests, 
by including a TOML-fenced code block:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:278 on 2024-10-10 10:55_

```suggestion
It should be possible to include a TOML-fenced code block in a single test (as shown), or in a
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:281 on 2024-10-10 10:56_

I'd prefer "configurations" rather than "configs"... but you use "configs" pretty consistently, so I won't comment all instances of this 😛

```suggestion
grouping section, in which case it applies to all nested tests within that grouping section.
Configurations at multiple level are allowed and merged, with the most-nested (closest to the test)
taking precedence.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:288 on 2024-10-10 10:59_

```suggestion
of these can be replaced by real red-knot config options; some or all may also be kept long-term
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:296 on 2024-10-10 11:00_

```suggestion
Other future planned changes to configuration include:

- We should be able to configure the default workspace root to something other than `/src/` using a
`workspace-root` config option.

- We should be able to add a third-party root using the `third-party-root` config option.

- We may want to add additional config options for setting additional search path kinds.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:303 on 2024-10-10 11:01_

```suggestion
### Specifying a custom typeshed
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:331 on 2024-10-10 11:01_

scary

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:342 on 2024-10-10 11:03_

```suggestion
output of a tested Python snippet. Or sometimes (see “incremental tests” below) we will want to assert on diagnostics
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:345 on 2024-10-10 11:03_

```suggestion
`output`; this would contain the full diagnostic output for the preceding test file:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:355 on 2024-10-10 11:04_

```suggestion
This is just an example, not a proposal that red-knot would ever actually output diagnostics in
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:364 on 2024-10-10 11:04_

```suggestion
blocks, when tests are run in an update-output mode (probably specified by an environment variable.)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:366 on 2024-10-10 11:05_

```suggestion
By default, an `output` block will specify diagnostic output for the file `<workspace-root>/test.py`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:422 on 2024-10-10 11:06_

```suggestion
It will be possible to provide any number of stages in an incremental test. If a stage re-specifies a filename that
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:423 on 2024-10-10 11:06_

```suggestion
was specified in a previous stage (or the initial stage), that file is modified. A new filename
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:424 on 2024-10-10 11:06_

```suggestion
appearing for the first time in a new stage will create a new file. To delete a previously created
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:426 on 2024-10-10 11:07_

Closing parens should come before the period unless it's a full sentence inside the parens:

```suggestion
provide non-empty contents). Any previously-created files that are not re-specified in a later stage
```

---

_@AlexWaygood reviewed on 2024-10-10 11:09_

Thank you!! This is extremely valuable.

Despite having previously reviewed your proposal for this framework in Notion, and the actual implementation, it actually made me realise there were several aspects I wasn't aware of before reading this :-)

Some comments below, none huge:

---

_@carljm reviewed on 2024-10-10 14:30_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:43 on 2024-10-10 14:30_

Using a footnote for the digression is a good idea, I'll do that.

> I'd wager that 99% of the time you'll want to import it from `typing_extensions` rather than `typign`

I don't think this is true; I think rather that 99% of tests will not import it at all, and the only ones that do will be a few tests that are explicitly testing the handling of imported vs pseudo-builtin `reveal_type`, and of those few tests probably roughly half (i.e. just one or two tests each) will import it each way.

I think the more important consideration here is that the preferred and modern thing to do, if on a Python version where it's possible, is to import from `typing`, so that's what should be featured, and the legacy approach should be a parenthetical.

---

_@carljm reviewed on 2024-10-10 14:59_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:58 on 2024-10-10 14:59_

I think the current text accurately and precisely describes how they are handled, and this information is relevant to authors/readers of tests, because the details of how they are handled can be visible to authors of tests. For example, if you do `rt = return_type` and then `rt(foo) # revealed: ...` on another line, you will see unimported-reveal diagnostic on the `rt = return_type` line, and it won't be matched by the `# revealed: ...` assertion. And we will certainly have at least some tests that depend on these distinctions: the tests of the undefined-reveal diagnostic itself.

The proposed new text isn't noticeably shorter, and it has the disadvantage of being wrong :) It's simply not true that "this diagnostic is never emitted" in tests using mdtest. It is emitted as normal, but then matched, if present, by the `# revealed` assertion.

---

_@carljm reviewed on 2024-10-10 15:12_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:58 on 2024-10-10 15:12_

Reading this again, I do think the text could be re-ordered a bit to emphasize the most important thing (`# revealed: ` matches a revealed type from `reveal_type`) and then follow up with the stuff about undefined-reveal separately; I'll look at an edit along those lines.

---

_@carljm reviewed on 2024-10-10 15:14_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:63 on 2024-10-10 15:14_

I agree that we should probably never use this, it's just the most natural implementation to allow it. But it wouldn't be hard to issue an error if it's used. Personally I think it's OK to allow such assertions but just avoid using them. If we want to prohibit them, we have to decide what the precise rule should be. Is an assertion that only asserts the column OK? Or should the error code always be required? Or should it be "either error code or message-contains" that is required?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:63 on 2024-10-10 15:16_

> Personally I think it's OK to allow such assertions but just avoid using them.

I think it's inevitable that either one of us or a contributor adds one (either deliberately or accidentally) at some point, if we allow it!

> Or should it be "either error code or message-contains" that is required?

I would say this

---

_@AlexWaygood reviewed on 2024-10-10 15:16_

---

_@carljm reviewed on 2024-10-10 15:17_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:119 on 2024-10-10 15:17_

I would love to do that! But probably not a priority :)

---

_@carljm reviewed on 2024-10-10 15:18_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:127 on 2024-10-10 15:18_

I can try to make it more clear, maybe with reference to `sys.path`.

---

_@carljm reviewed on 2024-10-10 15:18_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:137 on 2024-10-10 15:18_

Agreed, I prefer "Markdown" and will aim to be consistent with that.

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:167 on 2024-10-10 15:19_

I'll probably be more specific here and just say due to `cargo test` limitations

---

_@carljm reviewed on 2024-10-10 15:19_

---

_@carljm reviewed on 2024-10-10 15:21_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:237 on 2024-10-10 15:21_

I think "chevron" (at least in the US context) is not an improvement in clarity; I've rarely seen that word used to describe `<` or `>` characters. I'll replace "brackets" with "angle brackets"; that seems clearest.

---

_@carljm reviewed on 2024-10-10 15:22_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:245 on 2024-10-10 15:22_

I was trying to steer away from language that suggests an ironclad commitment to do any of these things precisely as described...

---

_@carljm reviewed on 2024-10-10 15:24_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:263 on 2024-10-10 15:24_

Same comment as above; I was preferring to avoid definitive "will" language.

---

_@carljm reviewed on 2024-10-10 15:26_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:342 on 2024-10-10 15:26_

I'll probably use "embedded Python file" here for consistency

---

_@carljm reviewed on 2024-10-10 15:29_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:63 on 2024-10-10 15:29_

Ok, that seems reasonable, I'll look at making that behavior change in this PR.

---

_@AlexWaygood reviewed on 2024-10-10 15:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:245 on 2024-10-10 15:42_

Hmm... maybe "might" in that case? "Should" doesn't feel sufficiently future-tense-y to me

---

_@carljm reviewed on 2024-10-10 16:25_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:168 on 2024-10-10 16:25_

Added some discussion of this in the "future features" section.

---

_@carljm reviewed on 2024-10-10 16:26_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:245 on 2024-10-10 16:26_

But I guess this one we definitely will need, so sure, I'll use "will" :)

---

_@carljm reviewed on 2024-10-10 16:27_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:253 on 2024-10-10 16:27_

Updated the text here to not mention using `json` language and just say exact syntax is TBD.

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:263 on 2024-10-10 16:28_

Ok, looking at it again, I think this is another case where we are pretty committed to doing this.

---

_@carljm reviewed on 2024-10-10 16:28_

---

_@carljm reviewed on 2024-10-10 16:29_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:263 on 2024-10-10 16:29_

I don't think "TOML-fenced" is correct here. The code block is fenced by triple back ticks, it is not fenced by TOML. It is a "fenced code block" whose language is TOML.

---

_Comment by @carljm on 2024-10-10 16:33_

Ok, pushed an updated that I think addresses all comments.

I also realized in working on the change to require either rule or message-contains, that when a diagnostic doesn't match any assertion we should include its actual column in the "unexpected error" output, otherwise it's hard to debug a failure to match on the column, so I made this change as well. And in the course of making that change, I realized that some tests were not using `dedent` when they should, resulting in silly column numbers, so I fixed that as well.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:263 on 2024-10-10 16:33_

Ah I see! In that case I think maybe the adjective order is incorrect/confusing; that's certainly not how I read it. Possibly "fenced TOML code block" would be better?

---

_@AlexWaygood reviewed on 2024-10-10 16:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:68 on 2024-10-10 18:19_

nit: some very long lines here!

```suggestion
- `# error: "Some text"` requires that the matched diagnostic's full message contain the text
    `Some text`. (The double quotes are required in the assertion comment; they are not 
    part of the matched text.)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:74 on 2024-10-10 18:19_

```suggestion
then contains-text, if present. For example, an assertion using all three would look like
`# error: 8 [invalid-assignment] "Some text"`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:163 on 2024-10-10 18:20_

```suggestion
The tests are run independently, in independent in-memory file systems and with new red-knot
[Salsa](https://github.com/salsa-rs/salsa) databases. This means that each is a
from-scratch run of the type checker, with no data persisting from any previous test.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:279 on 2024-10-10 18:20_

```suggestion
It should be possible to include a TOML code block in a single test (as shown), or in a
```

---

_@AlexWaygood approved on 2024-10-10 18:21_

Thanks!

---

_@carljm reviewed on 2024-10-10 18:26_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:68 on 2024-10-10 18:26_

Oops, thanks.

---

_Comment by @carljm on 2024-10-10 18:44_

Thanks for the thorough review!

---

_Merged by @carljm on 2024-10-10 19:02_

---

_Closed by @carljm on 2024-10-10 19:02_

---

_Branch deleted on 2024-10-10 19:02_

---
