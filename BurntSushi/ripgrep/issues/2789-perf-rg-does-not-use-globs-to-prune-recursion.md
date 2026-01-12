```yaml
number: 2789
title: "[perf] rg does not use globs to prune recursion when it can"
type: issue
state: open
author: BGR360
labels:
  - enhancement
assignees: []
created_at: 2024-04-25T01:11:02Z
updated_at: 2024-10-07T17:25:14Z
url: https://github.com/BurntSushi/ripgrep/issues/2789
synced_at: 2026-01-12T16:13:24Z
```

# [perf] rg does not use globs to prune recursion when it can

---

_@BGR360_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

Build from source with the following patch applied:

```diff
diff --git a/crates/core/flags/hiargs.rs b/crates/core/flags/hiargs.rs
index e027a2c..140f006 100644
--- a/crates/core/flags/hiargs.rs
+++ b/crates/core/flags/hiargs.rs
@@ -896,6 +896,7 @@ impl HiArgs {
             .ignore_case_insensitive(self.ignore_file_case_insensitive);
         if !self.no_ignore_dot {
             builder.add_custom_ignore_filename(".rgignore");
+            builder.add_custom_ignore_filename(".hgignore");
         }
         // When we want to sort paths lexicographically in ascending order,
         // then we can actually do this during directory traversal itself.
```

### What operating system are you using ripgrep on?

Linux 5.15.0-60-generic #66~20.04.1-Ubuntu SMP x86_64 GNU/Linux

### Describe your bug.

I'm trying to search through a massive corpus of log files (~10M files), on a remote NFS mount, to see if a particular string is present **in a certain type of log file**. I have a glob that filters down to the log files I care about. The key point is that the files that match my glob are **a small subset of all the files**.

The corpus looks like this:

<details>
<summary>Expand for preview of corpus</summary>

```
.
├── Customer1
│   ├── cluster1
│   │   ├── 2024-01-01
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-02-02
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-03-03
│   │   │   └── ...lots-of-logs
│   │   └── ...many-more-dates
│   ├── cluster2
│   │   ├── 2024-01-01
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-02-02
│   │   │   ├── ...lots-of-logs
│   │   │   └── perf_123
│   │   │       └── profile
│   │   │           └── 0-trigger
│   │   │               └── oplogs
│   │   │                   └── SOME-INTERESTING-LOGS
│   │   ├── 2024-03-03
│   │   │   └── ...lots-of-logs
│   │   └── ...many-more-dates
│   └── cluster3
│       ├── 2024-01-01
│       │   └── ...lots-of-logs
│       ├── 2024-02-02
│       │   └── ...lots-of-logs
│       ├── 2024-03-03
│       │   └── ...lots-of-logs
│       └── ...many-more-dates
├── Customer2
│   ├── cluster1
│   │   ├── 2024-01-01
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-02-02
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-03-03
│   │   │   ├── ...lots-of-logs
│   │   │   └── perf_456
│   │   │       └── profile
│   │   │           └── 0-trigger
│   │   │               └── oplogs
│   │   │                   └── SOME-INTERESTING-LOGS
│   │   └── ...many-more-dates
│   ├── cluster2
│   │   ├── 2024-01-01
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-02-02
│   │   │   └── ...lots-of-logs
│   │   ├── 2024-03-03
│   │   │   └── ...lots-of-logs
│   │   └── ...many-more-dates
│   └── cluster3
│       ├── 2024-01-01
│       │   └── ...lots-of-logs
│       ├── 2024-02-02
│       │   └── ...lots-of-logs
│       ├── 2024-03-03
│       │   └── ...lots-of-logs
│       └── ...many-more-dates
├── ...a-few-hundred-more
```

</details>

With my glob being `*/*/*/perf_*/profile/0-trigger/oplogs/*.log`.

The problem is that ripgrep is **not limiting its recursive walk to only the paths that definitely match the glob.** It is enumerating directories that could not possibly match the glob, and the number of files that end up being considered really adds up. It's considering far more files than it needs to.

### What are the steps to reproduce the behavior?

Create the following directory tree. It mimics my corpus.

```
$ tree
.
├── 0-many
│   ├── blah
│   │   ├── 0-lots
│   │   ├── 1-of
│   │   └── 2-files
│   └── GOOD
│       └── COOL.log
└── 1-dirs
    ├── blah
    │   ├── 0-lots
    │   ├── 1-of
    │   └── 2-files
    └── GOOD
        └── COOL.log
```

Use the following glob search. It mimics my search. I only want to search through the directories I know will contain my interesting files.

```
$ rg --debug --files -g '*/GOOD/*.log'
```

### What is the actual behavior?

ripgrep recurses into the `*/blah/` directories when there's no chance that they could match the glob.

Problematic lines emphasized with `>>>`

```
$ rg --debug --files -g '*/GOOD/*.log'
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1100: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Files)
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1110: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1261: found hostname for hyperlink configuration: timmy
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1271: hyperlink format: ""
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./1-dirs/blah/2-files: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./1-dirs/blah/1-of: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./1-dirs/blah/0-lots: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1802: whitelisting ./1-dirs/GOOD/COOL.log: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "*/GOOD/*.log", actual: "*/GOOD/*.log", is_whitelist: false, is_only_dir: false })))))
1-dirs/GOOD/COOL.log
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./0-many/blah/2-files: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./0-many/blah/1-of: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./0-many/blah/0-lots: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1802: whitelisting ./0-many/GOOD/COOL.log: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "*/GOOD/*.log", actual: "*/GOOD/*.log", is_whitelist: false, is_only_dir: false })))))
0-many/GOOD/COOL.log
```

Same result if I try `/*/GOOD/*.log`

### What is the expected behavior?

Ripgrep should skip recursing into directories that do not match the glob.

Something like this:

```
$ rg --debug --files -g '/*/GOOD/*.log'
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1100: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Files)
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1110: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1261: found hostname for hyperlink configuration: timmy
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1271: hyperlink format: ""
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs: ignoring ./1-dirs/blah/: does not match glob
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1802: whitelisting ./1-dirs/GOOD/COOL.log: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/*/GOOD/*.log", actual: "*/GOOD/*.log", is_whitelist: false, is_only_dir: false })))))
1-dirs/GOOD/COOL.log
>>>rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs: ignoring ./0-many/blah/: does not match glob
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1802: whitelisting ./0-many/GOOD/COOL.log: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/*/GOOD/*.log", actual: "*/GOOD/*.log", is_whitelist: false, is_only_dir: false })))))
0-many/GOOD/COOL.log
```

---

_Label `enhancement` added by @BurntSushi on 2024-04-25 01:15_

---

_Comment by @BurntSushi on 2024-04-25 01:18_

I thought there was already an open issue for this, but I couldn't find it.

This is a rather difficult optimization to do and is blocked on a rewrite of `globset` and probably `ignore`. It's something I've been working on off-and-on for a while now, but it's unlikely to land any time soon. I'm not even 100% certain it's possible unless the `--no-ignore` flag is also passed. The interaction point between `-g/--glob` and ignore files will need to be carefully considered for something like this.

Your best bet is to use some other tool to filter out files first. Possibly even using your shell's glob support. Although those could in theory end up being slower than ripgrep even when ripgrep visits more than it needs to. It depends.

---

_Comment by @BGR360 on 2024-04-25 01:32_

Alright, thanks for clarifying. Using `fd` to write all of the interesting filepaths to a file and then `xargs`-ing that into `rg` got me results way faster!

It would be neat if `rg` supported piping in filenames from stdin. So I wouldn't have to wait for `fd` to finish its scan. Should I post another enhancement issue for that?

---

_Comment by @BurntSushi on 2024-04-25 01:42_

That's #273. But you shouldn't need it. You should be able to pipe the output of `fd` straight into ripgrep with `xargs` without writing to an intermediate file.

---

_Comment by @BGR360 on 2024-04-25 01:43_

I can use xargs yes but ripgrep won't start searching any of those paths until the `fd` scan completes.

EDIT: oh nvm i can make xargs chunk it up into multiple `rg` invocations

---

_Comment by @Bluesman74 on 2024-10-07 17:25_

@BGR360 
The FD readme at Github says that you can use -x to invoke the command when it finds the match, and -X which is when it passes all the matches to the executed program.

Were you using -X, as that would explain it?

---
