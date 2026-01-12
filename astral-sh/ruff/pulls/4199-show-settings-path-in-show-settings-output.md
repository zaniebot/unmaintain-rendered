```yaml
number: 4199
title: "Show settings path in `--show-settings` output"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: feat/show-settings-path
created_at: 2023-05-03T03:33:49Z
updated_at: 2023-05-04T14:19:58Z
url: https://github.com/astral-sh/ruff/pull/4199
synced_at: 2026-01-12T15:55:14Z
```

# Show settings path in `--show-settings` output

---

_@dhruvmanila_

## Summary

Create a new struct called `PyprojectConfig` which holds all the `pyproject.toml` configuration related information. This includes the discovery strategy used (`Fixed` or `Hierarchical`), the settings itself and absolute path to the `pyproject.toml` file.

Using `PyprojectConfig::new` will try to resolve the given config path to absolute.

## Test Plan

***Project structure:***

```
.
├── pyproject.toml
├── src
│   └── ...
└── test
    ├── foo
    │   └── __init__.py
    └── pyproject.toml
```

<details><summary><b>Using `--isolated` (no config path):</b></summary>
<p>

```console
$ ~/contributing/astral/ruff/target/debug/ruff check --show-settings --isolated . | head -n8  
Resolved settings for: "/Users/dhruv/playground/python/ruff-play/src/B031.py"
Settings {
    rules: RuleTable {
        enabled: {
            MixedSpacesAndTabs,
            MultipleImportsOnOneLine,
            ModuleImportNotAtTopOfFile,
            LineTooLong,
```

</p>
</details> 

<details><summary><b>Using <code>--config pyproject.toml</code></b></summary>
<p>

```console
$ ~/contributing/astral/ruff/target/debug/ruff check --show-settings --config=pyproject.toml . | head -n8
Resolved settings for: "/Users/dhruv/playground/python/ruff-play/src/B031.py"
Settings path: "/Users/dhruv/playground/python/ruff-play/pyproject.toml"
Settings {
    rules: RuleTable {
        enabled: {
            UnnecessaryCallAroundSorted,
            UnnecessaryCollectionCall,
            UnnecessaryComprehension,
```

</p>
</details> 

<details><summary><b>Using <code>--config ../pyproject.toml</code></b></summary>
<p>

```console
$ ~/contributing/astral/ruff/target/debug/ruff check --show-settings --config=../pyproject.toml . | head -n8 
Resolved settings for: "/Users/dhruv/playground/python/ruff-play/test/foo/__init__.py"
Settings path: "/Users/dhruv/playground/python/ruff-play/pyproject.toml"
Settings {
    rules: RuleTable {
        enabled: {
            UnnecessaryCallAroundSorted,
            UnnecessaryCollectionCall,
            UnnecessaryComprehension,
```

</p>
</details> 

<details><summary><b>Using hierarchical strategy to find config file</b></summary>
<p>

```console
$ ~/contributing/astral/ruff/target/debug/ruff check --show-settings . | head -n8          
Resolved settings for: "/Users/dhruv/playground/python/ruff-play/src/B031.py"
Settings path: "/Users/dhruv/playground/python/ruff-play/pyproject.toml"
Settings {
    rules: RuleTable {
        enabled: {
            UnnecessaryCallAroundSorted,
            UnnecessaryCollectionCall,
            UnnecessaryComprehension,
```

</p>
</details> 

<details><summary><b>No config file (commented out <code>[tool.ruff]</code> section)</b></summary>
<p>

```console
$ ~/contributing/astral/ruff/target/debug/ruff check --show-settings . | head -n8  
Resolved settings for: "/Users/dhruv/playground/python/ruff-play/src/B031.py"
Settings {
    rules: RuleTable {
        enabled: {
            MixedSpacesAndTabs,
            MultipleImportsOnOneLine,
            ModuleImportNotAtTopOfFile,
            LineTooLong,
```

</p>
</details> 

resolves: #3202

---

_Comment by @github-actions[bot] on 2023-05-03 04:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.36ms     2.4 MB/sec    1.00     16.9±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.02ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.2±4.92µs     6.0 MB/sec    1.01    499.6±4.02µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.6 MB/sec    1.00      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.11ms     4.9 MB/sec    1.01      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1752.1±27.96µs     9.5 MB/sec    1.02   1790.3±7.83µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.2±8.17µs    15.4 MB/sec    1.04    199.4±5.22µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.09ms     7.0 MB/sec    1.03      3.7±0.08ms     6.8 MB/sec
parser/large/dataset.py                    1.07      6.9±0.06ms     5.9 MB/sec    1.00      6.5±0.08ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.05  1324.6±14.54µs    12.6 MB/sec    1.00  1255.7±22.26µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    129.5±3.95µs    22.8 MB/sec    1.00    130.2±0.25µs    22.7 MB/sec
parser/pydantic/types.py                   1.04      2.9±0.05ms     8.9 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.22ms     2.4 MB/sec    1.00     16.9±0.25ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.05ms     4.0 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    486.9±8.39µs     6.1 MB/sec    1.01   491.2±11.89µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.01      7.1±0.22ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.10ms     4.8 MB/sec    1.00      8.4±0.10ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1798.3±21.15µs     9.3 MB/sec    1.00  1791.3±18.77µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.5±3.12µs    14.8 MB/sec    1.03    205.4±5.39µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.9±0.05ms     5.9 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1309.9±21.99µs    12.7 MB/sec    1.00  1303.8±15.94µs    12.8 MB/sec
parser/numpy/globals.py                    1.01    133.1±1.84µs    22.2 MB/sec    1.00    132.3±2.00µs    22.3 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.7 MB/sec    1.00      2.9±0.05ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-03 09:19_

Overall looks good to me. I personally would prefer to store the path on the `PyprojectDiscovery` rather than the `AllSettings` because the `path` isn't a setting. But that likely results in a larger change because it requires re-modelling `PyprojectDiscovery`:

```rust
struct PyprojectDiscovery {
	strategy: PyprojectDiscoveryStrategy,
	root_settings: AllSettings,
	root_settings_path: Option<PathBuf>
}

#[derive(Debug, Copy, Clone, Eq, PartialEq)]
enum PyprojectDiscoveryStrategy {
	Fixed,
	Hierarchical
}
```

What's your take on this?

Could you add a test plan to the PR summary showing the new output?

---

_Comment by @dhruvmanila on 2023-05-03 11:07_

> What's your take on this?

This is a much better idea. At first I thought to add it as an additional element to the tuple itself but I think a struct is a better option for the long term.

> Could you add a test plan to the PR summary showing the new output?

Yes, will add it.

---

I'll make both the changes in the evening.

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-05-03 17:10_

---

_Comment by @dhruvmanila on 2023-05-03 17:12_

I've updated the code and PR description :)

I'm getting `error: Broken pipe (os error 32)` when I'm piping it to the `head` command. Is it a bug or some problem in my shell?

---

_@MichaReiser approved on 2023-05-04 06:21_

---

_Comment by @MichaReiser on 2023-05-04 06:22_

> I'm getting `error: Broken pipe (os error 32)` when I'm piping it to the `head` command. Is it a bug or some problem in my shell?

I experienced this in the past too when using `less` but haven't spent any time investigating why this is happening.

---

_Merged by @MichaReiser on 2023-05-04 06:22_

---

_Closed by @MichaReiser on 2023-05-04 06:22_

---

_Branch deleted on 2023-05-04 14:03_

---

_Comment by @dhruvmanila on 2023-05-04 14:19_

I think this is related: https://github.com/rust-lang/rust/issues/46016

---
