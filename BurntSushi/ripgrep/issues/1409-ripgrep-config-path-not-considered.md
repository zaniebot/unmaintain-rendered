```yaml
number: 1409
title: RIPGREP_CONFIG_PATH not considered
type: issue
state: closed
author: ghetto-ch
labels:
  - invalid
assignees: []
created_at: 2019-10-26T18:12:17Z
updated_at: 2019-12-08T13:45:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1409
synced_at: 2026-01-12T16:13:23Z
```

# RIPGREP_CONFIG_PATH not considered

---

_@ghetto-ch_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

pacman -S ripgrep

#### What operating system are you using ripgrep on?

Arch linux

#### Describe your question, feature request, or bug.

I set the RIPGREP_CONFIG_PATH as explained in the guide by adding this to my .profile:
`export RIPGREP_CONFIG_PATH=$HOME/.rgrc`

I confirm is set editing the file with:
`vim $(echo $RIPGREP_CONFIG_PATH)`

In the file I have:
```
--hidden
--smart-case
```

But when using rg the options are not used:
```
# rg --debug export
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(export)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob sep
```

Am I missing something?

---

_Comment by @BurntSushi on 2019-10-26 18:19_

I can't reproduce. Is that really the entire output from the `--debug` flag? Can you try with `--trace` instead?

---

_Comment by @ghetto-ch on 2019-10-26 18:53_

Sorry, no is not the full output:
[debug.txt](https://github.com/BurntSushi/ripgrep/files/3775162/debug.txt)
And also --trace:
[trace.txt](https://github.com/BurntSushi/ripgrep/files/3775163/trace.txt)





---

_Comment by @BurntSushi on 2019-10-26 18:56_

As far as I can tell, ripgrep doesn't think `RIPGREP_CONFIG_PATH` is set. This doesn't appear to be a bug with ripgreo. Please confirm that your environment is configured correctly and that you are actually using the latest version ripgrep.

You can tell it's working since the debug output will indicate when a config file is loaded.

---

_Comment by @ghetto-ch on 2019-10-26 18:59_

Well, if I `echo $RIPGREP_CONFIG_PATH` I get the rigth path. What else can I do to check?
I'm using the version from the Arch repo, 11.0.2. I will try to build it from source.

EDIT: I removed the Arch package and installed with `cargo install ripgrep` but doesn't help. The output from --trace is the same.

EDIT: tried on another machine and work as expected, if I will find the reason I'll edit the issue for reference.

---

_Comment by @BurntSushi on 2019-11-07 16:53_

@ghetto-ch Any updates? Thanks!

---

_Comment by @ghetto-ch on 2019-12-08 13:09_

@BurntSushi no sorry, I reinstalled everything on the machine and after that I had no problems. I don't know what was going on but seems not related to ripgrep. I think this can be closed.

---

_Closed by @ghetto-ch on 2019-12-08 13:09_

---

_Label `invalid` added by @BurntSushi on 2019-12-08 13:45_

---
