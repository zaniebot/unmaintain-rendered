```yaml
number: 12747
title: Gracefully handle errors in CLI
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-run-gracefully-handle-errors
created_at: 2024-08-08T09:52:36Z
updated_at: 2024-08-09T06:41:47Z
url: https://github.com/astral-sh/ruff/pull/12747
synced_at: 2026-01-12T15:55:42Z
```

# Gracefully handle errors in CLI

---

_@MichaReiser_

## Summary

Replace the remaining `unwrap` calls in the CLI with more gracefully propagating the errors. 

Long term, my goal is to replace `anyhow::Error` with a more structured representation (ideally it's just another form of Diagnostic)
because these errors don't look as nice as they could. But this matches Ruff's current behavior

## Test Plan

Examples:

```
ruff failed
  Cause: Provided current-directory path '../test/a.py' is not a directory.
```

```
ruff failed
  Cause: Failed to search the virtual environment directory for `site-packages`
  Cause: No such file or directory (os error 2)
```




---

_Review requested from @carljm by @MichaReiser on 2024-08-08 09:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-08 09:52_

---

_Label `red-knot` added by @MichaReiser on 2024-08-08 09:53_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:96 on 2024-08-08 09:54_

```suggestion
        // This communicates that this isn't a linter error but red-knot itself hard-errored for
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:152 on 2024-08-08 09:55_

```suggestion
    // TODO: Move verification of the other search path settings
    // out of the module resolver 
```

---

_@AlexWaygood approved on 2024-08-08 09:58_

---

_@AlexWaygood reviewed on 2024-08-08 10:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/site_packages.rs`:93 on 2024-08-08 10:07_

Since this is a path yielded by a call to `read_directory()`, I don't think it's possible for `path.file_name()` to be `None`. Given that, I find the current code on `main` marginally easier to understand, since this `continue` can never be executed. We could also use `path.file_name().expect(...)`, I suppose?

---

_Comment by @github-actions[bot] on 2024-08-08 10:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-08-08 10:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/site_packages.rs`:93 on 2024-08-08 10:10_

(But using `.file_name()` instead of `.components().next_back()` is definitely an improvement, of course :-)

---

_@MichaReiser reviewed on 2024-08-08 10:50_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:152 on 2024-08-08 10:50_

I prefer the comment as is because I don't think we want to move the verification logic *out* of the module resolver We mainly want to call it earlier.

---

_@AlexWaygood reviewed on 2024-08-08 10:53_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:152 on 2024-08-08 10:53_

Then maybe this? I don't think the way you have it is accurate because we do verify them currently, we just do it at the wrong stage

```suggestion
    // TODO: Verify the remaining search path settings eagerly
```

---

_@MichaReiser reviewed on 2024-08-08 10:55_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:152 on 2024-08-08 10:55_

Let's not overthink the todo comment and instead make a decision of how we want to restructure the code ;P

---

_@AlexWaygood reviewed on 2024-08-08 10:58_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:100 on 2024-08-08 10:58_

```suggestion
        writeln!(stderr, "{}", "Red Knot failed".red().bold()).ok();
```

---

_Merged by @MichaReiser on 2024-08-08 11:02_

---

_Closed by @MichaReiser on 2024-08-08 11:02_

---

_Branch deleted on 2024-08-08 11:02_

---

_@AlexWaygood reviewed on 2024-08-08 15:14_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:100 on 2024-08-08 15:14_

This was resolved in https://github.com/astral-sh/ruff/commit/d28c5afd14a9ae01f960bf812298065d3450a91f

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:135 on 2024-08-08 21:00_

Nit: `s/unicode/UTF-8/`.

We have no idea if it contains unicode or not, because unicode is just a set of numeric code points, it's not an encoding. It may well be valid Unicode encoded as UTF-32, for all we know. It's just not valid UTF8-encoded text.

---

_@carljm reviewed on 2024-08-08 21:00_

---

_@MichaReiser reviewed on 2024-08-09 05:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:135 on 2024-08-09 05:52_

I'm not so sure if we're limited to UTF8 here. [`Path::to_string_lossly`](https://doc.rust-lang.org/stable/std/ffi/struct.OsString.html#method.to_string_lossy) specifically mentioned unicode and not UTF8

---

_@carljm reviewed on 2024-08-09 06:25_

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:135 on 2024-08-09 06:25_

It only handles UTF-8. (Well, on Windows it handles a weird Windows-specific variant of UTF-8, but close enough.) It's not possible to transparently and automatically handle all the various Unicode encodings; they are ambiguous with each other. You have to either assume or specify a particular encoding. 

It's too bad that the rust docs are also careless with this wording. It's meaningless to say that a byte string "is Unicode" or "is not Unicode", unless by "Unicode" you mean (or assume) some specific encoding.

---

_@MichaReiser reviewed on 2024-08-09 06:30_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:135 on 2024-08-09 06:30_

Yeah, still not sure about this. It's true that you can only store one encoding, but you can decode between encodings. E.g. Rust converts the Windows UTF16 paths to UTF8. That means, it isn't about what unicode encoding paths use on the file system, for as long as Rust decodes the paths to UTF8 before hand.

Also, from the camino docs https://docs.rs/camino/latest/camino/ 

Either way. I think using Unicode here is "correct" enough.

---

_@carljm reviewed on 2024-08-09 06:41_

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:135 on 2024-08-09 06:41_

I see. You (and probably the rust docs too) don't want to say UTF-8 (even though the actual decode that's failing is definitely a decode from UTF-8, or the windows "wide" variant of it) because the original path on disk on windows is UTF-16 (decoded by rust and re-encoded as "wide" UTF-8 in the OsStr). So we use "Unicode" as imprecise shorthand for "the typical Unicode encoding used on your OS, ie UTF-16 on Windows and UTF-8 everywhere else." I guess maybe that's reasonable, for lack of a more precise term. 

---
