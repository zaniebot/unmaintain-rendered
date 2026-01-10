```yaml
number: 16668
title: "[red-knot] Support custom `__getattr__` methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/custom-getattr
created_at: 2025-03-12T11:08:02Z
updated_at: 2025-03-12T12:44:13Z
url: https://github.com/astral-sh/ruff/pull/16668
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Support custom `__getattr__` methods

---

_Pull request opened by @sharkdp on 2025-03-12 11:08_

## Summary

Add support for calling custom `__getattr__` methods in case an attribute is not otherwise found. This allows us to get rid of many ecosystem false positives where we previously emitted errors when accessing attributes on `argparse.Namespace`.

closes astral-sh/ruff#16614

## Test Plan

* New Markdown tests
* Observed expected ecosystem changes (the changes for `arrow` also look fine, since the `Arrow` class has a custom [`__getattr__` here](https://github.com/arrow-py/arrow/blob/1d70d0091980ea489a64fa95a48e99b45f29f0e7/arrow/arrow.py#L802-L815))

---

_Label `red-knot` added by @sharkdp on 2025-03-12 11:08_

---

_Comment by @github-actions[bot] on 2025-03-12 11:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/pyp/pyp.py:634:25
-     |
- 632 | def run_pyp(args: argparse.Namespace) -> None:
- 633 |     config = PypConfig()
- 634 |     tree = PypTransform(args.before, args.code, args.after, args.define_pypprint, config).build()
-     |                         ^^^^^^^^^^^ Type `Namespace` has no attribute `before`
- 635 |     if args.explain:
- 636 |         print(config.shebang)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/pyp/pyp.py:634:38
-     |
- 632 | def run_pyp(args: argparse.Namespace) -> None:
- 633 |     config = PypConfig()
- 634 |     tree = PypTransform(args.before, args.code, args.after, args.define_pypprint, config).build()
-     |                                      ^^^^^^^^^ Type `Namespace` has no attribute `code`
- 635 |     if args.explain:
- 636 |         print(config.shebang)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/pyp/pyp.py:634:49
-     |
- 632 | def run_pyp(args: argparse.Namespace) -> None:
- 633 |     config = PypConfig()
- 634 |     tree = PypTransform(args.before, args.code, args.after, args.define_pypprint, config).build()
-     |                                                 ^^^^^^^^^^ Type `Namespace` has no attribute `after`
- 635 |     if args.explain:
- 636 |         print(config.shebang)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/pyp/pyp.py:634:61
-     |
- 632 | def run_pyp(args: argparse.Namespace) -> None:
- 633 |     config = PypConfig()
- 634 |     tree = PypTransform(args.before, args.code, args.after, args.define_pypprint, config).build()
-     |                                                             ^^^^^^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `define_pypprint`
- 635 |     if args.explain:
- 636 |         print(config.shebang)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/pyp/pyp.py:635:8
-     |
- 633 |     config = PypConfig()
- 634 |     tree = PypTransform(args.before, args.code, args.after, args.define_pypprint, config).build()
- 635 |     if args.explain:
-     |        ^^^^^^^^^^^^ Type `Namespace` has no attribute `explain`
- 636 |         print(config.shebang)
- 637 |         print(unparse(tree))
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/pyp/pyp.py:686:12
-     |
- 684 |                 "Perhaps you forgot to define something or PYP_CONFIG_PATH is set incorrectly?"
- 685 |             )
- 686 |         if args.before and isinstance(e, NameError):
-     |            ^^^^^^^^^^^ Type `Namespace` has no attribute `before`
- 687 |             var = str(e)
- 688 |             var = var[var.find("'") + 1 : var.rfind("'")]
-     |
- 
- Found 27 diagnostics
+ Found 21 diagnostics

git-revise (https://github.com/mystor/git-revise)
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:124:8
-     |
- 122 |     assert head.target is not None
- 123 |
- 124 |     if args.target or args.root:
-     |        ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 125 |         base = repo.get_commit(args.target) if args.target else None
- 126 |         to_rebase = commit_range(base, head.target)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:124:23
-     |
- 122 |     assert head.target is not None
- 123 |
- 124 |     if args.target or args.root:
-     |                       ^^^^^^^^^ Type `Namespace` has no attribute `root`
- 125 |         base = repo.get_commit(args.target) if args.target else None
- 126 |         to_rebase = commit_range(base, head.target)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:125:32
-     |
- 124 |     if args.target or args.root:
- 125 |         base = repo.get_commit(args.target) if args.target else None
-     |                                ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 126 |         to_rebase = commit_range(base, head.target)
- 127 |     else:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:125:48
-     |
- 124 |     if args.target or args.root:
- 125 |         base = repo.get_commit(args.target) if args.target else None
-     |                                                ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 126 |         to_rebase = commit_range(base, head.target)
- 127 |     else:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:136:8
-     |
- 134 |         todos = autosquash_todos(todos)
- 135 |
- 136 |     if args.interactive:
-     |        ^^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `interactive`
- 137 |         todos = edit_todos(repo, todos, msgedit=args.edit)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:137:49
-     |
- 136 |     if args.interactive:
- 137 |         todos = edit_todos(repo, todos, msgedit=args.edit)
-     |                                                 ^^^^^^^^^ Type `Namespace` has no attribute `edit`
- 138 |
- 139 |     if todos != original:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:141:54
-     |
- 139 |     if todos != original:
- 140 |         # Perform the todo list actions.
- 141 |         new_head = apply_todos(base, todos, reauthor=args.reauthor)
-     |                                                      ^^^^^^^^^^^^^ Type `Namespace` has no attribute `reauthor`
- 142 |
- 143 |         # Update the value of HEAD to the new state.
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:150:8
-     |
- 149 | def enable_autosquash(args: Namespace, repo: Repository) -> bool:
- 150 |     if args.autosquash:
-     |        ^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `autosquash`
- 151 |         return True
- 152 |     if args.no_autosquash:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:152:8
-     |
- 150 |     if args.autosquash:
- 151 |         return True
- 152 |     if args.no_autosquash:
-     |        ^^^^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `no_autosquash`
- 153 |         return False
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:166:8
-     |
- 164 |     assert head.target is not None
- 165 |
- 166 |     if args.root:
-     |        ^^^^^^^^^ Type `Namespace` has no attribute `root`
- 167 |         raise ValueError(
- 168 |             "Incompatible option: "
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:172:8
-     |
- 170 |         )
- 171 |
- 172 |     if args.target is None:
-     |        ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 173 |         raise ValueError("<target> is a required argument")
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:175:32
-     |
- 173 |         raise ValueError("<target> is a required argument")
- 174 |
- 175 |     head = repo.get_commit_ref(args.ref)
-     |                                ^^^^^^^^ Type `Namespace` has no attribute `ref`
- 176 |     if head.target is None:
- 177 |         raise ValueError("Invalid target reference")
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:179:42
-     |
- 177 |         raise ValueError("Invalid target reference")
- 178 |
- 179 |     current = replaced = repo.get_commit(args.target)
-     |                                          ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 180 |     to_rebase = commit_range(current, head.target)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:179:42
-     |
- 177 |         raise ValueError("Invalid target reference")
- 178 |
- 179 |     current = replaced = repo.get_commit(args.target)
-     |                                          ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 180 |     to_rebase = commit_range(current, head.target)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:185:46
-     |
- 183 |     final = head.target.tree()
- 184 |     if staged:
- 185 |         print(f"Applying staged changes to '{args.target}'")
-     |                                              ^^^^^^^^^^^ Type `Namespace` has no attribute `target`
- 186 |         current = current.update(tree=staged.rebase(current).tree())
- 187 |         final = staged.rebase(head.target).tree()
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:190:8
-     |
- 189 |     # Update the commit message on the target commit if requested.
- 190 |     if args.message:
-     |        ^^^^^^^^^^^^ Type `Namespace` has no attribute `message`
- 191 |         message = b"\n".join(l.encode("utf-8") + b"\n" for l in args.message)
- 192 |         current = current.update(message=message)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:191:65
-     |
- 189 |     # Update the commit message on the target commit if requested.
- 190 |     if args.message:
- 191 |         message = b"\n".join(l.encode("utf-8") + b"\n" for l in args.message)
-     |                                                                 ^^^^^^^^^^^^ Type `Namespace` has no attribute `message`
- 192 |         current = current.update(message=message)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:195:8
-     |
- 194 |     # Prompt the user to edit the commit message if requested.
- 195 |     if args.edit:
-     |        ^^^^^^^^^ Type `Namespace` has no attribute `edit`
- 196 |         current = edit_commit_message(current)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:199:8
-     |
- 198 |     # Rewrite the author to match the current user if requested.
- 199 |     if args.reauthor:
-     |        ^^^^^^^^^^^^^ Type `Namespace` has no attribute `reauthor`
- 200 |         current = current.update(author=repo.default_author)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:203:8
-     |
- 202 |     # If the commit should be cut, prompt the user to perform the cut.
- 203 |     if args.cut:
-     |        ^^^^^^^^ Type `Namespace` has no attribute `cut`
- 204 |         current = cut_commit(current)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:231:8
-     |
- 229 |     # If '-a' or '-p' was specified, stage changes.
- 230 |     # Note that stdout=None means "inherit current stdout".
- 231 |     if args.all:
-     |        ^^^^^^^^ Type `Namespace` has no attribute `all`
- 232 |         repo.git("add", "-u", stdout=None)
- 233 |     if args.patch:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:233:8
-     |
- 231 |     if args.all:
- 232 |         repo.git("add", "-u", stdout=None)
- 233 |     if args.patch:
-     |        ^^^^^^^^^^ Type `Namespace` has no attribute `patch`
- 234 |         repo.git("add", "-p", stdout=None)
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:236:8
-     |
- 234 |         repo.git("add", "-p", stdout=None)
- 235 |
- 236 |     if args.gpg_sign:
-     |        ^^^^^^^^^^^^^ Type `Namespace` has no attribute `gpg_sign`
- 237 |         repo.sign_commits = True
- 238 |     if args.no_gpg_sign:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:238:8
-     |
- 236 |     if args.gpg_sign:
- 237 |         repo.sign_commits = True
- 238 |     if args.no_gpg_sign:
-     |        ^^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `no_gpg_sign`
- 239 |         repo.sign_commits = False
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:243:12
-     |
- 241 |     # Create a commit with changes from the index
- 242 |     staged = None
- 243 |     if not args.no_index:
-     |            ^^^^^^^^^^^^^ Type `Namespace` has no attribute `no_index`
- 244 |         staged = repo.index.commit(message=b"<git index>")
- 245 |         if staged.tree() == staged.parent_tree():
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:249:32
-     |
- 248 |     # Determine the HEAD reference which we're going to update.
- 249 |     head = repo.get_commit_ref(args.ref)
-     |                                ^^^^^^^^ Type `Namespace` has no attribute `ref`
- 250 |     if head.target is None:
- 251 |         raise ValueError("Head reference not found!")
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:254:8
-     |
- 253 |     # Either enter the interactive or non-interactive codepath.
- 254 |     if args.interactive or args.autosquash:
-     |        ^^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `interactive`
- 255 |         interactive(args, repo, staged, head)
- 256 |     else:
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/git-revise/gitrevise/tui.py:254:28
-     |
- 253 |     # Either enter the interactive or non-interactive codepath.
- 254 |     if args.interactive or args.autosquash:
-     |                            ^^^^^^^^^^^^^^^ Type `Namespace` has no attribute `autosquash`
- 255 |         interactive(args, repo, staged, head)
- 256 |     else:
-     |
- 
- error: lint:unresolved-attribute
- Found 58 diagnostics
+ Found 30 diagnostics

arrow (https://github.com/arrow-py/arrow)
- error: lint:unresolved-attribute
- 
- error: lint:unresolved-attribute
- 
- error: lint:unresolved-attribute
- 
-      |
-      |
-      |
-      |
-      |
-      |
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1869:26
- 1867 |     def _is_last_day_of_month(date: "Arrow") -> bool:
- 1868 |         """Returns a boolean indicating whether the datetime is the last day of the month."""
- 1869 |         return cast(int, date.day) == calendar.monthrange(date.year, date.month)[1]
-      |                          ^^^^^^^^ Type `Arrow` has no attribute `day`
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1869:59
- 1867 |     def _is_last_day_of_month(date: "Arrow") -> bool:
- 1868 |         """Returns a boolean indicating whether the datetime is the last day of the month."""
- 1869 |         return cast(int, date.day) == calendar.monthrange(date.year, date.month)[1]
-      |                                                           ^^^^^^^^^ Type `Arrow` has no attribute `year`
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1869:70
- 1867 |     def _is_last_day_of_month(date: "Arrow") -> bool:
- 1868 |         """Returns a boolean indicating whether the datetime is the last day of the month."""
- 1869 |         return cast(int, date.day) == calendar.monthrange(date.year, date.month)[1]
-      |                                                                      ^^^^^^^^^^ Type `Arrow` has no attribute `month`
- Found 77 diagnostics
+ Found 74 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-03-12 12:13_

---

_Review requested from @carljm by @sharkdp on 2025-03-12 12:13_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-12 12:13_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-12 12:13_

---

_@sharkdp reviewed on 2025-03-12 12:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2034 on 2025-03-12 12:14_

We do this elsewhere, so I took the same approach here. I'm not 100% sure if this is justified here, though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2045 on 2025-03-12 12:23_

An interesting (and useful) behavior that I think falls out of this implementation, but isn't tested, is that if you annotate your `__getattr__` to accept e.g. a union of literal string types for the attribute name, then only those attribute names will fall back.

A less-useful corollary of this implementation is that if you make a dumb mistake in the signature of your `__getattr__` method, the only feedback you will get is attributes failing to resolve. Not sure if or how this could be helped. We don't need to worry about it for this PR, I think.

---

_@carljm approved on 2025-03-12 12:24_

Looks good, and confirmed that ecosystem results look good as well. Thank you!

---

_@carljm reviewed on 2025-03-12 12:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2034 on 2025-03-12 12:28_

Yes, I think this makes sense here. We use `ModuleType` to model attribute access fallbacks for known modules ("module literals"), and the typeshed definition of `ModuleType` is more geared towards "arbitrary unknown module object."

---

_@sharkdp reviewed on 2025-03-12 12:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2045 on 2025-03-12 12:37_

> An interesting (and useful) behavior that I think falls out of this implementation, but isn't tested, is that if you annotate your `__getattr__` to accept e.g. a union of literal string types for the attribute name, then only those attribute names will fall back.

Added an example to demonstrate this — thanks!

> A less-useful corollary of this implementation is that if you make a dumb mistake in the signature of your `__getattr__` method, the only feedback you will get is attributes failing to resolve. Not sure if or how this could be helped. We don't need to worry about it for this PR, I think.

I think those would show up as `try_call_dunder` errors (see TODO I added previously) and could potentially be transformed into `MemberLookup` errors once we implement #16298.

---

_Merged by @sharkdp on 2025-03-12 12:44_

---

_Closed by @sharkdp on 2025-03-12 12:44_

---

_Branch deleted on 2025-03-12 12:44_

---
