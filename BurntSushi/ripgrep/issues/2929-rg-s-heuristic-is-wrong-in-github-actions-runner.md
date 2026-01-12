```yaml
number: 2929
title: "Rg's heuristic is wrong in github actions runner"
type: issue
state: closed
author: tymscar
labels:
  - wontfix
assignees: []
created_at: 2024-11-11T16:03:25Z
updated_at: 2024-11-11T16:43:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2929
synced_at: 2026-01-12T16:13:25Z
```

# Rg's heuristic is wrong in github actions runner

---

_@tymscar_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

`1.4.1.1` and `1.3.0.0` (both same issue)

### How did you install ripgrep?

APT

### What operating system are you using ripgrep on?

ubuntu22, but more exactly, this image:
`ghcr.io/actions/actions-runner:2.320.0`

### Describe your bug.

Ripgrep, when run inside github actions, fails to search local files, even with `--ignore` or `-uuu`

It picks the heuristic wrong, and there are no env vars that rewrite those.

### What are the steps to reproduce the behavior?

Run the following example in a github actions runner:

```bash
touch testrg.txt
echo testrg > testrg.txt
rg --debug -uuu testrg > result
cat result
```

### What is the actual behavior?

```bash
touch testrg.txt
echo testrg > testrg.txt
rg --debug -uuu testrg > result
cat result
```

output:
```console
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|grep_cli|crates/cli/src/lib.rs:209: for heuristic stdin detection on Unix, found that is_file=false, is_fifo=true and is_socket=false, and thus concluded that is_stdin_readable=true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1121: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: REDACTED
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|/project/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
Error: Process completed with exit code 1.
```

### What is the expected behavior?

expected behaviour (and what I also get on local):
```console
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: REDACTED
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/private/tmp/nix-build-ripgrep-14.1.1.drv-0/source/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
testrg.txt:testrg
```

---

_Comment by @BurntSushi on 2024-11-11 16:11_

`--ignore` and `-uuu` have nothing to do with this. And while ripgrep does use a heuristic here, it can't realistically be changed without regressing something else. Probably GitHub Actions shouldn't be doing something that makes stdin looks readable in the first place, so I'd probably put the blame on Actions. But either way, there just really isn't anything ripgrep can do here.

ripgrep specifically calls this out in its man page, near the top, and even tells you how to fix it:

```
       ripgrep  will  automatically  detect  if stdin exists and search
       stdin for a regex pattern, e.g. ls | rg foo.  In  some  environ‐
       ments,  stdin may exist when it shouldn't. To turn off stdin de‐
       tection, one can explicitly specify  the  directory  to  search,
       e.g. rg foo ./.
```

---

_Closed by @BurntSushi on 2024-11-11 16:11_

---

_Label `wontfix` added by @BurntSushi on 2024-11-11 16:11_

---

_Comment by @tymscar on 2024-11-11 16:14_

Hey @BurntSushi, thanks for the quick reply.

I did read the docs and I have tried to specify the directory too and that did not help.
The way I "fixed" it, was like this:

`rg  'testrg' . </dev/null`

That made me think it's something to do with rg, because as you can see in the debug logs, theres nothing specified in the stdin.

---

_Comment by @tymscar on 2024-11-11 16:22_

Also worth mentioning: grep does not have this issue in the same environment

---

_Comment by @BurntSushi on 2024-11-11 16:23_

Please show `rg --debug -uuu testrg ./`. Because _none_ of your commands in your initial comment include the directory to search.

And yes, `grep` doesn't have this problem because it doesn't support this sort of thing in the first place. You can't just run `grep test`. It will never try to search the current working directory. It will always search `stdin`.

---

_Comment by @tymscar on 2024-11-11 16:42_

My bad! I have tried again and got this output and I think my eyes just glossed over the output there in the middle.
Thank you for the help and sorry for the bother.
I think you are right and the github image just tricks the heuristic, I will raise an issue with them!

```console
+ rg --debug -uuu testrg ./
DEBUG|grep_regex::literal|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-regex-0.1.9/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(testrg)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
./testrg.txt:testrg
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

---

_Comment by @BurntSushi on 2024-11-11 16:43_

Gotya. And yes, in particular, this line:

```
rg: DEBUG|grep_cli|crates/cli/src/lib.rs:209: for heuristic stdin detection on Unix, found that is_file=false, is_fifo=true and is_socket=false, and thus concluded that is_stdin_readable=true
```

That is, ripgrep is seeing a fifo on stdin.

---
