```yaml
number: 4406
title: "Opt-out `tool.uv.sources` support for `uv add`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: ibraheem/uv-add-sources
created_at: 2024-06-18T22:56:31Z
updated_at: 2024-06-19T18:20:16Z
url: https://github.com/astral-sh/uv/pull/4406
synced_at: 2026-01-10T13:54:02Z
```

# Opt-out `tool.uv.sources` support for `uv add`

---

_Pull request opened by @ibraheemdev on 2024-06-18 22:56_

## Summary

After this change, `uv add` will try to use `tool.uv.sources` for all source requirements. If a source cannot be resolved, i.e. an ambiguous Git reference is provided, it will error. Git references can be specified with the `--tag`, `--branch`, or `--rev` arguments. Editables are also supported with `--editable`.

Users can opt-out of `tool.uv.sources` support with the `--raw` flag, which will force uv to use `project.dependencies`.

Part of https://github.com/astral-sh/uv/issues/3959.

---

_Label `preview` added by @ibraheemdev on 2024-06-18 22:56_

---

_Review requested from @konstin by @ibraheemdev on 2024-06-18 22:56_

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-18 22:56_

---

_Review comment by @ibraheemdev on `crates/uv/tests/edit.rs`:966 on 2024-06-18 22:58_

I'm not sure how to make `--editable` a Clap flag with it being represented as an `Option<bool>`. It's optional because we don't want to write `editable = true/false` if it's not provided as that's implied.

---

_@ibraheemdev reviewed on 2024-06-18 22:58_

---

_@zanieb reviewed on 2024-06-19 00:04_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:966 on 2024-06-19 00:04_

I think you're looking for [`default_missing_value`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_missing_value)

---

_Review comment by @konstin on `crates/uv-distribution/src/pyproject.rs`:240 on 2024-06-19 14:19_

We normally just do `to_string_lossy` and skip the error handling

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:520 on 2024-06-19 14:21_

Odd that the lockfile is different, shouldn't we disambiguate before locking (or merge all to rev)?

---

_@konstin approved on 2024-06-19 14:27_

---

_@ibraheemdev reviewed on 2024-06-19 18:19_

---

_Review comment by @ibraheemdev on `crates/uv/tests/edit.rs`:520 on 2024-06-19 18:19_

Opened https://github.com/astral-sh/uv/issues/4417.

---

_Merged by @ibraheemdev on 2024-06-19 18:20_

---

_Closed by @ibraheemdev on 2024-06-19 18:20_

---

_Branch deleted on 2024-06-19 18:20_

---
