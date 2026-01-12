```yaml
number: 20143
title: "[`flake8-use-pathlib`] Fix `PTH101`, `PTH104`, `PTH105`, `PTH121` fixes"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth's-fixes
created_at: 2025-08-28T21:18:38Z
updated_at: 2025-09-18T14:26:49Z
url: https://github.com/astral-sh/ruff/pull/20143
synced_at: 2026-01-12T15:56:55Z
```

# [`flake8-use-pathlib`] Fix `PTH101`, `PTH104`, `PTH105`, `PTH121` fixes

---

_@chirizxc_

## Summary
Fixes https://github.com/astral-sh/ruff/issues/20134

## Test Plan
`cargo nextest run flake8_use_pathlib`

---

_Comment by @github-actions[bot] on 2025-08-28 21:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`flake8-use-pathlib`] Fix PTH101, PTH104, PTH105, PTH121 fixes" to "[`flake8-use-pathlib`] Fix `PTH101`, `PTH104`, `PTH105`, `PTH121` fixes" by @chirizxc on 2025-08-28 22:14_

---

_Comment by @chirizxc on 2025-08-30 16:25_

I don't know how to add a co-author to a PR, maybe only maintainers can do that?

---

_Comment by @chirizxc on 2025-09-02 21:38_

i use this cases:
```python
import os
from pathlib import Path

# os.chmod(mode=0o600, follow_symlinks=True, path="pth1_link")
os.chmod(mode=0o600, follow_symlinks=True, path="pth1_link")
Path("pth1_link").chmod(mode=0o600)

# os.chmod("pth1_link", mode=0o600, follow_symlinks=      False                    )
os.chmod("pth1_link", mode=0o600, follow_symlinks=      False                    )
Path("pth1_link").chmod(mode=0o600, follow_symlinks=False)

# os.chmod(path="pth1_link", mode=0o600, follow_symlinks=True)
os.chmod(path="pth1_link", mode=0o600, follow_symlinks=True)
Path("pth1_link").chmod(mode=0o600)

# os.chmod("pth1_link", follow_symlinks=True, mode=0o600)
os.chmod("pth1_link", follow_symlinks=True, mode=0o600)
Path("pth1_link").chmod(mode=0o600)

# os.chmod("pth1_link", follow_symlinks=False, mode=0o600, dir_fd=None)
os.chmod("pth1_link", follow_symlinks=False, mode=0o600, dir_fd=None)
Path("pth1_link").chmod(follow_symlinks=False, mode=0o600)

# os.chmod("pth1_link", 0o600)
os.chmod("pth1_link", 0o600)
Path("pth1_link").chmod(0o600)

# os.chmod("pth1_link", 0o600, dir_fd=None)
os.chmod("pth1_link", 0o600, dir_fd=None)
Path("pth1_link").chmod(0o600)

# os.chmod("pth1_link", 0o600, follow_symlinks=False)
os.chmod("pth1_link", 0o600, follow_symlinks=False)
Path("pth1_link").chmod(0o600, follow_symlinks=False)

# os.chmod("pth1_link", 0o600, dir_fd=None, follow_symlinks=False)
os.chmod("pth1_link", 0o600, dir_fd=None, follow_symlinks=False)
Path("pth1_link").chmod(0o600, follow_symlinks=False)

# Only diagnostic
os.chmod("pth1_link", 0o600, follow_symlinks="False")
os.chmod("pth1_file", 0o700, None, True, 1, *[1], **{"x": 1}, foo=1)
os.chmod()
os.chmod("pth1_link", follow_symlinks=True)
os.chmod("pth1_link", dir_fd=None)
```

---

_Comment by @ntBre on 2025-09-03 18:11_

> I don't know how to add a co-author to a PR, maybe only maintainers can do that?

You can add a co-author in git with a special line like this in a commit message:

```
Co-authored-by: NAME <NAME@EXAMPLE.COM>
```

But I can also add that when merging, no worries!

I'll try to give this a review soon.

---

_Label `bug` added by @ntBre on 2025-09-03 18:11_

---

_Label `fixes` added by @ntBre on 2025-09-03 18:11_

---

_Label `preview` added by @ntBre on 2025-09-03 18:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:203 on 2025-09-11 15:16_

I think this can use `is_none_or`:


```suggestion
    args.find_argument_value(name, pos).is_none_or(|expr| expr.is_boolean_literal_expr())
```

and assuming that `is_boolean_literal_expr` exists. I think all of the `Expr` variants should have `as_` and `is_` methods, but I could be wrong.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:199 on 2025-09-11 15:21_

It looks like we're often calling this either before or after a separate call to `args.find_argument_value(name, pos)`. Would it make sense just to pass in the `Expr` from that? Alternatively, maybe this could return `Some(&Expr)` or `Some(&ExprBooleanLiteral)` instead of a bool, just to avoid calling `find_argument_value` multiple times.

I don't think it's too big of a deal, just an idea.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:99 on 2025-09-11 15:23_

tiny nit: I'd probably check this before the number of arguments since that check is also for the fix.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:139 on 2025-09-11 15:29_

This will drop the `follow_symlink` kwarg if it's `True` right? I think we should probably just preserve it, even if it matches the default `True` value.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:142 on 2025-09-11 15:54_

I'm always hesitant to add a call to `unreachable!`, but just for my understanding, this _should_ be unreachable right? At most I'd say to add a `debug_assert`, but `has_unknown_keywords_or_starred_expr` above should verify all of the kwargs, if I'm following correctly.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:101 on 2025-09-11 15:56_

Thinking more about this, do we really need to check for a boolean literal? It seems like any boolean or even truthy value would be fine.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_full_name.py.snap`:853 on 2025-09-11 15:59_

This is very closely related to my other comment about this kwarg fix, but I think we should just use the whole `follow_symlinks=...` slice from the source code if possible. I don't think we really have to reformat it like this, unless I'm missing something.

---

_@ntBre reviewed on 2025-09-11 16:01_

Thanks! Just a few comments/suggestions

---

_@chirizxc reviewed on 2025-09-11 16:19_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:139 on 2025-09-11 16:19_

yea, but in other fixes we do the same thing

---

_@chirizxc reviewed on 2025-09-11 16:21_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:142 on 2025-09-11 16:21_

yes, `has_unknown_keywords_or_starred_expr` is already checking kwargs

---

_@chirizxc reviewed on 2025-09-11 16:32_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_full_name.py.snap`:853 on 2025-09-11 16:32_

I think we're already doing this in the other fix:

https://github.com/astral-sh/ruff/pull/20049/files#diff-78c4eb312beda28752a62632b77bee91f7c01d6ad4c57961069d4115e299ec83R155

---

_@chirizxc reviewed on 2025-09-11 16:36_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:139 on 2025-09-11 16:36_

https://github.com/astral-sh/ruff/pull/20049/files#diff-78c4eb312beda28752a62632b77bee91f7c01d6ad4c57961069d4115e299ec83L73

---

_@chirizxc reviewed on 2025-09-11 16:39_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:101 on 2025-09-11 16:39_

I think it is still necessary to be sure of the fix, although it is strange enough if a string ("True") is passed there, even if you fix such a variant it will turn out to be: garbage on input => garbage on output

---

_@chirizxc reviewed on 2025-09-11 16:40_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:101 on 2025-09-11 16:40_

I think for such variants it is better not to offer autofix, because broke the source code and it will be the same broken after fixing

---

_@ntBre reviewed on 2025-09-11 18:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:139 on 2025-09-11 18:42_

The logic in the other rule looks a bit different to me. It's okay to drop the extra whitespace, I forgot that that probably happens even if we slice the existing keyword range. I just don't think we need to check for a boolean literal or drop the user's explicit `follow_symlinks=True`. The only time we need to check for a default argument like that is so that we know it's safe to drop the argument _if_ the `pathlib` version doesn't support it, at least that's my understanding. In general we should preserve as much of the user's code as we can.

---

_@ntBre reviewed on 2025-09-11 18:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_makedirs.rs`:101 on 2025-09-11 18:50_

If the code was broken initially because the keyword value had the wrong type, I think it's fine to preserve that. We don't check for `name` having the right type, for example. That's beyond the scope of the rule and really even Ruff. This version won't even fix a case like:

```py
import os

exist_ok = True
os.makedirs("foo", exist_ok=exist_ok)
```

It just seems like an unnecessary restriction on the fix when we can just as easily forward whatever argument they passed. The only exception would be if `pathlib` has different behavior, but they both seem to accept truthy values just fine:

```pycon
>>> from pathlib import Path
>>> Path("foo").mkdir(exist_ok=True)
>>> Path("foo").mkdir(exist_ok=[1])
>>> import os
>>> os.makedirs("foo", exist_ok=[1])
>>>
```

---

_@chirizxc reviewed on 2025-09-11 19:18_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:139 on 2025-09-11 19:18_

I think I know what you mean, thanks

---

_Comment by @chirizxc on 2025-09-11 19:26_

I think this good now

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:134 on 2025-09-17 20:05_

Could we use the trick like in the other PR where we slice out the whole `kw`?


```suggestion
                Some("mode" | "follow_symlinks") => locator.slice(kw),
```

then we can avoid the `to_string` call for positional args too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:107 on 2025-09-17 20:08_

I think it should be okay to omit this check too, by the same reasoning as the thread at https://github.com/astral-sh/ruff/pull/20143#discussion_r2341568706. It looks like this is the last call of `is_optional_bool_literal`, so we may also be able to delete the helper function entirely.

---

_@ntBre reviewed on 2025-09-17 20:13_

Thanks, this is looking good! Two more small things (and a conflict from the other PTH PR, it looks like) and then I think this is good to go.

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:134 on 2025-09-17 20:34_

```
error[E0308]: `match` arms have incompatible types                                                                                                   
   --> crates\ruff_linter\src\rules\flake8_use_pathlib\rules\os_chmod.rs:132:22
    |
130 |               ArgOrKeyword::Keyword(kw) => match kw.arg.as_deref() {
    |  __________________________________________-
131 | |                 Some("mode" | "follow_symlinks") => locator.slice(kw),
    | |                                                     ----------------- this is found to be of type `&str`
132 | |                 _ => None,
    | |                      ^^^^ expected `&str`, found `Option<_>`
133 | |             },
    | |_____________- `match` arms have incompatible types
    |
    = note: expected reference `&str`
                    found enum `std::option::Option<_>
```

---

_@chirizxc reviewed on 2025-09-17 20:34_

---

_@ntBre reviewed on 2025-09-17 20:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs`:134 on 2025-09-17 20:50_

Oh oops, I forgot to wrap it in `Some`, sorry! I think we can still remove `to_string` as long as we remove it from the `ArgOrKeyword::Arg` arm too.

---

_Comment by @chirizxc on 2025-09-17 21:06_

I couldn't find another e-mail on github so I used `<danparizher@users.noreply.github.com>`

---

_@ntBre approved on 2025-09-18 14:14_

Thank you! I just pushed one commit adding back the PTH211 test case. It seemed fine to keep it and a bit unrelated to the other changes here, from what I could tell.

---

_Merged by @ntBre on 2025-09-18 14:17_

---

_Closed by @ntBre on 2025-09-18 14:17_

---

_Branch deleted on 2025-09-18 14:18_

---
