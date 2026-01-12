```yaml
number: 2887
title: "rg '<,'> file.txt | will clear the contents of file.txt"
type: issue
state: closed
author: AnthonyGithubCorner
labels: []
assignees: []
created_at: 2024-09-12T05:08:23Z
updated_at: 2024-09-12T13:07:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2887
synced_at: 2026-01-12T16:13:25Z
```

# rg '<,'> file.txt | will clear the contents of file.txt

---

_@AnthonyGithubCorner_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,+SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

MacOS 14.6.1

### Describe your bug.

ripgrep clears the entire content of the file or will replace the file with random garbage.
This happens when running the mistake command 
rg '<,'> file.txt



### What are the steps to reproduce the behavior?

You will see that the file is completely cleared after issuing the command. I would be careful debugging this problem as it might result in data lost.
```
echo hello >> hello.txt
cat hello.txt
rg '<,'> hello.txt
cat hello.txt 
```


### What is the actual behavior?

```
rg --debug '<,'> hello.txt
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: something_hidden.lan
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|grep_regex::config|/private/tmp/ripgrep-20240910-11501-ez7f5q/ripgrep-14.1.1/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

### What is the expected behavior?

I ended up running the command trying to see if I can pass through selected content in vim to ripgrep. I wasn't too worried but imagine my surprise when something I did cleared the file I was searching. I narrowed it down to this command. The expected behaviour is no matches and the file isn't touched.

---

_Comment by @okdana on 2024-09-12 09:57_

This is a shell command typo that ripgrep can't do anything about. The shell interprets the quoting and redirect syntax before ripgrep is even executed, all it knows is that you've told it to search for the pattern `<,` and output the results to a file. Even if it could see the raw shell command, it's perfectly valid, so there's no way for it (or the shell) to figure out that you meant to type something else

If you want, you can enable the `noclobber` option in your shell profile to make it so that `>` isn't allowed to overwrite the contents of existing files:

```
# In bash:
set -o noclobber

# In zsh:
setopt no_clobber
# Also set this to make it work like bash, where you can still use >> to create files:
setopt append_create
# ETA: Also set this to make it work like bash, where you can't use > to overwrite *empty* files:
setopt no_clobber_empty
```

If you do this you will have to remember to use `>|` instead of `>` whenever you want to actually overwrite a file

---

_Locked by @ghost on 2024-09-12 13:07_

---
