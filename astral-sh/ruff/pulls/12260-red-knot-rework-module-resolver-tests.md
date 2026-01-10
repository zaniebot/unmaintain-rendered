```yaml
number: 12260
title: "[red-knot] Rework module resolver tests"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: cleanup-module-resolver-tests
created_at: 2024-07-09T18:04:56Z
updated_at: 2024-07-10T11:04:57Z
url: https://github.com/astral-sh/ruff/pull/12260
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Rework module resolver tests

---

_Pull request opened by @AlexWaygood on 2024-07-09 18:04_

## Summary

This PR aims to improve the module-resolver tests in `path.rs` and `resolver.rs` in several ways:
- Reduce boilerplate when setting up the tests
- Make the tests more declarative.
- Enforce that a mock stdlib directory must include a `VERSIONS` file
- Rather than sharing one large typeshed mock between many tests in `path.rs`, create a minimal typeshed mock for each test. This makes it clearer what exactly we're testing in each test.

Logic to support the building of test cases is extracted into a single submodule. It's now easy to create an isolated, declarative test case where it's obvious exactly which files and directories exist in the mock. For example:

```rs
const SRC: &[FileSpec] = &[("functools.py", "def update_wrapper(): ...")];

const TYPESHED: MockedTypeshed = MockedTypeshed {
    stdlib_files: &[("functools.pyi", "def update_wrapper(): ...")],
    versions: "functools: 3.8-",
};

let TestCase { db, src, .. } = TestCaseBuilder::new()
    .with_src_files(SRC)
    .with_custom_typeshed(TYPESHED)
    .with_target_version(TargetVersion::Py38)
    .build();
```

## Test Plan

`cargo test -p red_knot_module_resolver`


---

_Label `red-knot` added by @AlexWaygood on 2024-07-09 18:04_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-09 18:04_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-09 18:04_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 18:18_

This seems a bit arbitrary?

---

_@carljm approved on 2024-07-09 18:26_

Looks nice, I like this a lot!

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 19:29_

Yeah, it _is_ arbitrary. However, we do need _something_ in the `VERSIONS` file or it will fail to parse:

https://github.com/astral-sh/ruff/blob/88abc6aed801995864bb4f8bcb401d1c2cb48baf/crates/red_knot_module_resolver/src/typeshed/versions.rs#L310-L314

E.g. two tests that don't test anything to do with the stdlib start failing if I make this diff:

```diff
diff --git a/crates/red_knot_module_resolver/src/testing.rs b/crates/red_knot_module_resolver/src/testing.rs
index 55f0d9d0a..ce38aca5c 100644
--- a/crates/red_knot_module_resolver/src/testing.rs
+++ b/crates/red_knot_module_resolver/src/testing.rs
@@ -186,7 +186,7 @@ impl TestCaseBuilder<UnspecifiedTypeshed> {
     pub(crate) fn build(self) -> TestCase<()> {
         const DEFAULT_TYPESHED: MockedTypeshed = MockedTypeshed {
             stdlib_files: &[("functools.pyi", "def update_wrapper(): ...")],
-            versions: "functools: 3.8-",
+            versions: "",
         };
 
         let TestCase {
```

And if the `VERSIONS` file says a module exists, then we probably should indeed put that module in the mock typeshed...

Maybe I should just put an empty `foo.pyi` file in the default typeshed mock, and put `foo` in the `VERSIONS` file? And add a comment? WDYT?

---

_@AlexWaygood reviewed on 2024-07-09 19:29_

---

_@carljm reviewed on 2024-07-09 19:35_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 19:35_

My initial feeling is that a VERSIONS file that exists but is empty should not be an error in the parser, it just means there are no stdlib stub files. And that should be the default for tests that don't either provide a mock typeshed or request to use the vendored typeshed.

---

_@AlexWaygood reviewed on 2024-07-09 19:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 19:45_

Yeah, that makes sense. Two counterpoints, however:
- I think it is useful to validate _somewhere_ that the contents of a custom typeshed directory a user passes to us makes some kind of sense. For example, mypy checks that a small selection of "core" stdlib modules exist in typeshed, and aborts the process immediately if they don't, since it knows no typechecking it does is going to make any kind of sense without them (`builtins`, `typing`, `collections.abc`, `types`, `typing_extensions`, I believe). I think it makes sense for us to do similarly at some place in `red_knot` (but it doesn't necessarily have to be in the `VERSIONS` parser).
- I'm somewhat uneasy about the fact that `File::read_to_string()` just returns an empty string if a file doesn't exist... in the specific ways we're currently using the `ruff_db` API to get the `VERSIONS` file in `versions.rs`, it should be impossible for us to try parsing an empty string that represents a nonexistent `VERSIONS` file, but I still sort of worry about it... https://github.com/astral-sh/ruff/blob/88abc6aed801995864bb4f8bcb401d1c2cb48baf/crates/ruff_db/src/files.rs#L181-L190

---

_@MichaReiser reviewed on 2024-07-09 20:12_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 20:12_

Regarding Files::read_to_string. The fallback path is only taken if the file has been deleted since the last system_path_to_file call. The analysis will most likely be aborted shortly after once the file watcher detects the change and marks the file as deleted (and system_path_to_fike returns None)

Regarding the check. I think that's a useful check for the settings loading code but feels a bit over eager in the parser

---

_@carljm reviewed on 2024-07-09 20:12_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 20:12_

I guess I am torn here between a) do the simple and obvious thing, and don't unnecessarily prevent users from doing what they want, even if it's odd (for example, an empty typeshed), and b) prevent users from shooting themselves in the foot accidentally.

I think that we should be able to roll with an empty stdlib, even an empty builtins; it'll just result in a lot of Unknown/Unbound in our type inference.

I guess I feel like we should either allow empty VERSIONS, or require an actually-usable stdlib (at least has builtins). Requiring a non-empty-but-can-have-whatever-nonsense-you-want-in-it isn't a middle ground that seems very useful; it just results in pointless workarounds like "let's have a stdlib containing only `foo.pyi`".

---

_@AlexWaygood reviewed on 2024-07-09 20:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 20:17_

Alrighty. For now I'll allow an empty `VERSIONS`, then. We can add more principled validation if/when we feel we need it.

---

_@AlexWaygood reviewed on 2024-07-09 20:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 20:23_

> The analysis will most likely be aborted shortly after once the file watcher detects the change and marks the file as deleted (and system_path_to_fike returns None)

Right. That still feels like an assumption that I feel like I'd prefer to check explicitly (and panic if it turns out to be incorrect). But not sure; happy to defer to you.

---

_@MichaReiser reviewed on 2024-07-09 21:04_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/testing.rs`:189 on 2024-07-09 21:04_

I like to verify that as well. But it should not panic, because that tears down the entire program. Rust analyzer has a few interesting techniques. It keeps the last working workspace tree around for the case that a user messes up the editing of a Cargo.toml. They use the old workspace structure in that case because it's still better than knowing nothing about the program. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:985 on 2024-07-10 06:55_

Nit: the `const` probably doesn't give you much here and is a bit more annoying to use because you have to annotate the type. That's why I would have opted to use a regular `let` here. But it's not worth the time to change it.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/testing.rs`:136 on 2024-07-10 06:58_

Nit: I would rename `system` to `fs` because it's not the system ;)

You also don't have to call `create_directory_all`. `write_all` creates all the necessary intermediate directories for you (it's designed as a testing FS and having to create the directories manually is cumbersome)

---

_@MichaReiser approved on 2024-07-10 07:02_

This is neat!

---

_@AlexWaygood reviewed on 2024-07-10 10:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:985 on 2024-07-10 10:26_

Yeah, agree that having to explicitly annotate the variables is a bit annoying. That said, I still kinda like using `const` for this, as it makes it explicit that this is fully static data, which is a nice property for test input.

---

_Merged by @AlexWaygood on 2024-07-10 10:40_

---

_Closed by @AlexWaygood on 2024-07-10 10:40_

---

_@MichaReiser reviewed on 2024-07-10 11:04_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:985 on 2024-07-10 11:04_

Maybe, until you hit the point where it can no longer be static :laughing: 

---

_Branch deleted on 2024-07-10 11:04_

---
