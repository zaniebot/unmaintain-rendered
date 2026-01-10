```yaml
number: 9588
title: "Formatter seems to ignore `#fmt: off`"
type: issue
state: closed
author: mkismy
labels:
  - formatter
assignees: []
created_at: 2024-01-20T02:41:50Z
updated_at: 2024-02-07T15:33:20Z
url: https://github.com/astral-sh/ruff/issues/9588
synced_at: 2026-01-10T11:09:51Z
```

# Formatter seems to ignore `#fmt: off`

---

_Issue opened by @mkismy on 2024-01-20 02:41_

`ruff format` seems to ignore `#fmt: off/on` and formatting everything on v0.1.8.

```python
test_payload = {
    # fmt: off
    "a": {
        "payload": "with",
        "some": {
            "nested": "json",
            "which": "shouldn't be formatted"
        }
    },
    # fmt: on
    "this": {
        "should": "be",
        "formatted": "well"
    },
    "foo": "bar"
}
```

is formatted to:

```python
test_payload = {
    # fmt: off
    "a": {"payload": "with", "some": {"nested": "json", "which": "shouldn't be formatted"}},
    # fmt: on
    "this": {"should": "be", "formatted": "well"},
    "foo": "bar",
}
```
sample: https://play.ruff.rs/728b4856-041d-4622-b991-f94d51d8d7fd

---

_Comment by @charliermarsh on 2024-01-20 03:11_

Thanks for writing this up. Unfortunately suppression comments are only respected at the statement level right now -- this is covered in [the docs](https://docs.astral.sh/ruff/formatter/#format-suppression) -- so, e.g., it'd need to be:

```python
# fmt: off
test_payload = {
    "a": {
        "payload": "with",
        "some": {
            "nested": "json",
            "which": "shouldn't be formatted"
        }
    },
    "this": {
        "should": "be",
        "formatted": "well"
    },
    "foo": "bar"
}
# fmt: on
```

Suppression comments within expressions are tricky (here are some confusing behaviors from [Black](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AObASldAD2IimZxl1N_Wg0-BMH4fcURaW_LLW3WS9oop2fRY6ZYC1cI8TycDNhlQOgjnQLCChkMJfQ18_jw8XEmKxgRVOYtgkJFYbl9i8jW2ALzMXyX4YIMlsGi1DPA0id2tA_vCWf3WpKPVq5pIwEbu5r0zN5504jhWs53ZkH6NITpNQxqRKhtnTXdnaC2z6wFsERlPb3rRnu6-XzLUBpKdrSofNBe8L3KCo_EDrGk2MU1GKGfVWHMm-2uGmpTz_6BOK5foqv73dQ0J5YbG592iFkKQFgEaE3My_RYE6_oaZ1UVfyMn09R_mQz1yiFs34jk8XMn2pU7beG3BvZYU3LbyVLD_kK7u8e83DELVgOo6f0NeS8bSQ4vxY7Qi7H9pUDTvzzkCeb9fxuB6kD4AAAAADza8uOe33xhgABxQKcBwAAI0BpCrHEZ_sCAAAAAARZWg==), as an example) so we limited the scope intentionally for now.


---

_Label `formatter` added by @charliermarsh on 2024-01-20 03:11_

---

_Comment by @charliermarsh on 2024-01-20 03:12_

Going to close as "working as intended" but happy to answer more questions and will take this as a vote to consider increasing suppression comment flexibility in the future.

---

_Closed by @charliermarsh on 2024-01-20 03:12_

---

_Comment by @mkismy on 2024-01-20 03:42_

Thanks for checking. Change the position of suppression comments did work.

I was doing the switch from black to ruff and encountered this unexpected amount of diffs. I understand covering every edge cases are difficult but it would be nice if ruff can support more patterns gradually and reduce surprises like I had.

---

_Comment by @mdczaplicki on 2024-02-07 09:41_

Are there plans to change the implementation?

---

_Comment by @MichaReiser on 2024-02-07 13:09_

> Are there plans to change the implementation?

There are currently no plans to change it because supporting more fine granular suppression comments comes with a significant increase in complexity, and we would rather focus on improving the formatting (fixing the root cause of why you need suppressions) rather than building a more powerful suppression system. 

You can help us prioritize by creating an issue for the case where you have to fall back to using suppression comments because the formatting doesn't look good. Is there a specific pattern you use it for? How do you manually format that code? That gives us an excellent starting point for discussing on how formatting can be improved.

---

_Comment by @mdczaplicki on 2024-02-07 15:33_

Here you go: https://github.com/astral-sh/ruff/issues/9875
:)

---
