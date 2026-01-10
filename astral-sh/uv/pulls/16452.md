```yaml
number: 16452
title: "Added support for [tool.uv.preview-features] in TOML configs (#15767)"
type: pull_request
state: open
author: j-helland
labels: []
assignees: []
base: main
head: jwh/toml-config-preview-features
created_at: 2025-10-26T07:41:57Z
updated_at: 2025-12-23T21:26:12Z
url: https://github.com/astral-sh/uv/pull/16452
synced_at: 2026-01-10T05:49:14Z
```

# Added support for [tool.uv.preview-features] in TOML configs (#15767)

---

_Pull request opened by @j-helland on 2025-10-26 07:41_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Relates to (#15767)


## Summary

Before this PR, specific preview features could only be specified via a comma-separated list in CLI `--preview-features`. Now, preview features can be specified as a list of strings in `pyproject.toml` or `uv.toml`. For example,
```toml
# pyproject.toml
[tool.uv]
preview-features = ["format"]
```

Commandline arguments (`--preview`, `--no-preview`, and `--preview-features`) take precedence over config settings (`tool.uv.preview`, `tool.uv.preview-features`), which matches the semantics of other settings in `uv`.

### Alternatives Considered

From https://github.com/astral-sh/uv/issues/15767#issue-3402381105:

> setting `tool.uv.preview-features.pylock = false` would disable it, taking precedence over the feature potentially being enabled at a user level. If tool.uv.preview were changed to false, all preview features would be disabled, regardless of the content of tool.uv.preview-features.

I chose not to take this approach because it adds complexity and the semantics don't align with `uv` today. For example, `--no-preview` will currently take priority, even if `pyproject.toml` has `uv.tool.preview = true`. I think driving `uv` from the top-down like this is generally the right decision (and changing the semantics for a small feature like this would certainly not be).


## Test Plan

* `cargo nextest run`
* `cargo build`
* New snapshot tests.

In addition to the above, I also tried a small test project:

### `pyproject.toml` test
From the `uv/` project root:
```shell
$ mkdir tmp && cd tmp
$ cargo run -- init

# Warning
$ cargo run -- format 
warning: `uv format` is experimental and may change without warning. Pass `--preview-features format` to disable this warning.
1 file left unchanged

# No warning
$ echo "\n[tool.uv]\npreview-features = [\"format\"]" >> pyproject.toml
$ cargo run -- format
1 file left unchanged

# `uv.toml` takes priority over `pyproject.toml`
$ echo "preview-features = []" >> uv.toml
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The following fields from `[tool.uv]` will be ignored in favor of the `uv.toml` file:
- preview-features
warning: `uv format` is experimental and may change without warning. Pass `--preview-features format` to disable this warning.
1 file left unchanged
```


---

_Comment by @j-helland on 2025-10-26 08:16_

Some docker CI jobs failing, all due to
```
Error: missing API token, please run `depot login`
Error: failed with: Error: missing API token, please run `depot login`
```

I've checked other recent PRs and I'm seeing similar failures ([example](https://github.com/astral-sh/uv/actions/runs/18797165291/job/53638969254)), so I suspect this is a transient issue not related to my changes.

---

_Marked ready for review by @j-helland on 2025-10-26 08:16_

---

_Comment by @j-helland on 2025-10-26 08:17_

This being my first PR in `uv`, I'm especially open to any and all feedback / changes requested (not that I wouldn't be if it was my 500th 🙂). If there's anything I can do to make the review process easier, don't hesitate to let me know!

---

_Comment by @zanieb on 2025-10-26 15:55_

Cool thanks!

Some quick high-level thoughts (I haven't looked at the code yet)

> Passing --preview-features via CLI does NOT combine with uv.tool.preview-features, it overrides it completely. This behavior makes sense to me, otherwise there isn't a clear way to say "No, don't use your config feature X, use Y and Z instead".

I think we should actually combine them. If we get feedback that people need to be able to disable specific features, then the canonical way to do so in uv's interface would be to add a `--no-preview-features <names>` flag.

Don't worry about those Depot authentication failures — we're working with them to resolve those.

---

_Comment by @j-helland on 2025-10-26 16:05_

> I think we should actually combine them. If we get feedback that people need to be able to disable specific features, then the canonical way to do so in uv's interface would be to add a --no-preview-features <names> flag.

Got it, thanks for explaining. That should be an easy fix, I’ll get to that later today.

---

_Comment by @j-helland on 2025-10-27 02:46_

@zanieb I want to clarify one thing before submitting my revision. Suppose we pass `--preview-features pylock` with an underlying config like
```toml
preview = false
preview-features = ["format"]
```
1. Would the final set of features be `format | pylock` or would we prefer to take only `pylock` since `uv.tool.preview = false`? 
2. Similarly, if `tool.uv.preview = true`, should any `--preview-features` merge into that de facto, resulting in all features being enabled?

I would intuitively expect that
1. We just take `pylock` when `tool.uv.preview = false`, ignoring the config features. 
2. Extra `--preview-features` are swallowed by `uv.tool.preview = true`.

---

_Comment by @j-helland on 2025-10-29 02:04_

I pushed what made intuitive sense to me:

> 1. We just take pylock when tool.uv.preview = false, ignoring the config features.
> 2. Extra --preview-features are swallowed by uv.tool.preview = true.

Happy to change this if requested!

---

_Comment by @j-helland on 2025-11-27 20:50_

Nice, no more Depot auth failures.

I'm curious if anyone will get a chance to look at this PR? It would be nice to have this functionality at my current job. I'm sure it's very low-priority, though, so no worries.

If I made any mistakes in my approach that makes this work too time-consuming to review, I'd be happy to take that feedback so I can contribute more productively to `uv` in the future 🙂

---

_Review requested from @zanieb by @konstin on 2025-11-28 08:19_

---

_@laundmo approved on 2025-12-18 12:44_

I'm just a random python/rust dev who was looking for something like this, but to me the code looks reasonable and seems to match how other config options are written.

Would be nice to get this feature, I specifically want to enable `python-upgrade` globally

---

_@zanieb reviewed on 2025-12-18 14:06_

---

_Review comment by @zanieb on `crates/uv-preview/src/lib.rs`:122 on 2025-12-18 14:06_

I think this serializer is wrong per https://github.com/astral-sh/uv/pull/16452/commits/9232ece843a60fd1eb97346f4744e88c314ee1b9

---

_Comment by @zanieb on 2025-12-18 14:08_

> Suppose we pass --preview-features pylock with an underlying config like [...]

I think if we see a config file like

```toml
preview = false
preview-features = ["format"]
```

we should probably just fail?

---

_Comment by @zanieb on 2025-12-18 14:09_

> Similarly, if tool.uv.preview = true, should any --preview-features merge into that de facto, resulting in all features being enabled?

I think I'd expect us just to activate the subset of preview features specified in the CLI since the CLI takes precedence over the configuration file, though I'm not entirely sure and don't feel strongly.

---

_@zanieb reviewed on 2025-12-18 14:11_

---

_Review comment by @zanieb on `crates/uv-preview/src/lib.rs`:181 on 2025-12-18 14:11_

Can you briefly explain why we need this new type?

---

_Comment by @zanieb on 2025-12-18 14:12_

Sorry for the delayed review, we've just had a lot on our backlog — it's not anything you did wrong.

---

_@zanieb reviewed on 2025-12-18 14:24_

---

_Review comment by @zanieb on `crates/uv-preview/src/lib.rs`:181 on 2025-12-18 14:24_

I presume it's for merging purposes?

---

_@zanieb reviewed on 2025-12-18 15:56_

---

_Review comment by @zanieb on `crates/uv-preview/src/lib.rs`:122 on 2025-12-18 15:56_

which fails with

```
        FAIL [   0.700s] uv-preview tests::test_serde_roundtrip
  stdout ───

    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: serde_roundtrip
    Source: crates/uv-preview/src/lib.rs:372
    ────────────────────────────────────────────────────────────────────────────────
    Expression: serialized
    ────────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬───────────────────────────────────────────────────────────────────
        1       │-["python-upgrade","format"]
              1 │+[["python-upgrade"],["format"]]
    ────────────┴───────────────────────────────────────────────────────────────────
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test tests::test_serde_roundtrip ... FAILED
```

---

_@j-helland reviewed on 2025-12-20 02:31_

---

_Review comment by @j-helland on `crates/uv-preview/src/lib.rs`:181 on 2025-12-20 02:31_

Yes, I introduced it for the merging behavior. On further reflection, it could probably be consolidated into the existing `Preview`.

> I think I'd expect us just to activate the subset of preview features specified in the CLI since the CLI takes precedence over the configuration file, though I'm not entirely sure and don't feel strongly.

Your intuition makes sense to me and I think minimizes surprise for the user. I'll update the implementation to align and get rid of `PreviewFeaturesMode`.

We could go a little further here and emit some kind of warning if `--preview-features` overwrites an existing config file, but I'm not sure if that's copacetic?

---

_@j-helland reviewed on 2025-12-20 02:35_

---

_Review comment by @j-helland on `crates/uv-preview/src/lib.rs`:122 on 2025-12-20 02:35_

Great catch, thanks. I'll fix that in my next push.

---

_Comment by @j-helland on 2025-12-20 02:42_

> I think if we see a config file like
> 
> ```toml
> preview = false
> preview-features = ["format"]
> ```
> 
> we should probably just fail?

That makes intuitive sense to me, but doesn't match the current behavior of the commandline arguments, where you can pass `--no-preview --preview-features upgrade-python` without complaint.

Unless I'm missing something (very possible), it seems like implementing the validation you suggest is nontrivial. The closest thing I can find is [validate_uv_toml](https://github.com/astral-sh/uv/blob/main/crates/uv-settings/src/lib.rs#L133), seems like we'd have to add an entirely new validation flow. Does that sound right?

Does it make sense to add that validation in this PR or in a separate effort to keep the scope here limited? I'm open to either option.

---

_Comment by @zanieb on 2025-12-20 04:24_

I think we can be more strict in the configuration file than in the CLI.

> The closest thing I can find is [validate_uv_toml](https://github.com/astral-sh/uv/blob/main/crates/uv-settings/src/lib.rs#L133), seems like we'd have to add an entirely new validation flow.

Ah, hm. That's a little unfortunate. It does seem useful long-term though.

> Does it make sense to add that validation in this PR or in a separate effort to keep the scope here limited? I'm open to either option.

I think the only trouble with doing it in a separate pull request is that we don't want to release support for including both fields then "break" it in a subsequent release. Would you mind sketching it in a separate pull request and we can merge them together?

---

_Comment by @j-helland on 2025-12-20 05:14_

> Would you mind sketching it in a separate pull request and we can merge them together?

Sure thing! I'll try to get to that this weekend.

---

_Comment by @codspeed-hq[bot] on 2025-12-20 05:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/j-helland%3Ajwh%2Ftoml-config-preview-features?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16452 will **not alter performance**

<sub>Comparing <code>j-helland:jwh/toml-config-preview-features</code> (dbe5958) with <code>main</code> (7865672)</sub>



### Summary

`✅ 5` untouched  





---

_Comment by @j-helland on 2025-12-21 02:08_

@zanieb I sketched what you suggested in https://github.com/astral-sh/uv/pull/17202 let me know what you think. 

---

_@j-helland reviewed on 2025-12-23 19:33_

---

_Review comment by @j-helland on `crates/uv-preview/src/lib.rs`:181 on 2025-12-23 19:33_

Marking this as resolved since I removed `PreviewFeaturesMode` while simplifying how CLI args and config settings interact.

---
