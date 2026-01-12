```yaml
number: 5059
title: "Add `ruff rule --all` subcommand (with JSON output)"
type: pull_request
state: merged
author: akx
labels:
  - cli
assignees: []
merged: true
base: main
head: rules-json
created_at: 2023-06-13T18:11:51Z
updated_at: 2023-07-04T20:03:53Z
url: https://github.com/astral-sh/ruff/pull/5059
synced_at: 2026-01-12T03:36:54Z
```

# Add `ruff rule --all` subcommand (with JSON output)

---

_Pull request opened by @akx on 2023-06-13 18:11_

## Summary

This adds a `ruff rule --all` switch that prints out a human-readable Markdown or a machine-readable JSON document of the lint rules known to Ruff.

I needed a machine-readable document of the rules [for a project](https://github.com/astral-sh/ruff/discussions/5078), and figured it could be useful for other people – or tooling! – to be able to interrogate Ruff about its arcane knowledge.

The JSON output is an array of the same objects printed by `ruff rule --format=json`.

## Test Plan

I ran `ruff rule --all --format=json`. I think more might be needed, but maybe a snapshot test is overkill?

---

_Marked ready for review by @akx on 2023-06-13 18:14_

---

_Comment by @akx on 2023-06-13 18:17_

Oh, darn, never mind, I just now noticed `ruff rule` has a JSON output mode! I'll amend this to use that...

---

_Converted to draft by @akx on 2023-06-13 18:17_

---

_Comment by @github-actions[bot] on 2023-06-13 18:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.35ms     4.4 MB/sec    1.06      9.8±0.44ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.2±0.19ms     7.6 MB/sec    1.00      2.1±0.10ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   246.9±10.21µs    12.0 MB/sec    1.00   247.1±19.90µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.25ms     5.4 MB/sec    1.04      4.9±0.22ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.76ms     2.4 MB/sec    1.05     17.6±0.77ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.16ms     4.2 MB/sec    1.08      4.3±0.21ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   519.5±28.21µs     5.7 MB/sec    1.07   555.1±20.90µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.26ms     3.6 MB/sec    1.09      7.7±0.32ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.28ms     5.2 MB/sec    1.08      8.4±0.30ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.5±96.01µs     9.7 MB/sec    1.03  1767.6±68.80µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   205.1±12.57µs    14.4 MB/sec    1.04   212.9±16.57µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.18ms     6.8 MB/sec    1.00      3.7±0.15ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.28ms     3.7 MB/sec    1.00     10.9±0.24ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.07ms     7.0 MB/sec    1.00      2.3±0.07ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   268.0±11.17µs    11.0 MB/sec    1.03   276.6±19.56µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.13ms     5.0 MB/sec    1.03      5.3±0.15ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.3±0.43ms     2.2 MB/sec    1.00     18.3±0.42ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.7±0.14ms     3.5 MB/sec    1.00      4.7±0.10ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   577.2±17.17µs     5.1 MB/sec    1.00   577.0±17.39µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.2±0.24ms     3.1 MB/sec    1.00      8.0±0.24ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.03      9.6±0.17ms     4.2 MB/sec    1.00      9.3±0.20ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.04ms     8.1 MB/sec    1.00      2.0±0.06ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    238.0±6.82µs    12.4 MB/sec    1.00    235.3±7.36µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.10ms     6.0 MB/sec    1.00      4.3±0.12ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add `ruff rules-json` subcommand" to "Add `ruff rules` subcommand (with JSON output)" by @akx on 2023-06-13 19:25_

---

_Marked ready for review by @akx on 2023-06-13 19:32_

---

_Comment by @akx on 2023-06-27 12:31_

Sorry for the ping, but... ping, @charliermarsh? 😅 

---

_Comment by @charliermarsh on 2023-06-27 16:45_

Thank you for the ping and sorry @akx. What do you think about adding an `--all` flag or something to `ruff rule`? It feels like a lot to add an additional subcommand for something that's somewhat niche.

---

_Comment by @akx on 2023-06-27 18:57_

@charliermarsh Sure, that sounds good to me too, I'll rework this :)

---

_Renamed from "Add `ruff rules` subcommand (with JSON output)" to "Add `ruff rule --all` subcommand (with JSON output)" by @akx on 2023-06-27 19:10_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:41 on 2023-06-28 11:56_

Nit: I prefer to make these conversion functions method on the corresponding types because I find methods easier to discover than free standing function: I want an explanation... okay lets type `Explanation::` and see what shows up
```suggestion
impl<'a> Explanation<'a> {
	fn from_rule(rule: &'a Rule) -> Self {
		let code = rule.noqa_code().to_string();
	    let (linter, _) = Linter::parse_code(&code).unwrap();
	    let autofix = rule.autofixable().to_string();
	    Self {
	        name: rule.as_ref(),
	        code,
	        linter: linter.name(),
	        summary: rule.message_formats()[0],
	        message_formats: rule.message_formats(),
	        autofix,
	        explanation: rule.explanation(),
	    }
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:86 on 2023-06-28 11:57_

@dhruvmanila is it OK if we use `stdout` here or should this command also support forwarding to a file?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:64 on 2023-06-28 11:59_

Should the `JSON` and human readable text have the same fields? IT seems that the `nursery` information is only available in the human readable output?


---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:43 on 2023-06-28 12:00_

Nit: You could instead also implement `Display` for e.g. `Explanation` (this would also ensure that the JSON output and the human-readable output contain the same information). 

---

_@MichaReiser reviewed on 2023-06-28 12:00_

---

_@dhruvmanila reviewed on 2023-06-28 12:54_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/commands/rule.rs`:86 on 2023-06-28 12:54_

I think it's fine to use `stdout` here. We can always add it in the future if there's a use case.

---

_@akx reviewed on 2023-06-29 07:45_

---

_Review comment by @akx on `crates/ruff_cli/src/commands/rule.rs`:64 on 2023-06-29 07:45_

I added a `nursery` boolean to the JSON output (and TBH I thought I had done that before...).

---

_@akx reviewed on 2023-06-29 07:50_

---

_Review comment by @akx on `crates/ruff_cli/src/commands/rule.rs`:43 on 2023-06-29 07:50_

I think I'll leave that for a future refactoring (I think the docs-generation code could be unified with this too?).

---

_Review requested from @MichaReiser by @akx on 2023-06-29 07:57_

---

_Label `cli` added by @charliermarsh on 2023-07-04 19:38_

---

_Merged by @charliermarsh on 2023-07-04 19:45_

---

_Closed by @charliermarsh on 2023-07-04 19:45_

---

_Branch deleted on 2023-07-04 19:46_

---
