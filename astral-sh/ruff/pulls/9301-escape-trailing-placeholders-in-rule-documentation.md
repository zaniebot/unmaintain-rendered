```yaml
number: 9301
title: Escape trailing placeholders in rule documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/html
created_at: 2023-12-28T14:34:55Z
updated_at: 2023-12-28T14:44:52Z
url: https://github.com/astral-sh/ruff/pull/9301
synced_at: 2026-01-10T23:07:18Z
```

# Escape trailing placeholders in rule documentation

---

_Pull request opened by @charliermarsh on 2023-12-28 14:34_

## Summary

If a rule ends with a trailing placeholder (like "Use {target}"), that gets interpreted as an HTML attribute adding, `target="target"` to the node. This PR escapes such cases. In reality, they're rare, since we almost always wrap placeholders in backticks, which avoids this problem -- but in some cases, they are in fact correct to be un-backticked.

Closes https://github.com/astral-sh/ruff/issues/9288.

## Test Plan

<img width="673" alt="Screen Shot 2023-12-28 at 9 33 40 AM" src="https://github.com/astral-sh/ruff/assets/1309177/14aaa168-c802-4258-b82d-217a85b42ebf">


---

_Label `bug` added by @charliermarsh on 2023-12-28 14:34_

---

_Comment by @github-actions[bot] on 2023-12-28 14:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The following rules may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.inline-quotes="single"` option is incompatible with the formatter's `format.quote-style="double"`. We recommend disabling `Q000` and `Q003` when using the formatter, which enforces a consistent quote style. Alternatively, set both options to either `"single"` or `"double"`.
warning: Detected debug build without --no-cache.
error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: The following rules may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.inline-quotes="single"` option is incompatible with the formatter's `format.quote-style="double"`. We recommend disabling `Q000` and `Q003` when using the formatter, which enforces a consistent quote style. Alternatively, set both options to either `"single"` or `"double"`.
warning: Detected debug build without --no-cache.
error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8
```

</p>
</details>




---

_Merged by @charliermarsh on 2023-12-28 14:44_

---

_Closed by @charliermarsh on 2023-12-28 14:44_

---

_Branch deleted on 2023-12-28 14:44_

---
