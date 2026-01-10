```yaml
number: 2331
title: autofix for flake8-use-pathlib (PTH) rules
type: issue
state: open
author: LefterisJP
labels:
  - fixes
assignees: []
created_at: 2023-01-29T23:44:48Z
updated_at: 2025-08-20T18:35:48Z
url: https://github.com/astral-sh/ruff/issues/2331
synced_at: 2026-01-10T11:09:45Z
```

# autofix for flake8-use-pathlib (PTH) rules

---

_Issue opened by @LefterisJP on 2023-01-29 23:44_

I enabled PTH rules and there was a few hits in rotki's codebase.

This is a rule I woud like to enforce but there is quite many places it hit and I suppose at least some of the rules should be autofixable. So if possible it would be cool to have ruff do it for us.

---

_Label `autofix` added by @charliermarsh on 2023-01-30 00:09_

---

_Comment by @sbrugman on 2023-01-30 11:38_

See https://github.com/charliermarsh/ruff/pull/2348

---

_Comment by @sigma67 on 2024-05-21 08:07_

Since the issue is still open, is this still on the table? I think most `PTH` rules have no autofixes yet

It seems there are no more open PRs on this topic

---

_Comment by @charliermarsh on 2024-05-21 11:48_

Certainly on the table, just needs someone to work on it. It may also be limited by our ability to do program-wide analysis beyond simple fixes.

---

_Comment by @JonathanPlasse on 2024-05-21 12:37_

Should the tag `type-inference` be added?

---

_Comment by @sigma67 on 2024-05-21 12:57_

>  It may also be limited by our ability to do program-wide analysis beyond simple fixes.

Most of the fixes seem simple enough to me, i.e.

> PTH102 "os.mkdir() should be replaced by Path.mkdir()"

 `os.mkdir(var, kwargs)` 

becomes 

 `Path(var).mkdir(kwargs)`

I may be missing something - not sure how it could affect a broader scope of code.

---

_Comment by @charliermarsh on 2024-05-21 13:54_

Yeah we can do simple stuff like tha, although I think the spirit of the rule is that you'd changed `var` to be of type `Path` more holistically in the code, then do `var.mkdir(kwargs)`.

---

_Closed by @ntBre on 2025-06-24 17:58_

---

_Comment by @ntBre on 2025-06-26 17:48_

Oops, shouldn't have closed this. #18763 added a fix for PTH202, but most of the rules still don't have fixes. I'll keep track of the status here:



| Rule                               | Code   | Status |
|------------------------------------|--------|--------|
| [os-path-abspath](https://docs.astral.sh/ruff/rules/os-path-abspath/)                    | PTH100 |   https://github.com/astral-sh/ruff/pull/19213     |
| [os-chmod](https://docs.astral.sh/ruff/rules/os-chmod/)                           | PTH101 |     https://github.com/astral-sh/ruff/pull/19404   |
| [os-mkdir](https://docs.astral.sh/ruff/rules/os-mkdir/)                           | PTH102 |   https://github.com/astral-sh/ruff/pull/19514     |
| [os-makedirs](https://docs.astral.sh/ruff/rules/os-makedirs/)                        | PTH103 |    https://github.com/astral-sh/ruff/pull/19514    |
| [os-rename](https://docs.astral.sh/ruff/rules/os-rename/)                          | PTH104 |    https://github.com/astral-sh/ruff/pull/19404    |
| [os-replace](https://docs.astral.sh/ruff/rules/os-replace/)                         | PTH105 |     https://github.com/astral-sh/ruff/pull/19404   |
| [os-rmdir](https://docs.astral.sh/ruff/rules/os-rmdir/)                           | PTH106 |   https://github.com/astral-sh/ruff/pull/19213     |
| [os-remove](https://docs.astral.sh/ruff/rules/os-remove/)                          | PTH107 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-unlink](https://docs.astral.sh/ruff/rules/os-unlink/)                          | PTH108 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-getcwd](https://docs.astral.sh/ruff/rules/os-getcwd/)                          | PTH109 |        |
| [os-path-exists](https://docs.astral.sh/ruff/rules/os-path-exists/)                     | PTH110 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-path-expanduser](https://docs.astral.sh/ruff/rules/os-path-expanduser/)                 | PTH111 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-path-isdir](https://docs.astral.sh/ruff/rules/os-path-isdir/)                      | PTH112 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-path-isfile](https://docs.astral.sh/ruff/rules/os-path-isfile/)                     | PTH113 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-path-islink](https://docs.astral.sh/ruff/rules/os-path-islink/)                     | PTH114 |   https://github.com/astral-sh/ruff/pull/19213     |
| [os-readlink](https://docs.astral.sh/ruff/rules/os-readlink/)                        | PTH115 |    https://github.com/astral-sh/ruff/pull/19213    |
| [os-stat](https://docs.astral.sh/ruff/rules/os-stat/)                            | PTH116 |        |
| [os-path-isabs](https://docs.astral.sh/ruff/rules/os-path-isabs/)                      | PTH117 |   https://github.com/astral-sh/ruff/pull/19213     |
| [os-path-join](https://docs.astral.sh/ruff/rules/os-path-join/)                       | PTH118 |   https://github.com/astral-sh/ruff/pull/19213     |
| [os-path-basename](https://docs.astral.sh/ruff/rules/os-path-basename/)                   | PTH119 |   https://github.com/astral-sh/ruff/pull/19213     |
| [os-path-dirname](https://docs.astral.sh/ruff/rules/os-path-dirname/)                    | PTH120 |        |
| [os-path-samefile](https://docs.astral.sh/ruff/rules/os-path-samefile/)                   | PTH121 |    https://github.com/astral-sh/ruff/pull/19404    |
| [os-path-splitext](https://docs.astral.sh/ruff/rules/os-path-splitext/)                   | PTH122 |        |
| [builtin-open](https://docs.astral.sh/ruff/rules/builtin-open/)                       | PTH123 |        |
| [py-path](https://docs.astral.sh/ruff/rules/py-path/)                            | PTH124 |        |
| [path-constructor-current-directory](https://docs.astral.sh/ruff/rules/path-constructor-current-directory/) | PTH201 | x      |
| [os-path-getsize](https://docs.astral.sh/ruff/rules/os-path-getsize/)                    | PTH202 |    https://github.com/astral-sh/ruff/pull/18763    |
| [os-path-getatime](https://docs.astral.sh/ruff/rules/os-path-getatime/)                   | PTH203 |   https://github.com/astral-sh/ruff/pull/18922     |
| [os-path-getmtime](https://docs.astral.sh/ruff/rules/os-path-getmtime/)                   | PTH204 |    https://github.com/astral-sh/ruff/pull/18922    |
| [os-path-getctime](https://docs.astral.sh/ruff/rules/os-path-getctime/)                   | PTH205 |  https://github.com/astral-sh/ruff/pull/18922      |
| [os-sep-split](https://docs.astral.sh/ruff/rules/os-sep-split/)                       | PTH206 |        |
| [glob](https://docs.astral.sh/ruff/rules/glob/)                               | PTH207 |        |
| [os-listdir](https://docs.astral.sh/ruff/rules/os-listdir/)                         | PTH208 |        |
| [invalid-pathlib-with-suffix](https://docs.astral.sh/ruff/rules/invalid-pathlib-with-suffix/)        | PTH210 | x      |
| [os-symlink](https://docs.astral.sh/ruff/rules/os-symlink/)                         | PTH211 |        |


---

_Reopened by @ntBre on 2025-06-26 17:48_

---

_Comment by @chirizxc on 2025-07-01 14:10_

i want to implement most of autofixes for these rules

---

_Comment by @chirizxc on 2025-07-01 14:14_

@ntBre what if i group multiple rules in one PR, as long as their autofixes are semantically equivalent, for example: [PTH119](https://docs.astral.sh/ruff/rules/os-path-basename/#os-path-basename-pth119) and [PTH120](https://docs.astral.sh/ruff/rules/os-path-dirname/#os-path-dirname-pth120)

---

_Comment by @chirizxc on 2025-07-01 14:15_

the overall logic can be put in `helpers.rs`, as in this PR: https://github.com/astral-sh/ruff/pull/18922

---

_Comment by @ntBre on 2025-07-01 14:18_

Yeah I think that makes sense, especially if they're using the new helper method! If they all use the same helper, we won't need to test each one as carefully either, which will also help keep the diff size down, even with multiple rules.

---

_Comment by @chirizxc on 2025-07-02 11:38_

i think first group includes PTH{102, 103, 106, 108, 110, 111, 112, 113, 114, 115, 116, 117, 119, 120}

---

_Comment by @ntBre on 2025-07-02 14:11_

That sounds like it could be quite a large PR, but I'll reserve judgment until I actually see the diff!

---

_Comment by @chirizxc on 2025-07-04 12:50_

i think we should be merged because they all have the same autofix template `os.something(“file.txt”)` => `pathlib.Path(“file.txt”).{something_in_pathlib}`

---

_Comment by @chirizxc on 2025-07-07 17:28_

I think I made a mistake, many functions have more than 1 argument, and the first thing to do is to create a generic function for functions that take only 1 argument, it seems `check_os_path_get_calls` from my PR will have to change a bit in the next one, since these get_{a, c, m}time rules also have only one argument (filename)

---

_Comment by @chirizxc on 2025-07-09 17:19_

Now we can consider the group `os.anything(a1, a2) => pathlib.Path(a1).pathlib_anything(a2)`

---
