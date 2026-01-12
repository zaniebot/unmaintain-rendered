```yaml
number: 20105
title: Deduplicate input paths
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: deduplicate-input-files
created_at: 2025-08-26T19:42:01Z
updated_at: 2025-09-19T18:40:23Z
url: https://github.com/astral-sh/ruff/pull/20105
synced_at: 2026-01-12T15:56:54Z
```

# Deduplicate input paths

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20035, fixes #19395

This is for deduplicating input paths to avoid processing the same file multiple times.

This is my first contribution, so I'm sorry if I miss something. Please tell me if this is needed for this feature.

## Test Plan

<!-- How was it tested? -->

I just added a test `find_python_files_deduplicated` in https://github.com/TaKO8Ki/ruff/blob/eee1020e322e693bf977d91bf3edd03b45420254/crates/ruff_workspace/src/resolver.rs#L1017
. This pull request adds changes to `WalkPythonFilesState::finish`, which is used in `python_files_in_path`, so they affect some commands such as `analyze`, `format`, `check` and so on. I will add snapshot tests for them if necessary.

I’ve already confirmed that the same thing happens with ruff check as well.

```
$ echo "x   = 1" > example/foo.py
$ uvx ruff check example example/foo.py
I002 [*] Missing required import: `from __future__ import annotations`
--> /path/to/example/foo.py:1:1
help: Insert required import: `from __future__ import annotations`

I002 [*] Missing required import: `from __future__ import annotations`
--> /path/to/example/foo.py:1:1
help: Insert required import: `from __future__ import annotations`

Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

---

_Comment by @TaKO8Ki on 2025-08-27 03:17_

I need to handle this cause `Explicitly pass test.py, should be linted regardless of it being excluded by lint.exclude`, so I will.

---

_Renamed from "Deduplicate input paths" to "[`ruff`] Deduplicate input paths" by @TaKO8Ki on 2025-08-27 04:58_

---

_Comment by @TaKO8Ki on 2025-08-28 00:39_

@ntBre Thank you for reviewing another pull request of mine. Could you approve the workflow and review this pull request if possible?

---

_Comment by @ntBre on 2025-08-28 01:28_

No problem! I'll get to this soon, it's on my todo list :) I'll kick off the workflow now

---

_Label `bug` added by @ntBre on 2025-08-28 01:29_

---

_Label `cli` added by @ntBre on 2025-08-28 01:29_

---

_Review requested from @ntBre by @ntBre on 2025-08-28 01:29_

---

_Comment by @github-actions[bot] on 2025-08-28 01:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @TaKO8Ki on 2025-08-28 02:27_

@ntBre Thank you. I fixed clippy errors on CI.

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:538 on 2025-08-28 19:02_

Did you run into this situation when testing this out? This is my first time looking at this code, but it seems like we will have already applied exclusion rules in `python_files_in_path` here:

https://github.com/astral-sh/ruff/blob/e42006ece8c53f5e4196849af5c0b3d6c6fadfd2/crates/ruff_workspace/src/resolver.rs#L470-L476

and in `PythonFilesVisitorBuilder`:

https://github.com/astral-sh/ruff/blob/e42006ece8c53f5e4196849af5c0b3d6c6fadfd2/crates/ruff_workspace/src/resolver.rs#L574-L581

which will both run before `WalkPythonFileState::finish`.

It makes sense to me to favor `Root` nodes over `Nested` ones when deduplicating, but I'm not sure if it actually affects exclusions. It might be good to add a test for that case, if it does.

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:1047 on 2025-08-28 19:04_

I think I would kind of prefer a CLI test or two. Maybe one in [`lint.rs`](https://github.com/astral-sh/ruff/blob/e42006ece8c53f5e4196849af5c0b3d6c6fadfd2/crates/ruff/tests/lint.rs#L5863) and one in [`format.rs`](https://github.com/astral-sh/ruff/blob/e42006ece8c53f5e4196849af5c0b3d6c6fadfd2/crates/ruff/tests/format.rs#L2329) to make sure it works for both cases?

I guess both of the bug reports were about formatting, but the linter shows the same behavior in my local testing.

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:539 on 2025-08-28 19:07_

I think if we take an owned `ResolvedFiles` here we could avoid some clones. That makes sense anyway since we're "consuming" the input `files` and returning a new version.

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:543 on 2025-08-28 19:20_

Would it be simpler just to sort the `files` and then deduplicate them? Something like this:

```rust
fn deduplicate_files(mut files: ResolvedFiles) -> ResolvedFiles {
    files.sort();
    files.dedup_by_key(|result| result.map(|file| file.path()));
    files
```

If we derive `PartialOrd` on `ResolvedFile`, I think this will automatically sort `Root` files before `Nested` files, and then the dedup call will take the first element with a given path.

I think this would also deduplicate the `Error`s, which could be a good thing or a bad thing. I'm not really sure.

Another idea, which seems like it could be cheaper overall, would be to filter out duplicates as we're walking the file system. Did you give that a try? It seems a bit more intuitive to me and avoids having to sort or filter anything at the end. I think if `PythonFilesVisitor::local_files` were an `FxHashMap` of path -> `Result<ResolvedFile>`, it might be pretty straightforward.

---

_@ntBre reviewed on 2025-08-28 19:28_

Thanks for working on this! I left more detailed comments inline, but I think it would be good to add CLI tests for this, and I'm also quite interested in an approach where we deduplicate as we visit the files.

---

_@TaKO8Ki reviewed on 2025-08-30 13:51_

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:538 on 2025-08-30 13:51_

Yes. They don't deduplicate input paths in this situation.

---

_@TaKO8Ki reviewed on 2025-08-30 13:53_

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:1047 on 2025-08-30 13:53_

Yes. You're right. As I said pull request description, ruff check show the same result. 

> I’ve already confirmed that the same thing happens with ruff check as well.

So I will add CLI tests too.

---

_Review requested from @ntBre by @TaKO8Ki on 2025-09-01 22:23_

---

_Comment by @TaKO8Ki on 2025-09-02 17:13_

@ntBre Thank you for the review. I have addressed your comments.

---

_@MichaReiser reviewed on 2025-09-15 13:29_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:538 on 2025-09-15 13:29_

I'm not entirely sure I follow why this logic is important. If the paths are identical, why does it matter that we return the root path first? 

Can you add an example why this is important? If it isn't important, should we use a `Set` instead of a `Vec` to avoid this collecting step altogether? Or could we do better and deduplicate the input paths instead? 

---

_@TaKO8Ki reviewed on 2025-09-16 13:27_

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:538 on 2025-09-16 13:27_

It's important and related to: https://github.com/astral-sh/ruff/blob/9edbeb44dd5869a32200cbb442221041ce4ace1a/crates/ruff/tests/lint.rs#L252

Root has to be prioritized because `ResolvedFile::Root` means “explicitly passed on the CLI,” and excludes only apply to non‑root entries unless --force-exclude is set.

Dropping the root entry means that the explicitly passed path may be unintentionally ignored, since it is treated as nested and can be excluded despite being requested.

Concretely, with `lint.exclude = ["foo.py"]` and `ruff check . foo.`py, we must keep Root(foo.py) and drop Nested(foo.py) so foo.py is linted as the user requested.


---

_@TaKO8Ki reviewed on 2025-09-16 13:28_

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:538 on 2025-09-16 13:28_

I have added this explanation to the comment.

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-09-16 13:39_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:758 on 2025-09-16 14:46_

I'm pretty sure this is equivalent to the implementation that would be derived, unless I'm missing some subtlety here. Could we replace the manual `Ord` and `PartialOrd` implementations with `derive(Ord, PartialOrd)`?

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:552 on 2025-09-16 14:48_

I think this is also nearly the same as the derived `PartialOrd` implementation for `Result`, except that we consider any errors equal. It seems okay to me just to call `files.sort()` and use the default implementation.

---

_@ntBre reviewed on 2025-09-16 14:53_

Thanks, I just had a couple more potential simplification suggestions. I'm also still interested in deduplicating the files as we traverse the file system instead of sorting and filtering at the end, as I mentioned in https://github.com/astral-sh/ruff/pull/20105#discussion_r2308327043, but if Micha is happy with this then I am too! This is probably the easier approach if it's not too expensive.

---

_@TaKO8Ki reviewed on 2025-09-16 15:26_

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:552 on 2025-09-16 15:26_

I also think it's ideal, but we cannot use `files.sort()` here since `ignore::Error` does not implement `Ord`.

```
type ResolvedFiles = Vec<Result<ResolvedFile, ignore::Error>>;
```

```
the trait bound `ignore::Error: std::cmp::Ord` is not satisfied
the trait `std::cmp::Ord` is implemented for `std::result::Result<T, E>`
required for `std::result::Result<resolver::ResolvedFile, ignore::Error>` to implement `std::cmp::Ord`
```

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:543 on 2025-09-16 15:40_

I will try using FxHashMap for local_files and confirm if it does not cause any side effects.

---

_@TaKO8Ki reviewed on 2025-09-16 15:40_

---

_@ntBre reviewed on 2025-09-16 16:01_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:552 on 2025-09-16 16:01_

Ahh okay, thanks!

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:758 on 2025-09-16 16:07_

I have added `derive(Ord, PartialOrd)`.

---

_@TaKO8Ki reviewed on 2025-09-16 16:07_

---

_Comment by @TaKO8Ki on 2025-09-16 16:24_

@ntBre 

> I'm also still interested in deduplicating the files as we traverse the file system instead of sorting and filtering at the end, as I mentioned in https://github.com/astral-sh/ruff/pull/20105#discussion_r2308327043, but if Micha is happy with this then I am too! This is probably the easier approach if it's not too expensive.

I tested the approach and confirmed it seems feasible to implement. However, changing the type of `local_files` would directly affect `python_files_in_path`, which is used by various commands, so the impact area is fairly large. It’s possible to contain the change within `PythonFilesVisitorBuilder`, but doing so would require modifying the current implementation to discard the `ignore::Error` that’s currently passed to `python_files_in_path`. Given the scope, this looks like a significant change—would it be alright if I handle it in a separate follow-up pull request?

---

_Review requested from @ntBre by @TaKO8Ki on 2025-09-17 16:26_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/resolver.rs`:1027 on 2025-09-17 17:18_

I think we could probably drop this test now that we have CLI tests covering the same code.

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:276 on 2025-09-17 17:19_

Could we add an exclusion to one of these to show the `Root` vs `Nested` behavior?

---

_@ntBre reviewed on 2025-09-17 17:23_

Thank you for looking into the other approach! I think this version is probably fine then, no need to follow up as long as it's okay with @MichaReiser too. Avoiding the work of checking the same files multiple times should more than offset the sorting, I would guess.

I just had two more small suggestions about tests, but I think this is good to go otherwise.

---

_@TaKO8Ki reviewed on 2025-09-18 14:28_

---

_Review comment by @TaKO8Ki on `crates/ruff_workspace/src/resolver.rs`:1027 on 2025-09-18 14:28_

I've removed it.

---

_Review comment by @TaKO8Ki on `crates/ruff/tests/lint.rs`:276 on 2025-09-18 14:28_

I've added exclude option to the test case for lint.

---

_@TaKO8Ki reviewed on 2025-09-18 14:28_

---

_Review requested from @ntBre by @TaKO8Ki on 2025-09-18 14:29_

---

_@ntBre approved on 2025-09-19 18:33_

Thank you! I tried out my hash map idea locally, and I think I agree with you that this is a nicer way to go.

I think we could get around the `local_files` type issues (https://github.com/astral-sh/ruff/pull/20105#issuecomment-3299500183) by using some kind of wrapper type (either wrapping a `Vec` or converting to a `Vec` at the end). I was playing with something like this locally:

```rust
struct LocalFiles(Vec<Result<ResolvedFile, ignore::Error>>);
```

and doing the deduplication in `LocalFiles::push`, but the validation is still tricky. I think what you have is good for now. 

---

_Comment by @ntBre on 2025-09-19 18:39_

I updated the summary to close https://github.com/astral-sh/ruff/issues/19395 too!

---

_Renamed from "[`ruff`] Deduplicate input paths" to "Deduplicate input paths" by @ntBre on 2025-09-19 18:40_

---

_Merged by @ntBre on 2025-09-19 18:40_

---

_Closed by @ntBre on 2025-09-19 18:40_

---
