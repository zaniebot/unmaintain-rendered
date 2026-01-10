```yaml
number: 13375
title: Update Black tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: micha/update-black-tests
created_at: 2024-09-17T09:09:16Z
updated_at: 2024-09-17T09:48:56Z
url: https://github.com/astral-sh/ruff/pull/13375
synced_at: 2026-01-10T21:30:32Z
```

# Update Black tests

---

_Pull request opened by @MichaReiser on 2024-09-17 09:09_

## Summary

Pull in the latest black snapshot tests

## Test Plan

`cargo test`. I reviewed the new snapshot tests


---

_Label `internal` added by @MichaReiser on 2024-09-17 09:09_

---

_Label `formatter` added by @MichaReiser on 2024-09-17 09:09_

---

_@MichaReiser reviewed on 2024-09-17 09:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__type_param_defaults.py.snap`:43 on 2024-09-17 09:10_

I prefer Ruff's formatting here which is more in line with other assignment formatting where the preference is to break the right side before breaking the left.

---

_@MichaReiser reviewed on 2024-09-17 09:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__type_param_defaults.py.snap`:52 on 2024-09-17 09:10_

I prefer Ruff's formatting here. Breaking the return type and arguments is more readable than breaking the type arguments (overall, prefer breaking the right before breaking the left)

---

_@MichaReiser reviewed on 2024-09-17 09:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pep_701.py.snap`:156 on 2024-09-17 09:11_

Related to https://github.com/astral-sh/ruff/issues/13237

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pattern_matching_with_if_stmt.py.snap`:54 on 2024-09-17 09:12_

Related to new preview style that parenthesizes long if conditions.

---

_@MichaReiser reviewed on 2024-09-17 09:12_

---

_@MichaReiser reviewed on 2024-09-17 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__function_trailing_comma.py.snap`:167 on 2024-09-17 09:13_

https://github.com/astral-sh/ruff/issues/13369

---

_@MichaReiser reviewed on 2024-09-17 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__form_feeds.py.snap`:171 on 2024-09-17 09:13_

Ruff does not support form feed characters.

@konstin I think you were right on the on-site. It's form feed characters :)

---

_@MichaReiser reviewed on 2024-09-17 09:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__dummy_implementations.py.snap`:148 on 2024-09-17 09:14_

Ruff does not preserve empty lines at the beginning of a class (intentional decision)

---

_@MichaReiser reviewed on 2024-09-17 09:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__comment_after_escaped_newline.py.snap`:32 on 2024-09-17 09:14_

Ruff does not preserve empty lines between the class and any leading comment (intentional decision)

---

_@MichaReiser reviewed on 2024-09-17 09:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__backslash_before_indent.py.snap`:27 on 2024-09-17 09:14_

Ruff does not preserve empty lines after a class (intentional deviation)

---

_Merged by @MichaReiser on 2024-09-17 09:16_

---

_Closed by @MichaReiser on 2024-09-17 09:16_

---

_Branch deleted on 2024-09-17 09:16_

---

_Comment by @github-actions[bot] on 2024-09-17 09:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 3 project errors)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (error)</summary>
<p>

```
Failed to clone apache/superset: error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
error: 3365 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>

```
Failed to clone zulip/zulip: error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
error: 992 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>

```
Failed to clone zanieb/huggingface-notebooks: fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@konstin reviewed on 2024-09-17 09:48_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__form_feeds.py.snap`:171 on 2024-09-17 09:48_

i thought we would, but maybe we only added this for the linter? https://github.com/astral-sh/ruff/pull/7489

---
