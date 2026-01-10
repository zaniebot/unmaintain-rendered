```yaml
number: 9370
title: add conflict markers to the lock file
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/i9289
created_at: 2024-11-22T20:38:12Z
updated_at: 2024-12-10T15:57:57Z
url: https://github.com/astral-sh/uv/pull/9370
synced_at: 2026-01-10T12:00:00Z
```

# add conflict markers to the lock file

---

_Pull request opened by @BurntSushi on 2024-11-22 20:38_

This PR adds a notion of "conflict markers" to the lock file as an
attempt to address #9289. The idea is to encode a new kind of boolean
expression indicating how to choose dependencies based on which extras
are activated.

As an example of what conflict markers look like, consider one of the cases
brought up in #9289, where `anyio` had unconditional dependencies on
two different versions of `idna`. Now, those are gated by markers, like this:

```toml
        [[package]]
        name = "anyio"
        version = "4.3.0"
        source = { registry = "https://pypi.org/simple" }
        dependencies = [
            { name = "idna", version = "3.5", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-7-project-foo'" },
            { name = "idna", version = "3.6", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-7-project-bar' or extra != 'extra-7-project-foo'" },
            { name = "sniffio" },
        ]
```

The odd extra values like `extra-7-project-foo` are an encoding of not just the conflicting extra (`foo`) but also the package it's declared for (`project`). We need both bits of information because different packages may have the same extra name, even if they are completely unrelated. The `extra-` part is a prefix to distinguish it from groups (which, in this case, would be encoded as `group-7-project-foo` if `foo` were a dependency group). And the `7` part indicates the length of the package name which makes it possible to parse out the package and extra name from this encoding. (We don't actually utilize that property, but it seems like good sense to do it in case we do need to extra information from these markers.)

While this preserves PEP 508 compatibility at a surface level, it does require utilizing this encoding scheme in order
to evaluate them when they're present (which only occurs when conflicting extras/groups are declared).

My sense is that the most complex part of this change is not just adding conflict markers, but their simplification. I tried to address this in the code comments and commit messages.

Reviewers should look at this commit-by-commit.

Fixes #9289, Fixes #9546, Fixes #9640, Fixes #9622, Fixes #9498, Fixes #9701, Fixes #9734


---

_Comment by @BurntSushi on 2024-11-22 20:41_

The relevant change to the lock file from https://github.com/astral-sh/uv/issues/9289#issuecomment-2489340846 is here:

```toml
[[package]]
name = "anyio"
version = "4.6.2.post1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "idna", version = "3.9", source = { registry = "https://pypi.org/simple" }, conflict-marker = "extra == 'bar' or extra != 'foo'" },
    { name = "idna", version = "3.10", source = { registry = "https://pypi.org/simple" }, conflict-marker = "extra == 'foo'" },
    { name = "sniffio" },
]
```

Notice the new `conflict-marker` field. It doesn't appear on any other entry in the lock file. (That was one of the hardest parts of this PR.)

---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-22 20:41_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:3870 on 2024-11-22 22:13_

So these _are_ going to show up in the lockfile? In that case, I remain somewhat biased towards using `group =` rather than encoding the names in the string value.

---

_@charliermarsh reviewed on 2024-11-22 22:13_

---

_@charliermarsh reviewed on 2024-11-22 22:15_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19625 on 2024-11-22 22:15_

Is this a bug, perhaps? Why write it when it's empty?

---

_@charliermarsh reviewed on 2024-11-22 22:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/graph_ops.rs`:91 on 2024-11-22 22:19_

I think I need to see an example of this.

---

_@BurntSushi reviewed on 2024-11-22 23:15_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:19625 on 2024-11-22 23:15_

I think writing it when it's empty is probably an extant bug, perhaps exacerbated by conflicting extras. (Or maybe even only made possible by it.) I can look into it.

But as for deleting the markers here, I believe it's correct. I wrote up an explanation for it in the commit message.

---

_@BurntSushi reviewed on 2024-11-22 23:17_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:3870 on 2024-11-22 23:17_

Encoding the names in the string value would be an internal detail _not_ exposed to the lock file. It would be written like it is here, and when groups are added, it would be `group != 'foobar'`.

(My three choices given on Discord all have the same behavior with respect to the lock file and how the conflict markers are written. The only differences between them are how we implement it.)

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:3870 on 2024-11-22 23:24_

Oh ok, ack.

---

_@charliermarsh reviewed on 2024-11-22 23:24_

---

_Comment by @BurntSushi on 2024-11-23 01:52_

OK, this PR should actually be ready for some very rough testing, but still _only_ with extras, not groups. At the very least, this PR does currently fix #9289:

```
[andrew@duff mre]$ run-uv pr1 -- sync
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Using CPython 3.11.9
Creating virtual environment at: .venv
Resolved 5 packages in 10ms
Audited in 0.01ms
[andrew@duff mre]$ run-uv pr1 -- sync --extra baz
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Resolved 5 packages in 2ms
Installed 3 packages in 1ms
 + anyio==4.6.2.post1
 + idna==3.9
 + sniffio==1.3.1
[andrew@duff mre]$ run-uv pr1 -- sync --extra baz --extra bar
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Resolved 5 packages in 2ms
Audited 3 packages in 0.02ms
[andrew@duff mre]$ run-uv pr1 -- sync --extra baz --extra foo
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Resolved 5 packages in 2ms
Uninstalled 1 package in 0.62ms
Installed 1 package in 0.85ms
 - idna==3.9
 + idna==3.10
[andrew@duff mre]$ run-uv pr1 -- sync --extra bar
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Resolved 5 packages in 2ms
Uninstalled 3 packages in 0.91ms
Installed 1 package in 0.83ms
 - anyio==4.6.2.post1
 - idna==3.10
 + idna==3.9
 - sniffio==1.3.1
[andrew@duff mre]$ run-uv pr1 -- sync --extra foo
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
Resolved 5 packages in 2ms
Uninstalled 1 package in 0.61ms
Installed 1 package in 0.86ms
 - idna==3.9
 + idna==3.10
```

If we can get some confidence in this technique with extras, then I'm pretty sure I know how to extend it to groups.

I think the main thing I'm worried about right now is that the logic in this PR might be treating extras as a global resource. So if your top-level package `foo` has an extra `x1` and some random transitive dependency `bar` also has an extra named `x1` that _isn't_ enabled (directly or indirectly by enabling `foo[x1]`), then I _think_ my current logic will falsely assume that `bar[x1]` is active. So that could result in following dependency edges that aren't actually activated.

I believe this can be resolved by enhancing conflict markers to include package names instead of just extra and group names. But this also doesn't feel right to me, for reasons that I'm finding difficult to articulate. Another hiccup here is `ExtrasSpecification` and `DevGroupsManifest`. As far as I can tell, I am likely misusing it (in the last commit in this PR). It seems like it is only intended to be used with the root package, but in this PR, I am using it for any arbitrary dependency to check which extras are active or not. So there are probably still some knots here to untangle, but I think it's probably an evolution on what I've done so far.

---

_Comment by @BurntSushi on 2024-11-23 13:45_

OK, I think I found a case where treating extras as more "global" than they ought to be leads to incorrect behavior. 

Consider this `pyproject.toml`:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11,<3.12"
dependencies = [
  "proxy1",
  "anyio>=4",
]

[tool.uv.workspace]
members = ["proxy1"]

[project.optional-dependencies]
x1 = ["idna==3.9"]

[tool.uv.sources]
proxy1 = { workspace = true }

[tool.uv]
conflicts = [
  [
    {package = "project", extra = "x1"},
    {package = "proxy1", extra = "x2"},
    {package = "proxy1", extra = "x3"},
  ],
]
```

And this at `proxy1/pyproject.toml`:

```toml
[project]
name = "proxy1"
version = "0.1.0"
requires-python = ">=3.11,<3.12"
dependencies = []

[project.optional-dependencies]
x2 = ["idna==3.8"]
x3 = ["idna==3.7"]
```

And now do a `sync`:

```
$ rm -rf .venv uv.lock && run-uv pr1 -- sync
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Using CPython 3.11.9
Creating virtual environment at: .venv
Resolved 7 packages in 13ms
Installed 3 packages in 2ms
 + anyio==4.6.2.post1
 + idna==3.7
 + sniffio==1.3.1
```

and notice that `idna==3.7` gets installed. I think it would be correct for _either_ `idna==3.7` or `idna==3.8` to be installed here, but the point is to notice the version that gets picked. Now let's watch what happens when we enable extra `x2`:

```
$ rm -rf .venv uv.lock && run-uv pr1 -- sync --extra x2
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
Using CPython 3.11.9
Creating virtual environment at: .venv
Resolved 7 packages in 54ms
Installed 3 packages in 1ms
 + anyio==4.6.2.post1
 + idna==3.8
 + sniffio==1.3.1
```

In this case, `idna==3.8` gets installed, because that's the dependency attached to the `x2` extra in the `proxy1` package. But, crucially, notice that the top-level package does _not_ have an extra named `x2`. So I believe `--extra x2` should have no effect whatsoever. So the fact that `--extra x2` influences which package is installed here seems wrong. I believe it should have no effect.

So I guess our conflict markers need to include not just the extra/group name, but the package activating it. I think that would solve this.

---

_Comment by @charliermarsh on 2024-11-23 13:55_

Nice.

---

_Label `internal` added by @BurntSushi on 2024-11-23 15:58_

---

_Comment by @BurntSushi on 2024-11-26 20:05_

OK, I _finally_ have #9289 fixed in this PR (including all examples in that issue and some other examples that I came up with). I do not believe there are any remaining _known_ cases of ambiguity for extras (I haven't handled groups yet), so if anyone is willing to try and "break" this PR with just conflicting extras, that would be great.

This PR is not in a mergeable state. So my next focus is to get it into one.

One major change in this PR is that, while the markers being written to the lock file are still PEP 508 compatible, they may now include some very odd `extra` values. I haven't fully fleshed it out in this PR yet, but they will ultimately look something like `platform_system == 'Darwin' and (extra == 'extra-0-7-8-11-pytorch-cpu' or extra == 'group-0-7-8-11-pytorch-wat'`. The reasoning for this (and the precise format shown here) is pretty complicated. There are other points in the design space, but I believe they all require abdicating PEP 508 for markers in the lock file. The high level reasoning here is that some edges in the lock file, when conflicting extras/groups are specified, must be _conditionally_ walked at installation time depending on which extras/groups are activated. And that conditional logic can actually vary depending on platform, which means that the conditional logic for extras/groups has to be married to the conditional logic for the current platform.

---

_Marked ready for review by @BurntSushi on 2024-12-06 17:13_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-12-06 17:14_

---

_Review requested from @konstin by @BurntSushi on 2024-12-06 17:14_

---

_@BurntSushi reviewed on 2024-12-06 17:41_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:3870 on 2024-12-06 17:41_

To follow up here: when I wrote the above comment, I was still thinking we'd have "normal PEP 508 marker" and "conflict marker" as separate things in the lock file. And in that scenario, it seemed more justifiable to have our own little custom syntax for the conflict marker that we mapped back to PEP 508 markers _internally_. But since then, I've come to understand that these markers need to be combined since the conflict markers themselves can be dependent on the PEP 508 markers.

I didn't want to make the markers no longer parseable by PEP 508... Maybe that's something we should do in the future, but I felt it was good not to break that property at least for now. But that does mean that we end up with some funny looking markers that I was originally trying to avoid. (The comments and commit messages give more detail.)

---

_Renamed from "WIP for adding conflict markers to the lock file" to "add conflict markers to the lock file" by @BurntSushi on 2024-12-06 17:50_

---

_Label `bug` added by @BurntSushi on 2024-12-06 17:50_

---

_Label `lock` added by @BurntSushi on 2024-12-06 17:50_

---

_Label `internal` removed by @BurntSushi on 2024-12-06 17:50_

---

_@charliermarsh reviewed on 2024-12-06 18:26_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/target.rs`:200 on 2024-12-06 18:26_

Is it possible to make these unowned?

---

_@charliermarsh reviewed on 2024-12-06 18:27_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/target.rs`:220 on 2024-12-06 18:27_

My initial instinct is... should the group and extras not somehow be pushed onto `queue`? (I guess they're "indexed" by package name? It just seems vaguely off for them to be tracked as "global" state in the algorithm like this.)

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/universal_marker.rs`:28 on 2024-12-06 18:29_

"have different a different"

---

_@charliermarsh reviewed on 2024-12-06 18:29_

---

_@charliermarsh reviewed on 2024-12-06 18:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/universal_marker.rs`:48 on 2024-12-06 18:30_

What is "3" here?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-06 18:35_

Ohh ok. I see now. It's sort of unfortunate that this representation is "user-facing" in the lockfile.

---

_@charliermarsh reviewed on 2024-12-06 18:35_

---

_@charliermarsh reviewed on 2024-12-06 18:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-06 18:40_

I don't know that I have a better answer. We could do something that's legal but not normalized...? Like use dashes for one piece?

---

_@charliermarsh approved on 2024-12-06 18:40_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:48 on 2024-12-06 18:59_

I thought I explained this somewhere, but it might have only been in a commit message. So I added a code comment here too:

```rust
    /// Why `extra-3-foo-x1`?
    ///
    /// * The `extra` prefix is there to distinguish it from `group`.
    /// * The `3` is there to indicate the length of the package name,
    /// in bytes. This isn't strictly necessary for encoding, but is required
    /// if we were ever to need to decode a package and extra/group name from a
    /// conflict marker.
    /// * The `foo` package name ensures we namespace the extra/group name,
    /// since multiple packages can have the same extra/group name.
```

Does that help?

---

_@BurntSushi reviewed on 2024-12-06 18:59_

---

_@BurntSushi reviewed on 2024-12-06 19:04_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/target.rs`:220 on 2024-12-06 19:04_

I am pretty sure you need them to be global. A conflict marker might be something like `extra == 'foo' or extra != 'bar'`, for example. So you really do need to know the full set of activated extras/groups.

I _think_ part of the confusion here is that while conflict markers are using `extra`, and conflicts can be declared on extras, these don't really resemble anything like what the PEP 508 `extra` stands for. We're basically just abusing it to encode the conflict marker semantics into PEP 508 markers. So the standard practice for how we previously used markers for extras doesn't really apply to conflict markers AIUI.

---

_@BurntSushi reviewed on 2024-12-06 19:08_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/target.rs`:200 on 2024-12-06 19:08_

It's a little awkward, but yes, I'll do that. I didn't do it originally because these are what I call "marginal" allocations. That is, the operation being done on them will still do `O(n)` allocs even if these are eliminated. Which in turn means that eliminating them is usually "marginal," if that makes sense. But constant factors can still matter, so it's a balancing act.

---

_@BurntSushi reviewed on 2024-12-06 19:10_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-06 19:10_

Can you elaborate?

---

_@BurntSushi reviewed on 2024-12-06 19:11_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-06 19:11_

And yes, I 100% agree that this is unfortunate. I just don't know of a better way to go about it without inventing our own marker system.

---

_Comment by @BurntSushi on 2024-12-06 19:12_

@konstin I'm going to hold off merging this until you're able to review it since this is making user visible changes to the lock file. I want to give folks a chance to give input before this gets merged, because once this goes out to end users, making changes at that point will become more annoying (or provoke churn).

---

_Comment by @NellyWhads on 2024-12-07 03:04_

Confirmed to resolve #9701 as well.

---

_Review comment by @konstin on `crates/uv/tests/it/lock_conflict.rs`:4674 on 2024-12-09 11:48_

The torch indices are missing the upload date

---

_Review comment by @konstin on `crates/uv-resolver/src/graph_ops.rs`:145 on 2024-12-09 13:11_

Aren't the propagated value set order dependent? Below, the green and the red items from A and C are propagated through, but B -> D happens before E -> D, so B -> D isn't propagated. Numbers indicate the order of edge traversals.

![PXL_20241209_130608932~2](https://github.com/user-attachments/assets/253196db-d21d-41a3-9ae0-c4f1dbab97e4)

---

_Review comment by @konstin on `crates/uv-resolver/src/graph_ops.rs`:145 on 2024-12-09 13:32_

Would it be easier if we started at the conflicting nodes and propagated knowledge about the "side" of the conflict through from them? E.g.

```
[tool.uv]
conflicts = [
    [
      { extra = "extra1" },
      { extra = "extra2" },
    ],
]
```

we could start at `project[extra1]` and `project[extra2]` respectively, mark every node with `(project, extra1)` or `(project, extra2)` respectively and then simplify the outgoing edges of all nodes that only have either marker. This might make the world-knowledge easier to move in? (Caveat: That's from reading code only)

---

_Review comment by @konstin on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-09 14:11_

Are we bound to PEP 508, or could we do something like adding a sigil that only allowed here and in the lockfile but gets rejected everywhere else, or even as a specific new marker type? I see the problem with the MarkerTree being nested and such, but this looks like a potential collision and it's user facing

---

_@konstin reviewed on 2024-12-09 14:15_

We should fix the graph traversal order (if my example is correct), other looks good.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-09 14:34_

> but this looks like a potential collision and it's user facing

I find this very difficult to reason about. On the one hand, it is certainly true that someone could use `extra-3-pkg-foo` as an extra name, and thus potentially collide with a separately defined extra named `foo` for the package `pkg`. (I think such cases would be pathological given the baroqueness of `extra-3-pkg-foo`. You not only need an extra with that name but it _also_ has to be declared as a conflicting extra.) On the other hand, even if a collision does occur, it's actually not totally clear to me that it would cause any problems. That's because we tend to strip the "normal" `extra == 'foo'` from markers at various points.

> Are we bound to PEP 508, or could we do something like adding a sigil that only allowed here and in the lockfile but gets rejected everywhere else, or even as a specific new marker type?

A new marker type is possible (I think I suggested a new marker type or even something that isn't a marker somewhere), but I believe it's costly in terms of implementation effort. And even if we do have a new marker type, there is enough inter-operation going on that we'd need to be able to convert between the marker types. I think it's doable, but yeah, a fair bit of work.

Some extra sigil that isn't typically allowed in extra names is also possible... I think it's probably less work than adding a new marker type. And the [implementation kinda-sorta already makes room for it](https://github.com/astral-sh/uv/blob/e9d545e4a6257337b399cfa2f87d88018f42c174/crates/uv-pep508/src/marker/tree.rs#L421-L428). There's definitely some refactoring cost involved here because we wouldn't be able to use `ExtraName` or `GroupName`. So that means new APIs for evaluating markers with something looser than `&[ExtraName]`. And new parsing APIs to "allow" the sigil in certain cases but continue to reject it in others. Overall possible, but I'd say it's at least a couple days of work.

Since I think that, in order for a collision to occur, it has to be pathological, I'm not sure it's worth doing. One thing I can do as a mitigation is add a test that specifically provokes a collision and ensure behavior is reasonable.

---

_@BurntSushi reviewed on 2024-12-09 14:34_

---

_@BurntSushi reviewed on 2024-12-09 14:35_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-09 14:35_

> Are we bound to PEP 508

I think we are bound to PEP 508 insomuch as we want to be. While these conflict markers are _technically_ valid PEP 508 markers, I think they are definitely a step toward PEP 508 incompatibility since they require an encoding scheme to use properly.

---

_@BurntSushi reviewed on 2024-12-09 15:35_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:458 on 2024-12-09 15:35_

I've added a test that instigates a collision and things seem to be okay:

https://github.com/astral-sh/uv/blob/cc1d8e62ef3d7d84bc687bc5d4ca219d16b2b5ed/crates/uv/tests/it/lock_conflict.rs#L4795-L5015

I think it's because we don't tend to have both `extra == "foo"` and `extra == "pkg-3-foo-extra"` in the same marker at the same time. (And I use the weasel words "tend to" because I find it _very_ hard to reason about this.)

---

_Review requested from @konstin by @BurntSushi on 2024-12-10 15:08_

---

_@BurntSushi reviewed on 2024-12-10 15:11_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/graph_ops.rs`:145 on 2024-12-10 15:11_

This should be fixed in https://github.com/astral-sh/uv/pull/9370/commits/8a5ce95134a4daa497c732f751572dfcc3532283

I opted to fix this by revisiting nodes when the set of extras gets updated.

---

_Comment by @konstin on 2024-12-10 15:51_

We probably want to replace `extra_inferences` with something slimmer in the future, but it's no so bad that's it's blocking this.

---

_@konstin approved on 2024-12-10 15:51_

---

_Merged by @BurntSushi on 2024-12-10 15:57_

---

_Closed by @BurntSushi on 2024-12-10 15:57_

---

_Branch deleted on 2024-12-10 15:57_

---

_Comment by @BurntSushi on 2024-12-10 15:57_

Note: I squashed and merged this PR because the commits were more optimized for review and didn't individually each pass tests.

---
