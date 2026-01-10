```yaml
number: 13172
title: Use dynamic builtins list based on Python version
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/builtins
created_at: 2024-08-30T23:00:46Z
updated_at: 2024-09-06T07:58:26Z
url: https://github.com/astral-sh/ruff/pull/13172
synced_at: 2026-01-10T21:38:32Z
```

# Use dynamic builtins list based on Python version

---

_Pull request opened by @charliermarsh on 2024-08-30 23:00_

## Summary

Closes https://github.com/astral-sh/ruff/issues/13037.


---

_Comment by @charliermarsh on 2024-08-30 23:01_

I used this Python script then did some manual editing:

```python
import subprocess
import json

def enumerate_builtins_by_version(python_version):
    cmd = [
        "uv", "run", "--python", f"{python_version}", "--no-project", "python",
        "-c",
        """
import sys
import builtins
import json

python_version = f"{sys.version_info.major}.{sys.version_info.minor}"
builtin_list = sorted(dir(builtins))

print(json.dumps(builtin_list))
        """
    ]
    result = subprocess.run(cmd, check=True, capture_output=True, text=True)
    return json.loads(result.stdout)

if __name__ == "__main__":
    python_versions = ["3.8", "3.9", "3.10", "3.11", "3.12", "3.13"]
    all_results = {}
    for version in python_versions:
        print(f"\nRunning for Python {version}")
        result = enumerate_builtins_by_version(version)
        all_results[version] = result

    print("\nRust code for listing builtins:")
    print("match (major, minor) {")
    for version, builtins in all_results.items():
        major, minor = version.split('.')
        print(f"    ({major}, {minor}) => &[")
        for builtin in builtins:
            print(f'        "{builtin}",')
        print("    ],")
    print("    _ => &[],")
    print("}")

    print("\nRust code for checking if a string is a builtin:")
    print("matches!(minor, {")

    # Collect all unique builtins across versions
    all_builtins = set()
    for builtins in all_results.values():
        all_builtins.update(builtins)

    print("pub fn is_known_standard_library(minor_version: u8, module: &str) -> bool {")
    print("    matches!(")
    print("        (minor_version, module),")
    print("        (")

    # Print the builtins that are common to all versions
    common_builtins = set.intersection(*map(set, all_results.values()))
    for builtin in sorted(common_builtins):
        print(f'            (_, "{builtin}"),')

    # Print version-specific builtins
    for version, builtins in all_results.items():
        _, minor = version.split('.')
        version_specific = set(builtins) - common_builtins
        if version_specific:
            for builtin in sorted(version_specific):
                print(f'            ({minor}, "{builtin}"),')

    print("        )")
    print("    )")
    print("}")
```

---

_Label `rule` added by @charliermarsh on 2024-08-30 23:01_

---

_Comment by @codspeed-hq[bot] on 2024-08-30 23:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/builtins)

### Merging #13172 will **not alter performance**

<sub>Comparing <code>charlie/builtins</code> (de081b6) with <code>main</code> (3abd5c0)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review requested from @AlexWaygood by @charliermarsh on 2024-08-30 23:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_builtins/helpers.rs`:12 on 2024-08-31 17:41_

What's the use case (when checking a Jupyter notebook) for distinguishing between Python builtins and IPython builtins? Since we're refactoring the `is_python_builtin` function anyway, should we just make it a function that also accepts the `PySourceType` as well as the Python minor version? (And get rid of the `is_ipython_builtin` function?)

It seems less error-prone: currently you have to remember to call both functions if you want to know if a symbol is a builtin symbol in a notebook file

---

_Review comment by @AlexWaygood on `crates/ruff_python_stdlib/src/builtins.rs`:362 on 2024-08-31 17:45_

What version of Python 3.13 do you have installed locally? I think `IncompleteInputError` was removed from builtins in June: https://github.com/python/cpython/pull/119680

---

_Review comment by @AlexWaygood on `crates/ruff_python_stdlib/src/builtins.rs`:194 on 2024-08-31 17:47_

Same here, I don't think `IncompleteInputError` exists on the latest 3.13 version

---

_@AlexWaygood reviewed on 2024-08-31 17:48_

Nice! I wonder if we should check the script you used into the repo, similar to what we do for `scripts/generate_known_standard_library.py` and `scripts/generate_builtin_modules.py`? But it also shouldn't be too hard to update when 3.14 comes around

---

_@charliermarsh reviewed on 2024-09-01 16:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/builtins.rs`:362 on 2024-09-01 16:20_

I have `3.13.0b1` -- thank you!

---

_@charliermarsh reviewed on 2024-09-01 16:20_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_builtins/helpers.rs`:12 on 2024-09-01 16:20_

I'll try this out.

---

_Comment by @charliermarsh on 2024-09-01 16:21_

I think I'm gonna punt on adding the script. It was mostly written by GPT and still requires some massaging of the outputs, and future updates to the generated output should be easy, I think...

---

_@charliermarsh reviewed on 2024-09-01 16:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_builtins/helpers.rs`:12 on 2024-09-01 16:27_

Oh, I think the motivation here is that the builtins check is in `uv_python_stdlib` which doesn't depend on `PySourceType`.

---

_@AlexWaygood reviewed on 2024-09-01 16:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_builtins/helpers.rs`:12 on 2024-09-01 16:33_

Ah I see. You could probably just do it with a boolean `include_ipython_builtins` parameter... but also this doesn't need to be done in this PR!

---

_@AlexWaygood approved on 2024-09-01 16:34_

LGTM, though the CI has a complaint about the docs somewhere

---

_Merged by @charliermarsh on 2024-09-01 17:03_

---

_Closed by @charliermarsh on 2024-09-01 17:03_

---

_Branch deleted on 2024-09-01 17:03_

---

_Comment by @github-actions[bot] on 2024-09-01 17:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/4604387ae444a4f3478243f6d12f40b0cc48e121/testing/_py/test_local.py#L22'>testing/_py/test_local.py:22:45:</a> F821 Undefined name `EncodingWarning`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F821 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -14 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_core/_run.py#L54'>src/trio/_core/_run.py:54:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_core/_tests/test_exceptiongroup_gc.py#L16'>src/trio/_core/_tests/test_exceptiongroup_gc.py:16:32:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_core/_tests/test_run.py#L49'>src/trio/_core/_tests/test_run.py:49:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_core/_tests/test_run.py#L49'>src/trio/_core/_tests/test_run.py:49:52:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_highlevel_open_tcp_listeners.py#L16'>src/trio/_highlevel_open_tcp_listeners.py:16:32:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_highlevel_open_tcp_stream.py#L15'>src/trio/_highlevel_open_tcp_stream.py:15:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_highlevel_open_tcp_stream.py#L15'>src/trio/_highlevel_open_tcp_stream.py:15:52:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_tests/test_highlevel_open_tcp_listeners.py#L27'>src/trio/_tests/test_highlevel_open_tcp_listeners.py:27:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_tests/test_highlevel_open_tcp_stream.py#L25'>src/trio/_tests/test_highlevel_open_tcp_stream.py:25:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_tests/test_testing_raisesgroup.py#L14'>src/trio/_tests/test_testing_raisesgroup.py:14:32:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_tests/type_tests/raisesgroup.py#L24'>src/trio/_tests/type_tests/raisesgroup.py:24:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/_tests/type_tests/raisesgroup.py#L24'>src/trio/_tests/type_tests/raisesgroup.py:24:52:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/testing/_check_streams.py#L30'>src/trio/testing/_check_streams.py:30:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/51b2dffb916ae513e1c44b1afc243cdda072e690/src/trio/testing/_raises_group.py#L45'>src/trio/testing/_raises_group.py:45:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/4604387ae444a4f3478243f6d12f40b0cc48e121/testing/_py/test_local.py#L22'>testing/_py/test_local.py:22:45:</a> F821 Undefined name `EncodingWarning`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A004 | 14 | 0 | 14 | 0 | 0 |
| F821 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `preview` added by @dhruvmanila on 2024-09-05 15:23_

---

_Comment by @bonastreyair on 2024-09-06 06:30_

for some reason running using python 3.11 `anext` does not get properly detected.
I am getting this error: 

> F821 Undefined name `anext`

---

_Review comment by @bonastreyair on `crates/ruff_python_stdlib/src/builtins.rs`:90 on 2024-09-06 06:32_

`anext` is now missing, it does not appear in the new built-in list 

---

_@bonastreyair reviewed on 2024-09-06 06:32_

---

_@bonastreyair reviewed on 2024-09-06 06:38_

---

_Review comment by @bonastreyair on `crates/ruff_python_stdlib/src/builtins.rs`:115 on 2024-09-06 06:38_

as you can see here it does not appear the `anext` builtin

---

_Comment by @AlexWaygood on 2024-09-06 06:44_

@bonastreyair, what `--target-version` do you set when running Ruff? `anext` was added in Python 3.10. If you look closely you can see that we do still recognize `anext` as a Python builtin, but only if `--target-version` is set to Python 3.10 or newer. If it's set to an older version, we assume that you'll be interested in knowing that `anext` doesn't exist on that older version :-)

https://github.com/astral-sh/ruff/blob/a4ebe7d34407ea353762ebde1fef4a718edddb55/crates/ruff_python_stdlib/src/builtins.rs#L185-L187

---

_@MichaReiser reviewed on 2024-09-06 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/builtins.rs`:186 on 2024-09-06 06:45_

@bonastreyair `anext` is in the list if targeting python 3.10 or newer.

---

_Comment by @bonastreyair on 2024-09-06 06:51_

> @bonastreyair, what `--target-version` do you set when running Ruff? `anext` was added in Python 3.10. If you look closely you can see that we do still recognize `anext` as a Python builtin, but only if `--target-version` is set to Python 3.10 or newer. If it's set to an older version, we assume that you'll be interested in knowing that `anext` doesn't exist on that older version :-)
> 
> 
> 
> https://github.com/astral-sh/ruff/blob/a4ebe7d34407ea353762ebde1fef4a718edddb55/crates/ruff_python_stdlib/src/builtins.rs#L185-L187

I am using pre-commit with python target version 3.11 and in pyproject.toml with python3.11 so I don't know what is going wrong (even during CI it fails where only python3.11 is installed)

---

_@bonastreyair reviewed on 2024-09-06 06:57_

---

_Review comment by @bonastreyair on `crates/ruff_python_stdlib/src/builtins.rs`:186 on 2024-09-06 06:57_

sorry for the confusion did not spot that in the code I will investigate further why it is not working on my end then...

---

_Comment by @bonastreyair on 2024-09-06 07:58_

fixed adding `target-version = "py310"` in the `pyproject.toml` under the `[tool.ruff]` section

---
