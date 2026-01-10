```yaml
number: 14624
title: Do not consider f-strings with escaped newlines as multiline
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/f-string-multiline-triple-quoted
created_at: 2024-11-27T05:05:14Z
updated_at: 2024-11-27T10:27:07Z
url: https://github.com/astral-sh/ruff/pull/14624
synced_at: 2026-01-10T20:50:57Z
```

# Do not consider f-strings with escaped newlines as multiline

---

_Pull request opened by @dhruvmanila on 2024-11-27 05:05_

## Summary

This PR fixes a bug in the f-string formatting to not consider the escaped newlines for `is_multiline`. This is done by checking if the f-string is triple-quoted or not similar to normal string literals.

This is not required to be gated behind preview because the logic change for `is_multiline` was added in https://github.com/astral-sh/ruff/pull/14454.

## Test Plan

Add a test case which formats differently on `main`: https://play.ruff.rs/ea3c55c2-f0fe-474e-b6b8-e3365e0ede5e


---

_Label `bug` added by @dhruvmanila on 2024-11-27 05:05_

---

_Label `formatter` added by @dhruvmanila on 2024-11-27 05:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-27 05:05_

---

_Comment by @github-actions[bot] on 2024-11-27 05:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `preview` added by @MichaReiser on 2024-11-27 07:34_

---

_Comment by @MichaReiser on 2024-11-27 07:34_

> This is not required to be gated behind preview because the logic change for is_multiline was added in https://github.com/astral-sh/ruff/pull/14454.

I'm not sure I understand this part. Isn't the reason why no preview gating is required because this new logic is only used in preview mode?

---

_Comment by @dhruvmanila on 2024-11-27 07:38_

> Isn't the reason why no preview gating is required because this new logic is only used in preview mode?

The `is_multiline` is also used in binary expression formatting which isn't gated behind preview. You can see that in the playground by turning off the `preview` flag: https://play.ruff.rs/8ef99e9b-ea54-44b4-a483-f7339424865d. The logic for f-strings was changed from just checking for newlines in the entire f-string to checking individual elements in the linked PR.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/binary.py`:430 on 2024-11-27 07:39_

Could we add some tests in assignment positions and with implicit concatenated strings (especially with inline comments) just to make sure strings with line continuations are handled correctly in those positions too

---

_@MichaReiser approved on 2024-11-27 07:41_

Looking at https://github.com/astral-sh/ruff/pull/14454 `is_multiline` again, I think the change we made there has a high potential for unwanted stable style changes. We should change the `is_multiline` to keep using the "old" `memchr2(b'\n', b'\r', source[fstring.range].as_bytes()).is_some()` if f-string formatting is disabled and only use the new logic if f-string formatting is enabled. We otherwise risk that bugs like the one you found here impact stable style users. 



---

_Comment by @dhruvmanila on 2024-11-27 07:50_

> I think the change we made there has a high potential for unwanted stable style changes. We should change the `is_multiline` to keep using the "old" `memchr2(b'\n', b'\r', source[fstring.range].as_bytes()).is_some()` if f-string formatting is disabled and only use the new logic if f-string formatting is enabled. We otherwise risk that bugs like the one you here impact users of our stable style.

I see, I think that makes sense. I'll do that as a follow-up.

---

_@dhruvmanila reviewed on 2024-11-27 08:48_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/binary.py`:430 on 2024-11-27 08:48_

Adding these test case reveals one more change in implicitly concatenated strings. Given:
```py
f"hellooooooooooooooooooooooo \
        worldddddddddddddddddd" "another string"
```

On `main`, we don't concatenate it because the f-string is considered as multiline but here we would. I'm going to make the `is_multiline` change gated behind preview first, rebase this PR and add the test cases.

---

_@MichaReiser reviewed on 2024-11-27 08:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/binary.py`:430 on 2024-11-27 08:49_

I think concatenating the string makes sense as long as it fits into the line length

---

_Merged by @dhruvmanila on 2024-11-27 10:25_

---

_Closed by @dhruvmanila on 2024-11-27 10:25_

---

_Branch deleted on 2024-11-27 10:25_

---
