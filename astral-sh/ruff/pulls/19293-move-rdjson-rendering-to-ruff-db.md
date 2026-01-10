```yaml
number: 19293
title: "Move RDJSON rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/rdjson
created_at: 2025-07-11T21:19:59Z
updated_at: 2025-07-15T12:47:46Z
url: https://github.com/astral-sh/ruff/pull/19293
synced_at: 2026-01-10T17:58:13Z
```

# Move RDJSON rendering to `ruff_db`

---

_Pull request opened by @ntBre on 2025-07-11 21:19_

## Summary

Another output format like #19133. This is the [reviewdog](https://github.com/reviewdog/reviewdog) output format, which is somewhat similar to regular JSON. Like #19270, in the first commit I converted from using `json!` to `Serialize` structs, then in the second commit I moved the module to `ruff_db`.

The reviewdog [schema](https://github.com/reviewdog/reviewdog/blob/320a8e73a94a09248044314d8ca326a6cd710692/proto/rdf/jsonschema/DiagnosticResult.json) seems a bit more flexible than our JSON schema, so I'm not sure if we need any preview checks here. I'll flag the places I wasn't sure about as review comments.

## Test Plan

New tests in `rdjson.rs`, ported from the old `rjdson.rs` module, as well as the new CLI output tests.


---

_Label `internal` added by @ntBre on 2025-07-11 21:20_

---

_Label `diagnostics` added by @ntBre on 2025-07-11 21:20_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:112 on 2025-07-11 21:22_

This is the main preview check I think we could skip, based on this definition of `Location`:

https://github.com/reviewdog/reviewdog/blob/320a8e73a94a09248044314d8ca326a6cd710692/proto/rdf/jsonschema/DiagnosticResult.json#L126-L141

As I mention in the module comment, only the `description` field says "Optional," not the `type` field.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:160 on 2025-07-11 21:24_

Compare this to `main`, where we don't include `suggestions` in the `json!` call if the fix is `None`:

https://github.com/astral-sh/ruff/blob/7154b64248e7d42f277d892f347527a7ec1410d6/crates/ruff_linter/src/message/rdjson.rs#L81-L90

I think we could probably just serialize this as an empty array or as an `Option`. It's described as optional in the [schema](https://github.com/reviewdog/reviewdog/blob/320a8e73a94a09248044314d8ca326a6cd710692/proto/rdf/jsonschema/DiagnosticResult.json#L102-L108).

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:234 on 2025-07-11 21:25_

This is probably silly, but `json!` again seem to have different sorting behavior on the fields. If we can accept some snapshot changes flipping `line` and `column` we can just delete this. I didn't think that could be breaking but wanted to be safe.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:125 on 2025-07-11 21:30_

This one is actually not optional in the [schema](https://github.com/reviewdog/reviewdog/blob/320a8e73a94a09248044314d8ca326a6cd710692/proto/rdf/jsonschema/DiagnosticResult.json#L45-L59), but this is consistent with `main`. I guess we may need to `unwrap_or_default` here. The same might be true for `url`. Both are currently `null` for syntax errors:

https://github.com/astral-sh/ruff/blob/7154b64248e7d42f277d892f347527a7ec1410d6/crates/ruff/tests/snapshots/lint__output_format_rdjson.snap#L77-L80

---

_Comment by @github-actions[bot] on 2025-07-11 21:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-07-11 21:30_

---

_Marked ready for review by @ntBre on 2025-07-11 21:31_

---

_Review requested from @carljm by @ntBre on 2025-07-11 21:31_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-11 21:31_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-11 21:31_

---

_Review requested from @sharkdp by @ntBre on 2025-07-11 21:31_

---

_Review requested from @dcreager by @ntBre on 2025-07-11 21:31_

---

_Review request for @dcreager removed by @ntBre on 2025-07-11 21:31_

---

_Review request for @carljm removed by @ntBre on 2025-07-11 21:31_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-11 21:31_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-11 21:31_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:176 on 2025-07-11 21:37_

This is also consistent with `main`. We could use our real severity here, but it would change the default from `warning` to `error` for existing Ruff diagnostics.

---

_@ntBre reviewed on 2025-07-11 21:37_

---

_@MichaReiser reviewed on 2025-07-12 10:34_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:112 on 2025-07-12 10:34_

I suggest using a schema validator to validate our understanding, e.g. https://www.jsonschemavalidator.net/

---

_@MichaReiser reviewed on 2025-07-12 10:35_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:125 on 2025-07-12 10:35_

My suggestion here is to use the lint id if the secondary code is missing.

---

_@MichaReiser reviewed on 2025-07-12 10:35_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:176 on 2025-07-12 10:35_

I suggest making this change under preview

---

_@MichaReiser reviewed on 2025-07-12 10:37_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:234 on 2025-07-12 10:37_

I wouldn't consider changing the ordering to be a breaking change (as JSON fields aren't ordered)

---

_@ntBre reviewed on 2025-07-12 20:37_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:234 on 2025-07-12 20:37_

This didn't actually end up changing snapshots either, I guess the `json!` call in the renderer preserves the original sorting behavior. It must have only mattered in an intermediate version I had locally.

---

_@ntBre reviewed on 2025-07-12 20:45_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:176 on 2025-07-12 20:45_

I just realized that this is a single severity for the whole collection. I guess we can use the max severity in preview.

There's also a severity field on individual diagnostics that we aren't using. I'll try that in the schema validator too.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:125 on 2025-07-12 20:51_

Good idea, updated! The [validator](https://www.jsonschemavalidator.net/s/uvGWl3nq) is also complaining about the URL. I guess I'll just put the `CARGO_PKG_HOMEPAGE` if there's no rule-specific page available.

---

_@ntBre reviewed on 2025-07-12 20:51_

---

_@ntBre reviewed on 2025-07-12 21:01_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:112 on 2025-07-12 21:01_

A `null` range is invalid according to the validator, so this should just be `unwrap_or_default` irrespective of preview.

Actually, completely omitting it from the JSON also passes the validator, so I guess another option is skipping serialization if it's `None`.

---

_@MichaReiser reviewed on 2025-07-13 12:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:112 on 2025-07-13 12:04_

That makes sense, we should skip it then (which should be easy enough with serde)

---

_Comment by @MichaReiser on 2025-07-13 14:38_

@ntBre is this ready for re-review or are there more changes that you plan on making (I have no particular reason to believe that there's anything that needs changing. I just want to avoid taking a look if you plan on making changes to due the schema discussion we had)

---

_Comment by @ntBre on 2025-07-13 14:44_

Yes it's ready again, I just added your suggestion about skipping the `range` instead of using a default.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:27 on 2025-07-14 07:18_

This comment here seems outdated. I think we should either remove it entirely or change it to say that additional properties are optional?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:115 on 2025-07-14 07:20_

`location` is also an additional property. Can you verify if the JSON is valid if you omit the `location`? 

If it can be omitted, we then shouldn't call back to `unwrap_or_default` and instead omit the `location` when a file is missing (a diagnostic should never have just a range because a range without a file is not very useful)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:117 on 2025-07-14 07:22_

I think we should keep setting `WARNING` for all diagnostics until we officially support a WARNING severity. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:132 on 2025-07-14 07:28_

I'm inclined to go with what they have in ther protobuf implementation where, as far as I can understand, it's valid to omit the url property when empty

https://github.com/reviewdog/reviewdog/blob/294118b12c6b85c28d21deb0458703cc7a878608/proto/rdf/reviewdog.pb.go#L660-L669

I'm inclined to go with that for now (the same with the range on location)

Even their own tests don't specify a url: https://github.com/reviewdog/reviewdog/blob/master/parser/rdjson_test.go

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:99 on 2025-07-14 07:30_

I think we should add an assertion to `Diagnostic::set_fix` instead that asserts that it has an associated source files. Edits without a source file aren't very useful.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:160 on 2025-07-14 07:37_

Using `skip_serializing_if` seems correct to me? What would be the benefit of using an `Option`?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:226 on 2025-07-14 07:37_

I'm not sure what the benefit is of setting the severity at the file level. I'm also inclined to just keep this `None` until we add a `WARNING` severity. Adding severities feels like an unrelated change

---

_@MichaReiser reviewed on 2025-07-14 07:39_

---

_Converted to draft by @ntBre on 2025-07-14 12:10_

---

_@ntBre reviewed on 2025-07-14 13:39_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:27 on 2025-07-14 13:39_

I'll just remove it. I think testing against the schema validator is much better than any notes I could write here. Thanks again for that suggestion!

---

_@ntBre reviewed on 2025-07-14 13:41_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:115 on 2025-07-14 13:41_

Yeah it's valid to omit the location. I'll do that instead.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:160 on 2025-07-14 13:48_

I guess I'm not sure, `skip_serializing_if` makes sense to me too.

---

_@ntBre reviewed on 2025-07-14 13:48_

---

_@ntBre reviewed on 2025-07-14 13:50_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:117 on 2025-07-14 13:50_

We weren't setting the per-diagnostic severity at all before. Would it be better to go back to that, or to set them all to WARNING?

---

_@ntBre reviewed on 2025-07-14 13:58_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:226 on 2025-07-14 13:58_

Ah okay, I'll revert the per-diagnostic severity and just keep `WARNING` as the global severity. I agree that this feels unrelated to the rest of the refactor.

---

_@MichaReiser reviewed on 2025-07-14 14:11_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:117 on 2025-07-14 14:11_

I would probably remove it for now. We can add the severity when introducing a warning severity to ruff

---

_@ntBre reviewed on 2025-07-14 14:37_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:99 on 2025-07-14 14:37_

Added a `debug_assert!` and then updated some tests that were actually setting fixes on diagnostics without files. Happy to upgrade to `assert` if you prefer.

---

_Marked ready for review by @ntBre on 2025-07-14 14:38_

---

_Comment by @ntBre on 2025-07-14 14:39_

I think this should be ready for another look. The main thing I'm unsure about is the `debug_assert` in `Diagnostic::set_fix` combined  with the runtime `unwrap` in `rdjson_suggestions`. Should they both be `debug_assert` or both full `assert`/`unwrap`?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/json.rs`:311 on 2025-07-14 15:35_

Do we have any other test for edits? If not, I think it's worth preserving testing fixes in some test or another.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:29 on 2025-07-14 16:21_

```suggestion
            serde_json::json!(RdjsonDiagnostics::new(diagnostics, self.resolver))
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:78 on 2025-07-14 16:22_

Nit: Could we use `source_code.name()` here? It makes it clearer that `range` depends on the same value

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:102 on 2025-07-14 16:24_

Nit: You could make this code more error resilient by skipping all edits if `source_code` is empty (but use a `debug_assert` to make it fail if that happens).

It has the benefit that Ruff doesn't panic if we ever fail to set the source file

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:160 on 2025-07-14 16:25_

Can you say more about why? I understand that an empty suggestion or an omitted suggestion are the same. Omitting has the benefit that there's less JSON to serialize / deserialize which is smaller and probably faster.

---

_@MichaReiser approved on 2025-07-14 16:26_

Thank you. 

---

_@ntBre reviewed on 2025-07-14 16:29_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:160 on 2025-07-14 16:29_

Sorry, I may have phrased that poorly. I agree with you that `skip_serializing_if` makes more sense. I wasn't sure what my original reasoning was for even mentioning an `Option` :sweat_smile: I think I was just listing possibilities.

---

_@ntBre reviewed on 2025-07-14 16:32_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/json.rs`:311 on 2025-07-14 16:32_

Yes, the `output` and `notebook_output` unit tests and the CLI integration tests all have fixes/edits.

---

_@ntBre reviewed on 2025-07-14 19:26_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:78 on 2025-07-14 19:26_

At the risk of missing something obvious here, I don't see a `SourceCode::name` method. I think the `SourceFile` has the name but not the `SourceCode`. The `Span`  seems like the common ancestor for both `range` and `location`, unless I factored out a shared `span.file()` call.

---

_@MichaReiser reviewed on 2025-07-15 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:78 on 2025-07-15 07:24_

Oh, it's `SourceCode` and not `SourceFile`...


I would rewrite this to

```rust
	let source_file = span.map(|span| {
        let file = span.file();
        (file.path(resolver), file.diagnostic_source(resolver))
    });

    let location = source_file.as_ref().map(|(path, source)| {
        let range = diagnostic.range().map(|range| {
            let source_code = source.as_source_code();
            let start = source_code.line_column(range.start());
            let end = source_code.line_column(range.end());
            RdjsonRange::new(start, end)
        });

        RdjsonLocation { path, range }
    });

    let edits = diagnostic.fix().map(Fix::edits).unwrap_or_default();

    RdjsonDiagnostic {
        message: diagnostic.body(),
        location,
        code: RdjsonCode {
            value: diagnostic
                .secondary_code()
                .map_or_else(|| diagnostic.name(), |code| code.as_str()),
            url: diagnostic.to_ruff_url(),
        },
        suggestions: rdjson_suggestions(edits, source_file.map(|(_, source)| source)),
    }
```

It makes it clearer that `range` can only be set when `span` isn't `None`. 

---

_@ntBre reviewed on 2025-07-15 12:36_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/rdjson.rs`:78 on 2025-07-15 12:36_

Thanks! I like this better than the version in this PR with many separate `map` calls and also better than the mutable version in #19340. I'll try to use this pattern going forward.

---

_Merged by @ntBre on 2025-07-15 12:39_

---

_Closed by @ntBre on 2025-07-15 12:39_

---

_Branch deleted on 2025-07-15 12:39_

---
