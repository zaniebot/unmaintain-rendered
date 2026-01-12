```yaml
number: 968
title: Add JUnit xml output format
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: junit
created_at: 2022-11-30T04:39:12Z
updated_at: 2022-11-30T05:48:37Z
url: https://github.com/astral-sh/ruff/pull/968
synced_at: 2026-01-12T15:55:05Z
```

# Add JUnit xml output format

---

_@messense_

Closes #963 

---

_Review comment by @messense on `Cargo.toml`:40 on 2022-11-30 04:39_

https://github.com/nextest-rs/nextest/tree/main/quick-junit

---

_@messense reviewed on 2022-11-30 04:39_

---

_Comment by @messense on 2022-11-30 04:40_

I'm not sure how to organize the output, needs more research, currently:

```bash
cargo run resources/test/fixtures/A001.py --format junit
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/ruff resources/test/fixtures/A001.py --format junit`
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="ruff" tests="4" failures="4" errors="0">
    <testsuite name="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" tests="4" disabled="0" errors="0" failures="4">
        <testcase name="Pyflakes" file="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="1">
            <failure message="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:1:8: `some` imported but unused" type="F401"/>
        </testcase>
        <testcase name="Pyflakes" file="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="2">
            <failure message="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:2:18: `some.other` imported but unused" type="F401"/>
        </testcase>
        <testcase name="Pyflakes" file="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="5">
            <failure message="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:5:12: Undefined name `annotation`" type="F821"/>
        </testcase>
        <testcase name="Pyflakes" file="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="18">
            <failure message="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:18:1: Local variable `ValueError` is assigned to but never used" type="F841"/>
        </testcase>
    </testsuite>
</testsuites>
```

Some questions:

* `testsuites name="ruff"` how should we fill `testsuites.name` value?
* `testsuite name="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py"` how should we fill `testsuite.name` value?
* `testcase name="Pyflakes"` how should we fill `testcase.name` value?
* `<failure message="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:18:1: Local variable `ValueError` is assigned to but never used" type="F841"/>` how should we fill the `failure.type` value?
* Should we make file names relative instead of absolute?

---

_Comment by @messense on 2022-11-30 04:47_

References:

* [flake8-formatter-junit-xml](https://github.com/astj/flake8-formatter-junit-xml): https://github.com/astj/flake8-formatter-junit-xml/blob/master/tests/expected.xml

---

_Comment by @charliermarsh on 2022-11-30 04:53_

This is great! I have to look into the conventions here too.

---

_Comment by @charliermarsh on 2022-11-30 04:54_

Another reference: https://eslint.org/docs/latest/user-guide/formatters/#junit

---

_Comment by @charliermarsh on 2022-11-30 04:56_

So for ESLint:

- `testsuites` (name is empty)
- `testsuite package="org.ruff" name="/path/to/file.py"`
- `testcase name="org.ruff.F841" classname="/path/to/file"` (maybe `path.to.module`?)
- They don't seem to use any failure type, just the message

---

_Review comment by @messense on `src/commands.rs`:65 on 2022-11-30 04:59_

I don't think it's useful to have `--explain` to output junit xml.

---

_@messense reviewed on 2022-11-30 04:59_

---

_@charliermarsh reviewed on 2022-11-30 05:02_

---

_Review comment by @charliermarsh on `src/commands.rs`:65 on 2022-11-30 05:02_

I agree, log some feedback?

---

_Comment by @messense on 2022-11-30 05:16_

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="ruff" tests="4" failures="4" errors="0">
    <testsuite name="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" tests="4" disabled="0" errors="0" failures="4" package="org.ruff">
        <testcase name="org.ruff.F401" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="1" column="8">
            <failure message="`some` imported but unused"/>
        </testcase>
        <testcase name="org.ruff.F401" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="2" column="18">
            <failure message="`some.other` imported but unused"/>
        </testcase>
        <testcase name="org.ruff.F821" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="5" column="12">
            <failure message="Undefined name `annotation`"/>
        </testcase>
        <testcase name="org.ruff.F841" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" line="18" column="1">
            <failure message="Local variable `ValueError` is assigned to but never used"/>
        </testcase>
    </testsuite>
</testsuites>
```

* I guess `testsuites.name` is irrelevant, but `quick-junit` requires a name so give it `ruff`.
* Should we add `end_location` to `testcase`? `location` is already added as `line` and `column`.

---

_@charliermarsh reviewed on 2022-11-30 05:27_

---

_Review comment by @charliermarsh on `src/printer.rs`:122 on 2022-11-30 05:27_

The ESLint version strips the `.js` suffix, so maybe we should do that? I'm not sure.

---

_Comment by @charliermarsh on 2022-11-30 05:28_

I think it's fine to omit the end location.

---

_@messense reviewed on 2022-11-30 05:34_

---

_Review comment by @messense on `src/printer.rs`:122 on 2022-11-30 05:34_

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="ruff" tests="4" failures="4" errors="0">
    <testsuite name="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" tests="4" disabled="0" errors="0" failures="4" package="org.ruff">
        <testcase name="org.ruff.F401" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="1" column="8">
            <failure message="`some` imported but unused">/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:1:8: `some` imported but unused</failure>
        </testcase>
        <testcase name="org.ruff.F401" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="2" column="18">
            <failure message="`some.other` imported but unused">/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:2:18: `some.other` imported but unused</failure>
        </testcase>
        <testcase name="org.ruff.F821" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="5" column="12">
            <failure message="Undefined name `annotation`">/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:5:12: Undefined name `annotation`</failure>
        </testcase>
        <testcase name="org.ruff.F841" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="18" column="1">
            <failure message="Local variable `ValueError` is assigned to but never used">/Users/messense/Projects/ruff/resources/test/fixtures/A001.py:18:1: Local variable `ValueError` is assigned to but never used</failure>
        </testcase>
    </testsuite>
</testsuites>
```

---

_Marked ready for review by @messense on 2022-11-30 05:35_

---

_@charliermarsh reviewed on 2022-11-30 05:40_

---

_Review comment by @charliermarsh on `src/printer.rs`:122 on 2022-11-30 05:40_

Can we omit the filename from the message, like in ESLint?

---

_@messense reviewed on 2022-11-30 05:45_

---

_Review comment by @messense on `src/printer.rs`:122 on 2022-11-30 05:45_

Done.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="ruff" tests="4" failures="4" errors="0">
    <testsuite name="/Users/messense/Projects/ruff/resources/test/fixtures/A001.py" tests="4" disabled="0" errors="0" failures="4" package="org.ruff">
        <testcase name="org.ruff.F401" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="1" column="8">
            <failure message="`some` imported but unused">line 1, col 8, `some` imported but unused</failure>
        </testcase>
        <testcase name="org.ruff.F401" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="2" column="18">
            <failure message="`some.other` imported but unused">line 2, col 18, `some.other` imported but unused</failure>
        </testcase>
        <testcase name="org.ruff.F821" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="5" column="12">
            <failure message="Undefined name `annotation`">line 5, col 12, Undefined name `annotation`</failure>
        </testcase>
        <testcase name="org.ruff.F841" classname="/Users/messense/Projects/ruff/resources/test/fixtures/A001" line="18" column="1">
            <failure message="Local variable `ValueError` is assigned to but never used">line 18, col 1, Local variable `ValueError` is assigned to but never used</failure>
        </testcase>
    </testsuite>
</testsuites>
```

---

_Merged by @charliermarsh on 2022-11-30 05:47_

---

_Closed by @charliermarsh on 2022-11-30 05:47_

---

_Comment by @charliermarsh on 2022-11-30 05:47_

Stellar, thank you!

---

_Branch deleted on 2022-11-30 05:48_

---
