```yaml
number: 3795
title: "Fix `is_module_name()` and improve perf of `is_identifier()`"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-is-module-name
created_at: 2023-03-29T15:11:27Z
updated_at: 2023-03-31T19:28:31Z
url: https://github.com/astral-sh/ruff/pull/3795
synced_at: 2026-01-12T04:28:19Z
```

# Fix `is_module_name()` and improve perf of `is_identifier()`

---

_Pull request opened by @JonathanPlasse on 2023-03-29 15:11_

- For `is_module_name()`
  - Must not be a reserved keyword
  - use is_ascii_lowercase() and is_ascii_digit()
- Improve perf of `is_indentifier()`

---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/identifiers.rs`:12 on 2023-03-29 15:23_

Unrelated to this PR but this could be rewritten to

```suggestion
let mut chars = s.chars();

if !s.next().map_or(false, |c| is.alphabetic() || c == '_') {
	return false;
}

chars.all(|c| c.is_alphanumeric() || c == '_')
```

to avoid iterating over the first char twice. 

---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/identifiers.rs`:38 on 2023-03-29 15:24_

NIT: It could make sense to duplicate the `is_identifier` logic to only iterate two times over the string, instead of 3 times (at least once to check if it is in the KWARGS list, once to test if it is an identifier, and once to test if it is a module)

---

_@MichaReiser approved on 2023-03-29 15:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/identifiers.rs`:9 on 2023-03-29 15:27_

Can we go through the usages of this method, and see if we're checking for `KWLIST` already at the call-sites? If so, can we then remove those extra checks?

---

_@charliermarsh reviewed on 2023-03-29 15:27_

---

_Comment by @github-actions[bot] on 2023-03-29 16:45_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>zulip (+1, -0)</summary>
<p>

```diff
+ zerver/management/commands/import.py:1:1: N999 Invalid module name: 'import'
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.2±0.10ms     2.9 MB/sec    1.00     14.0±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.8±1.94µs     6.5 MB/sec    1.01    456.9±2.26µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.06ms     4.3 MB/sec    1.00      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.05ms     5.6 MB/sec    1.00      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1626.9±5.40µs    10.2 MB/sec    1.01   1637.5±4.37µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.0±0.33µs    16.2 MB/sec    1.01    184.1±2.67µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.6 MB/sec    1.00      3.4±0.02ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.1±0.53ms     2.3 MB/sec    1.00     18.1±0.47ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.13ms     3.6 MB/sec    1.02      4.8±0.23ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   577.4±26.31µs     5.1 MB/sec    1.02   590.6±33.34µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.24ms     3.3 MB/sec    1.00      7.8±0.23ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.44ms     4.3 MB/sec    1.01      9.5±0.30ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     8.1 MB/sec    1.02      2.1±0.10ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    244.4±8.81µs    12.1 MB/sec    1.08   263.9±15.92µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.14ms     6.0 MB/sec    1.06      4.5±0.33ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-29 17:20_

Looking at the ecosystem checks, I'd forgotten that I think we shipped a strict version like this in the past and it caused a ton of problems (e.g. with Django migrations), we had to release a hotfix. Let me dig up the issues.

---

_Review comment by @JonathanPlasse on `crates/ruff_python_stdlib/src/identifiers.rs`:9 on 2023-03-29 17:54_

`KWLIST` is used for converting named_tuple or typed_dict.
Will remove it.

---

_@JonathanPlasse reviewed on 2023-03-29 17:54_

---

_Renamed from "Fix is_module_name() and is_identifier()" to "Fix is_module_name() and improve perf of is_identifier()" by @JonathanPlasse on 2023-03-29 18:25_

---

_Comment by @JonathanPlasse on 2023-03-29 18:29_

All the errors except one seem to be migrations. It could easily be ignored with `per-file-ignores`.
The remaining error is with a file named `import.py` in _zulip_. This is not a valid module name and cannot be imported by other modules.
For `is_module_name()`, the string must be valid ASCII as stated in [PEP8](https://peps.python.org/pep-0008/#ascii-compatibility).

---

_Comment by @charliermarsh on 2023-03-29 18:30_

I think I was mixing this up with the fact that we accidentally flagged modules like `__main__.py` and `__version.py__`.

---

_Comment by @MichaReiser on 2023-03-29 19:56_

Woah, nice performance improvement. I'm surprised that it improves by so much

---

_Comment by @JonathanPlasse on 2023-03-29 20:02_

Indeed, I was not expecting this.

---

_Comment by @charliermarsh on 2023-03-29 22:24_

My only hangup here is that I'm wondering if we should special-case migration files. If users are always going to end up ignoring them, it'd be nice for us to just "do the right thing" out of the box. At the same time, it's weird to allow what is actually an invalid module name. What do you think?

---

_Comment by @JonathanPlasse on 2023-03-30 06:04_

How could we avoid false negative if we ignore migration files?

---

_Comment by @konstin on 2023-03-30 11:35_

Maybe we could start checking if the path contains a component called `migrations` and whether the filename starts with four digits? This should cover the django projects. alembic seems to use `versions/[0-9a-f]{12}(_.*)?.py`, but from a quick search i couldn't get the exact semantics

---

_Comment by @charliermarsh on 2023-03-30 13:14_

I'm temped to say we just ignore anything in a `migrations` or `versions` subdirectory.

---

_Comment by @charliermarsh on 2023-03-30 15:51_

How bout: if `migrations` or `versions` is in the path, allow it to start with a number.

---

_Comment by @JonathanPlasse on 2023-03-30 18:24_

Done!

---

_Comment by @charliermarsh on 2023-03-30 18:25_

Thanks!

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/rules/invalid_module_name.rs`:69 on 2023-03-31 18:52_

@JonathanPlasse - What do you think about this? It lets us keep the "project convention" logic out of `ruff_python_stdlib` (and keeps `is_module_name` simple since it doesn't rely on directory in any way)... but it does mean we have to check the name twice for valid migrations. However, the most common path is that a module has a valid name anyway, so it may not matter in practice.

---

_@charliermarsh reviewed on 2023-03-31 18:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/rules/invalid_module_name.rs`:100 on 2023-03-31 18:52_

(E.g., to me, this check is similar to the `is_module_file` check we already do here -- it feels more at-home in the rule than in the compliance utilities...)

---

_@charliermarsh reviewed on 2023-03-31 18:52_

---

_Renamed from "Fix is_module_name() and improve perf of is_identifier()" to "Fix `is_module_name()` and improve perf of `is_identifier()`" by @charliermarsh on 2023-03-31 18:56_

---

_@JonathanPlasse reviewed on 2023-03-31 18:59_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pep8_naming/rules/invalid_module_name.rs`:69 on 2023-03-31 18:59_

It is great. Good separation of concerns.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/rules/invalid_module_name.rs`:70 on 2023-03-31 19:01_

@JonathanPlasse - Tweaked it to do the check once like you had before, just separate functions now.

---

_@charliermarsh reviewed on 2023-03-31 19:01_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pep8_naming/rules/invalid_module_name.rs`:70 on 2023-03-31 19:06_

Great!

---

_@JonathanPlasse reviewed on 2023-03-31 19:06_

---

_Merged by @charliermarsh on 2023-03-31 19:15_

---

_Closed by @charliermarsh on 2023-03-31 19:15_

---

_Branch deleted on 2023-03-31 19:28_

---
