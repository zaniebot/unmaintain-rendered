```yaml
number: 2638
title: "Somehow `--no-ignore` does not searches dotfiles"
type: issue
state: closed
author: xnuk
labels:
  - invalid
assignees: []
created_at: 2023-10-22T14:18:43Z
updated_at: 2023-10-22T15:27:47Z
url: https://github.com/BurntSushi/ripgrep/issues/2638
synced_at: 2026-01-12T16:13:24Z
```

# Somehow `--no-ignore` does not searches dotfiles

---

_@xnuk_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

### How did you install ripgrep?

[`pacman -S ripgrep`](https://archlinux.org/packages/extra/x86_64/ripgrep/)

```sh
$ sha256sum $(which rg)
2724960723c7204008ca21c783cb89ee1318cfdb0822dc757c528e6af0b637d6  /usr/bin/rg
```

### What operating system are you using ripgrep on?

- OS: Arch Linux x86_64
- Kernel: 6.5.7-zen2-1-zen

### Describe your bug.

Cannot search for dotfiles even if `--no-ignore-*` options are given.

### What are the steps to reproduce the behavior?

### Arch Linux
```
$ echo apple > ./.dotfile

$ rg --no-config --no-ignore apple

$ rg --debug --no-config --no-ignore apple
DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(apple)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.dotfile: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

$ rg --debug --no-config --no-ignore \
	--no-ignore-dot \
	--no-ignore-exclude \
	--no-ignore-files \
	--no-ignore-global \
	--no-ignore-messages \
	--no-ignore-parent \
	--no-ignore-vcs \
	apple
(same output as above)

$ cat ./.dotfile
apple
```

### Alpine Linux 3.18.4 amd64 in container (Podman 4.7.1)
```Dockerfile
FROM --platform=linux/amd64 alpine:3.18.4
WORKDIR /root/

RUN wget -O ./ripgrep.tar.gz \
	https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep-13.0.0-x86_64-unknown-linux-musl.tar.gz
RUN tar -xOf ./ripgrep.tar.gz ripgrep-13.0.0-x86_64-unknown-linux-musl/rg > ./rg
RUN chmod +x ./rg

RUN sha256sum ./ripgrep.tar.gz
# ee4e0751ab108b6da4f47c52da187d5177dc371f0f512a7caaec5434e711c091  ./ripgrep.tar.gz

RUN sha256sum ./rg
# 4ef156371199b3ddac1bf584e0e52b1828279af82e4ea864b4d9c816adb5db40  ./rg

RUN ./rg --version
# ripgrep 13.0.0 (rev af6b6c543b)
# -SIMD -AVX (compiled)
# +SIMD +AVX (runtime)

RUN echo apple > ./.dotfile

RUN ./rg --debug --no-config apple || true
# DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(apple)], limit_size: 250, limit_class: 10 }
# DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

RUN ./rg --debug --no-config --no-ignore apple || true
# DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
# DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(apple)], limit_size: 250, limit_class: 10 }
# DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

RUN ./rg --debug --no-config --no-ignore \
	--no-ignore-dot \
	--no-ignore-exclude \
	--no-ignore-files \
	--no-ignore-global \
	--no-ignore-messages \
	--no-ignore-parent \
	--no-ignore-vcs \
	apple || true
# DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
# DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(apple)], limit_size: 250, limit_class: 10 }
# DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

RUN cat ./.dotfile
# apple
```

### What is the actual behavior?

See above.

While making reproducible way in Alpine Linux, I surprised that Arch Linux version has this log while Alpine version doesn't have:
```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.dotfile: Ignore(IgnoreMatch(Hidden))
```

### What is the expected behavior?

```
$ rg --no-config --no-ignore apple
.dotfile
1:apple
```

---

_Comment by @BurntSushi on 2023-10-22 14:32_

This is correct and expected behavior. Could you please share why you thought otherwise?

From the man page:

```
       --no-ignore
           Don’t respect ignore files (.gitignore, .ignore,
           etc.). This implies --no-ignore-dot,
           --no-ignore-exclude, --no-ignore-global,
           no-ignore-parent and --no-ignore-vcs.
```

And also:

```
       -., --hidden
           Search hidden files and directories. By default,
           hidden files and directories are skipped. Note that
           if a hidden file or a directory is whitelisted in an
           ignore file, then it will be searched even if this
           flag isn’t provided.

           A file or directory is considered hidden if its base
           name starts with a dot character (.). On operating
           systems which support a hidden file attribute, like
           Windows, files with this attribute are also
           considered hidden.

           This flag can be disabled with --no-hidden.
```

This is also discussed in some depth in the [GUIDE's section on automatic filtering](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering).

The `--debug` output is even telling you that it was ignored because it was hidden, not because it matched a glob in a `.gitignore` file.

I can't explain the differences in logging output between Archlinux and Alpine here.

---

_Closed by @BurntSushi on 2023-10-22 14:32_

---

_Label `invalid` added by @BurntSushi on 2023-10-22 14:32_

---

_Comment by @xnuk on 2023-10-22 15:22_

Okay then I should use `rg --hidden --no-ignore`, or `rg -uu` for short (I just read `rg --help` again). But I think I will eventually face the same brain fart and search for this issue again since, maybe,  it's similar words.

> Could you please share why you thought otherwise?

My thought, maybe, was that ripgrep is 'ignoring dot files from search result' so I should do `--no-ignore-dot` which is included by `--no-ignore`.

> I can't explain the differences in logging output between Archlinux and Alpine here.

I also didn't get used to Alpine, so free to ignore this for now. Here's interesting result in Alpine btw:

```Dockerfile
RUN ./rg --debug --no-config --hidden --no-ignore apple || true
# DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
# DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(apple)], limit_size: 250, limit_class: 10 }
# DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

---

_Comment by @BurntSushi on 2023-10-22 15:27_

> My thought, maybe, was that ripgrep is 'ignoring dot files from search result' so I should do `--no-ignore-dot` which is included by `--no-ignore`.

Gotya. The naming is unfortunate, but the docs for `--no-ignore-dot` do specifically call this out and suggest `-./--hidden`.

```
       --no-ignore-dot
           Don’t respect .ignore files.

           This does not affect whether ripgrep will ignore
           files and directories whose names begin with a dot.
           For that, see the -./--hidden flag.
```

---
