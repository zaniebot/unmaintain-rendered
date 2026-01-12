```yaml
number: 2208
title: Adding --replace to a --multiline search can expand what content is matched
type: issue
state: closed
author: AndydeCleyre
labels:
  - duplicate
assignees: []
created_at: 2022-05-11T04:11:24Z
updated_at: 2022-05-11T13:19:26Z
url: https://github.com/BurntSushi/ripgrep/issues/2208
synced_at: 2026-01-12T16:13:24Z
```

# Adding --replace to a --multiline search can expand what content is matched

---

_@AndydeCleyre_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`apt`:

```
ripgrep:
  Installed: 13.0.0-2
  Candidate: 13.0.0-2
  Version table:
 *** 13.0.0-2 500
        500 http://us.archive.ubuntu.com/ubuntu jammy/universe amd64 Packages
        100 /var/lib/dpkg/status
```

#### What operating system are you using ripgrep on?

`Pop!_OS 22.04 LTS`

#### Describe your bug.

I have a multiline (not dot-all) search with capture groups that works as expected without `--replace`, but with it, the last capture group seems to capture more content unexpectedly.

#### What are the steps to reproduce the behavior?

Content to search, `patternfuncs.zsh`:
```bash
# Compile requirements.txt files from all found or specified requirements.in files (compile).
# Use -h to include hashes, -u dep1,dep2... to upgrade specific dependencies, and -U to upgrade all.
pipc () {  # [-h] [-U|-u <pkgspec>[,<pkgspec>...]] [<reqs-in>...] [-- <pip-compile-arg>...]
    emulate -L zsh
    unset REPLY
    if [[ $1 == --help ]] { zpy $0; return }
    [[ $ZPY_PROCS ]] || return

    local gen_hashes upgrade upgrade_csv
    while [[ $1 == -[hUu] ]] {
        if [[ $1 == -h ]] { gen_hashes=--generate-hashes; shift   }
        if [[ $1 == -U ]] { upgrade=1;                    shift   }
        if [[ $1 == -u ]] { upgrade=1; upgrade_csv=$2;    shift 2 }
    }
}
```

```console
$ rg --no-config --color never -NU -r '$usage' '^(?P<predoc>\n?(# .*\n)*)(alias (?P<aname>pipc)="[^"]+"|(?P<fname>pipc) \(\) \{)(  #(?P<usage> .+))?' patternfuncs.zsh
```

#### What is the actual behavior?

```console
$ rg --no-config --debug --color never -NU -r '$usage' '^(?P<predoc>\n?(# .*\n)*)(alias (?P<aname>pipc)="[^"]+"|(?P<fname>pipc) \(\) \{)(  #(?P<usage> .+))?' patternfuncs.zsh
```
```shell
DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
 [-h] [-U|-u <pkgspec>[,<pkgspec>...]] [<reqs-in>...] [-- <pip-compile-arg>...]
    emulate -L zsh
    unset REPLY
    if [[ $1 == --help ]] { zpy $0; return }
    [[ $ZPY_PROCS ]] || return

    local gen_ha
```

The above output includes content which is not present when not using `-r`:

```console
$ rg --no-config --debug --color never -NU '^(?P<predoc>\n?(# .*\n)*)(alias (?P<aname>pipc)="[^"]+"|(?P<fname>pipc) \(\) \{)(  #(?P<usage> .+))?' patternfuncs.zsh
```
```shell
DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
# Compile requirements.txt files from all found or specified requirements.in files (compile).
# Use -h to include hashes, -u dep1,dep2... to upgrade specific dependencies, and -U to upgrade all.
pipc () {  # [-h] [-U|-u <pkgspec>[,<pkgspec>...]] [<reqs-in>...] [-- <pip-compile-arg>...]
```

#### What is the expected behavior?

I'd expect the command with `-r '$usage'` to output only the first line of its current output, that is:

```shell
 [-h] [-U|-u <pkgspec>[,<pkgspec>...]] [<reqs-in>...] [-- <pip-compile-arg>...]
```

---

_Comment by @BurntSushi on 2022-05-11 11:55_

I believe this is a duplicate of #2095.

---

_Closed by @BurntSushi on 2022-05-11 11:55_

---

_Label `bug` added by @BurntSushi on 2022-05-11 11:55_

---

_Label `bug` removed by @BurntSushi on 2022-05-11 11:55_

---

_Label `duplicate` added by @BurntSushi on 2022-05-11 11:55_

---

_Comment by @AndydeCleyre on 2022-05-11 13:14_

I read that one but thought it was about repeated output of content that is usually matched, whereas this one is about matching previously unmatched content. 

---

_Comment by @BurntSushi on 2022-05-11 13:19_

Yes, that's reasonable, but they both have the same root cause.

---
