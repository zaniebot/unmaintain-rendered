```yaml
number: 2422
title: PCRE2 lookbehind, not convincing
type: issue
state: closed
author: McUsr
labels:
  - invalid
assignees: []
created_at: 2023-02-20T10:25:30Z
updated_at: 2023-11-22T21:46:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2422
synced_at: 2026-01-12T16:13:24Z
```

# PCRE2 lookbehind, not convincing

---

_@McUsr_

#### What version of ripgrep are you using?
13.0 installed todayt
Replace this text with the output of `rg --version`.
ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
#!/bin/bash
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep_13.0.0_amd64.deb
sudo dpkg -i ripgrep_13.0.0_amd64.deb

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

#### What operating system are you using ripgrep on?

Distributor ID: Debian
Description:    Debian GNU/Linux 11 (bullseye)
Release:        11
Codename:       bullseye


#### Describe your bug.

I wanted to return a list of bash function headers, but not those that were commented out. I thought I'd use a backreference for that:

 rg --pcre2 '(?>!# ).*\(\)'

I have tried some derivations of the above, and none seem to work, as I get '# funcname()' in the output, all the time, now, I don't NEED to use back references to achieve my goals, so, I am all good.


Give a high level description of the bug.

#### What are the steps to reproduce the behavior?

You have the meat above, I am on bash 5.14 when I'm doing this.


#### What is the actual behavior?

It displays the lines that starts with '#' 
Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If the output is large, put it in a gist: https://gist.github.com/

If the output is small, put it in code fences:

```
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

```

#### What is the expected behavior?

What do you think ripgrep should have done?

I think it is the pcrelib, not ripgrep.


---

_Comment by @BurntSushi on 2023-02-20 11:50_

Look behind and backreferences are two different things. I don't see any backreferences in your comment.

Otherwise I can't help you because I don't see a reproduction. Please provide some example input, actual output and desired output.

---

_Comment by @BurntSushi on 2023-11-22 21:46_

Closing due to inactivity.

---

_Closed by @BurntSushi on 2023-11-22 21:46_

---

_Label `invalid` added by @BurntSushi on 2023-11-22 21:46_

---
