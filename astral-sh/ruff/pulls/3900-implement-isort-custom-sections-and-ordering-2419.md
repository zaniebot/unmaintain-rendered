```yaml
number: 3900
title: "Implement isort custom sections and ordering (#2419)"
type: pull_request
state: merged
author: hackedd
labels: []
assignees: []
merged: true
base: main
head: isort-custom-sections
created_at: 2023-04-06T13:06:13Z
updated_at: 2023-04-13T21:39:10Z
url: https://github.com/astral-sh/ruff/pull/3900
synced_at: 2026-01-12T15:55:14Z
```

# Implement isort custom sections and ordering (#2419)

---

_@hackedd_

Similar to some other people in #2419 I'm currently converting a Django project from flake8 + isort to use ruff. The isort settings currently contain (among other things):

```
known_django = django
sections = FUTURE,STDLIB,THIRDPARTY,DJANGO,FIRSTPARTY,LOCALFOLDER
```

With these changes, I can configure ruff to do the same grouping:

```toml
[tool.ruff.isort]
section-order = ["future", "standard-library", "third-party", "django", "first-party", "local-folder"]

[tool.ruff.isort.sections]
django = ["django"]
```

---

_Comment by @github-actions[bot] on 2023-04-06 13:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.03ms     2.7 MB/sec    1.00     15.1±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.9±1.67µs     7.0 MB/sec    1.00    417.1±1.32µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.01ms     3.9 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.01ms     5.1 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1758.9±2.44µs     9.5 MB/sec    1.00   1744.5±3.35µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.6±0.45µs    16.2 MB/sec    1.00    182.4±0.64µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.4±0.52ms     2.1 MB/sec    1.01     19.6±0.64ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.19ms     3.4 MB/sec    1.01      5.0±0.24ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   608.9±31.17µs     4.8 MB/sec    1.00   605.0±25.46µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.30ms     3.1 MB/sec    1.03      8.5±0.41ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.28ms     4.2 MB/sec    1.04     10.1±0.44ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.8 MB/sec    1.05      2.2±0.10ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    246.9±9.40µs    12.0 MB/sec    1.07   265.3±15.11µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.17ms     5.8 MB/sec    1.05      4.6±0.22ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-04-07 02:26_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/categorize.rs`:317 on 2023-04-11 06:32_

Can a user configure a submodule so that it belongs to multiple sections? If so, what happens in that case? 

Can we change the configuration format/internal representation to make this impossible (e.g. have a `module` -> `ImportSection` mapping)


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/categorize.rs`:289 on 2023-04-11 06:38_

Do the call sites need access to the "full" `ImportSection` with e.g. the custom name? 

If not, then we could introduce a `ImportSectionKind` enum that is copy and has no associated values:

```
#[derive(Debug, Copy, Clone, Eq, PartialEq)]
enum ImportSectionKind {
	UserDefined,
	KnownFirstParty,
	KnownThirdParty,
	KnownLocalFolder,
	KnownStandardLibrary
}

// or 

#[derive(Debug, Copy, Clone, Eq, PartialEq)]
enum ImportSectionKind {
	UserDefined,
	Known(ImportType)
}
```
That would remove the need for `cloning` the `section` (which requires allocating and copying a string)

---

_@MichaReiser reviewed on 2023-04-11 06:38_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/mod.rs`:978 on 2023-04-11 09:06_

You have to use  `assert_messages` after #3906 lands. It pretty prints the diagnostics instead of serializing them to yaml. 

```suggestion
        assert_messages!(snapshot, diagnostics);
```

---

_@MichaReiser reviewed on 2023-04-11 09:06_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/mod.rs`:1010 on 2023-04-11 09:07_

You have to use  `assert_messages` after #3906 lands. It pretty prints the diagnostics instead of serializing them to yaml. 

```suggestion
        assert_messages!(snapshot, diagnostics);
```

---

_@MichaReiser reviewed on 2023-04-11 09:07_

---

_Review comment by @hackedd on `crates/ruff/src/rules/isort/categorize.rs`:317 on 2023-04-12 07:57_

yes, just like there is currently nothing stopping a user from doing something like `extra-standard-library = ["path"]` and `known-first-party = ["path"]`. The first match found 'wins', and is the section that will be used. Configurations like that should probably result in an error or warning. Constructing a mapping sounds like a good idea, it would make this lookup  simpler and makes it easy to detect conflicts.

---

_@hackedd reviewed on 2023-04-12 07:57_

---

_@hackedd reviewed on 2023-04-12 08:03_

---

_Review comment by @hackedd on `crates/ruff/src/rules/isort/categorize.rs`:289 on 2023-04-12 08:03_

The custom name is used during formatting, it determines the order of the sections within the block. It could probably be made to work with a smaller type for the section by mapping section names to IDs when the configuration is read, but I didn't think it would be worth the additional complexity.

---

_Comment by @charliermarsh on 2023-04-13 20:55_

This is really nice work.

---

_Merged by @charliermarsh on 2023-04-13 21:28_

---

_Closed by @charliermarsh on 2023-04-13 21:28_

---
