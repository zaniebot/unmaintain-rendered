```yaml
number: 261
title: Feature/per file ignores
type: pull_request
state: merged
author: Seamooo
labels: []
assignees: []
merged: true
base: main
head: feature/per-file-ignores
created_at: 2022-09-23T12:17:41Z
updated_at: 2022-09-26T15:15:19Z
url: https://github.com/astral-sh/ruff/pull/261
synced_at: 2026-01-12T05:48:45Z
```

# Feature/per file ignores

---

_Pull request opened by @Seamooo on 2022-09-23 12:17_

resolves #241

This PR implements per-file-ignores as a per-path built filter for the output of `crate::linter::check_path`.
The goal is feature parity.

## Caveats

There is definitely room for optimisation here. Currently filtering the output by removing checks to ignore, after they've been checked leaves the obvious improvement of not executing the check in the first place. This is fairly intrusive on the current approach to the Checker as `settings.selected` is used ubiquitously as the source for code checking.

Additionally, all pairs are matched against each entry to generate the set of ignores to filter against. It's possible to use a better representation of these patterns such that iterating over the each match is possible (although this still has a worst case of O(n)).

Errors for incorrect `pattern:code` pairs currently fail silently in config, and fail with the corresponding anyhow message dumped to stdout. This behaviour is potentially debatable and feedback would be appreciated.

## Description

Per file ignores is available from both the cli via `--per-file-ignores=<stuff>` and in the pyproject.toml under `tool.ruff.per-file-ignores`. Additionally the cache-key is generated using per-file-ignores, as changes to the setting leads to a different output that  couldn't be represented by the previous cache-key generation


---

_@Stranger6667 reviewed on 2022-09-23 12:24_

---

_Review comment by @Stranger6667 on `src/fs.rs`:127 on 2022-09-23 12:24_

nit: Would it be possible to use `.collect()` instead of manually creating `rv`?

---

_Comment by @charliermarsh on 2022-09-23 12:28_

Awesome! Excited to review. Thanks for putting this together.

---

_@charliermarsh reviewed on 2022-09-23 21:30_

---

_Review comment by @charliermarsh on `src/linter.rs`:65 on 2022-09-23 21:30_

Can we omit the entire `into_iter`, `filter`, and `collect` if `ignores` is none / empty?

---

_@charliermarsh reviewed on 2022-09-23 21:32_

---

_Review comment by @charliermarsh on `src/fs.rs`:138 on 2022-09-23 21:32_

I think I'd prefer to just return `Result<BTreeSet<CheckCode>>` and have the client check `.is_empty`. Using `None` to represent the empty state creates ambiguity in the typing (though I know you commented on it above).

---

_Review comment by @charliermarsh on `src/fs.rs`:128 on 2022-09-23 21:34_

Can we try changing `is_excluded` to take `exclude: &[&FilePattern]`, and avoid this clone?

---

_@charliermarsh reviewed on 2022-09-23 21:34_

---

_@charliermarsh reviewed on 2022-09-23 21:34_

---

_Review comment by @charliermarsh on `src/fs.rs`:123 on 2022-09-23 21:34_

Also wondering if this can return `Result<BTreeSet<&CheckCode>>`?

---

_@charliermarsh reviewed on 2022-09-23 21:34_

---

_Review comment by @charliermarsh on `src/fs.rs`:138 on 2022-09-23 21:34_

(That also lets you get rid of `code_set` entirely and just return `rv`.)

---

_@charliermarsh reviewed on 2022-09-23 21:35_

---

_Review comment by @charliermarsh on `src/main.rs`:259 on 2022-09-23 21:35_

I think we should just error hard if they provide a failing pattern or non-existent error code here. That's what we do with ignores and selects. Do you mind rewriting to fail-on-error?

---

_Review comment by @charliermarsh on `src/settings.rs`:164 on 2022-09-23 21:36_

Can we rename `x` to `ignore_string`? (Like the singular of `ignore_strings`.)

---

_@charliermarsh reviewed on 2022-09-23 21:36_

---

_@charliermarsh reviewed on 2022-09-23 21:38_

---

_Review comment by @charliermarsh on `src/fs.rs`:128 on 2022-09-23 21:38_

(Maybe we even want a version of `is_excluded` that takes a single pattern, since passing a slice here is a little awkward, but not a big deal.)

---

_Comment by @charliermarsh on 2022-09-23 21:38_

> Currently filtering the output by removing checks to ignore, after they've been checked leaves the obvious improvement of not executing the check in the first place. This is fairly intrusive on the current approach to the Checker as settings.selected is used ubiquitously as the source for code checking.

Yeah, I think the approach you took is good. We should likely be filtering checks at the end regardless. The fact that we avoid including erroneous checks by checking `Settings` everywhere should be viewed more as an optimization (though the code currently relies on it).

---

_Comment by @charliermarsh on 2022-09-23 21:39_

This is great! Thank you. I left a variety of comments. If you're able to address them, it'd be much appreciated. If you'd rather not, I'd understand and can also take over, adjust, and merge -- totally up to you.


---

_@Seamooo reviewed on 2022-09-24 04:02_

---

_Review comment by @Seamooo on `src/fs.rs`:128 on 2022-09-24 04:02_

changed `is_excluded` to take an iterator of refs, that way a slice ref can call `iter` and a slice of refs can call `into_iter()`

---

_@Seamooo reviewed on 2022-09-24 04:04_

---

_Review comment by @Seamooo on `src/main.rs`:259 on 2022-09-24 04:04_

There now exists a type `crate::pyproject::StrCheckCodePair` with FromStr and Deserialize implemented such that errors can be caught in cli parsing and pyproject deserializing

---

_Comment by @Seamooo on 2022-09-24 07:15_

Resolved the comments, I suspect there may be areas still to clean up for semantic purposes (e.g. struct locations), feel free to edit as needed 

---

_Comment by @charliermarsh on 2022-09-24 14:43_

Awesome, thank you, will review and merge today!

---

_Comment by @charliermarsh on 2022-09-24 14:46_

Once this is merged, I'll cut 0.0.46.

---

_Merged by @charliermarsh on 2022-09-24 17:02_

---

_Closed by @charliermarsh on 2022-09-24 17:02_

---

_Comment by @amotl on 2022-09-26 15:15_

The improvement works excellently. Thanks a stack!

- https://github.com/earthobservations/wetterdienst/pull/729

---
