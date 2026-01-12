```yaml
number: 12009
title: Remove deprecated top-level lint settings
type: pull_request
state: closed
author: MichaReiser
labels:
  - breaking
  - configuration
  - cli
assignees: []
base: ruff-0.5
head: remove-deprecated-top-level-lint-settings
created_at: 2024-06-24T08:17:19Z
updated_at: 2024-08-12T07:54:41Z
url: https://github.com/astral-sh/ruff/pull/12009
synced_at: 2026-01-12T15:55:40Z
```

# Remove deprecated top-level lint settings

---

_@MichaReiser_

## Summary

This PR removes the top-level lint settings in favor of the `lint.*` settings. 

The top-level settings were deprecated when Ruff formatter was released in October 2023. 

Part of https://github.com/astral-sh/ruff/issues/7650

## Test Plan

Verified the updated integration tests taht show, that the top-level settings are no longer accepted. 


---

_Added to milestone `v0.5.0` by @MichaReiser on 2024-06-24 08:17_

---

_Label `breaking` added by @MichaReiser on 2024-06-24 08:53_

---

_Label `configuration` added by @MichaReiser on 2024-06-24 08:53_

---

_Label `cli` added by @MichaReiser on 2024-06-24 08:53_

---

_Comment by @github-actions[bot] on 2024-06-24 10:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 12 project errors)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/PostHog:HouseWatch/pyproject.toml
  Cause: TOML parse error at line 16, column 1
   |
16 | select = [
   | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/RasaHQ:rasa/pyproject.toml
  Cause: TOML parse error at line 348, column 1
    |
348 | ignore = [
    | ^^^^^^
unknown field `ignore`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/aiven:aiven-client/pyproject.toml
  Cause: TOML parse error at line 95, column 1
   |
95 | extend-select = [
   | ^^^^^^^^^^^^^
unknown field `extend-select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/bloomberg:pytest-memray/pyproject.toml
  Cause: TOML parse error at line 157, column 1
    |
157 | ignore = [
    | ^^^^^^
unknown field `ignore`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/bokeh:bokeh/pyproject.toml
  Cause: TOML parse error at line 236, column 1
    |
236 | select = [
    | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/demisto:content/pyproject.toml
  Cause: TOML parse error at line 93, column 1
   |
93 | select = [
   | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/docker:docker-py/pyproject.toml
  Cause: TOML parse error at line 82, column 1
   |
82 | extend-select = [
   | ^^^^^^^^^^^^^
unknown field `extend-select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/freedomofpress:securedrop/pyproject.toml
  Cause: TOML parse error at line 13, column 1
   |
13 | select = [
   | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/jrnl-org:jrnl/pyproject.toml
  Cause: TOML parse error at line 131, column 1
    |
131 | select = [
    | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/latchbio:latch/pyproject.toml
  Cause: TOML parse error at line 20, column 1
   |
20 | extend-select = [
   | ^^^^^^^^^^^^^
unknown field `extend-select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/model-bakers:model_bakery/pyproject.toml
  Cause: TOML parse error at line 105, column 1
    |
105 | select = [
    | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/mesonbuild:meson-python/pyproject.toml
  Cause: TOML parse error at line 82, column 1
   |
82 | extend-ignore = [
   | ^^^^^^^^^^^^^
unknown field `extend-ignore`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 12 project errors)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/PostHog:HouseWatch/pyproject.toml
  Cause: TOML parse error at line 16, column 1
   |
16 | select = [
   | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/RasaHQ:rasa/pyproject.toml
  Cause: TOML parse error at line 348, column 1
    |
348 | ignore = [
    | ^^^^^^
unknown field `ignore`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/aiven:aiven-client/pyproject.toml
  Cause: TOML parse error at line 95, column 1
   |
95 | extend-select = [
   | ^^^^^^^^^^^^^
unknown field `extend-select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/bloomberg:pytest-memray/pyproject.toml
  Cause: TOML parse error at line 157, column 1
    |
157 | ignore = [
    | ^^^^^^
unknown field `ignore`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/bokeh:bokeh/pyproject.toml
  Cause: TOML parse error at line 236, column 1
    |
236 | select = [
    | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/demisto:content/pyproject.toml
  Cause: TOML parse error at line 93, column 1
   |
93 | select = [
   | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/docker:docker-py/pyproject.toml
  Cause: TOML parse error at line 82, column 1
   |
82 | extend-select = [
   | ^^^^^^^^^^^^^
unknown field `extend-select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/freedomofpress:securedrop/pyproject.toml
  Cause: TOML parse error at line 13, column 1
   |
13 | select = [
   | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/jrnl-org:jrnl/pyproject.toml
  Cause: TOML parse error at line 131, column 1
    |
131 | select = [
    | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/latchbio:latch/pyproject.toml
  Cause: TOML parse error at line 20, column 1
   |
20 | extend-select = [
   | ^^^^^^^^^^^^^
unknown field `extend-select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/model-bakers:model_bakery/pyproject.toml
  Cause: TOML parse error at line 105, column 1
    |
105 | select = [
    | ^^^^^^
unknown field `select`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
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
  Cause: TOML parse error at line 82, column 1
   |
82 | extend-ignore = [
   | ^^^^^^^^^^^^^
unknown field `extend-ignore`, expected one of `cache-dir`, `extend`, `output-format`, `fix`, `unsafe-fixes`, `fix-only`, `show-source`, `show-fixes`, `required-version`, `preview`, `exclude`, `extend-exclude`, `extend-include`, `force-exclude`, `include`, `respect-gitignore`, `builtins`, `namespace-packages`, `target-version`, `src`, `line-length`, `indent-width`, `tab-size`, `lint`, `format`
```

</p>
</details>




---

_Comment by @charliermarsh on 2024-06-24 10:52_

Now having seen the ecosystem checks, I think I'd prefer to leave this for now. It seems like it will cause a lot of churn and pain for users, and it doesn't unlock anything new for us. What do you think?

---

_Comment by @MichaReiser on 2024-06-24 11:32_

Yeah this seems to disruptive. We'll need better tooling to support users doing the migration. Closing and deferring until later

---

_Closed by @MichaReiser on 2024-06-24 11:32_

---

_Branch deleted on 2024-08-12 07:54_

---
