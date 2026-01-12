```yaml
number: 3173
title: hidden files whitelisted by ancestor .ignore are not searched when . is directory argument
type: issue
state: closed
author: bcc32
labels:
  - bug
assignees: []
created_at: 2025-10-08T19:16:44Z
updated_at: 2025-10-09T18:14:00Z
url: https://github.com/BurntSushi/ripgrep/issues/3173
synced_at: 2026-01-12T16:13:25Z
```

# hidden files whitelisted by ancestor .ignore are not searched when . is directory argument

---

_@bcc32_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```

### How did you install ripgrep?

dnf
```
rpm -q ripgrep  
ripgrep-14.1.1-1.el8.x86_64
```

### What operating system are you using ripgrep on?

Rocky Linux 8.10 (Green Obsidian)

### Describe your bug.

When the current directory is passed to ripgrep as `.`, hidden-but-whitelisted files that are immediate children of the current directory are not searched.  This does not happen if no directory argument is provided, or if the directory is provided as `./.` instead.

(For extra context, the Emacs [consult](https://github.com/minad/consult) package always passes a `.` argument to ripgrep when searching under the current working directory, which is how I discovered this.)

### What are the steps to reproduce the behavior?

```sh
cd "$(mktemp -d)"
mkdir subdir
echo "foo text" >subdir/.foo.txt
cat <<EOF >.ignore
!.foo.txt
EOF

rg -l 'text' . # finds subdir/.foo.txt
cd subdir

# STEP 1
rg -l 'text'     # finds .foo.txt as expected
rg -l 'text' .   # bad: no results
rg -l 'text' ./. # finds ././.foo.txt as expected

mkdir subsubdir
echo "foo text" >subsubdir/.foo.txt

# STEP 2
rg -l 'text'     # finds both .foo.txt and subsubdir/.foo.txt
rg -l 'text' .   # only finds ./subsubdir/.foo.txt
rg -l 'text' ./. # finds both ././.foo.txt and ././subsubdir/.foo.txt
```

### What is the actual behavior?

## At STEP 1 in above example

```
+ rg --debug -l text
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(FilesWithMatches))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: igm-qws-u12685a
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/builddir/build/BUILD/ripgrep-14.1.1/vendor/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/azeng/.cvsignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /tmp/tmp.dUv7poBlXQ/.ignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/walk.rs:1802: whitelisting ./.foo.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/tmp.dUv7poBlXQ/.ignore"), original: "!.foo.txt", actual: "**/.foo.txt", is_whitelist: true, is_only_dir: false })))
.foo.txt
+ rg --debug -l text .
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: igm-qws-u12685a
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/builddir/build/BUILD/ripgrep-14.1.1/vendor/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/azeng/.cvsignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /tmp/tmp.dUv7poBlXQ/.ignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/walk.rs:1799: ignoring ./.foo.txt: Ignore(IgnoreMatch(Hidden))
```

## At STEP 2 in above example

```
+ rg --debug -l text
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(FilesWithMatches))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: igm-qws-u12685a
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/builddir/build/BUILD/ripgrep-14.1.1/vendor/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/azeng/.cvsignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /tmp/tmp.nwh74HI5Fg/.ignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/walk.rs:1802: whitelisting ./.foo.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/tmp.nwh74HI5Fg/.ignore"), original: "!.foo.txt", actual: "**/.foo.txt", is_whitelist: true, is_only_dir: false })))
rg: DEBUG|ignore::walk|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/walk.rs:1802: whitelisting ./subsubdir/.foo.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/tmp.nwh74HI5Fg/.ignore"), original: "!.foo.txt", actual: "**/.foo.txt", is_whitelist: true, is_only_dir: false })))
.foo.txt
subsubdir/.foo.txt
+ rg --debug -l text .
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: igm-qws-u12685a
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/builddir/build/BUILD/ripgrep-14.1.1/vendor/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/azeng/.cvsignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /tmp/tmp.nwh74HI5Fg/.ignore
rg: DEBUG|globset|/builddir/build/BUILD/ripgrep-14.1.1/vendor/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/walk.rs:1799: ignoring ./.foo.txt: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|/builddir/build/BUILD/ripgrep-14.1.1/vendor/ignore-0.4.23/src/walk.rs:1802: whitelisting ./subsubdir/.foo.txt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/tmp.nwh74HI5Fg/.ignore"), original: "!.foo.txt", actual: "**/.foo.txt", is_whitelist: true, is_only_dir: false })))
```

### What is the expected behavior?

In step 1, ripgrep should have printed one result regardless of whether it was invoked as `rg -l text` or `rg -l text .`.

In step 2, ripgrep should have printed two results, regardless of path arguments.

---

_Comment by @huonw on 2025-10-09 00:23_

This has been bugging me and my use of `M-x consult-ripgrep` too.

I initially wondered if #2933 / 14f4957b3d605f14ad58bc67e54197a3084fef5a may address it, but I think I could still reproduce the issue with the latest master (bb88a1ac45c70bef97e0d6ccd6e91595610de860)... although I'm not 100% sure I tested the right thing, and ran of out time for my experiments.

The `./.` workaround is nifty, and can be used in an advice:

```elisp
(defun advice/consult--ripgrep-make-builder/workaround-rg-3173 (args)
  "Workaround https://github.com/BurntSushi/ripgrep/issues/3173: `rg .'
doesn't follow inverse .ignores properly, while `rg ./.' does."
  ;; consult--ripgrep-make-builder accepts one arg: `paths'
  (pcase-let ((`(,paths) args))
    (list
     (mapcar (lambda (path) (if (equal path ".") "./." path)) paths))))

(advice-add 'consult--ripgrep-make-builder :filter-args #'advice/consult--ripgrep-make-builder/workaround-rg-3173)
```

---

_Comment by @BurntSushi on 2025-10-09 00:39_

It looks like this is still present on `master` yeah.

And it looks like you can just do `./` to work around this. You don't need `./.`.

---

_Label `bug` added by @BurntSushi on 2025-10-09 00:39_

---

_Closed by @BurntSushi on 2025-10-09 01:17_

---

_Comment by @bcc32 on 2025-10-09 18:14_

Thank you for looking at this!

---
