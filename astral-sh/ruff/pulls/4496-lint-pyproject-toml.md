```yaml
number: 4496
title: Lint pyproject.toml
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: lint_pyproject_toml
created_at: 2023-05-18T14:37:12Z
updated_at: 2023-05-25T12:24:46Z
url: https://github.com/astral-sh/ruff/pull/4496
synced_at: 2026-01-12T03:50:03Z
```

# Lint pyproject.toml

---

_Pull request opened by @konstin on 2023-05-18 14:37_

This adds a new rule `InvalidPyprojectToml` that lints pyproject.toml by checking if https://github.com/PyO3/pyproject-toml-rs can parse it. This means the linting is currently very basic, e.g. we don't check whether the name is actually a valid python project name or appropriately normalized. It does catch errors e.g. with invalid dependency requirements or problems withs the license specifications. It is open to be extended in the future (validate name, SPDX expressions, classifiers, ...), either in ruff or in pyproject-toml-rs.

TODOs:

 - [x] Why does `FilePattern::Builtin("pyproject.toml")` alone not work? I've added `*.toml` for now to test but this should be changed before merging. Edit: Because it matches as a glob against entire paths
 - [x] Run this over the ecosystem CI dataset
 - [x] Re-check the unicode offsets

---

_@konstin reviewed on 2023-05-18 14:38_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_bleach.snap`:6 on 2023-05-18 14:38_

This breaks the normal style but unfortunately serde has no concept of spans so pep440_rs/pep508_rs can't forward this to toml; i think that's the best we can do

---

_@konstin reviewed on 2023-05-18 14:41_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_invalid_author.snap`:9 on 2023-05-18 14:41_

i'm afraid the korean font isn't monospace, it looks at least a bit better on my machine (but the columns are still slightly misaligned)

![image](https://github.com/charliermarsh/ruff/assets/6826232/a3fbc620-f5ad-476c-ab83-c2237b5ce914)


---

_@konstin reviewed on 2023-05-18 14:53_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_invalid_author.snap`:9 on 2023-05-18 14:53_

opened https://github.com/toml-rs/toml/issues/556 

---

_Comment by @github-actions[bot] on 2023-05-18 15:47_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>build (+4, -0)</summary>
<p>

```diff
+      ^
+ tests/packages/test-invalid-requirements/pyproject.toml:2:12: RUF200 Failed to parse pyproject.toml: Expected one of `@`, `(`, `<`, `=`, `>`, `~`, `!`, `;`, found `i`
+ tests/packages/test-no-requires/pyproject.toml:1:1: RUF200 Failed to parse pyproject.toml: missing field `requires`
+ this is invalid
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF200 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.02ms     2.7 MB/sec    1.00     15.0±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.3±1.03µs     7.9 MB/sec    1.02    383.5±1.20µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.4 MB/sec    1.00      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1604.8±3.61µs    10.4 MB/sec    1.00   1598.4±3.55µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.4±0.30µs    16.7 MB/sec    1.03    181.3±1.62µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.01      6.0±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1154.3±1.76µs    14.4 MB/sec    1.01   1171.5±0.83µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    118.2±0.22µs    25.0 MB/sec    1.00    118.4±0.47µs    24.9 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.02      2.5±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.18ms     2.4 MB/sec    1.01     16.8±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.5±9.55µs     5.9 MB/sec    1.02    513.1±6.25µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.06ms     4.9 MB/sec    1.00      8.2±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1772.5±21.20µs     9.4 MB/sec    1.00  1777.0±17.53µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    207.6±3.44µs    14.2 MB/sec    1.04    216.0±6.12µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.01      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.3 MB/sec    1.01      6.5±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1237.7±13.35µs    13.5 MB/sec    1.00  1234.7±13.23µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    126.2±2.11µs    23.4 MB/sec    1.01    127.4±1.73µs    23.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.02ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin reviewed on 2023-05-18 19:34_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_invalid_author.snap`:9 on 2023-05-18 19:34_

it's byte offsets indeed, confused why it's 30 chars in line 8 and 34 chars whitespaces below

---

_Review comment by @MichaReiser on `crates/ruff/src/pyproject_toml.rs`:32 on 2023-05-19 07:46_

Can we follow up on this with an issue to make `TextRange` and `SourceFile` on `Diagnostic` optional and then link to it here in a TODO?

---

_Review comment by @MichaReiser on `crates/ruff/src/pyproject_toml.rs`:21 on 2023-05-19 07:47_

```suggestion
            let expect_err = "pyproject.toml file to be smaller than 4GB";
```

Or we could even output this as a diagnostic or handle it generically inside of the CLI (calling the RustPython parser with a string longer than 4GB panics right now)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_bleach.snap`:13 on 2023-05-19 07:49_

I assume this is a "bug" in the PEP440 crate. It should only return the range of the qualifier without leading and trailing whitespace.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_invalid_author.snap`:9 on 2023-05-19 07:52_

Can you look into the `TextEmitter` and verify if we pass the right character positions to `annotate_snippets` to know if it is a bug from us or `annotate_snippets`?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:144 on 2023-05-19 07:56_

Nit:

```suggestion
			..Diagnostics::default(),
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:138 on 2023-05-19 07:58_

This `clones` the whole string. I think it would be better only to construct the `SourceFile` when there are any messages. Or we could create a `SourceFileRef` if we want to pass the path and contents as a single unit.

---

_@MichaReiser approved on 2023-05-19 07:58_

---

_@konstin reviewed on 2023-05-22 06:59_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_bleach.snap`:13 on 2023-05-22 06:59_

that's from the toml crate i think, the definition is at https://github.com/PyO3/pyproject-toml-rs/blob/e3ae25d4fa43c29351e5b6844b9428a0adb37d95/src/lib.rs#L63

---

_@konstin reviewed on 2023-05-22 08:09_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_invalid_author.snap`:9 on 2023-05-22 08:09_

Apparently the width of hangul syllables is 2, but for historic reasons there no proper hangul monospace. I've replaced the test string with something else.

Below: http://www.unicode.org/reports/tr11/#Set_Relations

![image](https://github.com/charliermarsh/ruff/assets/6826232/181b1a9b-b511-4271-a4d8-be10143514a4)


---

_@konstin reviewed on 2023-05-22 08:13_

---

_Review comment by @konstin on `crates/ruff/src/pyproject_toml.rs`:32 on 2023-05-22 08:13_

done

---

_@konstin reviewed on 2023-05-22 08:23_

---

_Review comment by @konstin on `crates/ruff/src/pyproject_toml.rs`:21 on 2023-05-22 08:23_

Changed this to emitting a diagnostic, no reason to panic

---

_@MichaReiser reviewed on 2023-05-22 08:46_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF200_bleach.snap`:13 on 2023-05-22 08:46_

Oh, right. There's another crate :) Do you think it is because how `pyproject-toml-rs` creates the error or is it because the requirement range includes the whitespace too?

---

_Comment by @konstin on 2023-05-24 14:55_

I made `[build-system]` optional to remove a bunch of false positives

---

Updated test plan:

```
scripts/ecosystem_all_check.sh check --select RUF200
```
This lead to a bunch of 
```
RUF200 Failed to parse pyproject.toml: missing field `name`
```
(e.g. https://github.com/amitsk/fastapi-todos/blob/main/pyproject.toml) which is indeed invalid (https://packaging.python.org/en/latest/specifications/declaring-project-metadata/#specification).

Filtering those out, the following other problems were found by `cd target/ecosystem_all_results/ && rg RUF200`:
```
UCL-ARC:rred-reports.stdout.txt
1:pyproject.toml:27:16: RUF200 Failed to parse pyproject.toml: Version specifier `>='3.9'` doesn't match PEP 440 rules
EndlessTrax:python-start-project.stdout.txt
1:pyproject.toml:14:16: RUF200 Failed to parse pyproject.toml: Expected package name starting with an alphanumeric character, found '#'
redjax:gardening-api.stdout.txt
1:pyproject.toml:7:11: RUF200 Failed to parse pyproject.toml: Version `` doesn't match PEP 440 rules
ajslater:codex.stdout.txt
2:  3:17 RUF200 Failed to parse pyproject.toml: invalid type: sequence, expected a string
LDmitriy7:404_AvatarsBot.stdout.txt
1:pyproject.toml:3:11: RUF200 Failed to parse pyproject.toml: Version `` doesn't match PEP 440 rules
ajslater:comicbox.stdout.txt
1:pyproject.toml:3:17: RUF200 Failed to parse pyproject.toml: invalid type: sequence, expected a string
manueldevillena:forecast-earnings.stdout.txt
1:pyproject.toml:24:12: RUF200 Failed to parse pyproject.toml: Expected one of `@`, `(`, `<`, `=`, `>`, `~`, `!`, `;`, found `^`
redjax:ohio_utility_scraper.stdout.txt
1:pyproject.toml:11:11: RUF200 Failed to parse pyproject.toml: Version `` doesn't match PEP 440 rules
agronholm:typeguard.stdout.txt
1:pyproject.toml:40:8: RUF200 Failed to parse pyproject.toml: Expected a valid marker name, found 'python_implementation'
cyuss:decathlon-turnover.stdout.txt
1:pyproject.toml:7:12: RUF200 Failed to parse pyproject.toml: invalid type: string "Youcef", expected a table with 'name' and 'email' keys
ajslater:boilerplate.stdout.txt
1:pyproject.toml:3:17: RUF200 Failed to parse pyproject.toml: invalid type: sequence, expected a string
kaparoo:lightning-project-template.stdout.txt
1:pyproject.toml:56:16: RUF200 Failed to parse pyproject.toml: You can't mix a >= operator with a local version (`+cu117`)
dijital20:pytexas2023-decorators.stdout.txt
1:pyproject.toml:5:11: RUF200 Failed to parse pyproject.toml: Version `` doesn't match PEP 440 rules
pfouque:django-anymail-history.stdout.txt
1:pyproject.toml:137:12: RUF200 Failed to parse pyproject.toml: Version specifier `> = 1.2.0` doesn't match PEP 440 rules
pfouque:django-fakemessages.stdout.txt
1:pyproject.toml:130:12: RUF200 Failed to parse pyproject.toml: Version specifier `> = 1.2.0` doesn't match PEP 440 rules
pypa:build.stdout.txt
1:tests/packages/test-invalid-requirements/pyproject.toml:2:12: RUF200 Failed to parse pyproject.toml: Expected one of `@`, `(`, `<`, `=`, `>`, `~`, `!`, `;`, found `i`
4:tests/packages/test-no-requires/pyproject.toml:1:1: RUF200 Failed to parse pyproject.toml: missing field `requires`
UnoYakshi:FRAAND.stdout.txt
2:  3:11 RUF200 Failed to parse pyproject.toml: Version `` doesn't match PEP 440 rules
DHolmanCoding:python-template.stdout.txt
1:pyproject.toml:22:1: RUF200 Failed to parse pyproject.toml: missing field `requires`
```
Overall, this emitted errors in 43 out of 3408 projects (`rg -c RUF200 target/ecosystem_all_results/ | wc -l`)


---

_Review requested from @MichaReiser by @konstin on 2023-05-24 14:56_

---

_@MichaReiser approved on 2023-05-25 06:28_

This is way less code than I expected it to be. Well done! 

The ranges for the requirements is unfortunate but I don't think there's anything we can do about it. Is there?

---

_Comment by @konstin on 2023-05-25 11:49_

> The ranges for the requirements is unfortunate but I don't think there's anything we can do about it. Is there?

i'm afraid not, at least not without digging deep into the toml crate

---

_Merged by @konstin on 2023-05-25 12:05_

---

_Closed by @konstin on 2023-05-25 12:05_

---

_Branch deleted on 2023-05-25 12:05_

---
