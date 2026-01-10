```yaml
number: 17826
title: "[`pylint`] add fix safety section (`PLR1722`)"
type: pull_request
state: merged
author: yunchipang
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-safety-sys-exit-alias
created_at: 2025-05-04T04:16:50Z
updated_at: 2025-06-07T01:47:46Z
url: https://github.com/astral-sh/ruff/pull/17826
synced_at: 2026-01-10T18:45:04Z
```

# [`pylint`] add fix safety section (`PLR1722`)

---

_Pull request opened by @yunchipang on 2025-05-04 04:16_

parent: #15584 
fix introduced at: #816 


---

_Label `documentation` added by @AlexWaygood on 2025-05-04 09:58_

---

_Comment by @github-actions[bot] on 2025-05-04 10:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @VascoSch92 on 2025-05-04 18:25_

I think the fix is always unsafe...

---

_Comment by @yunchipang on 2025-05-04 23:56_

@VascoSch92 Thanks for the input! I’m new to the project—could you elaborate a bit? I was referencing this:
https://github.com/astral-sh/ruff/blob/e95130ad8062f64a11247d51a72f62bb735ddd19/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs#L44

---

_Comment by @ntBre on 2025-05-05 14:09_

Fix availability is separate from fix safety. In some cases we might offer a diagnostic without a fix at all, regardless of safety. For this rule, that can happen if we fail to generate the new import edit, for example.

There's not really a centralized safety marker, you generally just have to search for `Fix::` or `safe`/`unsafe` in the file (at least that's what I do). If all of the places where a `Fix` is constructed use something like `Fix::unsafe_edit`, then you know it's always unsafe.

Thanks for working on this!

---

_@ntBre requested changes on 2025-05-05 14:12_

Related to my other comment, this is more of a description of the fix's availability rather than its safety. It's not immediately clear to me why this  fix is unsafe, which is one of the challenging parts of these cases. You may need to look back at the original PR adding the rule for more discussion.

---

_Comment by @dscorbett on 2025-05-05 15:13_

The rule description says “`exit` and `quit` come from the `site` module, which is typically imported automatically during startup. However, it is not _guaranteed_ to be imported”. If it’s not imported, the fix can change behavior.

```console
$ cat >plr1722_1.py <<'# EOF'
exit(2)
# EOF

$ python -S plr1722_1.py; echo $?
Traceback (most recent call last):
  File "plr1722_1.py", line 1, in <module>
    exit()
    ^^^^
NameError: name 'exit' is not defined
1

$ ruff --isolated check plr1722_1.py --select PLR1722 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python -S plr1722_1.py; echo $?
2
```

Another difference between `site.exit` and `sys.exit` is how they treat 0- and 1-element tuples. (This is undocumented and is arguably a bug in `sys.exit`.)
```console
$ cat >plr1722_2.py <<'# EOF'
exit(())
# EOF

$ cat >plr1722_3.py <<'# EOF'
exit((2,))
# EOF

$ python plr1722_2.py; echo $?
()
1

$ python plr1722_3.py; echo $?
(2,)
1

$ ruff --isolated check plr1722_{2,3}.py --select PLR1722 --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ python plr1722_2.py; echo $?
0

$ python plr1722_3.py; echo $?
2
```

---

_@yunchipang reviewed on 2025-05-05 18:05_

---

_Review comment by @yunchipang on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:24 on 2025-05-05 18:05_

@ntBre @dscorbett thanks you for the context! the reviews helped a lot.
I have pushed the changes, please let me know if it's clearer this time.

---

_Review requested from @ntBre by @yunchipang on 2025-05-05 18:07_

---

_@ntBre approved on 2025-05-05 18:37_

Thanks! This looks good to me.

---

_Merged by @ntBre on 2025-05-05 23:13_

---

_Closed by @ntBre on 2025-05-05 23:13_

---

_Branch deleted on 2025-06-07 01:47_

---
