```yaml
number: 2546
title: exits unexpectedly with code 1 when ran within systemd sanbox
type: issue
state: closed
author: adrian-gierakowski
labels: []
assignees: []
created_at: 2023-06-30T15:28:10Z
updated_at: 2023-07-01T10:21:38Z
url: https://github.com/BurntSushi/ripgrep/issues/2546
synced_at: 2026-01-12T16:13:24Z
```

# exits unexpectedly with code 1 when ran within systemd sanbox

---

_@adrian-gierakowski_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

via [nix](https://nixos.org/)

#### What operating system are you using ripgrep on?

NixOS

#### Describe your bug.

running the following inside systemd sandbox, in an empy directory:

```sh
echo something > test
rg --no-ignore --debug something
```

produces the following output, followed by exit with code 1:

```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(shared)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

When ran on the same machine, but outside the sandbox produces the following, so I'm assuming in the previous case it exited prematurely:

```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(something)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
test
1:something
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What are the steps to reproduce the behavior?

as above

#### What is the actual behavior?

as above

#### What is the expected behavior?

results are listed and exit with code 0


---

_Comment by @BurntSushi on 2023-06-30 15:37_

I don't see a complete set of commands required to reproduce this issue, so I'm not sure what to say there.

Otherwise, at a guess, I'd expect that perhaps the systemd sandbox is messing with ripgrep's tty detection. Since you've called ripgrep with a pattern but didn't tell it what to search, you're asking ripgrep to guess. The two choices are "search the current directory" or "search stdin." It's possible systemd has configured the environment in a way that ripgrep incorrectly chooses to search stdin, and if stdin is closed/empty, then ripgrep will search it and report no results. And thus exit with `1`.

I'd suggest you tell ripgrep what to search instead of asking it to guess.

---

_Renamed from "exists unexpectedly with code 1 when ran within systemd sanbox" to "exits unexpectedly with code 1 when ran within systemd sanbox" by @adrian-gierakowski on 2023-07-01 07:36_

---

_Comment by @adrian-gierakowski on 2023-07-01 10:21_

>  It's possible systemd has configured the environment in a way that ripgrep incorrectly chooses to search stdin, and if stdin is closed/empty, then ripgrep will search it and report no results. And thus exit with 1.

That was it :facepalm: 

Thank you for your help (ant the great software) and apologies for taking your time. 

---

_Closed by @adrian-gierakowski on 2023-07-01 10:21_

---
