```yaml
number: 755
title: gitignore not quite respected?
type: issue
state: closed
author: davidszotten
labels: []
assignees: []
created_at: 2018-01-18T15:01:13Z
updated_at: 2018-01-23T13:48:09Z
url: https://github.com/BurntSushi/ripgrep/issues/755
synced_at: 2026-01-12T16:13:22Z
```

# gitignore not quite respected?

---

_@davidszotten_

Hi. Thanks for an awesome library. If i understand things correctly, this isn't quite right..

```
$ ls .foo/
bar

$ git status --short
?? .foo/

$ cat .gitignore
.*
!.foo

$ rg --files
[empty]

$ rg --version
ripgrep 0.7.1
-AVX -SIMD
```

---

_Comment by @BurntSushi on 2018-01-18 15:28_

The problem here reveals itself if you run ripgrep with the `--debug` flag. In particular:

```
DEBUG:ignore::walk: ignoring ./.baz: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".*", actual: "**/.*", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".*", actual: "**/.*", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./.gitignore.swp: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".*", actual: "**/.*", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: whitelisting ./.foo: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!.foo", actual: "**/.foo", is_whitelist: true, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./.git: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".*", actual: "**/.*", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./.foo/bar: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".*", actual: "**/.*", is_whitelist: false, is_only_dir: false }))
```

Notice that `.foo` is correctly whitelisted, but `.foo/bar` is still ignored. The reason is that the `.*` pattern matches `.foo/bar`, and as far as I can tell, these are correct gitignore semantics from `man gitignore`:

> If the pattern does not contain a slash /, Git treats it as a shell glob pattern and checks for a match against the pathname relative to the location of the .gitignore file (relative to the toplevel of the work tree if not from a .gitignore file).

That is, in the rule `.*`, the `.` will match the `.` in `.foo/bar`, and the `*` will match the rest. This is in contrast to what happens when a `/` is in the glob:

> Otherwise, Git treats the pattern as a shell glob suitable for consumption by fnmatch(3) with the FNM_PATHNAME flag: wildcards in the pattern will not match a / in the pathname. For example, "Documentation/*.html" matches "Documentation/git.html" but not "Documentation/ppc/ppc.html" or "tools/perf/Documentation/perf.html".

In particular, `*` can no longer match a `/`.

It does seem like `git` has a different interpretation of the `.*` pattern. In particular, `.foo/bar` isn't ignored by `git`, but I don't understand why.

You should be able to fix this by replacing `.*` with `**/.*`. In particular, this will ensure that the `.` can only match in the basename of a file path, since the trailing `*` in this case will never match a `/` (because the glob contains a slash).

---

_Comment by @davidszotten on 2018-01-18 15:54_

>  In particular, `.foo/bar` isn't ignored by git, but I don't understand why.

is this a bug in git?

---

_Comment by @BurntSushi on 2018-01-18 15:55_

@davidszotten I don't know, that's not my call to make.

---

_Comment by @BurntSushi on 2018-01-18 15:56_

This wouldn't be the first inconsistency: https://github.com/BurntSushi/ripgrep/issues/373

---

_Comment by @davidszotten on 2018-01-23 10:02_

thanks for the amazingly detailed and helpful response! 

not sure how you prefer to manage your issues, but feel free to close this if you want

---

_Closed by @BurntSushi on 2018-01-23 13:48_

---
