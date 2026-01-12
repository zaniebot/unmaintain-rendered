```yaml
number: 11566
title: Use default settings if initialization options is empty or not provided
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/avoid-flatten
created_at: 2024-05-27T11:23:53Z
updated_at: 2024-05-27T15:36:35Z
url: https://github.com/astral-sh/ruff/pull/11566
synced_at: 2026-01-12T15:55:38Z
```

# Use default settings if initialization options is empty or not provided

---

_@dhruvmanila_

## Summary

This PR fixes the bug to avoid flattening the global-only settings for the new server.

This was added in https://github.com/astral-sh/ruff/pull/11497, possibly to correctly de-serialize an empty value (`{}`). But, this lead to a bug where the configuration under the `settings` key was not being read for global-only variant.

By using #[serde(default)], we ensure that the settings field in the `GlobalOnly` variant is optional and that an empty JSON object `{}` is correctly deserialized into `GlobalOnly` with a default `ClientSettings` instance.

fixes: #11507 

## Test Plan

Update the snapshot and existing test case. Also, verify the following settings in Neovim:

1. Nothing

```lua
ruff = {
  cmd = {
    '/Users/dhruv/work/astral/ruff/target/debug/ruff',
    'server',
    '--preview',
  },
}
```

2. Empty dictionary

```lua
ruff = {
  cmd = {
    '/Users/dhruv/work/astral/ruff/target/debug/ruff',
    'server',
    '--preview',
  },
  init_options = vim.empty_dict(),
}
```

3. Empty `settings`

```lua
ruff = {
  cmd = {
    '/Users/dhruv/work/astral/ruff/target/debug/ruff',
    'server',
    '--preview',
  },
  init_options = {
    settings = vim.empty_dict(),
  },
}
```

4. With some configuration:

```lua
ruff = {
  cmd = {
    '/Users/dhruv/work/astral/ruff/target/debug/ruff',
    'server',
    '--preview',
  },
  init_options = {
    settings = {
      configuration = '/tmp/ruff-repro/pyproject.toml',
    },
  },
}
```



---

_Comment by @dhruvmanila on 2024-05-27 11:27_

This might not be the correct solution because now the empty initialization options fail to deserialize.

---

_Label `bug` added by @dhruvmanila on 2024-05-27 12:18_

---

_Label `server` added by @dhruvmanila on 2024-05-27 12:18_

---

_Comment by @codspeed-hq[bot] on 2024-05-27 12:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/avoid-flatten)

### Merging #11566 will **improve performances by 11.03%**

<sub>Comparing <code>dhruv/avoid-flatten</code> (0b9fd71) with <code>main</code> (6be00d5)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/avoid-flatten` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1.2 ms | 1 ms | +11.03% |


---

_Marked ready for review by @dhruvmanila on 2024-05-27 12:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-27 12:24_

---

_Renamed from "Avoid flattening global only server settings" to "Use default settings if initialization options is empty or not provided" by @dhruvmanila on 2024-05-27 12:25_

---

_Comment by @github-actions[bot] on 2024-05-27 12:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-05-27 13:11_

If we have none. Should we add a test for an empty settings object?

---

_Comment by @dhruvmanila on 2024-05-27 15:36_

There is a test :)

https://github.com/astral-sh/ruff/blob/0b9fd71e68bcea1b3901554bd19c08361b53a511/crates/ruff_server/src/session/settings.rs#L710-L715

---

_Merged by @dhruvmanila on 2024-05-27 15:36_

---

_Closed by @dhruvmanila on 2024-05-27 15:36_

---

_Branch deleted on 2024-05-27 15:36_

---
