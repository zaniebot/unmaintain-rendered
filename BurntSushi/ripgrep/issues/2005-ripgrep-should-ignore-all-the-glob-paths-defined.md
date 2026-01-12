```yaml
number: 2005
title: "Ripgrep should ignore all the `--glob !{paths}` defined in the `$RIPGREP_CONFIG_PATH`."
type: issue
state: closed
author: ericbn
labels:
  - invalid
assignees: []
created_at: 2021-09-24T16:35:15Z
updated_at: 2021-09-24T18:11:58Z
url: https://github.com/BurntSushi/ripgrep/issues/2005
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep should ignore all the `--glob !{paths}` defined in the `$RIPGREP_CONFIG_PATH`.

---

_@ericbn_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
brew install --formula ripgrep
```

#### What operating system are you using ripgrep on?

macOS 11.6

#### Describe your bug.

Ripgrep is not ignoring the `--glob !{paths}` defined in the `$RIPGREP_CONFIG_PATH`.

#### What are the steps to reproduce the behavior?

In a clean temporary directory:
```
❯ cat << EOF > /Users/me/.ripgreprc
--smart-case
--no-ignore
--hidden
--glob
!{.direnv,.git,.idea,.svn,cdk.out,/System/Volumes,/Volumes}
--max-columns
512
EOF
❯ export RIPGREP_CONFIG_PATH=/Users/me/.ripgreprc
❯ git clone https://github.com/zimfw/utility.git
❯ rg --debug --files
```

#### What is the actual behavior?

Ripgrep is not ignoring the `.git` path as expected. This is the output with debug enabled:
```
DEBUG|rg::config|crates/core/config.rs:40: /Users/me/.ripgreprc: arguments loaded from config file: ["--smart-case", "--no-ignore", "--hidden", "--glob", "!{.direnv,.git,.idea,.svn,cdk.out,/System/Volumes,/Volumes}", "--max-columns", "512"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--smart-case", "--no-ignore", "--hidden", "--glob", "!{.direnv,.git,.idea,.svn,cdk.out,/System/Volumes,/Volumes}", "--max-columns", "512", "--debug", "--files"]
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "{.direnv,.git,.idea,.svn,cdk.out,/System/Volumes,/Volumes}", re: "(?-u)^(/Volumes|/System/Volumes|cdk\\.out|\\.svn|\\.idea|\\.git|\\.direnv)$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Alternates([Tokens([Literal('/'), Literal('V'), Literal('o'), Literal('l'), Literal('u'), Literal('m'), Literal('e'), Literal('s')]), Tokens([Literal('/'), Literal('S'), Literal('y'), Literal('s'), Literal('t'), Literal('e'), Literal('m'), Literal('/'), Literal('V'), Literal('o'), Literal('l'), Literal('u'), Literal('m'), Literal('e'), Literal('s')]), Tokens([Literal('c'), Literal('d'), Literal('k'), Literal('.'), Literal('o'), Literal('u'), Literal('t')]), Tokens([Literal('.'), Literal('s'), Literal('v'), Literal('n')]), Tokens([Literal('.'), Literal('i'), Literal('d'), Literal('e'), Literal('a')]), Tokens([Literal('.'), Literal('g'), Literal('i'), Literal('t')]), Tokens([Literal('.'), Literal('d'), Literal('i'), Literal('r'), Literal('e'), Literal('n'), Literal('v')])])]) }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
utility/.git/packed-refs
utility/.git/index
utility/.git/refs/remotes/origin/HEAD
utility/.git/refs/heads/master
utility/.git/hooks/push-to-checkout.sample
utility/.git/hooks/update.sample
utility/.git/hooks/pre-push.sample
utility/.git/hooks/pre-applypatch.sample
utility/.git/hooks/pre-merge-commit.sample
utility/.git/hooks/post-update.sample
utility/.git/hooks/prepare-commit-msg.sample
utility/.git/hooks/pre-receive.sample
utility/.git/hooks/fsmonitor-watchman.sample
utility/.git/hooks/applypatch-msg.sample
utility/.git/hooks/pre-commit.sample
utility/.git/hooks/pre-rebase.sample
utility/.git/hooks/commit-msg.sample
utility/.git/description
utility/.git/logs/refs/remotes/origin/HEAD
utility/.git/logs/refs/heads/master
utility/.git/logs/HEAD
utility/.git/info/exclude
utility/.git/HEAD
utility/.git/objects/pack/pack-d8be03228476b6d44a25ce74da3c45ca2bd200b2.idx
utility/.git/objects/pack/pack-d8be03228476b6d44a25ce74da3c45ca2bd200b2.pack
utility/.git/config
utility/functions/mkpw
utility/functions/mkcd
utility/init.zsh
utility/.gitignore
utility/README.md
utility/LICENSE
```

#### What is the expected behavior?

Ripgrep should have ignored all the `--glob !{paths}` defined in the `$RIPGREP_CONFIG_PATH`.


---

_Renamed from "Ripgrep should ignored all the `--glob !{paths}` defined in the `$RIPGREP_CONFIG_PATH`." to "Ripgrep should ignore all the `--glob !{paths}` defined in the `$RIPGREP_CONFIG_PATH`." by @ericbn on 2021-09-24 16:35_

---

_Comment by @BurntSushi on 2021-09-24 17:03_

I can't reproduce your problem. Please provide an actual reproduction. I should be able to run precisely your commands on some precise input and get an output that matches yours. Otherwise, from what I can see, `--glob !{paths}` is working just fine:

```
$ tree -a
.
├── .git
│   └── foo
├── test1
├── .test2
└── .test3
    └── foo

2 directories, 4 files

$ RIPGREP_CONFIG_PATH=/tmp/ripgreprc-i2005 rg --files
.test2
.test3/foo
test1

$ RIPGREP_CONFIG_PATH=/tmp/ripgreprc-i2005 rg --files -g '!.test2'
.test3/foo
test1

$ RIPGREP_CONFIG_PATH=/tmp/ripgreprc-i2005 rg --files -g '!{.test2,.test3}'
test1

$ RIPGREP_CONFIG_PATH=/tmp/ripgreprc-i2005 rg --files
.test2
.test3/foo
test1

$ cat /tmp/ripgreprc-i2005
--smart-case
--no-ignore
--hidden
--glob
!{.direnv,.git,.idea,.svn,cdk.out,/System/Volumes,/Volumes}
--max-columns
512

$ vim /tmp/ripgreprc-i2005

$ cat /tmp/ripgreprc-i2005
--smart-case
--no-ignore
--hidden
--glob
!{.direnv,.git,.idea,.svn,cdk.out,/System/Volumes,/Volumes}
--max-columns
512
--glob
!{.test2,.test3}

$ RIPGREP_CONFIG_PATH=/tmp/ripgreprc-i2005 rg --files
test1
```


---

_Comment by @ericbn on 2021-09-24 17:21_

Hi @BurntSushi. I've updated the steps to reproduce. I can reproduce it with the files from `git clone https://github.com/zimfw/utility.git` for example.

---

_Comment by @ericbn on 2021-09-24 17:28_

Looks like the issue happens when the matched globs are more than one level deep on the tree:
```
$ exa -Ta
.
├── .git
│  └── foo
├── .test2
├── .test3
│  └── foo
├── test1
└── test4
   └── .git
      └── foo
$ rg --files
test4/.git/foo
test1
.test2
.test3/foo
```

---

_Comment by @BurntSushi on 2021-09-24 17:33_

It's not that. It's actually a semantic of gitignore here that I myself forgot about. Basically, if you have a glob that contains a `/`, then the glob has to match from the beginning of the path:

```rust
// If there is a literal slash, then this is a glob that must match the
// entire path name. Otherwise, we should let it match anywhere, so use
// a **/ prefix.
if !is_absolute && !line.chars().any(|c| c == '/') {
    // ... but only if we don't already have a **/ prefix.
    if !glob.has_doublestar_prefix() {
        glob.actual = format!("**/{}", glob.actual);
    }
}
```

You can see this in action when you split the `{...}` part into separate globs:

```
$ rg --files --hidden -g '!{.git,/quux}'
.test2
utility/LICENSE
utility/.gitignore
utility/.git/objects/pack/pack-d8be03228476b6d44a25ce74da3c45ca2bd200b2.idx
utility/.git/objects/pack/pack-d8be03228476b6d44a25ce74da3c45ca2bd200b2.pack
utility/.git/description
utility/.git/refs/remotes/origin/HEAD
utility/.git/refs/heads/master
utility/.git/packed-refs
utility/.git/logs/refs/remotes/origin/HEAD
utility/.git/logs/HEAD
utility/.git/logs/refs/heads/master
utility/.git/config
utility/.git/HEAD
utility/.git/hooks/pre-commit.sample
utility/.git/hooks/fsmonitor-watchman.sample
utility/.git/info/exclude
utility/.git/hooks/post-update.sample
utility/.git/hooks/pre-rebase.sample
utility/.git/hooks/pre-merge-commit.sample
utility/.git/hooks/update.sample
utility/.git/hooks/pre-push.sample
utility/.git/hooks/prepare-commit-msg.sample
utility/.git/hooks/pre-receive.sample
utility/.git/hooks/applypatch-msg.sample
utility/.git/hooks/commit-msg.sample
utility/.git/hooks/push-to-checkout.sample
utility/.git/hooks/pre-applypatch.sample
utility/.git/index
utility/README.md
utility/init.zsh
test1
.test3/foo
.test3/.git/foo
utility/functions/mkpw
utility/functions/mkcd

$ rg --files --hidden -g '!.git' -g '!/quux'
.test2
utility/LICENSE
utility/.gitignore
utility/functions/mkcd
utility/functions/mkpw
utility/README.md
utility/init.zsh
.test3/foo
test1
```

So you just need to split your glob into things that contain a `/` and things that don't.


---

_Closed by @BurntSushi on 2021-09-24 17:33_

---

_Label `invalid` added by @BurntSushi on 2021-09-24 17:36_

---

_Comment by @ericbn on 2021-09-24 18:11_

Oh, got it! Thanks for the details! Gitignore globs are tricky... 😄 

---
