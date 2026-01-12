```yaml
number: 1726
title: "Improve confusing output for badly formatted files containing \"\\r\" control codes"
type: issue
state: closed
author: twe4ked
labels:
  - wontfix
assignees: []
created_at: 2020-11-05T01:10:57Z
updated_at: 2020-11-05T01:35:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1726
synced_at: 2026-01-12T16:13:24Z
```

# Improve confusing output for badly formatted files containing "\r" control codes

---

_@twe4ked_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Cargo.

#### What operating system are you using ripgrep on?

macOS 10.15.4 (19E287)

#### Describe your bug.

I've recently encountered a file containing `\r` (`0x0d`) characters that caused some confusing output when searching.

#### What are the steps to reproduce the behavior? / What is the actual behavior?

Given the following file:
```
$ hexyl backslash-r.txt
┌────────┬─────────────────────────┬─────────────────────────┬────────┬────────┐
│00000000│ 61 20 6c 6f 74 20 6f 66 ┊ 20 74 65 78 74 20 66 6f │a lot of┊ text fo│
│00000010│ 6f 0d 62 61 72 20 61 6e ┊ 64 20 73 6f 6d 65 20 6d │o_bar an┊d some m│
│00000020│ 6f 72 65 20 74 65 78 74 ┊ 0a                      │ore text┊_       │
└────────┴─────────────────────────┴─────────────────────────┴────────┴────────┘
```

When searching for `lot` the `\r` causes the filename/line number etc. to be omitted:

```
$ rg lot backslash-r.txt
bar and some more text
```

File was created in Ruby (don't judge me!) with:

```ruby
File.open("backslash-r.txt", "w").tap { |f| f.write("a lot of text foo\rbar and some more text\n") }.close
```

Debug output, not sure if it's useful in this case:

```
$ rg --debug lot backslash-r.txt
DEBUG|grep_regex::literal|/Users/odin/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.8/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(lot)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/Users/odin/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/odin/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.5/src/lib.rs:431: built glob set; 6 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
bar and some more text
```

#### What is the expected behavior?

Honestly I'm not sure. This could definitely be closed as wontfix/not a bug because using only `\r` seems to be an old (pre-X) Mac OS thing. I think it would have been nice to maybe turn the `\r` into `\r\n` so that it doesn't overwrite the ripgrep output.

I'd be happy to have a crack at fixing this if we come up with a solution.

Love the tool and all of your contributions to the community!

---

_Comment by @BurntSushi on 2020-11-05 01:20_

Yeah I don't think there is much to be done here. Changing the output doesn't seem like a good idea to me. ripgrep should emit the contents of the files. grep has the same behavior here.

The issue here is that the `\r` is being interpreted by your terminal as a special command to move the cursor back to the beginning of the line before writing subsequent contents. You can perhaps see this more clearly if you use `a lot of text foo\rWAT\n` as the contents instead:

```
$ rg lot /tmp/rg-1726.txt
WAT lot of text foo
```

---

_Closed by @BurntSushi on 2020-11-05 01:20_

---

_Label `wontfix` added by @BurntSushi on 2020-11-05 01:20_

---

_Comment by @twe4ked on 2020-11-05 01:34_

Fair enough, following `grep` makes sense. Thanks.

One further thought, what do you think about adding an option to escape all control characters when printing? Something like `--escape-output`.

---
