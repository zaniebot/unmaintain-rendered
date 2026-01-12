```yaml
number: 2899
title: "Bug: Inconsistent output for same input"
type: issue
state: closed
author: luochen1990
labels:
  - invalid
assignees: []
created_at: 2024-09-19T07:10:32Z
updated_at: 2024-09-20T01:54:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2899
synced_at: 2026-01-12T16:13:25Z
```

# Bug: Inconsistent output for same input

---

_@luochen1990_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

### How did you install ripgrep?

From NixOS

```
environment.systemPackages = (with pkgs; [ ripgrep ])
```

### What operating system are you using ripgrep on?

NixOS 24.11.20240914.6ca042d (Vicuna) x86_64

### Describe your bug.

Run `nix registry list | rg nixpkgs` multiple times (for me 3~5 times is enough for reproduce), and you can get different output.

Tried to run `sha256sum =(nix registry list)`  multiple times and the hash is consistent

### What are the steps to reproduce the behavior?

1. Run `nix registry list | rg nixpkgs`  multiple times (10 might be enough)

### What is the actual behavior?

```
$ nix registry list | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ nix registry list | rg nixpkgs


$ nix registry list | rg nixpkgs


$ nix registry list | rg nixpkgs


$ nix registry list | rg nixpkgs

global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable

```

### What is the expected behavior?

```
$ nix registry list | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ nix registry list | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ nix registry list | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ nix registry list | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ nix registry list | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
```

---

_Comment by @BurntSushi on 2024-09-19 13:46_

I cannot reproduce:

```
$ cat /tmp/haystack
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ cat /tmp/haystack | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ cat /tmp/haystack | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ cat /tmp/haystack | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ cat /tmp/haystack | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
$ cat /tmp/haystack | rg nixpkgs
system flake:nixpkgs path:/nix/store/asmpdfb2aixsjl95a557c269ybzybjm3-source
global flake:nixpkgs github:NixOS/nixpkgs/nixpkgs-unstable
```

Please provide a reproduction that doesn't require `nix`. As far as I can tell, your bug report doesn't exclude the possibility that `nix registry list`'s output is not itself consistent.

Moreover, from looking at https://github.com/NixOS/nix/issues/4848, it looks like `nix registry list` is emitting color escape codes even though it isn't writing to a tty and isn't being instructed to force color (based on the data you've given me). I'd consider that a bug in `nix registry list`, and indeed, all manner of things can go wrong. So as far as I can tell, this isn't an issue in ripgrep and there really isn't anything ripgrep can do about it anyway.

---

_Closed by @BurntSushi on 2024-09-19 13:46_

---

_Label `invalid` added by @BurntSushi on 2024-09-19 13:46_

---

_Comment by @luochen1990 on 2024-09-19 22:51_

Thanks for your reply, but I tested same code use `grep` and `ag`, they are all ok, only `rg` have this problem. It is probably a nix cli issue, but maybe we can figure out why and improve the compatibility of rg, that's why I report this.

---

_Comment by @BurntSushi on 2024-09-19 23:10_

It can just be an artifact of how color escape codes are generated. Bottom line is I need a reproduction. I'm not installing and setting up nix. Maybe you can provide a docker container or something.

---

_Comment by @luochen1990 on 2024-09-19 23:17_

I think a devcontainer like [this](https://github.com/ocfox/den/blob/master/.devcontainer.json) will just work.

Based on the results of discussion with my peers, it seems that the nix cli prints control chars to stderr for progress display which cause this issue

```
strace --trace=write -o strace-nix-registry-list.log nix registry list
```

Here is the [output](https://fars.ee/LM23) of above command

---

_Comment by @BurntSushi on 2024-09-20 01:25_

Please provide a sequence of commands to reproduce the problem.

---

_Comment by @luochen1990 on 2024-09-20 01:44_

> Please provide a sequence of commands to reproduce the problem.

It might be:

```
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
nix registry list | rg nixpkgs
```

---

_Comment by @BurntSushi on 2024-09-20 01:54_

I don't have `nix` installed on my system.

I'm done asking for a reproduction. This is the last comment I made until you provide a complete set of instructions for reproducing this problem workout installing nix on my system.

---
