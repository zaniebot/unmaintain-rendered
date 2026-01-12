```yaml
number: 20443
title: "Display diffs for `ruff format --check` and add support for different output formats"
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - preview
  - diagnostics
assignees: []
merged: true
base: main
head: brent/formatter-diagnostics
created_at: 2025-09-16T22:08:46Z
updated_at: 2026-01-08T16:34:10Z
url: https://github.com/astral-sh/ruff/pull/20443
synced_at: 2026-01-12T15:57:02Z
```

# Display diffs for `ruff format --check` and add support for different output formats

---

_@ntBre_

## Summary

This PR uses the new `Diagnostic` type for rendering formatter diagnostics. This allows the formatter to inherit all of the output formats already implemented in the linter and ty. For example, here's the new `full` output format, with the formatting diff displayed using the same infrastructure as the linter:

<img width="592" height="364" alt="image" src="https://github.com/user-attachments/assets/6d09817d-3f27-4960-aa8b-41ba47fb4dc0" />


<details><summary>Resolved TODOs</summary>
<p>

~~There are several limitiations/todos here still, especially around the `OutputFormat` type~~:
- [x] A few literal `todo!`s for the remaining `OutputFormat`s without matching `DiagnosticFormat`s
- [x] The default output format is `full` instead of something more concise like the current output
- [x] Some of the output formats (namely JSON) have information that doesn't make much sense for these diagnostics

The first of these is definitely resolved, and I think the other two are as well, based on discussion on the design document. In brief, we're okay inheriting the default `OutputFormat` and can separate the global option into `lint.output-format` and `format.output-format` in the future, if needed; and we're okay including redundant information in the non-human-readable output formats.

My last major concern is with the performance of the new code, as discussed in the `Benchmarks` section below.

A smaller question is whether we should use `Diagnostic`s for formatting errors too. I think the answer to this is yes, in line with changes we're making in the linter too. I still need to implement that here.

</p>
</details> 

<details><summary>Benchmarks</summary>
<p>


The values in the table are from a large benchmark on the CPython 3.10 code
base, which involves checking 2011 files, 1872 of which need to be reformatted.
`stable` corresponds to the same code used on `main`, while `preview-full` and
`preview-concise` use the new `Diagnostic` code gated behind `--preview` for the
`full` and `concise` output formats, respectively. `stable-diff` uses the
`--diff` to compare the two diff rendering approaches. See the full hyperfine
command below for more details. For a sense of scale, the `stable` output format
produces 1873 lines on stdout, compared to 855,278 for `preview-full` and
857,798 for `stable-diff`.

| Command           |     Mean [ms] | Min [ms] | Max [ms] |     Relative |
|:------------------|--------------:|---------:|---------:|-------------:|
| `stable`          |   201.2 ± 6.8 |    192.9 |    220.6 |         1.00 |
| `preview-full`    | 9113.2 ± 31.2 |   9076.1 |   9152.0 | 45.29 ± 1.54 |
| `preview-concise` |   214.2 ± 1.4 |    212.0 |    217.6 |  1.06 ± 0.04 |
| `stable-diff`     | 3308.6 ± 20.2 |   3278.6 |   3341.8 | 16.44 ± 0.56 |

In summary, the `preview-concise` diagnostics are ~6% slower than the stable
output format, increasing the average runtime from 201.2 ms to 214.2 ms. The
`full` preview diagnostics are much more expensive, taking over 9113.2 ms to
complete, which is ~3x more expensive even than the stable diffs produced by the
`--diff` flag.

My main takeaways here are:
1. Rendering `Edit`s is much more expensive than rendering the diffs from `--diff`
2. Constructing `Edit`s actually isn't too bad

### Constructing `Edit`s

I also took a closer look at `Edit` construction by modifying the code and
repeating the `preview-concise` benchmark and found that the main issue is
constructing a `SourceFile` for use in the `Edit` rendering. Commenting out the
`Edit` construction itself has basically no effect:

| Command   |   Mean [ms] | Min [ms] | Max [ms] |    Relative |
|:----------|------------:|---------:|---------:|------------:|
| `stable`  | 197.5 ± 1.6 |    195.0 |    200.3 |        1.00 |
| `no-edit` | 208.9 ± 2.2 |    204.8 |    212.2 | 1.06 ± 0.01 |

However, also omitting the source text from the `SourceFile` construction
resolves the slowdown compared to `stable`. So it seems that copying the full
source text into a `SourceFile` is the main cause of the slowdown for non-`full`
diagnostics.

| Command          |   Mean [ms] | Min [ms] | Max [ms] |    Relative |
|:-----------------|------------:|---------:|---------:|------------:|
| `stable`         | 202.4 ± 2.9 |    197.6 |    207.9 |        1.00 |
| `no-source-text` | 202.7 ± 3.3 |    196.3 |    209.1 | 1.00 ± 0.02 |

### Rendering diffs

The main difference between `stable-diff` and `preview-full` seems to be the diffing strategy we use from `similar`. Both versions use the same algorithm, but in the existing [`CodeDiff`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/source_kind.rs#L259) rendering for the `--diff` flag, we only do line-level diffing, whereas for `Diagnostic`s we use `TextDiff::iter_inline_changes` to highlight word-level changes too. Skipping the word diff for `Diagnostic`s closes most of the gap:

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| `stable-diff` | 3.323 ± 0.015 | 3.297 | 3.341 | 1.00 |
| `preview-full` | 3.654 ± 0.019 | 3.618 | 3.682 | 1.10 ± 0.01 |

(In some repeated runs, I've seen as small as a ~5% difference, down from 10% in the table)

This doesn't actually change any of our snapshots, but it would obviously change the rendered result in a terminal since we wouldn't highlight the specific words that changed within a line.

Another much smaller change that we can try is removing the deadline from the `iter_inline_changes` call. It looks like there's a fair amount of overhead from the default 500 ms deadline for computing these, and using `iter_inline_changes(op, None)` (`None` for the optional deadline argument) improves the runtime quite a bit:

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| `stable-diff` | 3.322 ± 0.013 | 3.298 | 3.341 | 1.00 |
| `preview-full` | 5.296 ± 0.030 | 5.251 | 5.366 | 1.59 ± 0.01 |

<hr>

<details><summary>hyperfine command</summary>

```shell
cargo build --release --bin ruff && hyperfine --ignore-failure --warmup 10 --export-markdown /tmp/table.md \
  -n stable -n preview-full -n preview-concise -n stable-diff \
  "./target/release/ruff format --check ./crates/ruff_linter/resources/test/cpython/ --no-cache" \
  "./target/release/ruff format --check ./crates/ruff_linter/resources/test/cpython/ --no-cache --preview --output-format=full" \
  "./target/release/ruff format --check ./crates/ruff_linter/resources/test/cpython/ --no-cache --preview --output-format=concise" \
  "./target/release/ruff format --check ./crates/ruff_linter/resources/test/cpython/ --no-cache --diff"
```

</details>

</p>
</details> 

## Test Plan

Some new CLI tests and manual testing



---

_Comment by @github-actions[bot] on 2025-09-16 22:21_

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

_Comment by @MichaReiser on 2025-09-17 07:16_

> More broadly, it seems pretty expensive to add the entire file's contents as an Edit. I'm guessing that might show up in the benchmarks on this PR. On a related note, this is a pretty shallow conversion, only constructing the Diagnostics right before rendering. There might be a better way to use the new infrastructure more. We could also use them for rendering FormatCommandErrors, not just FormatPathResults.

A solution here could be to add a `diff` field to `Diagnostic` so that the "edit" is computed lazily or the diff is rendered directly instead of using the edit rendering.


> The default output format is full instead of something more concise like the current output

Hmm, that's an interesting find. This needs some design work.

---

_Comment by @ntBre on 2025-09-24 22:33_

I think this is mostly ready for review now. I did a lot of squashing today to make it easier to review commit-by-commit. 

The first 7 commits are all very small, standalone refactors used in later steps. The 8th commit is the biggest, converting `FormatPathResult`s into diagnostics. The 9th commit is an incremental improvement on that to split notebook `Edit`s by cell so that they can actually be rendered in the `full` output format (otherwise they never satisfy [this check](https://github.com/astral-sh/ruff/blob/main/crates/ruff_db/src/diagnostic/render/full.rs#L141)). Finally, the last commit emits errors as diagnostics too. 

I still have a bunch of TODO comments on the errors. I did my best to match up the error variants with `DiagnosticId`s, including adding two new `DiagnosticId`s, but I think there's still room for improvement. I should probably also add a test for some or all of these.

I haven't tried to truncate the lines in the diff yet either. I know that came up in the design discussion, so I'm happy to tackle it now or leave it for a follow up if desired.

Hopefully it's not too glaring, but in the screenshot in the summary you can see that the `|` for line numbers doesn't line up with the middle of the filename `-->` like it does for lint diagnostics. I don't think there's really a good way around this since the arrow alignment comes from `annotate-snippets` and only gets indented if `annotate-snippets` renders line numbers too. I'm sure we could hack another option into `annotate-snippets`, but it would likely make https://github.com/astral-sh/ruff/issues/20411 harder still.

---

_Label `preview` added by @ntBre on 2025-09-24 22:34_

---

_Label `diagnostics` added by @ntBre on 2025-09-24 22:34_

---

_Label `formatter` added by @ntBre on 2025-09-24 22:34_

---

_Comment by @github-actions[bot] on 2025-09-25 21:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-25 21:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @ntBre on 2025-09-25 22:02_

I'm marking this ready for review. I pushed a few more commits resolving the main issues I noted above:
- I added a `lineno_offset`/`header_offset` field for improving the `-->` alignment:
  
  <img width="555" height="417" alt="image" src="https://github.com/user-attachments/assets/829cce57-0fe2-4ad1-b5b7-6f8b77fd2a94" />

  It can still go wrong and makes it harder to un-fork `annotate-snippets`, so I'm happy to revert this, but I think it's a visual improvement in general.
- I added tests for all of the `FormatCommandError` variants and fixed a couple of oversights in their formatting, including factoring out and using some of ty's panic rendering. There are still TODOs on a few of these, but I think I could use some input on the best `DiagnosticId`s to use. The tests at least show what they look like to help us iterate on them.

I think the performance hit is acceptable. It's only ~10 ms (~5%) for the `concise` output on a large project, and the larger discrepancy between `full` output and `--diff` seems justified given the additional work of computing more granular diffs. I am seeing slightly worse performance today compared to the last time I ran the benchmarks, so it may be worth a bit more profiling, but I think the point still stands.

<details><summary>New benchmark table</summary>
<p>

| Command           |     Mean [ms] | Min [ms] | Max [ms] |     Relative |
| :---------------- | ------------: | -------: | -------: | -----------: |
| `stable`          |   200.0 ± 2.3 |    196.6 |    203.5 |         1.00 |
| `preview-full`    | 9134.8 ± 32.1 |   9108.0 |   9197.5 | 45.67 ± 0.55 |
| `preview-concise` |   232.6 ± 4.2 |    227.0 |    240.7 |  1.16 ± 0.02 |
| `stable-diff`     | 3354.8 ± 15.9 |   3333.7 |   3383.6 | 16.77 ± 0.21 |

`stable` is basically identical to last time, but both `preview` versions are ~20 ms slower. `--diff` is almost 50 ms slower, which really doesn't make sense.

</p>
</details> 

Similarly, I think we may want to truncate large diffs at some point (Zanie even mentioned possibly making the limit configurable), but I think we can hold off on that for now.

Oh, one other TODO is that it would be nice to emit a warning if the `output-format` is set without `preview`, but I don't think that's so easy to do since `output-format` is a global option and will have already been `unwrap_or_default`ed by the time we see it.

---

_Marked ready for review by @ntBre on 2025-09-25 22:03_

---

_Review requested from @carljm by @ntBre on 2025-09-25 22:03_

---

_Review requested from @MichaReiser by @ntBre on 2025-09-25 22:03_

---

_Review requested from @sharkdp by @ntBre on 2025-09-25 22:03_

---

_Review requested from @dcreager by @ntBre on 2025-09-25 22:03_

---

_Review requested from @BurntSushi by @ntBre on 2025-09-25 22:03_

---

_Review requested from @dhruvmanila by @ntBre on 2025-09-25 22:03_

---

_Review request for @dcreager removed by @ntBre on 2025-09-25 22:03_

---

_Review request for @carljm removed by @ntBre on 2025-09-25 22:03_

---

_Review request for @BurntSushi removed by @ntBre on 2025-09-25 22:03_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-25 22:03_

---

_Review request for @dhruvmanila removed by @ntBre on 2025-09-25 22:03_

---

_Renamed from "[WIP] Use `Diagnostic`s for rendering formatting results" to "Use `Diagnostic`s for rendering formatting results" by @ntBre on 2025-09-25 22:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:542 on 2025-09-26 07:33_

We should document that this option is only respected in preview mode. Ideally, we'd emit a warning message if it is set when not using preview

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:355 on 2025-09-26 07:43_

It seems the only reason that we need to add `unformatted` here is for the `From<FormattedSource> for FormatResult` implementation. This seems unfortunate because we do have an owned `unformatted` in all code paths where we convert from `FormattedSource` to `FormatResult`. 

I suggest removing `unformatted` here and making `From<FormattedSource>` a method on `FormattedSource` instead:

```rust
impl FormattedSource {
	fn into_format_result(unformatted: SourceKind) -> FormatResult { ... }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:527 on 2025-09-26 07:46_

Do we still need both `Formatted` and `Diff`, given that they now represent the same?

This makes me wonder: Do your changes regress the performance of `format` (without `--check` or `--diff`). I suspect it could because we're now always capturing `formatted` and `unformatted` even if the changes were written right to disk. 

I think we should keep both those variants but use `Diff` when using `--check` but keep using `Formatted` when using a bare `format` command.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:617 on 2025-09-26 07:49_

This seems a bit surprising to me. I saw that `self. diagnostics` does sort the diagnostics. What's the reason that we don't sort the entire `diagnostics` instead?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:650 on 2025-09-26 07:51_

Nit: It seems unfortunate that we now have the same match statement in the linter and formatter. What makes it challenging to share this match statement?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:922 on 2025-09-26 07:57_

Nit: Could we unify some of the code here with what we have in the linter?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:868 on 2025-09-26 08:05_

Using IO here is probably fine and roughly matches what we do in the lint command. 

We match on all `ignore::Error` in ty and:

* Log a warning for any gitignore related errors but keep going (unlike Ruff)
* Create IO errors for all other errors

https://github.com/astral-sh/ruff/blob/959f55ae057843f8b3fe59a23577f5ae3948a903/crates/ruff_db/src/system/os.rs#L527-L533

What's special about the ignore errors is that the path isn't guaranteed to be the current file path. E.g. the error could be related to the gitignore file in the same or even a parent directory.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:893 on 2025-09-26 08:07_

`FormatError` would be more correct as I consider this a formatter bug (The constructed IR is invalid).

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:912 on 2025-09-26 08:18_

I agree that using a `ParseError` here directly would be better. How hard is it to change `FormatCommandError::Parse` to store a `ParseError` instead ?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:794 on 2025-09-26 08:23_

I think it's worth trying to do a little better here and trim the edit to the range that is different. This can be done in `O(n)` by searching the first (or last) character that's different (I'm not sure if we should then move forward/backward to find the start/end of the line or if that doesn't matter). I think that would also remove the need to use `set_file_level(true)` and for `set_header_offset`?

While not as good as separate Edits, it should already narrow the `Edits significantly in practice.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:945 on 2025-09-26 08:28_

Can we remove this error? It seems that it is only created if we fail to write the diff to stdout... We've already lost if that happens.

I think we can simply ignore that error or log a warning with tracing. It might also be worth checking what ruff check does.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/panic.rs`:35 on 2025-09-26 08:32_

Should we use this in ruff linter too?

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/diagnostics.rs`:41 on 2025-09-26 08:33_

lol

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 08:53_

Using `set_file_level` here seems odd to me. I understand that the motivation for `set_file_level` was to suppress code snippets for diagnostics starting at 1:1 (or don't have a range).

But we do want to show the code snippet for formatter diagnostics and I believe it's the reason why the `-->` arrow gets misplaced. 

Why do we need `set_file_level` here, could we avoid using it as it seems a misuse?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1040 on 2025-09-26 09:02_

I don't think it's worth having a diagnostic ID for every CLI option that can possibly be invalid. Maybe `invalid-cli-option`?



---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1045 on 2025-09-26 09:03_

Similar to `RangeFormatNotebook`. I wonder if we should just have one `internal-error` code. The main advantage I see is that it makes it very clear that this is a bug in Ruff vs. some user error (we should probalby add a note saying that too)

---

_@MichaReiser reviewed on 2025-09-26 09:03_

Nice: I've a few suggestions. The part I feel most unsure about is if using `set_file_level` is correct for format diagnostics.

---

_@ntBre reviewed on 2025-09-26 12:28_

---

_Review comment by @ntBre on `crates/ruff/src/args.rs`:542 on 2025-09-26 12:28_

Yeah, I thought about the warning, but I wasn't sure how easy it would be to add since the output format is a global setting. I can go look again too, but is there a place where we know we're in the `format` sub-command without preview and before unwrapping to a default output format?

It seems a bit easier if we want to check for non-default output formats, at least.

---

_@ntBre reviewed on 2025-09-26 12:28_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:617 on 2025-09-26 12:28_

I was just trying to preserve the stable behavior of emitting errors first, but it makes sense to me to sort them all together here.

---

_@MichaReiser reviewed on 2025-09-26 12:32_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:542 on 2025-09-26 12:32_

I think this is a bit different because this option is specific to the format command and, if set, it should always take precedence over any other configuration option. 

We should be able to check this directly in the format command

---

_@ntBre reviewed on 2025-09-26 12:33_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:650 on 2025-09-26 12:33_

The snippet in your comment is the main difference. In the linter we still use the `TextEmitter` for these output formats and mix in a few other checks in the `match`:

https://github.com/astral-sh/ruff/blob/2af8c53110227598cb4758a777667dbe18bc7f15/crates/ruff/src/printer.rs#L251-L268

We also use at least one different configuration setting in the formatter to always suppress the fix availability icon.

---

_@ntBre reviewed on 2025-09-26 12:34_

---

_Review comment by @ntBre on `crates/ruff/src/args.rs`:542 on 2025-09-26 12:34_

Ohhh okay, yes a warning for the new CLI option does make sense. I was over-complicating it by trying to check for the configuration option too.

---

_@ntBre reviewed on 2025-09-26 12:40_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 12:40_

I wasn't sure that we do want to show the code snippet for these diagnostics as it also requires us to underline/annotate a range in the file. I didn't want to add a multi-line annotation possibly spanning the entire file, but maybe we can underline the first changed line? That could work well with your other suggestion about narrowing the edit range. It does seem slightly redundant to render any code if we're immediately going to render it again in the diff, but I can give it a try.

But yes, you're right. If we set a real range and thus render a snippet we can avoid making these file-level diagnostics, and I can also revert my arrow changes.

---

_@ntBre reviewed on 2025-09-26 12:45_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:922 on 2025-09-26 12:45_

Yes, I think so. I liked how ty rendered panics, so I pulled from there instead, but I can either switch to how the linter constructs panics or port this to the linter too.

---

_@MichaReiser reviewed on 2025-09-26 12:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:650 on 2025-09-26 12:50_

Hmm, why do we still need `TextEmitter`? I worry that it will be difficult to understand why some of those differences exist in the future.

---

_@MichaReiser reviewed on 2025-09-26 12:53_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 12:53_

Oh, I see. The difference is that we don't actually render a code snippet, we only render a fix (which is something different in annotated snippet?)

And the issue is that the rendering of `-->` isn't aligned with the rendering of the fix.

---

_@MichaReiser reviewed on 2025-09-26 12:53_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:542 on 2025-09-26 12:53_

Yeah, warning on the option seems difficult. 

---

_@ntBre reviewed on 2025-09-26 12:56_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 12:56_

Yep, the fixes/diffs aren't actually rendered through `annotate-snippets`, they're my best attempt to emulate `annotate-snippets` after we render the rest of the diagnostic. That's why the arrow header can't align itself, the two steps are separate.

---

_@ntBre reviewed on 2025-09-26 12:58_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:650 on 2025-09-26 12:58_

That's a good point, I can look into refactoring the linter calls and then reusing something here.

---

_@MichaReiser reviewed on 2025-09-26 13:18_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 13:18_

I see. I think we should rename `set_file_level` to `hide_snippet` or similar to make the distinction clear now that it's used for more than just hiding snippets for file level diagnostics. 

For the alignment. We can probably get it "right" if we compute the line number where the first change is. 

An alternative would be to remove the need for the alignment altogether. Unfortunately, this might be another breaking change except if all we do is change the number of whitespace before after the arrow



---

_@ntBre reviewed on 2025-09-26 13:31_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 13:31_

> I see. I think we should rename `set_file_level` to `hide_snippet` or similar to make the distinction clear now that it's used for more than just hiding snippets for file level diagnostics.

I guess I was still thinking of these conceptually as file-level diagnostics. That's what they are on stable anyway, "File would be reformatted." But I'm happy to rename the setting, `hide_snippet` makes the actual effect more clear for sure.

> For the alignment. We can probably get it "right" if we compute the line number where the first change is.

That's roughly what I'm doing now, except it's a bit better to compute the line number of the _last_ change (currently the end of the file) since that's likely to need a wider alignment than the start. Notebooks are also more complicated since the width is for the per-cell line number.

> An alternative would be to remove the need for the alignment altogether. Unfortunately, this might be another breaking change except if all we do is change the number of whitespace before after the arrow

That's interesting. I do think it looks nice aligning the middle of the arrow with the line number separator, but maybe it's not worth the trouble.


---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:527 on 2025-09-26 17:43_

Hmm that's a good point. They do look mostly the same comparing results from my branch release build and my system ruff 0.13.0 binary, but the branch version is a little slower:

| Command  |   Mean [ms] | Min [ms] | Max [ms] |    Relative |
| :------- | ----------: | -------: | -------: | ----------: |
| `branch` | 213.1 ± 2.5 |    209.7 |    217.4 | 1.00 ± 0.02 |
| `stable` | 212.4 ± 3.5 |    206.4 |    217.6 |        1.00 |

It makes sense to me to reuse `Diff` anyway, though.

<details><summary>hyperfine</summary>
<p>

```shell
hyperfine --ignore-failure --warmup 10 --export-markdown /tmp/table.md \
  --prepare "git restore ."  \
  -n branch -n stable \
  "~/astral/ruff/target/release/ruff format . --no-cache" \
  "ruff format . --no-cache"
```

</p>
</details> 

---

_@ntBre reviewed on 2025-09-26 17:43_

---

_@ntBre reviewed on 2025-09-26 18:01_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:355 on 2025-09-26 18:01_

It looks like reusing the `Diff` variant below also let me revert this!

---

_@ntBre reviewed on 2025-09-26 19:23_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:868 on 2025-09-26 19:23_

Ah that makes sense, thank you! I was wondering if the path might be a bit redundant in both the header and in the diagnostic message, but I didn't realize the two paths could be different. I'll just delete this todo for now then, although it makes sense to me to warn and keep going like in ty at some point.

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:912 on 2025-09-26 20:18_

`DisplayParseError` does a fair amount of work to compute its `ErrorLocation`, and we still use the `Display` impl in a couple of places. For now I just added an `error` method to retrieve the underlying `ParseError`.

---

_@ntBre reviewed on 2025-09-26 20:18_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:794 on 2025-09-26 20:22_

Sounds good, I had an earlier version that did this before I rebased over it. I'll add it back.

---

_@ntBre reviewed on 2025-09-26 20:22_

---

_@ntBre reviewed on 2025-09-26 21:07_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:945 on 2025-09-26 21:07_

Oh I think you're right. Can we just reuse the `Write` variant? That's what we do for the other failed stdout writes in the only place this is constructed:

https://github.com/astral-sh/ruff/blob/6b3c493cff01ad1ab51102048d0d4f489b1e1aa4/crates/ruff/src/commands/format_stdin.rs#L111-L116

In the linter I think we just bubble up an exit code that ends up reported here:

https://github.com/astral-sh/ruff/blob/6b3c493cff01ad1ab51102048d0d4f489b1e1aa4/crates/ty/src/main.rs#L28

```shell
$ ruff check try.py > /dev/full
ruff failed
  Cause: No space left on device (os error 28)
$ ruff format --check try.py > /dev/full
ruff failed
  Cause: No space left on device (os error 28)
```

I guess that's effectively the same in the formatter since any of our attempts to write diagnostics will fail.

---

_@ntBre reviewed on 2025-09-26 21:42_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:845 on 2025-09-26 21:42_

I renamed the file-level references to `hide_snippet` and updated my todo about removing it. I also added an expanded new todo about the offset hack for now.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:756 on 2025-09-29 08:03_

Should we use a match here that explicitly skips over `Unchanged` and `Skipped`, panics for `Formatted` (at least in debug builds), and returns `unformatted` and `formatted` for diff?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:831 on 2025-09-29 08:04_

I think we should use this range for the Edit as well if we already computed it anyway. It can help to reduce the size of the edit in common cases where only a few files are incorrectly formatted

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:816 on 2025-09-29 08:07_

I think we can do better than traversing the entire file by:

1. Search from the beginning and find the first changed character
2. Slice the remaining document to `&source[start..]`
3. Search from the end to find the last changed character


This is still `O(n)` in the worst case, but should do much better if a large portion of the file changed. It might be worth extracting this logic into a helper function to not distract the reading flow

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:816 on 2025-09-29 08:10_

We should use the line number of the `Edit`'s end + rendered context window size. The arrow is otherwise over intended if only the first line changed in a 1000 lines file (which I think looks worse than always aligning with two spaces).





---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:888 on 2025-09-29 08:16_

Nit: I'd also classify this as an internal error. At least, how we're using it in the formatter. 

The way we use this error variant is to avoid panicking if we didn't see a specific syntax element when doing some ad-hoc lexing. This means, we incorrectly assumed some invariant that isn't true because the formatter is never called on code with syntax errors. This is always an implementation error -> `InternalError`)

The original intended use case for this error variant comes from biome where the formatter supports formatting code with syntax error. The way it works is that it formats all code without syntax errors. For sections that contain syntax errors, it falls back to preserving the formatting from the source by catching `FormatError::SyntaxError` at the statement level. But even for Biome, a `SyntaxError` should never be propagated passt the statement formatting. If it does, than that's consider an internal error too.

---

_@MichaReiser approved on 2025-09-29 08:18_

I've a few more suggestions about the computed diagnostic and `Edit` range and on how to compute the offset for the arrow.

I still very much dislike the offset hack but I think we can get it "correct" enough outside annotated snippet. However, it might be worth spending like a day to see if we can move the diff rendering to annotated snippets, removign the need for the offset workaround. But this can be a separate PR



---

_@ntBre reviewed on 2025-09-29 12:55_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:756 on 2025-09-29 12:55_

Sure, I was just following the logic in `write_changed`, but a debug assertion makes sense to me.

https://github.com/astral-sh/ruff/blob/00c8851ef8d643434340577f1b1ee65b5e728ac1/crates/ruff/src/commands/format.rs#L555-L559

---

_@ntBre reviewed on 2025-09-29 13:02_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:831 on 2025-09-29 13:02_

Oops, of course, thank you! I found my earlier commit in the reflog but obviously didn't finish integrating it with the other new code.

---

_@ntBre reviewed on 2025-09-29 17:05_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:816 on 2025-09-29 17:05_

Ah good call. This was a bit surprising to me, so I just wanted to note it here: I think the context window size is up to 6 lines. We pass `3` to similar's `grouped_ops` function here:

https://github.com/astral-sh/ruff/blob/1d3e4a91534448121c6db99c2baf83c1ccd2d909/crates/ruff_db/src/diagnostic/render/full.rs#L157-L159

which is used in similar [here](https://github.com/mitsuhiko/similar/blob/a169e29954502d67e26c9e766f2f719e99a52559/src/common.rs#L134-L136):

```rust
            // End the current group and start a new one whenever
            // there is a large range with no changes.
            if len > n * 2 {
```

I guess in a strict sense we should also `min` this with the actual number of lines in the file, so we don't end up with 105 in a 99 line file, for example.


---

_Comment by @ntBre on 2025-09-29 19:55_

Thank you for the reviews! 

I think this could use one more look. I was over-complicating the range calculations for a while today, but they felt kind of tricky, at least when I was trying to use `zip` like the `start` calculation[^1]. The new `ModifiedRange` type seems to be working well now, though, unless I missed any edge cases.

The other changes seemed relatively straightforward, and I also merged the changes from #20595.

I'll timebox trying to move our diff rendering to annotate-snippets to one day later this week.

[^1]: The tricky part was that zipping and `find`ing the first different character could easily fail if one of the snippets was shorter than the other; at that point it's not clear which one was shorter and caused the failure. We also want an exclusive range, so we've gone one character too far by finding the first different character rather than the last common character. Now I just loop from the end and track the length of the common suffix, which we can subtract from both `text_len`s despite the actual offsets likely being different.


---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:782 on 2025-09-30 07:03_

It shouldn't matter so I think it's fine leaving as is but there's the third case where `modified_range.unformatted` is empty (e.g. when adding blank lines between two classes), in which case its an insertion. We could add a `Edit::from_text_and_range(new_text, range)` (with a better name) that does this dance.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:814 on 2025-09-30 07:04_

Is using `formatted` here correct? What if unformatted is shorter? 

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:1043 on 2025-09-30 07:07_

I wonder if a regular loop would have been easier here (similar to what you have below):

```rust
let mut prefix_length = TextSize::ZERO;

for (unformatted, formatted) in unformatted.chars().zip(formatted.chars()) {
    if unformatted != formatted {
        break;
    }

    prefix_length += unformatted.text_len();
}
```

---

_@MichaReiser approved on 2025-09-30 07:09_

---

_@ntBre reviewed on 2025-09-30 13:28_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:814 on 2025-09-30 13:28_

We discussed this in our 1:1, but we only display line numbers for the new code, so I think this is correct. Somewhat random lint rule example:

```
help: Replace for loop with list comprehension
1 | original = list(range(10000))
  - filtered = []
  - for i in original:
  -     if i % 2:
  -         filtered.append(i)
2 + filtered = [i for i in original if i % 2]
```

This is a deletion but still shows that we only render  the new line numbers.

---

_@ntBre reviewed on 2025-09-30 14:48_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:782 on 2025-09-30 14:48_

I think I'll leave this for now. I only split out deletions because I hit the `debug_assert!` that `content` is not empty in `Edit::range_replacement`. Insertions seem okay to group with full replacements since `content` is `Some` in both cases, and we already have a `TextRange`, which `Edit::insertion` would otherwise construct.

---

_@ntBre reviewed on 2025-09-30 14:58_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:1043 on 2025-09-30 14:58_

That is nicer, thanks. I used the guarded break for the suffix too instead of the if-else.

---

_Renamed from "Use `Diagnostic`s for rendering formatting results" to "Display diffs for `ruff format --check` and add support for different output formats" by @ntBre on 2025-09-30 16:00_

---

_Merged by @ntBre on 2025-09-30 16:00_

---

_Closed by @ntBre on 2025-09-30 16:00_

---

_Branch deleted on 2025-09-30 16:00_

---

_Comment by @kaddkaka on 2025-10-02 20:27_

Does this PR take any steps closer to solving https://github.com/astral-sh/ruff/issues/14452 ? 

---

_Comment by @ntBre on 2025-10-02 21:03_

No, I don't think so. This PR was for the `format` subcommand, so it shouldn't affect `ruff check` (the lint subcommand) at all. Micha's comment about Ruff only reporting leftover diagnostics after fixes have been applied is still accurate, as far as I know.

---

_Comment by @pygarap on 2025-10-03 02:25_

@ntBre 

Now, `ruff format --check`, become like `ruff format --diff`? But with more features.

So, `ruff format --diff` is deprecated for preview mode now?

And why the docs don't mention it?

```
      --check
          Avoid writing any formatted files back; instead, exit with a non-zero
          status code if any files would have been modified, and zero otherwise
      --diff
          Avoid writing any formatted files back; instead, exit with a non-zero
          status code and the difference between the current file and how the
          formatted file would look like
```

Looks like the `--check` CLI flag didn't change in the docs, But it did.

---

_Comment by @ntBre on 2025-10-03 03:59_

`--diff` produces a standalone diff that can still be useful for applying as a patch, for example, so it's not deprecated. The "diff" shown by the default `format --check` output uses the same format that's in preview for lint rules, which is a bit different.

I also think the `--check` help message is still accurate. The new information is in the new `--output-format` entry in the CLI help.

---

_Comment by @joukewitteveen on 2025-10-13 09:04_

This would be even more powerful when `ruff format --check` (and `ty check`) would also support `--output-file`. Currently, only `ruff check` supports that. Redirection (`ruff format --check [...] > DIR/OUTFILE`) doesn't fully solve the issue, since the directory containing the output file is required to exist.

---

_Comment by @kaddkaka on 2026-01-08 05:29_

> No, I don't think so. This PR was for the `format` subcommand, so it shouldn't affect `ruff check` (the lint subcommand) at all. Micha's comment about Ruff only reporting leftover diagnostics after fixes have been applied is still accurate, as far as I know.

Which comment are you referring to? 

---

_Comment by @ntBre on 2026-01-08 16:34_

> Which comment are you referring to?

I think I was referring to this comment: https://github.com/astral-sh/ruff/issues/14452#issuecomment-2487825730

---
