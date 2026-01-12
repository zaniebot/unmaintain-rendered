```yaml
number: 16711
title: "ruff_db: add new diagnostic renderer"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/new-renderer
created_at: 2025-03-13T16:30:43Z
updated_at: 2025-03-14T19:06:32Z
url: https://github.com/astral-sh/ruff/pull/16711
synced_at: 2026-01-12T15:55:56Z
```

# ruff_db: add new diagnostic renderer

---

_@BurntSushi_

This PR adds a new renderer using the new diagnostic type defined in
#16503 and built on top of our vendored copy of `annotate-snippets`.

It took me a fair bit of time to figure out how to arrange the types
and what kind of data was needed during each phase of the rendering. In
particular, there are a few different layers:

At the top are the `Diagnostic`, `SubDiagnostic` and `Annotation`
types. These encode the diagnostic data model and were added in #16503.

Once rendering starts, the `Diagnostic` type is converted into
"resolved" types that load any of the information we need to make
rendering _decisions_. For example, it associates line numbers and file
paths with annotations. This information is needed to group annotations
into _snippets_, which are a concept that is specifically not exposed
in the diagnostic data model. Instead, rendering is responsible for
deciding how to turn a collection of annotations into one or more
snippets.

The result of those rendering decisions are "renderable" types. I'm not
precisely sure how I'd best describe them, but I think it's fair to say
that the define the data model of _rendering_. They are also the last
stop before we convert them to `ruff_annotate_snippets` types and ask
for the _actual_ rendering of the diagnostics.

This PR does _not_ actually hook the new renderer up to anything.
There's just a pile of unit tests on very simplified data so that we
can test the various rendering logic at a very fine grained level.

I do think there are likely some decisions I made here with regards to
rendering that we will want to tweak in the future. For example, how
the order of snippets/annotations is determined. I tried to choose a
path that was sensible and simple for now, so that we can iterate on it
once we get more practical experience with it.

Closes #16506


---

_Review requested from @carljm by @BurntSushi on 2025-03-13 16:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-03-13 16:30_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-03-13 16:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-03-13 16:30_

---

_Comment by @github-actions[bot] on 2025-03-13 16:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2025-03-13 17:21_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:114 on 2025-03-13 17:22_

Nit: The convention is that `db` is always the first argument
```suggestion
        db: &dyn Db,
        config: &DisplayDiagnosticConfig,
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:125 on 2025-03-13 17:23_

```suggestion
        // diagnostic. But we should not punish them if they
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:88 on 2025-03-13 17:26_

Nit: Should this be `to_renderable` to communicate that it isn't a "cheap" operation?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:106 on 2025-03-13 17:27_

Nit: It's internal, so I don't think it matters much but we tried to avoid abbreviation in the Red Knot code base and the non-renderable type is also called `Diagnostic`. That's why I would go with `ResolvedDiagnostic`

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:263 on 2025-03-13 17:30_

I'm sorry to break this to you... but Ruff supports `\r` line endings 😆 
```suggestion
                if source.slice(range).ends_with(['\n', '\r']) {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:439 on 2025-03-13 17:33_

Nit: Should we add an explicit assertion for this pre-condition instead of relying on the implicit `&anns[0]` panicking.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:452 on 2025-03-13 17:34_

I like how you managed to put my awful context fiddling from Ruff behind a nice API

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:518 on 2025-03-13 17:34_

```suggestion
        // an error (red) and coloring for secondary annotations that
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:552 on 2025-03-13 17:35_

Oh yeah, the fact that Ruff uses `Path` might also be slightly annoying (and not `Utf8Path`). E.g. `file.path()` might not be able to return a `&str` but only a `String` (by calling `Display`)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:624 on 2025-03-13 17:37_

Delete?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2028 on 2025-03-13 17:39_

I guess this works... `SystemVirtualPath` is a very specific path that is only used in LSPs when a user creates a new file but without giving it a name. 

I think what you want to use here is `self.db.write_file` which writes a regular file and tests the more common path. This is still "cheap" and won't write an actual file to disk because `TestDb` uses a memory file system by default. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2047 on 2025-03-13 17:43_

This is poking into internals that we should probably avoid. Salsa makes it somewhat hard to avoid this using visibility only. 

The recommended way to get a file for a given path is to use `system_path_to_file`, which also verifies that the file actually exists.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2117 on 2025-03-13 17:44_

I somewhat suspect that we might end up with something like this in non-test code 😆 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:667 on 2025-03-13 17:45_

Would it make sense to have one non-ASCII test case or is this something you intentionally leave for later (e.g. easy to add in mdtests)

---

_@MichaReiser approved on 2025-03-13 17:47_

This is, once more, excellent. I'm always impressed by the quality of your code! 

The separation between `Diagnostic` and `RenderableDiagnostic` does make sense to me where `Diagnostic` is the generic representation and `RenderableDiagnostic` (or `ResolvedDiagnostic`) is the ideal representation for this renderer and Ruff/Red Knot will have multiple different renderers (e.g. a concise format can skip almost all of that work and the github renderer needs other information). 

The separation between the `Resolved` and `RenderableDiagnostic`s is less clear to me and I'd probably have to play with the code to fully crasp the difference but I don't think this matters much -- they're both internal types. 

---

_@BurntSushi reviewed on 2025-03-13 17:50_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:263 on 2025-03-13 17:50_

Wait you mean like, only `\r` line endings and not just `\r\n`? Wow.

---

_@MichaReiser reviewed on 2025-03-13 17:51_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:263 on 2025-03-13 17:51_

Yes :)

---

_@BurntSushi reviewed on 2025-03-14 15:29_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:88 on 2025-03-14 15:29_

I loosely agree. Fixed!

---

_@BurntSushi reviewed on 2025-03-14 15:30_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:106 on 2025-03-14 15:30_

Happy to follow convention here. Fixed.

Although I do like abbreviating things when it's "clear" what the abbreviation means. :-)

---

_@BurntSushi reviewed on 2025-03-14 15:33_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:439 on 2025-03-14 15:33_

Fine by me. Although I did document it as a pre-condition. :P 

Fixed!

---

_@BurntSushi reviewed on 2025-03-14 15:34_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:452 on 2025-03-14 15:34_

Yeah it took a few attempts. (You caught one aborted attempt in the commented out code.)

---

_@BurntSushi reviewed on 2025-03-14 15:36_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:552 on 2025-03-14 15:36_

Blech. We'll let future Andrew worry about that one haha.

We should _at least_ be able to manage a `Cow<'a, str>` though.

---

_@BurntSushi reviewed on 2025-03-14 15:37_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:2117 on 2025-03-14 15:37_

Plausibly. I tried hard to optimize for terseness though, and achieved that partially by filling in some test-only details.

I suspect the proc macro will alleviate most of the pressure here? Maybe.

---

_@MichaReiser reviewed on 2025-03-14 15:40_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2117 on 2025-03-14 15:40_

> I suspect the proc macro will alleviate most of the pressure here? Maybe.

Possibly, or the proc macro wants something like this to simplify the macro code. Either way. Something we don't have to worry about today

---

_@BurntSushi reviewed on 2025-03-14 18:38_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:667 on 2025-03-14 18:38_

Done. Coverage could definitely be improved here in the future. I think most of the thorniness for this will be in `annotate-snippets` itself though.

---

_@BurntSushi reviewed on 2025-03-14 18:52_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:2028 on 2025-03-14 18:52_

Oh I was just copying this from other tests: https://github.com/astral-sh/ruff/blob/4f2851982ddd3e8dc7e8e8fcb228f86cae19ce4d/crates/ruff_db/src/parsed.rs#L133-L135

It took me a bit to figure out how to fix this. I needed to use `self.db.test_system().write_file(..)` with the `WritableSystem` trait imported. Makes sense now (you need to skirt around things in order to get a "writable" system).

---

_@BurntSushi reviewed on 2025-03-14 18:53_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:2047 on 2025-03-14 18:53_

Fixed this along with https://github.com/astral-sh/ruff/pull/16711#discussion_r1996114702

---

_Merged by @BurntSushi on 2025-03-14 18:59_

---

_Closed by @BurntSushi on 2025-03-14 18:59_

---

_Branch deleted on 2025-03-14 18:59_

---

_@MichaReiser reviewed on 2025-03-14 19:06_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2028 on 2025-03-14 19:06_

Hmm, `write_file` should be available on `db` through the `DbWithWritableSystem` trait (which is implemented for every `Db` implementing `DbWithTestSystem` which `ruff_db::TestDb` implements) 

---

_@MichaReiser reviewed on 2025-03-14 19:06_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:2028 on 2025-03-14 19:06_

> Oh I was just copying this from other tests: 

That makes sense. This specific tests is that we can handle virtual files correctly :)

The test above does what's most commonly used:

https://github.com/astral-sh/ruff/blob/4f2851982ddd3e8dc7e8e8fcb228f86cae19ce4d/crates/ruff_db/src/parsed.rs#L113-L125

---
