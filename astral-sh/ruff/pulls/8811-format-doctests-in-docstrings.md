```yaml
number: 8811
title: format doctests in docstrings
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fmt/docstrings
created_at: 2023-11-21T20:14:21Z
updated_at: 2023-12-14T12:49:59Z
url: https://github.com/astral-sh/ruff/pull/8811
synced_at: 2026-01-10T23:31:11Z
```

# format doctests in docstrings

---

_Pull request opened by @BurntSushi on 2023-11-21 20:14_

## Summary

This PR adds opt-in support for formatting doctests in docstrings. This reflects initial support and it is intended to add support for Markdown and reStructuredText Python code blocks in the future. But I believe this PR lays the groundwork, and future additions for Markdown and reST should be less costly to add.

It's strongly recommended to review this PR commit-by-commit. The last few commits in particular implement the bulk of the work here and represent the denser portions.

Some things worth mentioning:

* The formatter is itself not perfect, and it is possible for it to produce invalid Python code. Because of this, reformatted code snippets are checked for Python validity. If they aren't valid, then we (unfortunately silently) bail on formatting that code snippet.
* There are a couple places where it would be nice to at least warn the user that doctest formatting failed, but it wasn't clear to me what the best way to do that is.
* I haven't yet run this in anger on a real world code base. I think that should happen before merging.

Closes #7146 

## Test Plan

* [x] Pass the local test suite.
* [x] Scrutinize ecosystem changes.
* [x] Run this formatter on extant code and scrutinize the results. (e.g., CPython, numpy.)


---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-21 20:14_

---

_Review requested from @konstin by @BurntSushi on 2023-11-21 20:14_

---

_Comment by @github-actions[bot] on 2023-11-21 20:30_

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

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1214 on 2023-11-21 20:49_

Nit: "should not be allowed"

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-21 20:51_

Yeah I'm not sure. I suspect the behavior you have here _is_ desirable? (Any idea what other tools do?)

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:275 on 2023-11-21 20:52_

This is getting skipped because the doctest fails to parse, right? As in, it's invalid Python?

---

_@charliermarsh reviewed on 2023-11-21 20:53_

---

_@charliermarsh reviewed on 2023-11-21 20:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1552 on 2023-11-21 20:55_

Slightly superficial comment, but what's your reaction to splitting the docstring-specific code out into its own file?

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-21 21:52_

I feel like we're going to end up bikeshedding on this name... a few ideas for consideration:

- `include_docstrings=true` (I like this but it doesn't convey that we're describing _code_ in docstrings)
- `docstrings=true`
- `include_embedded_code=true`
- `include_docstring_code=true`
- `include_docstring_snippets=true`

---

_@charliermarsh reviewed on 2023-11-21 21:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1196 on 2023-11-21 21:54_

Hmm yeah. I guess the warning utilities only exist in the linter. We could move those macros to (e.g.) `ruff_macro`, or something, but I'm not hugely concerned about this.

---

_@charliermarsh reviewed on 2023-11-21 21:54_

---

_@charliermarsh reviewed on 2023-11-21 21:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1274 on 2023-11-21 21:58_

Nit: "in which case" (I know this was just moved, but I re-read it so...)

---

_@charliermarsh reviewed on 2023-11-21 21:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-21 21:58_

Hmm, we may need to preserve line endings here, right? What if the code uses non-LF line endings, or the formatter has a line ending set?

---

_@charliermarsh reviewed on 2023-11-21 21:59_

Overall, this looks great, and I appreciate the way you broke it down by commit. I ended up reading the commits one-by-one and found the flow very clear.

---

_@BurntSushi reviewed on 2023-11-22 14:07_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-22 14:07_

So it looks like `blacken-docs` behaves like us. But [`blackdoc` passes the current indentation width to `black`](https://github.com/keewis/blackdoc/blob/822385ba8955e44397288caa07a2d2e3dc933b1e/blackdoc/blacken.py#L72) when reformatting a code snippet. From playing with it, it does indeed look like that the max line width is preserved uniformly.

I will also say that as someone who cares a lot about line width, I would want the formatter to wrap lines according to my setting in a global way. i.e., The code snippets shouldn't be treated as starting from the first column.

I'll take a quick look and see if I can make this work _robustly_. (Emphasis on "robust." This was why I didn't explore it too much initially.)

---

_@BurntSushi reviewed on 2023-11-22 14:08_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:275 on 2023-11-22 14:08_

Yup! I added a clarifying note to the comment.

---

_@BurntSushi reviewed on 2023-11-22 14:09_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1552 on 2023-11-22 14:09_

I thought about this while coding and I was torn. I think I ultimately didn't do it because I thought that we might eventually want to refactor it further (possibly) so that it can be used in other contexts aside from formatting Python code. So I didn't want to spend too much time on making it a "proper" abstraction. With that said, I do believe it lifts pretty cleanly out of this module. I'll try it out and see how it feels.

---

_@BurntSushi reviewed on 2023-11-22 14:13_

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-22 14:13_

Yeah that was why I just picked the first thing that came to mind.

Of your list, I like `include_docstring_code` the best. But I think I do actually like `docstring_code` the best overall. It kind of feels like the word "include" is a little redundant? Looking at the names of other options, it doesn't look like there is any strong precedent here. Although using "include" here could start a precedent. (I don't know whether we plan on adding a ton of options or not though.)

---

_@charliermarsh reviewed on 2023-11-22 14:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1552 on 2023-11-22 14:13_

It would also be fine as a separate change / PR after-the-fact if easier and less distracting.

---

_@BurntSushi reviewed on 2023-11-22 14:14_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1196 on 2023-11-22 14:14_

All righty. I'll keep it out of scope for now. cc @zanieb since I think you might care about this and might have other ideas  or opinions.

---

_@BurntSushi reviewed on 2023-11-22 14:53_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-22 14:53_

I don't believe so, no. I could use `\r\n` here and things would still turn out okay. I did it this way because immediately after this, we reformat the code snippet with the user's settings, which should include their line ending preferences.

With that said... I tried to write a test for this and failed. I _suspect_ something is mangling or normalizing line endings in our test infrastructure. If I run the formatter directly, then line endings are preserved:

```
$ rg '\x0D' /tmp/test.py
$ cat /tmp/ruff.toml
[format]
docstring-code = true
$ cargo run -qp ruff_cli format --config /tmp/ruff.toml - < /tmp/test.py | rg '\x0D'
$ cat /tmp/ruff.toml
[format]
docstring-code = true
line-ending = 'cr-lf'
$ cargo run -qp ruff_cli format --config /tmp/ruff.toml - < /tmp/test.py | rg '\x0D'
def doctest_line_ending():
    """
    Do cool stuff.
    >>> def foo(x):
    ...     print(x)
    ...
    ...     print(x)
    """
    pass
```

I'll take a look and see if I can figure out what's swallowing line endings. When I add a test with `line-ending = "CarriageReturnLineFeed"`, the resulting snapshot does not contain CRLF line endings, but LF only.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-22 15:57_

Take a look at the `.gitattributes` in the repo root. I believe we normalize to LF except in explicit cases.

---

_@charliermarsh reviewed on 2023-11-22 15:57_

---

_@zanieb reviewed on 2023-11-22 16:18_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-22 16:18_

I was okay with this on the first read, but on a second, I think `docstring_code = true | false` doesn't have enough meaning alone. Perhaps `format_docstring_code` would make sense? I also don't mind `include_docstring_code` but it's not necessarily clear what inclusion means.

---

_@zanieb reviewed on 2023-11-22 16:19_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/expression/string.rs`:1196 on 2023-11-22 16:19_

Seems really reasonable to move the warning macros unless it's infeasible for some reason. A warning seems better than silence — I'd be happy with a new issue to add it later.

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-22 16:29_

Yeah I hadn't gone with `format_docstring_code` since the option exists in the context of a formatter. But I think I do like that better than `include_docstring_code`.

---

_@BurntSushi reviewed on 2023-11-22 16:29_

---

_@BurntSushi reviewed on 2023-11-22 16:30_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-22 16:30_

Ahhhhhhhhhh right. Forgot about `.gitattributes`. That's it.

---

_@zanieb reviewed on 2023-11-22 16:56_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-22 16:56_

A separate setting _might_ make sense e.g. `docstring_code_line_width`. I can see arguments for both sides. For me, I care more about a reasonable width in my reference documentation generated from my docstrings and would be okay with starting on the first column.

On a slightly separate note, `docstring_code_...` settings suggest that `docstring_code_enabled` may be a better name for toggling this behavior?

---

_@BurntSushi reviewed on 2023-11-22 17:01_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-22 17:01_

Yeah, that's a good point.

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-22 17:24_

OK, here is what I suggest:

* We keep the current behavior which treats the code snippet as starting at column 1. This I think aligns with what @zanieb said about prioritizing the display of the code snippet.
* We create an issue tracking a new feature for overriding this behavior to do something different.

Probably `docstring_code_line_width` would be a good thing to add, but I think there's also a different mode of "respect the top-level line width setting." I don't think you can get that behavior by specifying a distinct concrete docstring code line width, since the formatter needs to take indentation into account.

So basically, there's some implementation work here to change the behavior and also some decisions that need to be made about how we present this behavior.

If this sounds good to y'all I can create a new issue capturing this.

---

_@BurntSushi reviewed on 2023-11-22 17:24_

---

_@charliermarsh reviewed on 2023-11-22 17:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-22 17:26_

I think it's reasonable to proceed with the current behavior and see how it lands with users. We can wait for feedback before prioritizing any further changes.

---

_@charliermarsh reviewed on 2023-11-22 17:31_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-22 17:31_

I still feel like none of these are quite right (the docstring itself _is_ code)... It's a really hard thing to convey succinctly. I wish we could do `format_doctests` but this needs to generalize to embedded Markdown and rST. Is `format_docstring_snippets` clearer or worse? (At the end of the day, I can live with `format_docstring_code`.)

---

_Comment by @BurntSushi on 2023-11-22 18:06_

I ran the docstring code formatter on CPython (after formatting the code _without_ docstring code formatting enabled) and put the results in one commit: https://github.com/BurntSushi/cpython/commit/19d1c85db9c862172f0cffd0872a942aef26d934

I did a quick pass through them and everything looks like it passes the eyeball test. My next step is to make sure the doctests still run and pass.

---

_Comment by @BurntSushi on 2023-11-22 19:00_

> My next step is to make sure the doctests still run and pass.

I checked out the `main` branch of CPython and built it:

```
$ export CFLAGS="${CFLAGS/-O2/-O3} -ffat-lto-objects"
$ ./configure --prefix=/usr \
              --enable-shared \
              --with-computed-gotos \
              --enable-optimizations \
              --with-lto \
              --enable-ipv6 \
              --with-system-expat \
              --with-dbmliborder=gdbm:ndbm \
              --with-system-libmpdec \
              --enable-loadable-sqlite-extensions \
              --without-ensurepip \
              --with-tzpath=/usr/share/zoneinfo
$ LC_CTYPE=en_US.UTF-8 make -j8 EXTRA_CFLAGS="$CFLAGS"
```

Then confirmed that I could run some doctests:

```
$ ./python -m doctest Lib/ipaddress.py -v
```

And some of that failed, but the [`summarize_address_range`](https://github.com/BurntSushi/cpython/commit/19d1c85db9c862172f0cffd0872a942aef26d934#diff-f67a8b5be6bdc48383d2ab907325a4c75ef998fa7cd0bba382ec2efa7e4f00b5R204-R205jj) doctest passed.

Then I checked out my branch with the reformatted code snippets and re-ran the above. The results remain unchanged.

So at least in that one specific case that looked potentially odd, reformatting didn't break the doctest. (Specifically, the doctest directive in this example is still picked up.)

I haven't been able to figure out how to run all of the doctests yet though.

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/normalizer.rs`:19 on 2023-11-23 11:23_

> This changes the AST normalizer to remove anything that looks like a
code snippet.

Could we instead remove docstrings from the equality check?

---

_Review comment by @konstin on `crates/ruff_workspace/src/options.rs`:2831 on 2023-11-23 11:31_

I think i'd want an option (opt-in) that requires examples to be at least valid python. Maybe a `docstring-code` variant for `always` or `enforce`?

---

_Review comment by @konstin on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-23 11:34_

Adding `format_embedded` to the list of options.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/string.rs`:1079 on 2023-11-23 11:46_

Taking micha's role here: This should not reference the `Formatter` but instead `impl Format<PyFormatContext<'_>> for DocstringLinePrinter<'_>`, or take the formatter as an argument in the print functions.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/string.rs`:1196 on 2023-11-23 12:34_

Strongly prefer showing a warning over failing silently

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/string.rs`:1229 on 2023-11-23 12:43_

We should create an issue for removing this, parsing twice every time seems wasteful

---

_@konstin approved on 2023-11-23 12:44_

---

_@BurntSushi reviewed on 2023-11-27 14:48_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-27 14:48_

So one thing I uncovered today is that by allowing docstring lines to exceed the globally configured line width, you wind up with linter errors on long lines. For example, after running the docstring code snippet formatter on polars: https://github.com/BurntSushi/polars/actions/runs/7006619426/job/19058868021?pr=1

---

_@BurntSushi reviewed on 2023-11-27 14:52_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/tests/normalizer.rs`:19 on 2023-11-27 14:52_

I think we could, but I think that would potentially reduce the effectiveness of the test. Today, we still test that the substance of the docstring remains the same (modulo whitespace and code snippets). But if we just removed docstrings entirely, we wouldn't be checking anything.

I don't feel too strongly personally.

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:2831 on 2023-11-27 14:54_

My first instinct here is that that would be more of a lint then a formatting option. I'm not sure though.

---

_@BurntSushi reviewed on 2023-11-27 14:54_

---

_@BurntSushi reviewed on 2023-11-27 14:56_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1079 on 2023-11-27 14:56_

Can you say more? I'm not sure I follow. The `DocstringLinePrinter` is basically one big logical function call split up into several actual function calls.

---

_Review requested from @MichaReiser by @BurntSushi on 2023-11-27 15:16_

---

_Comment by @BurntSushi on 2023-11-27 15:39_

I've split the commit that adds a user facing config option out into #8854 so that we should be able to merge this PR without any user facing changes.

---

_@BurntSushi reviewed on 2023-11-27 16:01_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:199 on 2023-11-27 16:01_

Issue created: https://github.com/astral-sh/ruff/issues/8855

---

_@BurntSushi reviewed on 2023-11-27 16:01_

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/settings.rs`:130 on 2023-11-27 16:01_

PR for the user facing change created (and removed from this PR): https://github.com/astral-sh/ruff/pull/8854

---

_@BurntSushi reviewed on 2023-11-27 16:05_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1196 on 2023-11-27 16:05_

Issue created: https://github.com/astral-sh/ruff/issues/8856

---

_@BurntSushi reviewed on 2023-11-27 16:08_

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:2831 on 2023-11-27 16:08_

Carried this over to #8854: https://github.com/astral-sh/ruff/pull/8854#issuecomment-1828141780

---

_@BurntSushi reviewed on 2023-11-27 16:12_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1229 on 2023-11-27 16:12_

Done https://github.com/astral-sh/ruff/issues/8857

---

_Comment by @BurntSushi on 2023-11-27 16:14_

All righty, I've filed issues for problems that probably aren't blocking this PR being merged:

* https://github.com/astral-sh/ruff/issues/8855
* https://github.com/astral-sh/ruff/issues/8856
* https://github.com/astral-sh/ruff/issues/8857
* https://github.com/astral-sh/ruff/issues/8860
* https://github.com/astral-sh/ruff/issues/8859

I'm going to go ahead and merge this PR. I'd be happy to address any other feedback folks might have!

---

_Merged by @BurntSushi on 2023-11-27 16:14_

---

_Closed by @BurntSushi on 2023-11-27 16:14_

---

_Branch deleted on 2023-11-27 16:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/normalizer.rs`:81 on 2023-11-27 23:59_

It may be helpful to replace the code snippets with a constant instead, e.g `<CODE_SNIPPED: Removed by `Normalizer`>`. It may otherwise be confusing if the AST comparison fails on a docstring that doesn't exist in the source code.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1079 on 2023-11-28 00:04_

I think the way it is implemented is fine, considering that `print` takes `lines` as an argument. 

The alternative (that @konstin) suggested is to instead pass `lines` to the `DocstringLinePrinter` constructor and implement `Format` for `DocstringLinePrinter`. I would then probably rename the struct to `DocstringLines`, because it is now a struct that knows how to format itself. 

However, I believe the problem with that is that `Format::format` takes `&self` and not `&mut self`, making it impossible for the printer to update its internal fields (without using internal mutability)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1278 on 2023-11-28 00:12_

It took me a while to understand when the line is owned or borrowed although it is well documented. It may help to repeat the struct documentation in a short form:

The actual text (Borrowed: original line in the source, Owned: formatted line) of the line

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1383 on 2023-11-28 00:14_

Could we add a link to where this regex can be found?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1394 on 2023-11-28 00:16_

Future perf improvement: Represent as an enum with `Tabs(count)`, `Spaces(count)`, and `Mixed(String)` to avoid allocating for each formatted line. 

Or could the indent be a ` &'a str` because it is the indent observed from the source text?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1453 on 2023-11-28 00:24_

Should `Ignore` be the `Print` action? Maybe use [`Ignore`](CodeExampleAddAction::Print) so that rustfmt can point out the invalid refernece.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1364 on 2023-11-28 00:25_

What's the reason for converting these to `String` rather than storing borrowed `&str`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1497 on 2023-11-28 00:28_

I addmit I haven't read the code in full, but would it be sufficient to check if the indents have the same column/character length as done in the Lexer or is it necessary that the strings match exactly?

See [`Indentation`](https://github.com/astral-sh/ruff/blob/8ce138760a182b4765fa6f57b865fe6e9507f41e/crates/ruff_python_parser/src/lexer/indentation.rs#L35-L80) in our lexer.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-28 00:36_

How about line endings inside of multiline strings? Ideally, we would preserve these. Although we may deliberately decide not to and encourage users to instead use explicit line endings. But changing the line endings inside of strings is theoretically a semantic change.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1240 on 2023-11-28 00:37_

Could we short-circuit if the source string doesn't contain any triple quotes (or escaped triple quotes)?

---

_@MichaReiser reviewed on 2023-11-28 00:39_

Thanks for writing the excellent documentation and organizing your commits so thoughtfully. It helped a lot when reviewing the PR (also to find savepoints when being interrupted)

Really impressive work. Well done. I've a few smaller comments. The most important one from a correctness point of view is the handling of newlines inside of multiline strings. 


I think I'll learn many new English words from your commit messages 😆. So far: 

* rejigger
* grok



---

_@BurntSushi reviewed on 2023-11-28 16:22_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/tests/normalizer.rs`:81 on 2023-11-28 16:22_

Yeah I agree that would be nice. I tried it out and unfortunately it's a little tricky to get this right with a non-empty replacement string. I believe the issue here is that reformatting a code snippet can result in more (or fewer) PS2 lines, which in turn means that we'll get more (or fewer) `<CODE_SNIPPET: ...>` replacements.

I could change the regex to try to match all PS1 and PS2 prompts within a single snippet and replace it in bulk... Hmmm... Let me try that. OK, that was easy. Hah.

Good suggestion!

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1079 on 2023-11-28 16:25_

Aye. I also expect this abstraction to get mangled and refactored at some point in the future. Namely, I expect that it will become useful to have "look for Python code snippets" logic outside of the context of the Python formatter. That is to say, I don't expect this is its final form. :-)

---

_@BurntSushi reviewed on 2023-11-28 16:25_

---

_@BurntSushi reviewed on 2023-11-28 16:27_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1278 on 2023-11-28 16:27_

Love it. Good catch.

---

_@BurntSushi reviewed on 2023-11-28 16:29_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1383 on 2023-11-28 16:29_

There are some links further down in the actual doctest parsing code, but I've added another one here.

---

_@BurntSushi reviewed on 2023-11-28 16:32_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1394 on 2023-11-28 16:32_

I believe the indent could indeed be a `&'src str` here, but I went with the simpler approach because the extra allocation here seems marginal to me. We're already allocating other stuff, invoking the formatter and so on.

The reason why I chose to thread through a lifetime parameter for `DocstringLine` (and not here) is that it gets created for every line in every docstring, regardless of whether there is a code snippet. So I wanted to be a little more cautious there. But by the time we create an `indent` string, we've already (very likely) committed ourselves to a bunch of extra costs too.

---

_@BurntSushi reviewed on 2023-11-28 16:34_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1453 on 2023-11-28 16:34_

Yup, good catch. I had originally named it `Ignore` but thought better of it. Forgot to update the docs referring to it.

---

_@BurntSushi reviewed on 2023-11-28 16:37_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1364 on 2023-11-28 16:37_

In addition to [my other answer](https://github.com/astral-sh/ruff/pull/8811#discussion_r1408057410), at least with respect to `code`, I think when I was writing the types it wasn't obvious to me that the code portion of a line would necessarily be a proper substring of it. But I can't think of any contrary case.

---

_@BurntSushi reviewed on 2023-11-28 16:43_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1497 on 2023-11-28 16:43_

The Python `doctest` module actually requires that the indent be made purely of ASCII space characters. Then it somewhat circuitously takes the length of the indentation in characters, reconstitutes the indentation as a string and then checks that all PS2 prompt lines have the same indentation prefix: https://github.com/python/cpython/blob/0ff6368519ed7542ad8b443de01108690102420a/Lib/doctest.py#L727-L733

Technically we are a little more relaxed here because we allow any kind of whitespace in the indentation. But I preserved the "indentation should be byte-for-byte equivalent" check.

So bottom line is that it's hard for me to say whether this is necessary or not. Probably not. But I do think it is pretty simple? Are you concerned about the costs of carrying the indentation string around? I think for code snippet formatting it is probably a lot less of a concern than when parsing arbitrary Python. The size scales are different.

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-28 16:46_

I think Charlie had a similar comment above. The thinking here is that while I'm using `\n` to assemble a code snippet, that snippet is then fed into the formatter and the line endings used correspond to the user's setting. There is a test for example that this writes out CRLF line endings in code snippets when the user has configured CRLF line endings.

Did I understand you concern correctly here? I feel like I might be missing it.

---

_@BurntSushi reviewed on 2023-11-28 16:46_

---

_@BurntSushi reviewed on 2023-11-28 16:47_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-28 16:47_

Also, there are places in the docstring handling (that existed before my changes) that seem to assume line feed terminators? For example:

https://github.com/astral-sh/ruff/blob/2ade84a29de99eafbac23ef4c6dc9cdf1101e014/crates/ruff_python_formatter/src/expression/string/docstring.rs#L237-L238

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string.rs`:1240 on 2023-11-28 16:48_

Yeah I think you mentioned this in the tracking issue for this. I _think_ you're correct. And if so, yes, I believe we could short circuit. I'm not sure though if we want to keep this check as-is here for now as a conservative posture.

---

_@BurntSushi reviewed on 2023-11-28 16:48_

---

_@MichaReiser reviewed on 2023-11-28 23:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1217 on 2023-11-28 23:33_

Yeah, I think the problem is not new due to your changes. It only became evident that this might now be a problem when thinking about code snippets in docstrings (it shouldn't be a problem for non-code text). 

```python
a = """A multiline string
with windows line endings
"""

b = c
```
<img width="859" alt="Screenshot 2023-11-29 at 07 23 49" src="https://github.com/astral-sh/ruff/assets/1203881/f6eb79e4-0844-486a-bb95-046de837fd6d">

My concern is (was) that changing the newlines **inside** of the multiline string could be a semantic change. I tried to read through the Python documentation and only found:

> In triple-quoted literals, unescaped newlines and quotes are allowed (and are retained), except that three unescaped quotes in a row terminate the literal. (A “quote” is the character used to open the literal, i.e. either ' or ".) [source](https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals)

What I understand from the spec is that newlines are retained. Although running the above in a `print` on my mac normalizes the newlines to `\n`. 

I then tested what the about for

```python
a = b"""A multiline string
with windows line endings
"""

from binascii import hexlify

print(hexlify(a))
```

and it seems python normalizes the newlines even for binary strings to `\n`. So the retain only means that the newlines are preserved but not in the form they're present in the source code.

So I guess this is not a problem after all (and ruff and Black both normalize newlines inside multiline strings)




---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1497 on 2023-11-28 23:35_

I was mainly trying to understand the semantics to ensure we are as strict as Python when it comes to handling indentation. Thanks for the explanation.

---

_@MichaReiser reviewed on 2023-11-28 23:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:1240 on 2023-11-28 23:37_

Understood. My only concern is performance because our parser isn't very fast today (I believe it's about 30% or more of the overall formatting time). An alternative could be to only lex the code and see if there are any lexer errors. But I don't know if that's sufficient to detect the kind of errors that could be introduced.

But agree, I don't think it's a high priority. Just trying to brainstorm a few ideas with the hope that there might be a low hanging perf improvement.

---

_@MichaReiser reviewed on 2023-11-28 23:37_

---

_Comment by @JacobCoffee on 2023-12-13 20:37_

~Can #3792, #8237 be closed then?~
Oh i see, 

> This reflects initial support and it is intended to add support for Markdown and reStructuredText Python code blocks in the future.

🔜 ™️ 

---

_Comment by @BurntSushi on 2023-12-14 12:49_

@JacobCoffee Yeah I think those issues are about applying `ruff` to Python code in contexts other than Python source files. Like `.md` or `.rst` files. And I think it also applies to `ruff` generally and not just `ruff format`.

---
