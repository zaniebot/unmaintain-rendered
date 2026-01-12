```yaml
number: 761
title: Behavior of ignoring files/folders in .gitignore does not align with git
type: issue
state: closed
author: Goblinlordx
labels: []
assignees: []
created_at: 2018-01-27T01:39:46Z
updated_at: 2018-01-29T19:11:00Z
url: https://github.com/BurntSushi/ripgrep/issues/761
synced_at: 2026-01-12T16:13:22Z
```

# Behavior of ignoring files/folders in .gitignore does not align with git

---

_@Goblinlordx_

This issue originally arose when using VS Code's search feature which actually uses `ripgrep`.  For specific details, you can see this issue:
https://github.com/Microsoft/vscode/issues/42174

The issue seems to be, `ripgrep` is not ignoring the same files and folders as `git`.  If this is intended behavior, it isn't really stated as the documentation simply states it will be "respecting your gitignore rules."  Currently, if I have a structure as the following:
```
- path1/
  - file_to_exclude_1
  - file_to_exclude_2
  - file_to_exclude_3
- path2/
  - file_to_include_1
  - path1/
    - file_to_include_2
```
If I also have a `.gitignore` which has the following entry:
```
path1/*
```
`git` will only ignore `./path1` in the Root of the project and not `./path2/path1`.  `ripgrep` seems to be ignoring both `./path1` and `./path2/path1` which is inconsistent with what `git` ignores when using these rules.

---

_Comment by @Goblinlordx on 2018-01-27 01:52_

As a note, this rule seems to be explicitly stated on the `git` documentation page:
https://git-scm.com/docs/gitignore

> If the pattern does not contain a slash /, Git treats it as a shell glob pattern and checks for a match against the pathname relative to the location of the .gitignore file (relative to the toplevel of the work tree if not from a .gitignore file).

> Otherwise, Git treats the pattern as a shell glob suitable for consumption by fnmatch(3) with the FNM_PATHNAME flag: wildcards in the pattern will not match a / in the pathname. For example, "Documentation/*.html" matches "Documentation/git.html" but not "Documentation/ppc/ppc.html" or "tools/perf/Documentation/perf.html".

In this case, it "contains a /" and so will ignore `./path1/some_file_or_folder` but not `./path2/path1/some_file_or_folder`.  I also found this exact same statement in the `man gitignore` pages on my git version:
```
git version 2.14.0
hub version 2.2.9
```

---

_Comment by @okdana on 2018-01-27 03:11_

>For example, "Documentation/*.html" matches "Documentation/git.html" but not "Documentation/ppc/ppc.html" or "tools/perf/Documentation/perf.html".

If i'm looking at it right, it seems like the `ignore` crate deliberately rejects this — there is a test asserting that `src/*.rs` should ignore `src/grep/src/main.rs`, which contradicts both the quoted text and git's actual behaviour:

```
% echo 'path1/*.rs' > .gitignore
% touch path1/foo.rs path2/path1/foo.rs
% git check-ignore -v path1/foo.rs; echo $?
.gitignore:1:path1/*.rs	path1/foo.rs
0
% git check-ignore -v path2/path1/foo.rs; echo $?
1
```

What causes this behaviour (and the issue with `path1/*`) is the implicit prepending of `**/` to a rule even when it contains a `/`.

---

_Closed by @BurntSushi on 2018-01-29 19:11_

---
