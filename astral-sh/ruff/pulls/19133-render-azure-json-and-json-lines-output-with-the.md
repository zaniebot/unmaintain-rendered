```yaml
number: 19133
title: Render Azure, JSON, and JSON lines output with the new diagnostics
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/json-rendering
created_at: 2025-07-03T22:02:07Z
updated_at: 2025-07-11T19:04:48Z
url: https://github.com/astral-sh/ruff/pull/19133
synced_at: 2026-01-12T15:56:32Z
```

# Render Azure, JSON, and JSON lines output with the new diagnostics

---

_@ntBre_

## Summary

This was originally stacked on #19129, but some of the changes I made for JSON also impacted the Azure format, so I went ahead and combined them. The main changes here are:

- Implementing `FileResolver` for Ruff's `EmitterContext`
- Adding `FileResolver::notebook_index` and `FileResolver::is_notebook` methods
- Adding a `DisplayDiagnostics` (with an "s") type for rendering a group of diagnostics at once
- Adding `Azure`, `Json`, and `JsonLines` as new `DiagnosticFormat`s

I tried a couple of alternatives to the `FileResolver::notebook` methods like passing down the `NotebookIndex` separately and trying to reparse a `Notebook` from Ruff's `SourceFile`. The latter seemed promising, but the `SourceFile` only stores the concatenated plain text of the notebook, not the re-parsable JSON. I guess the current version is just a variation on passing the `NotebookIndex`,  but at least we can reuse the existing `resolver` argument. I think a lot of this can be cleaned up once Ruff has its own actual file resolver.

As suggested, I also tried deleting the corresponding `Emitter` files in `ruff_linter`, but it doesn't look like git was able to follow this as a rename. It did, however, track that the tests were moved, so the snapshots should be easy to review.

## Test Plan

Existing Ruff tests ported to tests in `ruff_db`. I think some other existing ruff tests also cover parts of this refactor.

---

_Label `ty` added by @ntBre on 2025-07-03 22:02_

---

_Label `diagnostics` added by @ntBre on 2025-07-03 22:02_

---

_Comment by @github-actions[bot] on 2025-07-03 22:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-03 22:21_

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

_Renamed from "[WIP] Render JSON output with the new diagnostics" to "Render Azure, JSON, and JSON lines output with the new diagnostics" by @ntBre on 2025-07-04 16:19_

---

_Marked ready for review by @ntBre on 2025-07-04 16:19_

---

_Review requested from @carljm by @ntBre on 2025-07-04 16:19_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-04 16:19_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-04 16:19_

---

_Review requested from @sharkdp by @ntBre on 2025-07-04 16:19_

---

_Review requested from @dcreager by @ntBre on 2025-07-04 16:19_

---

_Review request for @dcreager removed by @ntBre on 2025-07-04 16:20_

---

_Review request for @carljm removed by @ntBre on 2025-07-04 16:20_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-04 16:20_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-04 16:20_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-04 16:20_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:24 on 2025-07-07 10:07_

We should update the naming here to not use `message` anymore (the concept no longer exists and is now only confusing)

---

_@MichaReiser reviewed on 2025-07-07 10:07_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:77 on 2025-07-07 10:15_

Keeping the existing JSON output as is does make sense for ruff but I don't think we should use it for ty:

* The `code` should be the diagnostic id (and we may want to align the field name)
* The output lacks the severity
* `noqa_row` isn't used by ty
* ty has many diagnostics without a file or range and we can't just omit those 

This JSON output also only supports a subset of what the new diagnostic system supports. E.g. the following information gets lost:

* All sub diagnostics get lost
* All, except the primary, annotation get lost


I think the right approach here is to add this legacy JSON output as is for now but only expose it in Ruff. We can design a new JSON output format (which is closer to our internal model) in a seperate PR and use that in ty and think about how we want to transition Ruff to the new format.


---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:406 on 2025-07-07 10:16_

This feels hacky. Can't we customize this by using the `DisplaySettings` (by either providing a base URL or have a documentation provider.)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1260 on 2025-07-07 10:19_

It would be great to include the references mentioned in https://github.com/astral-sh/ruff/pull/5048

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1261 on 2025-07-07 10:20_

I'm sort of inlined not to support JsonLines in ty until it comes up as a feature request. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:111 on 2025-07-07 10:23_

It would be great to have some tests demonstrating these new output formats. I know we have some tests in the old message based renderers but we'll loose them long term if we delete the message based rendering infrastructure (which is the end-goal?).

I'm also inclined to have a module for each output format

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:113 on 2025-07-07 10:26_

Hmm, mapping `info` to warning` seems a bit odd. But I'm not entirely sure what to do about info messages instead. Unless we use a different output (maybe `debug`?)

```
##[debug]Debug text
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:150 on 2025-07-07 10:27_

We should log every diagnostic. We can't just skip some diagnostics.

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:41 on 2025-07-07 10:30_

Hmm, I don't like how we're pulling in more and more dependencies for the diagnostic rendering into `ruff_db`. I think we might have reached the point where we should split out the diagnostic code or, at least, the rendering. @BurntSushi what were the options that you considered for this in the past?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/json.rs`:1 on 2025-07-07 10:32_

It might make sense to delete those files entirely. Git might then even be able to pick up that the `render/json` file is the same as this file, which would help to preserve the history

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:144 on 2025-07-07 10:37_

Same as for JSON. We can't use the `secondary_code` for ty. Instead, it should be the `id`.

---

_@MichaReiser requested changes on 2025-07-07 10:40_

I think this needs some more design on the output formats: what fields and content should they contain for ruff/ty, how can we make some of the behavior configurable (secondary code vs id), don't skip over diagnostics. 

I'd also suggest to not mix refactoring the existing emitters and adding the output formats to ty because that extends the design scope (I don't think the existing JSON output makes sense for ty, it only is the way it is for historic reasons)

---

_Converted to draft by @ntBre on 2025-07-07 12:21_

---

_Comment by @ntBre on 2025-07-07 12:23_

Sorry I got ahead of myself with the ty integration. I'll put this back in draft while I clean it up.

---

_Label `ty` removed by @ntBre on 2025-07-07 12:23_

---

_Label `internal` added by @ntBre on 2025-07-07 12:23_

---

_@ntBre reviewed on 2025-07-07 16:23_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1260 on 2025-07-07 16:23_

Added a link to jsonlines.org! The second link in the PR seems to have been taken over by someone else and was also in Korean.

---

_@ntBre reviewed on 2025-07-07 16:25_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:113 on 2025-07-07 16:25_

From my reading of the [docs](https://learn.microsoft.com/en-us/azure/devops/pipelines/scripts/logging-commands?view=azure-devops&tabs=bash#properties), I thought this could only be `warning` or `error` but happy to change it if `debug` or another tag would work.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:113 on 2025-07-07 16:30_

It also supports [formatting commands](https://learn.microsoft.com/en-us/azure/devops/pipelines/scripts/logging-commands?view=azure-devops&tabs=bash#formatting-commands) but I don't fully understand the difference.

---

_@MichaReiser reviewed on 2025-07-07 16:30_

---

_@ntBre reviewed on 2025-07-07 19:37_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:111 on 2025-07-07 19:37_

I ported over the diagnostic-generating test functions from `ruff_linter` and then deleted the `azure` module. It seems git wasn't smart enough to see that the module moved, but it did detect that the snapshots moved, which is helpful at least. I'll do the same for the JSON tests and modules.

---

_@ntBre reviewed on 2025-07-07 20:50_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:24 on 2025-07-07 20:50_

I guess I should update the `message` module too. Maybe in a separate PR since I think it will create a pretty big diff, if that's okay.

---

_Comment by @ntBre on 2025-07-07 20:53_

I think the most glaring issues should now be resolved. I reverted the ty-facing changes, ported over the tests from `ruff_linter`, gave each output format its own module, and made sure not to drop diagnostics just because we failed to convert all of their fields to JSON.

I still need to do something about the `to_url` method, the Azure `info` severity, and possibly divide the crate or otherwise handle the `serde` dependency.

---

_@MichaReiser reviewed on 2025-07-08 06:31_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:24 on 2025-07-08 06:31_

I think it's sightly less important in the modules that we plan to delete but we should avoid using `message` in new code.

---

_Comment by @MichaReiser on 2025-07-08 06:32_

> I still need to do something about the to_url method, the Azure info severity, and possibly divide the crate or otherwise handle the serde dependency.

This is also something we can follow up *after* this PR. It's definetely not something we should do as part of this PR

---

_@ntBre reviewed on 2025-07-08 15:26_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:406 on 2025-07-08 15:26_

I think I'll just revert this for now and add a `ruff` prefix to the method since we're not exposing the output formats that use it to ty.

I think configuring it through the diagnostic config makes sense long-term, though. If we dropped `/ruff` from our `homepage` in the main `Cargo.toml` we could also reuse it for ty.

---

_@ntBre reviewed on 2025-07-08 15:35_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:144 on 2025-07-08 15:35_

(marking these as resolved since I ripped out the ty integration for now)

---

_@ntBre reviewed on 2025-07-08 15:38_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:77 on 2025-07-08 15:38_

This makes sense to me. I think we'll also have a bit more freedom in the JSON output for ruff once ruff-lsp is fully unsupported.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:113 on 2025-07-08 16:06_

I didn't find this in the docs, but I asked [ChatGPT](https://chatgpt.com/share/686d4086-64a8-8006-8965-fbddd989cf6c), and it suggested a couple of options:
- logging without a special `##` command prefix, which should show up as normal text without an icon
- using `##[debug]` as you said, but it thinks that will only be visible when `system.debug=true` is set

Do either of those sound preferable to you? I'm also kind of tempted to defer this for now since Ruff doesn't use the `info` severity anyway.

---

_@ntBre reviewed on 2025-07-08 16:06_

---

_Comment by @ntBre on 2025-07-08 20:57_

I think this is ready for another look, at least from @MichaReiser, to make sure I'm on the right track with his suggestions.

---

_@ntBre reviewed on 2025-07-08 20:59_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:117 on 2025-07-08 20:59_

What kind of API were you picturing for each output format submodule? I kind of like the way the Azure one looks here and could do something similar for JSON and JSON lines.

I can move the `Concise` and `Full` formats to their own modules too.

---

_@ntBre reviewed on 2025-07-08 21:05_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json_lines__tests__notebook_output.snap`:1 on 2025-07-08 21:05_

The diff here is just the filename (`.py` vs `.ipynb` as mentioned in the code comments). I guess that broke git's rename detection with the long lines.

---

_@ntBre reviewed on 2025-07-08 21:07_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:2546 on 2025-07-08 21:07_

These functions are all adapted from the ones with the same names in `ruff_linter::message::mod.rs`:

https://github.com/astral-sh/ruff/blob/05139a323bb827f7273551c2b54d87e6f356bae0/crates/ruff_linter/src/message/mod.rs#L188-L190

I think we should be able to delete those once all the output formats are moved over.

---

_@MichaReiser reviewed on 2025-07-09 06:27_

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:41 on 2025-07-09 06:27_

A quick-fix for now would be to keep the `serde` feature and gate the `JSON` display formats behind that feature too. But then. It probably doesn't matter that much because all our top level crates use it anyway

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:60 on 2025-07-09 06:29_

We should remove the feature entirely and always enable the `camino/serde1` feature if we make the `serde` dependency non-optional. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1255 on 2025-07-09 06:33_

For ty's JSON output, I suggest you take a look at https://doc.rust-lang.org/cargo/commands/cargo-check.html 

We could also consider naming this variant `JsonConcise` because it only contains the concise message.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:117 on 2025-07-09 06:38_

I'm not sure if all formats need to have a shared API but if I would do it I'd probably do something similar to the `DiagnosticFormat::Full` format where each format has its own `Renderer` struct. This has the advantage that they could store (e.g. cache) additional data that's specific to their format. 

```rust
AzureRenderer::default().render(f, diagnostic)
```

Let's move `Concise` and `Full` in its own PR

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:123 on 2025-07-09 06:42_

It seems silly that we create the `annotated_renderer` even if the diagnostic then only gets rendered with `JSON`. Can't we construct the renderer in the `Full` branch instead?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:190 on 2025-07-09 06:43_

I wonder if we should make `Diagnostic::display` to call `DisplayDiagnostics` instead. This would allow us to use the same `annotated_renderer` for all diagnostics instead of having to create a new one for every single diagnostic. 

`Diagnostic::display` would then call `DisplayDiagnostics::new(std::slice::from_ref(self), config, resolver).fmt(f)`

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2238 on 2025-07-09 06:46_

```suggestion
    pub(super) struct TestEnvironment {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/azure.rs`:1 on 2025-07-09 06:48_

I assuem git doesn't regonize the move of this file because the deletion of the old file and the creation of the new file aren't in the same commit. You could try squashing all your commits into a single commit to see if git then recognize that the file was moved.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:77 on 2025-07-09 06:51_

Making `location` and `end_location` `Option` is a breaking change. It's a bit hard to tell if you made other breaking changes to the schema. Please double check that you didn't.

I think the change I would make here is to default to `TextRange::default` if the range is missing and preview mode is disabled (we can use an `Option` in preview mode).

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:13 on 2025-07-09 06:53_

What's the reason for the changed extension?

---

_Review comment by @MichaReiser on `ty.schema.json`:1 on 2025-07-09 06:54_

This should be reverted

---

_@MichaReiser reviewed on 2025-07-09 06:54_

Thanks. We need to be careful not to make any breaking changes to the existing schemas (at least, if preview is disabled)

---

_@ntBre reviewed on 2025-07-09 11:29_

---

_Review comment by @ntBre on `ty.schema.json`:1 on 2025-07-09 11:29_

I might be missing something obvious here, but I tried to revert this with the other ty changes, and then the tests failed until I ran `cargo dev generate-all` again, which brought this back. Is it because `DiagnosticFormat` derives `JsonSchema`?

---

_@MichaReiser reviewed on 2025-07-09 11:38_

---

_Review comment by @MichaReiser on `ty.schema.json`:1 on 2025-07-09 11:38_

The schema changes here suggest that the configuration now accepts a JSON option for output format, which it shouldn't. We shouldn't see any changes to the schema until we intentionally expose the new output formats in ty

---

_@ntBre reviewed on 2025-07-09 11:38_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:13 on 2025-07-09 11:38_

I think the `TestEnvironment` tries to parse the input as a real notebook if I use `.ipynb`, but I had the source as plain text from the `ruff_linter` test. I guess it's still a bit hacky, but I could use something like `ipyn` instead of `py` to make it slightly more clear. Or I could just make a real notebook.

https://github.com/astral-sh/ruff/blob/cc5a1525ca289d73bf660fc7628017ba061a9aae/crates/ruff_db/src/diagnostic/render.rs#L2666-L2668

---

_@ntBre reviewed on 2025-07-09 11:45_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/azure.rs`:1 on 2025-07-09 11:45_

It was a good idea, but no luck.  It still showed a new file and a deletion.

---

_@ntBre reviewed on 2025-07-09 11:51_

---

_Review comment by @ntBre on `ty.schema.json`:1 on 2025-07-09 11:51_

It looks like I can use `#[schemars(skip)]` to omit them for now.

---

_@MichaReiser reviewed on 2025-07-09 12:16_

---

_Review comment by @MichaReiser on `ty.schema.json`:1 on 2025-07-09 12:16_

I think the proper solution is that `TerminalOptions` uses its own enum that's distinct from `DiagnosticFormat` in `ruff_db` because they otherwise still can still be serialized into by serde.

---

_@ntBre reviewed on 2025-07-09 13:04_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:77 on 2025-07-09 13:04_

Good catch. I went through all of the fields passed to `json!` calls, and the only field besides the locations was the filename. I added a `preview` field to `DisplayDiagnosticConfig` to pass it down and `unwrap_or_default` the options when it's disabled.

I also tried a separate branch where I used real structs instead of `json!` to be sure what all the types were, but the order of fields changed in the derived `Serialize` implementations, changing snapshots. And I figured that would be kind of awkward for the preview behavior anyway.

---

_@ntBre reviewed on 2025-07-09 13:13_

---

_Review comment by @ntBre on `ty.schema.json`:1 on 2025-07-09 13:13_

There's already a very similar `OutputFormat` enum used for the CLI arguments. Could we reuse that, or is it better to add another one in `ty_project` next to `TerminalOptions`?

I guess we probably don't want to have to propagate a `clap` feature to `ty_project`, so I'll go with a separate one for now.

---

_@ntBre reviewed on 2025-07-09 13:30_

---

_Review comment by @ntBre on `crates/ruff_db/Cargo.toml`:41 on 2025-07-09 13:30_

I brought back the `serde` feature for now, happy to revert if we decide it doesn't matter.

---

_@ntBre reviewed on 2025-07-09 14:26_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:190 on 2025-07-09 14:26_

Nice, I like how this looks, especially combined with the `AzureRenderer` idea.

---

_Comment by @ntBre on 2025-07-09 14:46_

I think I've addressed all of the feedback, pending our discussion about serde on Discord, and with the exception of a possible `JsonConcise` rename. I also didn't change anything about the `.py` "notebook" extension yet, but I'm happy to do something there if you want.

---

_Marked ready for review by @ntBre on 2025-07-10 14:38_

---

_Review comment by @MichaReiser on `ty.schema.json`:68 on 2025-07-10 15:00_

It looks like the documentation is now missing. There should be no schema changes in this PR

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:100 on 2025-07-10 15:14_

Let's create an issue that changes the format in the upcoming minor

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1201 on 2025-07-10 15:20_

I don't think we need to gate the `preview` flag.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2677 on 2025-07-10 15:23_

I'm inclined to create an actual notebook here. It demnonstrates the end-to-end flow better. It also allows you to delete a lot of the custom behavior you added to `TestEnvironment`, which brings us closer to the actual implementation (ensures that we test what runs in production)

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:16 on 2025-07-10 15:24_

With the approach you've taken now, the correct gating here is to make the `ruff_diagnostic/serde` feature dependent on whether the `serde` feature is enabled for `ruff_db` (similar to what we do with camino)

---

_Review comment by @MichaReiser on `crates/ty/src/args.rs`:326 on 2025-07-10 15:25_

We can delete the implementation on line 326. It should now be unused

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:292 on 2025-07-10 15:26_

We should change `TerminalSettings` to use the ty specific `DiagnosticFormat`. It's otherwise confusing (and easy to get wrong) why it allows more variants.

---

_@MichaReiser approved on 2025-07-10 15:32_

This looks good now, minor a few smaller changes. We do need to fix the schema change before landing this PR.

I suggest that you change the notebook diagnostic test to use an actual notebook source. This should reduce the amount of code that we need to mock out for testing. 

I also think it would be a good idea to test the output formats on the CLI (unless there are integration tests covering those output formats) because all tests now use the `ty` `FileResolver` (the ruff case is untested).

---

_@ntBre reviewed on 2025-07-10 15:54_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:100 on 2025-07-10 15:54_

https://github.com/astral-sh/ruff/issues/19263

---

_@MichaReiser reviewed on 2025-07-10 15:56_

---

_Review comment by @MichaReiser on `ty.schema.json`:1 on 2025-07-10 15:56_

Yes, they're intentionally separate because the one in the CLI crate depends on clap. But you can use the one you use for the `TerminalOptions`

---

_@ntBre reviewed on 2025-07-10 16:03_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1201 on 2025-07-10 16:03_

I get warnings about the field and `TestEnvironment::preview` method being unused without this when running `cargo test -p ruff_db` because they're only used for JSON. I'll change this to `#[allow(dead_code)]` for now since this will be used soon by other formats anyway.

---

_Comment by @ntBre on 2025-07-10 16:15_

I introduced some intentional typos to see if any integration tests fail, and we have one for JSON (`ruff::integration_test::stdin_json`) but not for JSON lines or Azure. I'll write some CLI tests.



---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/json.rs`:125 on 2025-07-10 18:28_

Out of curiosity, how come the `json!` macro instead of types? I'd imagine `json!` to be slower since it has to shove everything into the `serde_json::Value` type, but I guess I'd also imagine it probably doesn't matter here.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/json.rs`:215 on 2025-07-10 18:39_

I feel like if you're using the `json!` macro everywhere already, it's probably not worth a hand-impl of the `Serialize` trait? There's a bit more ceremony with it compared to just building a `Value::Array(Vec<Value>)` directly.

---

_@BurntSushi approved on 2025-07-10 18:49_

Nice! Feel free to ignore/disagree with my nits about `json!`. :-)

---

_@ntBre reviewed on 2025-07-10 19:13_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:125 on 2025-07-10 19:13_

I just pulled these over from the current Ruff version, so I'm not too sure. @MichaReiser might know, it looks like there were types deriving `Serialize` before https://github.com/astral-sh/ruff/pull/3895.

`json!` is kind of handy here for the `preview` behavior, but otherwise I'd lean toward types too.

---

_@MichaReiser reviewed on 2025-07-10 20:18_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:125 on 2025-07-10 20:18_

I don't. Wasn't it already like this before my PR and all I did was splitting the code into different modules.

I'm generally in favor of structs. It makes accidental schema changes easier to spot 

---

_@BurntSushi reviewed on 2025-07-10 22:23_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/json.rs`:125 on 2025-07-10 22:23_

Ah yeah sorry, I didn't realize this was copied code. I thought it was newly written.

---

_@ntBre reviewed on 2025-07-10 23:01_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:125 on 2025-07-10 23:01_

No worries, I should have pointed it out to help with the review. I opened #19270 to explore using structs.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:126 on 2025-07-11 15:21_

This feels a bit silly, but callers of this function expect a `Display` type. This avoids having to update the callers to use `serde_json::to_writer`, which in turn avoids updating their arguments from `std::fmt::Formatter`s to something implementing `std::io::Write` and their return types to something that can accommodate a `serde_json::Error` instead of a `std::fmt::Error`.

We could also just call `serde_json::to_value` and unwrap it here or in the callers, which is all this macro expands into.

---

_@ntBre reviewed on 2025-07-11 15:21_

---

_@ntBre reviewed on 2025-07-11 15:30_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:87 on 2025-07-11 15:30_

This is actually correct based on testing a released version of Ruff against the real notebook. I think the manual notebook cell index was outdated.

---

_@MichaReiser reviewed on 2025-07-11 15:42_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:247 on 2025-07-11 15:42_

Using an `enum` here strikes me a bit weird. I would either use two separate structs or just ignore the `old` entirely and ensure that the values are all `Some` where the change isn't backwards compatible.

---

_@MichaReiser reviewed on 2025-07-11 15:50_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:126 on 2025-07-11 15:50_

I would probably move the `json!` call to where you need the `Display` implementation.

---

_@ntBre reviewed on 2025-07-11 15:54_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:247 on 2025-07-11 15:54_

Oh, for some reason just wrapping the `unwrap_or_default` calls in `Some` didn't occur to me. That makes a lot more sense. Thanks!

---

_Comment by @ntBre on 2025-07-11 18:50_

I'm going to merge this and start working on more output formats, happy to follow up if I missed anything. Thanks for the reviews!

---

_Merged by @ntBre on 2025-07-11 19:04_

---

_Closed by @ntBre on 2025-07-11 19:04_

---

_Branch deleted on 2025-07-11 19:04_

---
