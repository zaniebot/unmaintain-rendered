```yaml
number: 1374
title: "Whitelisting via .gitignore does not handle dot prefix (\"!./fname\")"
type: issue
state: closed
author: blueyed
labels:
  - invalid
  - gitignore
assignees: []
created_at: 2019-09-12T12:38:04Z
updated_at: 2021-05-29T13:32:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1374
synced_at: 2026-01-12T16:13:23Z
```

# Whitelisting via .gitignore does not handle dot prefix ("!./fname")

---

_@blueyed_

Given a `.gitignore` file:
```
*
!./PKGBUILD
```

`rg` will skip the `PKGBUILD` file:

> DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./PKGBUILD: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))

This appears to be due to the leading dot there.
Git handles this as expected.

Removing the dot (using `!/PKGBUILD`) whitelists it then:

> DEBUG|ignore::walk|ignore/src/walk.rs:1642: whitelisting ./PKGBUILD: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/PKGBUILD", actual: "PKGBUILD", is_whitelist: true, is_only_dir: false })))

#### What version of ripgrep are you using?

ripgrep 11.0.2

---

_Label `bug` added by @BurntSushi on 2019-09-12 12:44_

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:34_

---

_Comment by @goto-engineering on 2020-12-05 01:42_

I just tried this, and my `git` (2.25.1) seems to show the same behavior:
```
*
!./test
```
does not show the `test` file under `git status`, whereas
```
*
!test
```
does show it.

`ripgrep` has the exact same behavior.

Even when my .gitignore simply contains:
```
./test
```
the test file is NOT being git-ignored. Are we sure that the `./` syntax is supported by gitignore?

---

_Comment by @BurntSushi on 2020-12-05 01:57_

@blueyed Can you clarify what you meant by "git handles this as expected" in your initial bug report here?

---

_Comment by @czarnota on 2021-03-18 20:46_

IMHO this is the expected behavior

https://git-scm.com/docs/gitignore
>An optional prefix "!" which negates the pattern; any matching file excluded by a previous pattern will become included again. **It is not possible to re-include a file if a parent directory of that file is excluded.** Git doesn’t list excluded directories for performance reasons, so any patterns on contained files have no effect, no matter where they are defined.

the dot `.` is a synonym for the parent directory of `.gitignore`, so in the case of

```
!./PKGBUILD
```

"It is not possible to re-include a file if a parent directory (in this case `.`) of that file is excluded."


**Furthermore:**

If you specify this .gitignore

```
*
!foo/bar
```

```console
$ tree -a
.
├── foo
│   └── bar
└── .gitignore
```

Then `git status` also gives you nothing

```console
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```


---

_Comment by @BurntSushi on 2021-05-29 13:32_

Closing as not a bug. Happy to revisit with more compelling data.

---

_Closed by @BurntSushi on 2021-05-29 13:32_

---

_Label `bug` removed by @BurntSushi on 2021-05-29 13:32_

---

_Label `invalid` added by @BurntSushi on 2021-05-29 13:32_

---
