```yaml
number: 8104
title: B006 (mutable-argument-default) no longer offers autofix even with --unsafe-fixes
type: issue
state: closed
author: cjolowicz
labels:
  - fixes
assignees: []
created_at: 2023-10-21T13:08:39Z
updated_at: 2023-10-21T19:29:47Z
url: https://github.com/astral-sh/ruff/issues/8104
synced_at: 2026-01-12T15:54:47Z
```

# B006 (mutable-argument-default) no longer offers autofix even with --unsafe-fixes

---

_@cjolowicz_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello 👋 

B006 (`mutable-argument-default`) no longer offers an autofix even when `--unsafe-fixes` is passed.

Use the example from `ruff rule B006` to reproduce this:

```python
# /tmp/bad.py
def add_to_list(item, some_list=[]):
    some_list.append(item)
    return some_list
```

At HEAD (df807ff91248c896cf7f11b25e4f64ecb1d77066) and v0.1.1:

```sh
❯ cargo run -p ruff_cli -- check --select B --unsafe-fixes /tmp/bad.py --no-cache
/tmp/bad.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
```

I've bisected this to 22e18741bdf58bc15a69da2503d7b48443e5a038, but that may only be where the issue was surfaced in the CLI.

The output is slightly different in that commit, there's an additional line:

```sh
/tmp/bad.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
[*] 0 fixable with the --fix option.
```

If I omit `--unsafe-fixes` there's a hint about a hidden fix:

```sh
❯ cargo run -p ruff_cli -- check --select B /tmp/bad2.py --no-cache
/tmp/bad.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
1 hidden fix can be enabled with the `--unsafe-fixes` option.
```

(I don't see this hint in 0.1.1 or HEAD, but I haven't bisected where it went away.)

In the parent of the bisected commit and in v0.0.292, I get the expected output:

```sh
/tmp/bad.py:1:33: B006 [*] Do not use mutable data structures for argument defaults
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

(I noticed after the fact that I ran Ruff from its repository which has Ruff config in pyproject.toml, so that could have skewed results a little. But the repro should still be valid.)

---

_Comment by @cjolowicz on 2023-10-21 14:05_

FWIW the fix is deemed unapplicable in FixableStatistics::try_from

https://github.com/astral-sh/ruff/blob/df807ff91248c896cf7f11b25e4f64ecb1d77066/crates/ruff_cli/src/printer.rs#L521

Here's what the diagnostics look like:

```rust
Diagnostics {
    messages: [
        Message {
            kind: DiagnosticKind {
                name: "MutableArgumentDefault",
                body: "Do not use mutable data structures for argument defaults",
                suggestion: Some("Replace with `None`; initialize within function")
            },
            range: 32..34,
            fix: Some(Fix {
                edits: [Edit { range: 32..34, content: Some("None") },
                        Edit { range: 37..37, content: Some("    if some_list is None:\n        some_list = []\n") }],
                applicability: Display,
                isolation_level: NonOverlapping
            }),
            file: SourceFile {
                name: "/tmp/bad.py",
                code: "def add_to_list(item, some_list=[]):\n    some_list.append(item)\n    return some_list\n" },
            noqa_offset: 32 }],
    fixed: FixMap({}),
    imports: ImportMap { module_to_imports: {} },
    notebook_indexes: {}
}
```

---

_Comment by @cjolowicz on 2023-10-21 14:23_

I think I understand what's going on now.

#6131 added the fix as a "manual edit", a concept which later became `applicability: Display`. This means that the fix should only be displayed to the user but never applied by Ruff. Before 22e18741bdf58bc15a69da2503d7b48443e5a038 (the bisected commit), applicability levels weren't respected, which explains why the fix was available before 0.1.0 and then went away.

So I suppose this issue is a feature request, not a bug:

- Can we move the fix from display to unsafe (or even safe)?

I haven't found discussion of this (but may have missed it).

---

_Comment by @cjolowicz on 2023-10-21 14:28_

For context, I see three rules with _display edits_ in the codebase, which are likely all affected by the change:

- mutable-argument-default (flake8-bugbear)
- lambda-assignment (pycodestyle)
- commented-out-code (eradicate)

These rules are all marked as fixable in the docs, which is prone to create confusion? (Did for me.)

---

_Comment by @charliermarsh on 2023-10-21 15:01_

I support upgrading this to unsafe -- we could probably increase all of those to unsafe now that it's supported on the CLI.


---

_Comment by @charliermarsh on 2023-10-21 15:01_

(But also, we probably shouldn't mark "display" rules as fixable in the docs.)

---

_Comment by @zanieb on 2023-10-21 15:10_

> (But also, we probably shouldn't mark "display" rules as fixable in the docs.)

We do not know applicability levels of fixes when generating the documentation. Perhaps `FixAvailability` needs a maximum applicability field.

I'll need to look at those specific rules but I presume they were categorized as display-only for good reason?

Note after #7352 these fixes will be displayed instead of entirely hidden.

---

_Comment by @cjolowicz on 2023-10-21 15:43_

I opened #8108 to promote `mutable-argument-default` to unsafe.

> I'll need to look at those specific rules but I presume they were categorized as display-only for good reason?

I found some discussion of this, after all:

- https://github.com/astral-sh/ruff/pull/4880#discussion_r1257863238
- https://github.com/astral-sh/ruff/pull/4880#discussion_r1265110215

To my understanding, the rule was left in manual because of an edge case where the newline between the function signature and the body is escaped with a backslash (second link above). But it seems like there was some question whether this edge case justified making the fix display-only.

cc @qdegraaf @MichaReiser 

(As a reminder for myself, the reason the fix isn't safe is because mutable argument defaults can be used intentionally, e.g. for caching.)

---

_Comment by @zanieb on 2023-10-21 16:07_

It looks like it was marked as display/manual because it could result in invalid syntax if a continuation is used at the end of a function definition? Seems like a weird enough edge-case to just be unsafe.

---

_Label `autofix` added by @zanieb on 2023-10-21 16:07_

---

_Closed by @charliermarsh on 2023-10-21 19:29_

---
