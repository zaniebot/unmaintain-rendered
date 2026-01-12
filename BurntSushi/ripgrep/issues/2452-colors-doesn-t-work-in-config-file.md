```yaml
number: 2452
title: "--colors doesn't work in config file"
type: issue
state: closed
author: ruppde
labels:
  - invalid
assignees: []
created_at: 2023-03-14T17:03:16Z
updated_at: 2023-03-14T18:05:15Z
url: https://github.com/BurntSushi/ripgrep/issues/2452
synced_at: 2026-01-12T16:13:24Z
```

# --colors doesn't work in config file

---

_@ruppde_

#### What version of ripgrep are you using?
Tried these 2:
```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

ripgrep 13.0.0 (rev 44fb9fce2c)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
#### How did you install ripgrep?

apt and self compiled

#### What operating system are you using ripgrep on?

Linux Mint 21.1 = bookworm/sid
5.15.0-60-generic x86_64 GNU/Linux

#### Describe your bug.

--colors doesn't work in config file. Other paramters like --debug work. --colors works on the command line.

#### What are the steps to reproduce the behavior?

```bash
$ hexdump .ripgreprc
00000000  2d 2d 63 6f 6c 6f 72 73  20 6d 61 74 63 68 3a 62  |--colors match:b|
00000010  67 3a 30 2c 31 32 38 2c  32 35 35 0a              |g:0,128,255.|
0000001c
$ RIPGREP_CONFIG_PATH=~/.ripgreprc rg --debug test /tmp/test
DEBUG|rg::config|crates/core/config.rs:40: /home/a/.ripgreprc: arguments loaded from config file: ["--colors match:bg:0,128,255"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--colors match:bg:0,128,255", "--debug", "test", "/tmp/test"]
error: Found argument '--colors match:bg:0,128,255' which wasn't expected, or isn't valid in this context
	Did you mean --colors?

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] -e PATTERN ... [PATH ...]
    rg [OPTIONS] -f PATTERNFILE ... [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN
    rg [OPTIONS] --help
    rg [OPTIONS] --version

For more information try --help
```

It works if I source the file with `cat ~/.ripgreprc`
```bash
$ rg `cat ~/.ripgreprc` --debug test /tmp/test
DEBUG|grep_regex::literal|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-regex-0.1.9/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
1:test
```
With other paramters it works, e.g. if ~/.ripgreprc contains only `--debug`, it shows debug outout (even if I leave it out unlike in the example above ;)

#### What is the actual behavior?

see above

#### What is the expected behavior?

Use ~/.ripgreprc also for --colors


---

_Comment by @BurntSushi on 2023-03-14 17:27_

If there's a problem with your config file, then _you should share the contents of your config file._ However, in this case, because you shared your `--debug` output, the root problem can be determined:

```
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--colors match:bg:0,128,255", "--debug", "test", "/tmp/test"]
```

Notice that `--colors match:bg:0,128,255` is a single argument. But it should be two. As the [docs for configuration files say](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file):

> Every line is a shell argument, after trimming whitespace.

The docs go on to discuss this in more detail in the example below, and then again in prose below that.

So you can't do

```
--colors match:bg:0,128,255
```

You need to do either

```
--colors=match:bg:0,128,255
```

or

```
--colors
match:bg:0,128,255
```


---

_Closed by @BurntSushi on 2023-03-14 17:27_

---

_Label `invalid` added by @BurntSushi on 2023-03-14 17:27_

---

_Comment by @ruppde on 2023-03-14 18:05_

thanks. sent a PR to clarify it in the man page: 
https://github.com/BurntSushi/ripgrep/pull/2453

---
