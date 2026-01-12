```yaml
number: 6144
title: Skip partial duplicates when applying multi-edit fixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/duplicate-fixes
created_at: 2023-07-28T04:18:57Z
updated_at: 2023-07-29T12:27:24Z
url: https://github.com/astral-sh/ruff/pull/6144
synced_at: 2026-01-12T15:55:20Z
```

# Skip partial duplicates when applying multi-edit fixes

---

_@charliermarsh_

## Summary

Right now, if we have two fixes that have an overlapping edit, but not an _identical_ set of edits, they'll conflict, causing us to do another linter traversal. Here, I've enabled the fixer to support partially overlapping edits, which (as an example) let's us greatly reduce the number of iterations required in the test suite.

The most common case here is that in which a bunch of edits need to import some symbol, and then use that symbol, but in different ways. In that case, all edits will have a common fix (to import the symbol), but deviate in some way. With this change, we can do all of those edits in one pass.

Note that the simplest way to enable this was to store sorted edits on `Fix`. We don't allow modifying the edits on `Fix` once it's constructed, so this is an easy change, and allows us to avoid a bunch of clones and traversals later on.

Closes #5800.

---

_@charliermarsh reviewed on 2023-07-28 04:19_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:167 on 2023-07-28 04:19_

I'm disappointed with what's happening here. The ergonomics of it are fine, but it's so inefficient. `inorder` does a full clone, `retain` does a full clone, etc.

---

_@charliermarsh reviewed on 2023-07-28 04:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/test.rs`:88 on 2023-07-28 04:19_

Some payoff.

---

_Review comment by @charliermarsh on `crates/ruff/src/message/diff.rs`:45 on 2023-07-28 04:20_

It does feel good to DRY this up though -- this is now a method on `Edits`.

---

_@charliermarsh reviewed on 2023-07-28 04:20_

---

_Label `internal` added by @charliermarsh on 2023-07-28 04:20_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-07-28 04:34_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-28 04:34_

---

_Comment by @github-actions[bot] on 2023-07-28 04:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.6±0.20ms     4.7 MB/sec    1.00      8.4±0.13ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1662.2±29.19µs    10.0 MB/sec    1.00  1647.3±22.76µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    180.0±3.23µs    16.4 MB/sec    1.01    181.1±4.41µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.1±0.08ms     3.7 MB/sec    1.00     11.1±0.08ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.1±1.62µs     7.7 MB/sec    1.00    383.7±0.58µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.05ms     5.1 MB/sec    1.02      5.1±0.11ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.02ms     6.8 MB/sec    1.00      6.0±0.04ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1251.9±17.78µs    13.3 MB/sec    1.00  1251.7±18.74µs    13.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    136.6±0.18µs    21.6 MB/sec    1.00    135.5±0.96µs    21.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.03ms     9.7 MB/sec    1.01      2.6±0.06ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.6±0.19ms     3.8 MB/sec    1.00     10.5±0.13ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.2 MB/sec    1.00      2.0±0.06ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    218.4±6.69µs    13.5 MB/sec    1.01    221.3±6.65µs    13.3 MB/sec
formatter/pydantic/types.py                1.01      4.4±0.09ms     5.8 MB/sec    1.00      4.4±0.08ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.24ms     2.9 MB/sec    1.00     14.2±0.15ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.04ms     4.6 MB/sec    1.00      3.7±0.06ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    444.7±8.76µs     6.6 MB/sec    1.00   444.3±11.36µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.13ms     4.0 MB/sec    1.00      6.4±0.12ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.11ms     5.2 MB/sec    1.00      7.8±0.11ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1564.1±23.52µs    10.6 MB/sec    1.00  1557.2±29.38µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.1±3.28µs    17.3 MB/sec    1.01    171.9±5.03µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.6 MB/sec    1.01      3.4±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @dhruvmanila on `crates/ruff/src/autofix/mod.rs`:64 on 2023-07-28 05:19_

Do you think it's feasible to create a `Vec<&Edit>` which needs to be applied? i.e., filtering out the `Edit` which has been applied but not in place. This would avoid cloning the `Edit`.

---

_@dhruvmanila approved on 2023-07-28 05:28_

I like the `Edits` interface! :)

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:165 on 2023-07-28 05:38_

Nit: It should be fine to restrict `Edits` to `&'a [Edit]`. It doesn't have to be aware that some usages will use a `Cow` to avoid clones:

```rust
pub struct Edits<'a>(&'a [Edit]);

impl<'a> Edits<'a> {
	pub fn from_slice(edits: &'a [Edit]) -> Self {...}

	pub fn as_slice(&self) -> &[Edit] { self.0 }
}

# usage

let mut plain_edits = Cow::Borrowed(fix.edits().as_slice());

if needs_mutating {
	plain_edits = Cow::Owned(vec![edit]);
}

let edits = Edits::from_slice(&plain_edits);
```


---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:129 on 2023-07-28 05:39_

The methods above (like `min_start`) should now use `self.edits().min_start()` to avoid duplication.

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:184 on 2023-07-28 05:40_

Nit: I would call this method `to_sorted` to clarify that this operation is potentially expensive. 

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:191 on 2023-07-28 05:51_

This eagerly clones the edits even if `f` never returns false. 

We can avoid this by iterating manually and only take the slow path that allocates a new `Vec` if the predicate returns false

```rust
let mut edits = self.0.iter().enumerate();
let last_index = loop {
	match edits.next() {
		Some((index, edit)) => {
			if !f(edit) {
				break index;
			}
		},
		None => {
			return Cow::Borrowed(self.edits);
		}
}

let mut edits = Vec::from_slice(&self.edits[..last_index]);
edits.extend(self.edits[last_index + 1..].iter().filter(f).cloned()); // TODO requires bounds checks. 
Cow::Owned(edits)
```

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:186 on 2023-07-28 05:52_

```suggestion
    /// Retains the [`Edit`] elements in the [`Fix`] for which the predicate `f` returns true
```

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:183 on 2023-07-28 05:54_

Could you use `sort_unstable_by_key`? I mean, it doesn't really make a difference because `sort_unstable_by_key` allocates too, but you would then take benefit of only returning an Iterator

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:180 on 2023-07-28 05:54_


```suggestion
    pub fn inorder(&self) -> impl Iterator<Item = Edit> {
```

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:187 on 2023-07-28 05:59_

I find the `retain` method somewhat confusing. It isn't explicit that it doesn't necessary mutate the elemnt in place and, instead, creates new elements. 

We may even be able to get away by filtering the `edits` instead of calling `retain` (iterates over all edits once), `min` (iterates over all edits once), and `inorder` (iterates over all edits once). 

1. Sort the edits
2. Getting `min` is now for free -> It's the start position of the first edit
3. We can now check adhoc if this specific edit has already been applied, rather than filtering them out previously

---

_@MichaReiser requested changes on 2023-07-28 06:02_

This is an excellent observation. 

I think we can implement this in a way that significantly reduces the time we iterate over edits and removes the in place mutation by

1. Sort the edits of a fix
2. Skip over all edits of that fix that have already been applied
3. Take the start position of the first edit, this is the `min_start` and test if the fix doesn't overlap with another fix
4. Apply the fix(es)

I think this should simplify the implementation 

---

_@charliermarsh reviewed on 2023-07-28 12:38_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:183 on 2023-07-28 12:38_

Are you referring to the methods on `itertools`?

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:129 on 2023-07-28 12:40_

Ah I actually ended to remove that entirely.

---

_@charliermarsh reviewed on 2023-07-28 12:40_

---

_@MichaReiser reviewed on 2023-07-28 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:183 on 2023-07-28 12:42_

Yes, I think that's where they're defined. I should have marked this as a Nit.

---

_@charliermarsh reviewed on 2023-07-28 12:45_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:187 on 2023-07-28 12:45_

I think this is doable... but note that we want the `min` to be the position of the first non-filtered edit. We may still be able to check it ad hoc though.

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:183 on 2023-07-28 12:55_

Just confirming, I had the same instinct but this crate doesn't depend on `itertools` right now and I thought I'd get more heat for adding the dependency :joy:

---

_@charliermarsh reviewed on 2023-07-28 12:55_

---

_@charliermarsh reviewed on 2023-07-28 12:56_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:191 on 2023-07-28 12:56_

Part of what I meant when I commented that "I'm disappointed with what's happening here." :joy:

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-28 18:06_

---

_Comment by @charliermarsh on 2023-07-28 18:06_

This is a bit better could likely be better still.

---

_@charliermarsh reviewed on 2023-07-28 18:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/mod.rs`:87 on 2023-07-28 18:07_

This is kind of confusing (definitely easier to read locally than as a diff), but in short: we want to apply these constraints at the fix level, but if all of the fix's edits were already applied, we don't need to enforce them at all. So we do so lazily, only checking if the location is acceptable when we reach the first edit-to-apply.

---

_Comment by @charliermarsh on 2023-07-28 18:23_

Okay, the latest version stores sorted edits rather than sorting on the fly. I think this is a lot more efficient as we sort once (and iterate through the edits exactly once) and no longer need to do any clones anywhere. `min_start` also becomes O(1).

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:46 on 2023-07-29 08:06_

Nit: sorted by [`Edit::start`] in ascending order

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:144 on 2023-07-29 08:07_

Nit: sorted by [`Edit::start`] in ascending order

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:53 on 2023-07-29 08:33_

Nit: If you want to avoid the labels, but it requires an additional vector: 

```rust
        let mut edits = fix
            .edits()
            .iter()
            .filter(|edit| !applied.contains(edit))
            .peekable();

        // Lazily enforce any isolation and positional requirements (e.g., avoid applying
        // overlapping fixes, but avoid applying this requirement if all fixes in the edit were
        // already applied).
        if let Some(first) = edits.peek() {
            // If this fix requires isolation, and we've already applied another fix in the
            // same isolation group, skip it.
            if let IsolationLevel::Group(id) = fix.isolation() {
                if !isolated.insert(id) {
                    continue;
                }
            }

            // Best-effort approach: if this fix overlaps with a fix we've already applied,
            // skip it.
            if last_pos.map_or(false, |last_pos| last_pos >= first.start()) {
                continue;
            }
        }

        for edit in edits {
            // Add all contents from `last_pos` to `fix.location`.
            let slice = locator.slice(TextRange::new(last_pos.unwrap_or_default(), edit.start()));
            output.push_str(slice);

            // Add the start source marker for the patch.
            source_map.push_start_marker(edit, output.text_len());

            // Add the patch itself.
            output.push_str(edit.content().unwrap_or_default());

            // Add the end source marker for the added patch.
            source_map.push_end_marker(edit, output.text_len());

            // Track that the edit was applied.
            last_pos = Some(edit.end());
            applied_edits.push(edit);
        }

        applied.extend(applied_edits.drain(..));
        *fixed.entry(rule).or_default() += 1;
    }
```

I don't have a preference to be honest. I just thought this is complicated, and then noticed that it is because we read from and write to `applied` at the same time. 

---

_@MichaReiser approved on 2023-07-29 08:34_

---

_@charliermarsh reviewed on 2023-07-29 12:00_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/mod.rs`:53 on 2023-07-29 12:00_

I think what you have here is a big clarity improvement, worth the allocation.

---

_Merged by @charliermarsh on 2023-07-29 12:11_

---

_Closed by @charliermarsh on 2023-07-29 12:11_

---

_Branch deleted on 2023-07-29 12:11_

---
