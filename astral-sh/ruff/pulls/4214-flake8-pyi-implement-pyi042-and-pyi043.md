```yaml
number: 4214
title: "[`flake8-pyi`] Implement `PYI042` and `PYI043`"
type: pull_request
state: merged
author: arya-k
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi_042
created_at: 2023-05-03T20:55:07Z
updated_at: 2023-05-04T20:17:27Z
url: https://github.com/astral-sh/ruff/pull/4214
synced_at: 2026-01-12T04:28:19Z
```

# [`flake8-pyi`] Implement `PYI042` and `PYI043`

---

_Pull request opened by @arya-k on 2023-05-03 20:55_

PYI042 Type alias names should use CamelCase rather than snake_case
PYI043 Do not use names ending in "T" for private type aliases. (The "T" suffix implies that an object is a TypeVar.)

rel: https://github.com/charliermarsh/ruff/issues/848

---

_Converted to draft by @arya-k on 2023-05-03 21:26_

---

_Comment by @github-actions[bot] on 2023-05-03 21:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+12, -0, 0 error(s))

<details><summary>typeshed (+12, -0)</summary>
<p>

```diff
+ stdlib/typing.pyi:735:1: PYI042 Type alias `_get_type_hints_obj_allowed_types` should be CamelCase
+ stubs/cffi/_cffi_backend.pyi:113:5: PYI042 Type alias `buffer` should be CamelCase
+ stubs/cffi/cffi/api.pyi:13:1: PYI042 Type alias `basestring` should be CamelCase
+ stubs/cffi/cffi/api.pyi:18:5: PYI042 Type alias `buffer` should be CamelCase
+ stubs/pika/pika/spec.pyi:9:1: PYI042 Type alias `_str` should be CamelCase
+ stubs/pywin32/pythoncom.pyi:10:1: PYI042 Type alias `error` should be CamelCase
+ stubs/pywin32/win32/odbc.pyi:9:1: PYI042 Type alias `_odbcError` should be CamelCase
+ stubs/pywin32/win32comext/adsi/adsi.pyi:7:1: PYI042 Type alias `error` should be CamelCase
+ stubs/pywin32/win32comext/propsys/propsys.pyi:6:1: PYI042 Type alias `error` should be CamelCase
+ stubs/pywin32/win32comext/shell/shell.pyi:7:1: PYI042 Type alias `error` should be CamelCase
+ stubs/redis/redis/typing.pyi:17:1: PYI043 Private type alias `_StringLikeT` should not be suffixed with `T` (the `T` suffix implies that an object is a `TypeVar`)
+ stubs/requests/requests/compat.pyi:22:1: PYI042 Type alias `builtin_str` should be CamelCase
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.09ms     2.4 MB/sec    1.00     16.9±0.06ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    508.4±5.35µs     5.8 MB/sec    1.00    503.9±1.15µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.02ms     4.9 MB/sec    1.01      8.4±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1778.6±6.09µs     9.4 MB/sec    1.01   1797.0±4.76µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.2±2.15µs    14.8 MB/sec    1.02    202.3±4.97µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.01      3.7±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1275.4±1.81µs    13.1 MB/sec    1.00   1276.2±1.57µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    129.8±0.50µs    22.7 MB/sec    1.00    130.1±0.33µs    22.7 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.00ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.0±0.20ms     2.4 MB/sec    1.00     16.6±0.07ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.04ms     3.8 MB/sec    1.00      4.2±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    437.8±5.24µs     6.7 MB/sec    1.00    434.6±5.04µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.04ms     3.5 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.7±0.03ms     4.7 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1844.1±27.13µs     9.0 MB/sec    1.00  1803.8±13.18µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.1±5.68µs    15.0 MB/sec    1.00    195.4±4.30µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.03ms     6.5 MB/sec    1.00      3.9±0.04ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.8±0.03ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1295.2±7.08µs    12.9 MB/sec    1.00   1292.1±6.03µs    12.9 MB/sec
parser/numpy/globals.py                    1.01    134.9±1.42µs    21.9 MB/sec    1.00    133.6±1.01µs    22.1 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.8 MB/sec    1.00      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @arya-k on 2023-05-04 02:53_

Hmm I'm not able to repro this test failure locally (macOS), when I run `cargo test -p ruff_cli --test integration_test`. I'm not quite sure what to make of this, and the CLI tests seem unrelated to my changes– do you have any tips @charliermarsh?

p.s. I'm pretty new to the ruff community: is it bad form to tag you like this?

---

_Comment by @charliermarsh on 2023-05-04 03:04_

@arya-k - Not sure why I can't repro locally either, but I have a suspicion that we just hit a factor-of-two boundary and need to increase the `RuleSet` bit vector size. In short, we have a vector that needs to be long enough to match the number of total rules, and we may have just exceeded the previous limit. I tried pushing a fix, lets see what happens.

Also: totally fine to tag me or other maintainers.

---

_Review comment by @charliermarsh on `crates/ruff/src/registry/rule_set.rs`:13 on 2023-05-04 03:08_

@arya-k - Here's the change I was referring to (very unclear from the trace, but I saw the out-of-bounds with index 9 and thought of the 9 here).

---

_@charliermarsh reviewed on 2023-05-04 03:08_

---

_Marked ready for review by @arya-k on 2023-05-04 04:14_

---

_@arya-k reviewed on 2023-05-04 04:15_

---

_Review comment by @arya-k on `crates/ruff/src/registry/rule_set.rs`:13 on 2023-05-04 04:15_

Thank you! This is helpful, and I appreciate the fix. 

I wonder why this didn’t come up locally!

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI042_PYI042.py.snap`:2 on 2023-05-04 06:55_

Is it intentionally that this test case has no expected diagnostics?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:10 on 2023-05-04 07:03_

Is my understanding correct that this regex e.g. wouldn't match `Snake_camel_case`? Should we detect this pattern too? 


I think we should be able to implement this search without the need for a Regex.

```rust

let Some(first_character) = id.chars().next() else { return; }

let mut camel_case_name = String::new();
let mut last_index = 0

if first_character.is_lowercase() {
	camel_case_name.push(first_character.to_uppercase();
	last_index = 1;
}

for (index, _) in id.match_indices('_') {
	if let Some(next) = id.get(index + 1) {
		if next.is_ascii_alphabetic() {
			camel_case_name.push_str(&id[..last_index];
			camel_case_name.push(next.to_uppercase());
			last_index = index + 1;
		}
	}
}

if !camel_case_name.is_empty() {
	camel_case_name.push_str(&id[last_index..]);
	// create diagnostic
}
```



---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:17 on 2023-05-04 07:15_

I also think that we can implement this with plain string operations rather than using a Regex here (a regex has the added cost that it needs to be compiled once and is quiet a heavy machinery)

```rust
if !name.starts_with('_') {
	return;
}

let mut chars = id.chars();

let Some(mut last) = chars.next_back() else { return; }

// Skip over the trailing digit
if last.is_ascii_digit() {
	if let Some(second_last) = chars.next_back() {
		last = second_last;
	} else {
		return;
	}
}

if last == 'T' {
	// error 
}
```

---

_@MichaReiser reviewed on 2023-05-04 07:17_

Thank you for your contribution. 

I think we should try to implement these rules without having to rely on regular expressions. Regular expressions are great but can be overkill when matching on simple patterns and using plain string methods can then be better for performance

---

_Converted to draft by @arya-k on 2023-05-04 14:57_

---

_@arya-k reviewed on 2023-05-04 14:58_

---

_Review comment by @arya-k on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI042_PYI042.py.snap`:2 on 2023-05-04 14:58_

Yes- this tool only operates on stub files for now. I'll change the comments in the test to make this clearer

---

_@arya-k reviewed on 2023-05-04 15:49_

---

_Review comment by @arya-k on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:10 on 2023-05-04 15:49_

We should match that pattern!

I also realize that the regex should have been named `SNAKE_CASE_REGEX`, which may have caused some confusion.

---

_Marked ready for review by @arya-k on 2023-05-04 16:35_

---

_@charliermarsh reviewed on 2023-05-04 17:00_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:63 on 2023-05-04 17:00_

I did some testing against `typeshed`, and I think this matches their current behavior:

```rust
/// Return `true` if the given name is a snake_case type alias. In this context, we match against
/// any name that begins with an optional underscore, followed by at least one lowercase letter.
fn is_snake_case_type_alias(name: &str) -> bool {
    let mut chars = name.chars();
    match (chars.next(), chars.next()) {
        (Some('_'), Some(c)) | (Some(c), ..) => c.is_ascii_alphabetic() && c.is_ascii_lowercase(),
        _ => false,
    }
}
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:63 on 2023-05-04 17:01_

The current version seems to have some false positives, e.g., it flags this, which `flake8-pyi` allows:

```py
_TYPE_FIELDS: TypeAlias = _TYPE_FIELDS_SEQUENCE | Mapping[str, fields._FieldValueTuple]
```

---

_@charliermarsh reviewed on 2023-05-04 17:01_

---

_Renamed from "[flake8-pyi] PYI042, PYI043" to "[`flake8-pyi`] Implement `PYI042` and `PYI043`" by @charliermarsh on 2023-05-04 17:38_

---

_Label `rule` added by @charliermarsh on 2023-05-04 17:38_

---

_Merged by @charliermarsh on 2023-05-04 18:35_

---

_Closed by @charliermarsh on 2023-05-04 18:35_

---

_@arya-k reviewed on 2023-05-04 20:02_

---

_Review comment by @arya-k on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:63 on 2023-05-04 20:02_

That makes sense- and is the original behavior the regex supported (`^_?[a-z]`).

I changed it to support `Snake_case`- is it more important to maintain parity with the original plug-in?

---

_Comment by @arya-k on 2023-05-04 20:03_

Thank you for pushing this over the finish line- for my understanding, how did you format the python files? My installation of `black` produced a much larger diff so I ignored it. 

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/type_alias_naming.rs`:63 on 2023-05-04 20:07_

Ah yeah, I prefer to maintain parity with upstream plugins unless we have a good reason to deviate (examples: there's a clear bug that we can fix, diagnostic messages are confusing or inconsistent with our own conventions). It makes migrations easier for users, and reduces confusion.


---

_@charliermarsh reviewed on 2023-05-04 20:07_

---

_Comment by @charliermarsh on 2023-05-04 20:08_

@arya-k - Oh interesting, I just ran Black from my own editor.

---

_Comment by @charliermarsh on 2023-05-04 20:08_

I should correct that: we probably haven't run Black over all of our Python fixtures, so running `black` from root could produce a large diff. I just ran Black over the new fixtures in this PR.

---

_Comment by @arya-k on 2023-05-04 20:17_

> I should correct that: we probably haven't run Black over all of our Python fixtures, so running `black` from root could produce a large diff. I just ran Black over the new fixtures in this PR.

Ah got it. I ran it on all fixtures, then figured it wasn’t the way we were formatting python. Thanks for the clarification! 

---
