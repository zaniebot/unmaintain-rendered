```yaml
number: 2268
title: "isort: support forced_separate"
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: isort-separate
created_at: 2023-01-27T17:11:48Z
updated_at: 2023-02-02T13:08:04Z
url: https://github.com/astral-sh/ruff/pull/2268
synced_at: 2026-01-12T04:52:00Z
```

# isort: support forced_separate

---

_Pull request opened by @akx on 2023-01-27 17:11_

Support for [`forced_separate`](https://pycqa.github.io/isort/docs/configuration/options.html#forced-separate):

> Force certain sub modules to show separately


---

_Converted to draft by @akx on 2023-01-27 17:14_

---

_Comment by @scop on 2023-01-27 18:52_

Ah, cool to see you beat me to this. To be able to drop isort in Home Assistant, by chance? ;)

I hadn't thought of this much, but skimming quickly here, noticed one thing I actually had: this implementation appears to treat `forced_separate` as just list of strings, i.e. literal package names. The isort docs aren't that clear about it, but they're actually globs over there: https://github.com/PyCQA/isort/blob/0c8a8b8d12a16bc54a7fe053eaf52fcbf5e15e35/isort/place.py#L39

My vague overall thought was that I'd simply [assemble a `globset::GlobSet`](https://docs.rs/globset/latest/globset/#example-match-multiple-globs-at-once) out of these, then modify the existing code without refactoring, matching across all sections, inserting newlines when matching imports occur (avoiding inserting too many), and be done with it. Don't know if that would have cut it, but thought I'd mention it just in case.

---

_Comment by @akx on 2023-01-28 09:44_

> To be able to drop isort in Home Assistant, by chance? ;)

You guessed it!

> I hadn't thought of this much, but skimming quickly here, noticed one thing I actually had: this implementation appears to treat `forced_separate` as just list of strings, i.e. literal package names.

Yep, afaics nothing in the ruff isort world deals with globs at all, just package basenames - I figured this implementation could be refined if there's a real-world user who needs a glob.

> matching across all sections, inserting newlines when matching imports occur (avoiding inserting too many), and be done with it. Don't know if that would have cut it, but thought I'd mention it just in case.

Yeah, I took a look at the isort source for this, and was quite surprised to see it actually deals with `forced_separate`s as an entirely different block of imports, not separating them within the category sections, so I don't think this would've cut it :)

---

_Marked ready for review by @akx on 2023-01-28 09:44_

---

_@charliermarsh reviewed on 2023-01-28 16:44_

---

_Review comment by @charliermarsh on `src/rules/isort/split.rs`:17 on 2023-01-28 16:44_

Hmm, is this not being called? Can't find the reference. Maybe I'm just tired!

---

_@akx reviewed on 2023-01-28 16:46_

---

_Review comment by @akx on `src/rules/isort/split.rs`:17 on 2023-01-28 16:46_

It's over here -- Github just "helpfully" collapses the big mod.rs diff if you're not looking at each commit separately:

https://github.com/charliermarsh/ruff/pull/2268/files#diff-d896c56e3618229a80f0311bdd754bc1a0c033e5c9a08f99cce34fafeda51b60R139

---

_Review comment by @charliermarsh on `src/rules/isort/split.rs`:38 on 2023-01-28 16:46_

If you had this:

```py
import os
import foo
import bar
import typing
```

And `foo` and `typing` were in `forced_separate`, what's the right behavior?


---

_@charliermarsh reviewed on 2023-01-28 16:46_

---

_@charliermarsh reviewed on 2023-01-28 16:48_

---

_Review comment by @charliermarsh on `src/rules/isort/split.rs`:38 on 2023-01-28 16:48_

As in: what does `isort` do, and are we consistent?

---

_@charliermarsh reviewed on 2023-01-28 16:48_

---

_Review comment by @charliermarsh on `src/rules/isort/split.rs`:17 on 2023-01-28 16:48_

D'oh, thank you...

---

_Review comment by @akx on `src/rules/isort/split.rs`:38 on 2023-01-28 17:05_

Ah, duh... Actually, based on [the test in `isort`](https://github.com/PyCQA/isort/blob/82569d97210a487786cab5dae9e5ce0c0c9af82f/tests/unit/test_isort.py#L1039-L1099) we should partition the blocks _separately_ for each entry in `forced_separate`, and take the order of `forced_separate` into account too.

Back to the drawing board... :)

---

_@akx reviewed on 2023-01-28 17:05_

---

_Converted to draft by @akx on 2023-01-28 17:05_

---

_@akx reviewed on 2023-02-01 11:14_

---

_Review comment by @akx on `src/rules/isort/split.rs`:38 on 2023-02-01 11:14_

Okay, rewrote the splitting to partition into any number of separate blocks based on the order of `forced_separate`.

---

_Marked ready for review by @akx on 2023-02-01 11:50_

---

_@akx reviewed on 2023-02-01 14:42_

---

_Review comment by @akx on `src/rules/isort/split.rs`:55 on 2023-02-01 14:42_

One thing I'm a bit concerned about here is that if more things are added into `ImportBlock`, this will quietly leave them untouched in the `blocks`. Since `forced_separate` isn't a very mainstream configuration, that'd be easy to overlook... Any ideas?

---

_Review requested from @charliermarsh by @akx on 2023-02-01 14:51_

---

_Comment by @charliermarsh on 2023-02-01 15:06_

I'm not sure why the new Windows snapshots are failing -- I saw this in a separate PR. Maybe it's related to the `insta` upgrade in #2185.

---

_@charliermarsh reviewed on 2023-02-02 01:08_

---

_Review comment by @charliermarsh on `src/rules/isort/mod.rs`:144 on 2023-02-02 01:08_

How does this work in the event that we have a forced-separate entry that doesn't end up matching any imports? I think that would return a `Block::default()` corresponding to entry. Do we need to skip empty blocks? Or do they get materialized to the empty string?

---

_@charliermarsh reviewed on 2023-02-02 01:09_

---

_Review comment by @charliermarsh on `src/rules/isort/split.rs`:30 on 2023-02-02 01:09_

Could you destructure `block`? Like:

```rust
let Block { import, import_from, import_from_as, import_from_star } = block;
```

Then the destructure will fail if we add a new property.

---

_@akx reviewed on 2023-02-02 09:20_

---

_Review comment by @akx on `src/rules/isort/mod.rs`:144 on 2023-02-02 09:20_

They get materialized to the empty string – added a configuration to the test that verifies this 👍 

---

_Merged by @charliermarsh on 2023-02-02 13:08_

---

_Closed by @charliermarsh on 2023-02-02 13:08_

---
