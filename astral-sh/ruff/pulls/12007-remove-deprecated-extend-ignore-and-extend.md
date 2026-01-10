```yaml
number: 12007
title: "Remove deprecated `extend-ignore` and `extend-unfixable` options"
type: pull_request
state: closed
author: MichaReiser
labels:
  - breaking
  - configuration
  - cli
assignees: []
base: ruff-0.5
head: remove-deprecated-ignore-and-unfixable-options
created_at: 2024-06-24T07:18:55Z
updated_at: 2024-08-12T07:54:42Z
url: https://github.com/astral-sh/ruff/pull/12007
synced_at: 2026-01-10T21:38:31Z
```

# Remove deprecated `extend-ignore` and `extend-unfixable` options

---

_Pull request opened by @MichaReiser on 2024-06-24 07:18_

## Summary

This PR removes the deprecated `lint.extend-ignore` and `lint.extend-unfixable` options.  The options were deprecated in https://github.com/astral-sh/ruff/pull/2312 (0.0.238)


Part of https://github.com/astral-sh/ruff/issues/7650

## Test Plan

Running ruff with a configuration containing `extend-ignore` or `extend-unfixable` now fails

```
❯ ../ruff/target/debug/ruff check test.py --no-cache
ruff failed
  Cause: Failed to parse /home/micha/astral/test/pyproject.toml
  Cause: TOML parse error at line 38, column 1
   |
38 | [tool.ruff.lint]
   | ^^^^^^^^^^^^^^^^
unknown field `extend-ignore`
```

Changing the option to `ignore` "ignores" the rule as expected.

---

_@MichaReiser reviewed on 2024-06-24 07:27_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:1264 on 2024-06-24 07:27_

This is a bit unfortunate. We never added deprecation warnings to the CLI. I'm inclined to keep and hide the CLI options for now but add a deprecation warning instead. 

It also seems that python-lsp-ruff is using the CLI option :( 

https://github.com/python-lsp/python-lsp-ruff/blob/34acd6ed6daaaa3f2756a1a1cf1bfc2d76df7d0f/pylsp_ruff/plugin.py#L611-L612

---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-24 07:39_

---

_Label `breaking` added by @MichaReiser on 2024-06-24 07:39_

---

_Marked ready for review by @MichaReiser on 2024-06-24 07:40_

---

_Added to milestone `v0.5.0` by @MichaReiser on 2024-06-24 08:27_

---

_Label `configuration` added by @MichaReiser on 2024-06-24 08:52_

---

_Label `cli` added by @MichaReiser on 2024-06-24 08:52_

---

_Comment by @charliermarsh on 2024-06-24 10:49_

Yeah, I would vote not to remove these. They were never properly deprecated. Honestly, I would vote not to deprecate them, even (or to un-deprecate them). I think it would be surprising to users that they don't exist. If they didn't exist, I would consider adding them, even though they're undifferentiated from `ignore` and `unfixable`.

---

_Comment by @github-actions[bot] on 2024-06-24 10:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 4 project errors)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/latchbio:latch/pyproject.toml
  Cause: TOML parse error at line 18, column 1
   |
18 | [tool.ruff]
   | ^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/python-trio:trio/pyproject.toml
  Cause: TOML parse error at line 102, column 1
    |
102 | [tool.ruff.lint]
    | ^^^^^^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/mesonbuild:meson-python/pyproject.toml
  Cause: TOML parse error at line 80, column 1
   |
80 | [tool.ruff]
   | ^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pdm-project:pdm/pyproject.toml
  Cause: TOML parse error at line 169, column 1
    |
169 | [tool.ruff.lint]
    | ^^^^^^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 4 project errors)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/latchbio:latch/pyproject.toml
  Cause: TOML parse error at line 18, column 1
   |
18 | [tool.ruff]
   | ^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/python-trio:trio/pyproject.toml
  Cause: TOML parse error at line 102, column 1
    |
102 | [tool.ruff.lint]
    | ^^^^^^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/mesonbuild:meson-python/pyproject.toml
  Cause: TOML parse error at line 80, column 1
   |
80 | [tool.ruff]
   | ^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pdm-project:pdm/pyproject.toml
  Cause: TOML parse error at line 169, column 1
    |
169 | [tool.ruff.lint]
    | ^^^^^^^^^^^^^^^^
unknown field `extend-ignore`
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-06-24 11:47_

I'll create an issue to follow up on this. Closing as per @charliermarsh's comment.

Follow up in https://github.com/astral-sh/ruff/issues/12014

---

_Closed by @MichaReiser on 2024-06-24 11:47_

---

_Branch deleted on 2024-08-12 07:54_

---
