```yaml
number: 16550
title: "[`refurb`] Fix starred expressions fix (`FURB161`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: FURB161
created_at: 2025-03-07T11:08:31Z
updated_at: 2025-03-19T21:43:58Z
url: https://github.com/astral-sh/ruff/pull/16550
synced_at: 2026-01-10T19:40:36Z
```

# [`refurb`] Fix starred expressions fix (`FURB161`)

---

_Pull request opened by @VascoSch92 on 2025-03-07 11:08_

The PR partially solves issue #16457

Specifically, it solves the following problem:

```text
$ cat >furb161_1.py <<'# EOF'
print(bin(*[123]).count("1"))
# EOF

$ python furb161_1.py
6

$ ruff --isolated check --target-version py310 --preview --select FURB161 furb161_1.py --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

Now starred expressions are corrected handled.

---

_Comment by @github-actions[bot] on 2025-03-07 11:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `bug` added by @ntBre on 2025-03-07 17:53_

---

_Label `fixes` added by @ntBre on 2025-03-07 17:53_

---

_Comment by @MichaReiser on 2025-03-14 08:27_

@ntBre could you take a look at this PR

---

_Review requested from @ntBre by @MichaReiser on 2025-03-14 08:27_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/bit_count.rs`:131 on 2025-03-14 13:38_

I kind of think we should just return immediately if we see a starred expression instead of trying to do this unpacking. We would also need to do this for tuples. However, this only works for the exact case of a literal list or tuple, and I don't think code like `*[123]` or `*(123,)` is very likely in real scenarios (you would just write `123` directly). I was going to say we need to handled starred variables, but that's a syntax error:

```pycon
>>> (*lst).bit_count()
  File "<python-input-7>", line 1
    (*lst).bit_count()
     ^^^^
SyntaxError: cannot use starred expression here
>>>
```

In short, I think it's better to return early for *any* starred expression to save us the work of unpacking expressions that should be very rare in practice.

---

_@ntBre requested changes on 2025-03-14 13:41_

Thanks for doing this! I wrote a longer comment below explaining why I think we should just return on any starred expression, but I'm interested to hear your thoughts.

---

_Comment by @VascoSch92 on 2025-03-16 19:48_

> Thanks for doing this! I wrote a longer comment below explaining why I think we should just return on any starred expression, but I'm interested to hear your thoughts.

Thank you for the feedback. I don't have strong opinions on that matter. I agree that returning in the case of starred expressions makes perfect sense.

I have updated the code accordingly. Please let me know if it aligns with your suggestion


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/bit_count.rs`:115 on 2025-03-18 14:25_

nit: is there something like an `is_starred` or `is_starred_expr` method on `arg`? I'd probably just add

```rust
if arg.is_starred_expr() {  // or matches!(arg, Expr::Starred(_)) if there's no existing method
    return;
}
```

above the old code for `literal_text = ...`.

---

_@ntBre approved on 2025-03-18 14:25_

Thanks for following up! I have one more nit, but this otherwise looks good to me. If you push another commit or close and reopen the PR, it should trigger CI.

---

_Comment by @VascoSch92 on 2025-03-19 21:22_

> Thanks for following up! I have one more nit, but this otherwise looks good to me. If you push another commit or close and reopen the PR, it should trigger CI.

`is_starred_expr` is the correct function ;-) 
I changed.

---

_Renamed from "[refurb] Fix starred expressions fix (FURB161)" to "[`refurb`] Fix starred expressions fix (`FURB161`)" by @ntBre on 2025-03-19 21:25_

---

_Merged by @ntBre on 2025-03-19 21:43_

---

_Closed by @ntBre on 2025-03-19 21:43_

---
