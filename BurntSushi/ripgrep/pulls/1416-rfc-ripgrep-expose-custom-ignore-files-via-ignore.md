```yaml
number: 1416
title: "[RFC] ripgrep: expose custom ignore files via --ignore-file-name"
type: pull_request
state: closed
author: akarle
labels:
  - rollup
assignees: []
base: master
head: ign_file_name
created_at: 2019-10-31T18:00:47Z
updated_at: 2020-02-17T23:01:09Z
url: https://github.com/BurntSushi/ripgrep/pull/1416
synced_at: 2026-01-12T18:23:13Z
```

# [RFC] ripgrep: expose custom ignore files via --ignore-file-name

---

_@akarle_

The ignore crate has the concept of "custom ignore" files. These files
take precedence over all others and can be used to declare arbitrary
file names as files which should be parsed for ignore patterns.

This commit adds the `--ignore-file-name` switch to `ripgrep`, exposing this
functionality to the user. For instance, if a user wanted to put their
ignores in many files called `.my_ignore` they could:

    rg --ignore-file-name .my_ignore

This can also be used as a hack to honor `.gitignore` files outside .git
repositories (#1414), which can be useful if a git repo has been copied
somewhere without the .git directory (i.e. downloaded as a zip from
GitHub) or for SCM systems like Perforce which allow `.gitignore` files
but are not valid git repositories. Note, however, that this tactic
gives `.gitignore` precedence over `.ignore` and `.rgignore`, so consider
using multiple flags:

    rg --ignore-file-name .ignore --ignore-file-name .gitignore

-------

Aside: this is discussed in #1414 .

@BurntSushi : I have another branch on my fork, [ignore_outside](https://github.com/akarle/ripgrep/commit/efbed3298e9a5c2383c91e084012e0cbbda9e2ff), that implements the other possible way we discussed of solving this (via a `--force-gitignore` flag, which I called `--ignore-outside-git`, but am happy to change to whatever we find appropriate).

I also came up with a **third** solution that would work for our use-case: creating a `--alt-git-root-file` switch which specifies a token that is determined to indicate the root of a git repo. This is the most intrusive implementation though (getting into the guts of the `ignore` crate), and I found my knowledge of asynchronous rust didn't cut it :)

I'm proposing we merge the `--ignore-file-name` switch, even though it is the most hacky solution to #1414 , because it is the least change to the code, and could have other benefits / be considered a feature. However, I'm happy to go with either the `--ignore-outside-git` or `--alt-git-repo-files` solutions if you deem them more appropriate for the project.

Rust is not my strongest language, so I appreciate any and all pointers / tips / corrections!

Thanks for your time and help!
Alex

---

_Comment by @BurntSushi on 2020-02-17 19:49_

Thanks for this PR! The idea was interesting, but I think a more explicit `--no-require-git` flag is probably better. I've added this flag in #1486, which will be in the next release.

---

_Label `rollup` added by @BurntSushi on 2020-02-17 19:49_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Comment by @akarle on 2020-02-17 23:01_

@BurntSushi -- thank you so much for taking the time to revisit this!

I agree, the `--no-require-git` flag is more explicit (and less likely to cause confusion). Thank you for implementing it! Looking forward to using it in the next release.

---
