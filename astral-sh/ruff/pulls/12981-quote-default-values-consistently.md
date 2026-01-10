```yaml
number: 12981
title: Quote default values consistently
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2024-08-19T07:37:43Z
updated_at: 2024-08-19T08:12:05Z
url: https://github.com/astral-sh/ruff/pull/12981
synced_at: 2026-01-10T21:38:32Z
```

# Quote default values consistently

---

_Pull request opened by @InSyncWithFoo on 2024-08-19 07:37_

Fixes #12979 by quoting all unquoted values.

I also changed the type of `dummy-variable-rgx` from `re.Pattern` to `str` since the former is misleading (unless Ruff is silently triggering Python's regex engine under the hood).

---

_Label `cli` added by @MichaReiser on 2024-08-19 07:48_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-08-19 07:56_

---

_@MichaReiser approved on 2024-08-19 07:58_

Thanks

---

_Merged by @MichaReiser on 2024-08-19 08:02_

---

_Closed by @MichaReiser on 2024-08-19 08:02_

---

_Comment by @github-actions[bot] on 2024-08-19 08:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
Failed to clone mlflow/mlflow: error: RPC failed; curl 55 Failed sending HTTP2 data
error: 179 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to clone mlflow/mlflow: error: RPC failed; curl 55 Failed sending HTTP2 data
error: 988 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>




---
