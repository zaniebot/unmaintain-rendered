```yaml
number: 16494
title: Clarify that D417 only checks docstrings with an arguments section
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/d417-documentation
created_at: 2025-03-04T10:21:29Z
updated_at: 2025-03-06T09:53:44Z
url: https://github.com/astral-sh/ruff/pull/16494
synced_at: 2026-01-12T15:55:55Z
```

# Clarify that D417 only checks docstrings with an arguments section

---

_@MichaReiser_

## Summary

This came up in https://github.com/astral-sh/ruff/issues/16477

It's not obvious from the D417 rule's documentation that it only checks docstrings
with an arguments section. Functions without such a section aren't checked. 

This PR tries to make this clearer in the documentation.



---

_Label `documentation` added by @MichaReiser on 2025-03-04 10:21_

---

_Comment by @github-actions[bot] on 2025-03-04 10:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>

```
Failed to clone RasaHQ/rasa: error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 2876 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
Failed to clone zulip/zulip: error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 4078 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-03-05 16:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1173 on 2025-03-05 17:03_

This line looks like it might have gotten cut off somehow. And did you want to remove the note about the `google` convention? It is mentioned earlier in the docs, but this seems to add a little more clarification.

I think the change on 1170 might be enough here. (actually I slightly prefer line 1172, but either of those on line 1170 would be great)

I can't add a suggestion on deleted lines, but my suggestion is:

```rust
/// Note that this rule only checks docstrings with an arguments (e.g. `Args`) section.
///
/// This rule is enabled when using the `google` convention, and disabled when
/// using the `pep257` and `numpy` conventions.
```

---

_@ntBre approved on 2025-03-05 17:03_

I added one comment, but thanks for doing this!

---

_@MichaReiser reviewed on 2025-03-06 09:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1173 on 2025-03-06 09:45_

> And did you want to remove the note about the google convention?

Definitely not. Thanks for catching this!

---

_Merged by @MichaReiser on 2025-03-06 09:49_

---

_Closed by @MichaReiser on 2025-03-06 09:49_

---

_Branch deleted on 2025-03-06 09:49_

---
