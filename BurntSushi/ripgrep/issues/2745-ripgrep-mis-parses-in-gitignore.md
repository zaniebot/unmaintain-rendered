```yaml
number: 2745
title: "ripgrep mis-parses `*[\\<\\>\\:\\\"\\/\\\\\\|\\?\\*]*` in `.gitignore`"
type: issue
state: open
author: andreamah
labels:
  - bug
assignees: []
created_at: 2024-03-01T22:35:06Z
updated_at: 2024-04-10T07:13:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2745
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep mis-parses `*[\<\>\:\"\/\\\|\?\*]*` in `.gitignore`

---

_@andreamah_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev e50df40a19)

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

Github release `ripgrep_14.1.0-1_amd64.deb`


### What operating system are you using ripgrep on?

WSL2: Ubuntu (Linux x64 5.10.102.1-microsoft-standard-WSL2)

### Describe your bug.

Ripgrep doesn't interpret `*[\<\>\:\"\/\\\|\?\*]*` in `.gitignore` in the same way that git does.

### What are the steps to reproduce the behavior?

1. Create the following file structure:
```
.
├── folder
│   └── bar.txt
└── foo.txt
└── .gitignore
```

where the gitignore contains the following:
```
#-----------------------------
# INVALID FILES
# (for cross OS compatibility)
#-----------------------------
*[\<\>\:\"\/\\\|\?\*]*
```

2. `git init`
3. `git status` -> notice that no files are ignored by git.
4. `rg --files` -> notice that only `foo.txt` is picked up, not `bar.txt`.

### What is the actual behavior?

```
$ rg --files --debug
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Files)
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: DESKTOP-956ATPJ
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1264: found wsl_prefix for hyperlink configuration: wsl$/Ubuntu
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "*[\\<\\>\\:\\\"\\/\\\\\\|\\?\\*]*", re: "(?-u)^[^/]*[\\\\<\\\\>\\\\:\\\\\"\\\\/\\\\\\\\\\\\\\|\\\\\\?\\\\\\*][^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([ZeroOrMore, Class { negated: false, ranges: [('\\', '\\'), ('<', '<'), ('\\', '\\'), ('>', '>'), ('\\', '\\'), (':', ':'), ('\\', '\\'), ('"', '"'), ('\\', '\\'), ('/', '/'), ('\\', '\\'), ('\\', '\\'), ('\\', '\\'), ('|', '|'), ('\\', '\\'), ('?', '?'), ('\\', '\\'), ('*', '*')] }, ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./folder/bar.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*[\\<\\>\\:\\\"\\/\\\\\\|\\?\\*]*", actual: "*[\\<\\>\\:\\\"\\/\\\\\\|\\?\\*]*", is_whitelist: false, is_only_dir: false })))
foo.txt
```

### What is the expected behavior?

ripgrep should have returned:
```
folder/bar.txt
foo.txt
```

---

_Label `bug` added by @BurntSushi on 2024-03-02 01:19_

---

_Comment by @BurntSushi on 2024-03-02 01:20_

I'll call this a bug since it doesn't match what `git` does, but I'm not sure ripgrep will ever have precisely the same semantics as the globs supported by `git`.

I haven't investigated this one in particular. It's possible there's an easy fix. If so, patches are welcome.

---

_Comment by @gordonwwang on 2024-04-10 07:11_

I thought the example you provided was interesting.
Through analysis and test, I think  "* [\ <, > : \" \ / \ \? \ | \ \ *] * " actual effect in the git ignore behavior content, can be simplified as "*[\\/]*". (Because of the character in [], only one character takes effect at the end.)
In fact, it is "*[/]*" , and when "/" appears in "[]", it affects the judgment of git and rg, causing them to ignore the behavior is inconsistent.
However, when git and rg do not behave in the same way, it is not necessarily a problem with rg and we need to experiment further.

**Step 1. Create a directory as follows**
```
# tree
.
├── folder1
│   └── bar.txt
├── folder2
│   └── b.txt
├── folder3
│   └── c.txt
└── foo.txt
```
**Step 2. Edit .gitignore**
```
# -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -
# INVALID FILES
# -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -
folder1[/]*
folder2[/]
folder3/*
```
There are two controlled trials
1. Compare folder1[/]* with folder2[/].
2. Compare folder2[/] and folder3/.

**Step 3. View the result.**
git tracking results:
```
folder1/
folder2/
foo.txt
```
rg tracking results:
```
foo.txt
folder2/b.txt
```

**Analysis of results:**
Experiment 1: folder2 was consistent in its performance and was not ignored. folder1 is not ignored in git, it is ignored in rg. git and rg behave differently here. git's behavior should be reasonable, does rg need to fix this problem? @BurntSushi 
Experiment 2: folder2[/], folder3/, according to the regular expression expansion, the theoretical logic should be the same, but here actually folder2 is not ignored in the end, folder3 is ignored. I feel that there may be a problem with git's judgment logic, and it may be necessary for someone involved in git to look at this issue.


---
