```yaml
number: 195
title: "add support for inline diagnostic snapshotting for `mdtest`"
type: issue
state: open
author: BurntSushi
labels:
  - help wanted
  - diagnostics
  - testing
  - internal
assignees: []
created_at: 2025-02-19T14:57:16Z
updated_at: 2025-11-18T16:10:21Z
url: https://github.com/astral-sh/ty/issues/195
synced_at: 2026-01-10T02:06:24Z
```

# add support for inline diagnostic snapshotting for `mdtest`

---

_Issue opened by @BurntSushi on 2025-02-19 14:57_

## Summary

Add support for managing inline snapshots of Red Knot diagnostics to mdtests.

## Background

In astral-sh/ruff#15945, we added support for diagnostic snapshotting to [Red Knot's Markdown test framework](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_test/README.md). Basically, the snapshotting support lets you write something like this:

``````
## Unresolvable module import

<!-- snapshot-diagnostics -->

```py
import zqzqzqzqzqzqzq  # error: [unresolved-import] "Cannot resolve import `zqzqzqzqzqzqzq`"
```
``````

And then if you run:

```
$ cargo insta test -p red_knot_python_semantic -- mdtest__
```

the test runner will produce a snapshot of any diagnostics emitted by Red Knot on the Python code. This snapshot is then managed as an _external_ file by [`insta`](https://docs.rs/insta).

This works decently well, and it's better than _not_ having something like this, but it kinda inhibits the locality that is otherwise a huge benefit to mdtests. Instead of the diagnostic output being right there in the Markdown file, it's actually in a different snapshot file.

Now, `insta` does support inline snapshots. For example, in _Rust code_, you can do this:

```rust
insta::assert_snapshot!(
    some_op_that_returns_a_string(),
    @"",
);
```

And when you run `cargo insta test`, it will automatically manage the string on the right side of the assertion, _in your source file_, just like it does for external snapshots.

However, we want inline snapshots inside of Markdown files, which, AFAIK, `insta` does not support.

## Proposed design

### `mdtest` interaction

I don't think [this design](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_test/README.md#asserting-on-full-diagnostic-output) (originally @carljm AIUI) has explicit consensus from everyone on the Red Knot team (although I'm not aware of any specific objections to it), so it might require some iteration.

The idea here is that each mdtest would support an optional `output` code fence block (at most 1). And when present, the contents of that `output` block would be treated as an inline diagnostic snapshot. So the above example might look like this:

``````
## Unresolvable module import

```py
import zqzqzqzqzqzqzq  # error: [unresolved-import] "Cannot resolve import `zqzqzqzqzqzqzq`"
```

```output
error: lint:unresolved-import
 --> /home/andrew/astral/ruff/play/diags/play/test.py:1:8
  |
1 | import zqzqzqzqzqzqzq  # error: [unresolved-import] "Cannot resolve import `zqzqzqzqzqzqzq`"
  |        ^^^^^^^^^^^^^^ Cannot resolve import `zqzqzqzqzqzqzq`
  |
```
``````

In contrast to the mdtest README, I propose starting with just one `output` code fence block that captures the full diagnostic output of _all_ preceding _and_ following Python files. This is how the existing external snapshotting mechanism works. We can consider adding more granular testing in the future, but I think starting with something that just always captures the full diagnostic output is simplest and most useful for now.

## Snapshot management

I think this is probably the hardest part of this task and has the least thought put into it. So this section is pretty vague. But a critical part of snapshot testing is the tooling that surrounds the management of the snapshots. `cargo insta` is a decent model to follow. Everyone here at Astral uses it, and AFAIK, folks generally like it.

Inline snapshots do have a bit of a conceptually simpler model than external snapshots. For example, you don't need to worry about "unreferenced" snapshots (snapshot files that exist but which aren't connected to any actual test). But I believe these are the minimal operations that need to be supported:

* An actual assertion needs to be made to check whether the expected output matches the generated output. Bonus points for making an assertion failure look nice with a pleasant diff.
* When an assertion fails, the snapshot tooling provides a convenient way to update the _expected output_ inside the mdtest file with the output that was generated.

With that said, having to use two different snapshot test tools is undeniably annoying. So I think the gold standard implementation here would be to find a way to make this use case work with `cargo insta` itself. Possibly by working with upstream. (cc @mitsuhiko) I wouldn't be surprised if this wasn't feasible though. My guess is that `cargo insta`'s inline snapshot mechanism is probably tightly coupled with Rust source code (which makes sense).

If we can't make integration with `cargo insta` work, then here is what I propose as a starting point:

* Assertions use the [`similar`](https://docs.rs/similar/) crate (or something like it) to produce nice output when a snapshot test fails.
* An environment variable (e.g., `MDTEST_SNAPSHOT_UPDATE`) specifies how to deal with snapshot test failures. It has two values: `no` and `always`. The default is `no`, which results in a test failure when the expected and generated outputs do not match. When set to `always`, the snapshot in the file is rewritten with whatever the generated output is and the test always passes.

Notably absent from this proposal is any sort of review mechanism. I think this is a _nice to have_. Instead, since these are inline snapshots only, we can treat the output of `cargo insta test -p red_knot_python_semantic -- mdtest_` as our review mechanism. And if we only want to accept a single snapshot, we can do something like, `MDTEST_SNAPSHOT_UPDATE=always cargo insta test -p red_knot_python_semantic -- mdtest__diagnostics_invalid_argument_type`. This workflow is _not_ as nice as `cargo insta` because it doesn't de-couple test execution from snapshot updating. And it also doesn't permit updating individual `output` blocks, since I believe all mdtests in a single file get grouped together into a single Rust `#[test]` execution. So... there are definitely some shortcomings, but I think this might be a good state for an MVP that we can iterate on.

---

_Label `diagnostics` added by @BurntSushi on 2025-02-19 14:57_

---

_Label `help wanted` added by @BurntSushi on 2025-02-19 14:57_

---

_Label `testing` added by @BurntSushi on 2025-02-19 14:57_

---

_Comment by @carljm on 2025-02-24 18:31_

My main concern about inline snapshots is that because the snapshots are so big, I feel they can really interrupt the readability of tests. I look forward to having them but I still hope we use them judiciously. I personally find it very hard to review full diagnostic snapshots without my eyes glazing over, which makes me concerned that issues will slip through.

I would love to have an automatically-updating snapshot feature that can update "inline" error messages in `# error:` comments too, so that the desire for auto-update doesn't lead us to assert on full-rendered-output more often than necessary.

---

_Comment by @BurntSushi on 2025-02-24 18:34_

Yeah I hear that. We could do something like enforce artificial restrictions like, "inline diagnostic snapshots are only allowed in the `resources/mdtest/diagnostics` directory."

---

_Comment by @carljm on 2025-02-24 19:11_

> diagnostic snapshots are only allowed in the `resources/mdtest/diagnostics` directory

Based on the snapshotting usage I've seen so far, I expect we would find this rule too restrictive. Implementing correct semantics and asserting that the diagnostic output is useful and correctly describes the semantics are just closely related, and it often feels desirable to put them in the same test file.

What I think would help most here is what I mentioned, having auto-update support for inline `error: [code] "message"` assertions, not only for full diagnostic output assertions, so that we can choose the right tool for each test without manual update pain being a factor in the choice. But I realize this may add a lot of implementation difficulty, since it's even less likely we could ever use `insta` for this.

---

_Comment by @MichaReiser on 2025-02-24 19:15_

> What I think would help most here is what I mentioned, having auto-update support for inline error: [code] "message" assertions, not only for full diagnostic output assertions, so that we can choose the right tool for each test without manual update pain being a factor in the choice. But I realize this may add a lot of implementation difficulty, since it's even less likely we could ever use insta for this.

I also think that this solves a slightly different problem than what's outlined in this issue summary. 

* inline snapshots: Reduces the distance from snapshots to the expected diagnostic message (this issue)
* auto update inline-assertion messages: Reduce the appeal to use snapshotted diagnostics only to get automated message updates.

---

_Comment by @carljm on 2025-02-24 19:42_

Yes, I agree they are separate issues and solve separate problems, just slightly worried about the potential impact to readability of our mdtests if we implement one without the other.

---

_Comment by @carljm on 2025-02-24 19:45_

Filed astral-sh/ty#192 to separate them.

---

_Renamed from "[red-knot] add support for inline diagnostic snapshotting for `mdtest`" to "add support for inline diagnostic snapshotting for `mdtest`" by @MichaReiser on 2025-05-07 15:26_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:51_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
