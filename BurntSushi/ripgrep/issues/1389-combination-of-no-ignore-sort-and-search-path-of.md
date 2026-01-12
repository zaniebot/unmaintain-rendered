```yaml
number: 1389
title: Combination of --no-ignore, --sort, and search path of a symbolic link to a directory causes panic
type: issue
state: closed
author: HarrisonMc555
labels: []
assignees: []
created_at: 2019-09-25T22:03:04Z
updated_at: 2020-02-17T22:16:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1389
synced_at: 2026-01-12T16:13:23Z
```

# Combination of --no-ignore, --sort, and search path of a symbolic link to a directory causes panic

---

_@HarrisonMc555_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS Mojave 10.14.6 (18G95)

#### Describe your question, feature request, or bug.

When I run the following command that searches a symbolic link, ripgrep panics (after displaying output):

```rg -n --hidden --no-ignore --color=always --sort path pattern ~/symbolic-link-to-directory```

#### If this is a bug, what are the steps to reproduce the behavior?

Use the previous flags on a symbolic link to a directory.

#### If this is a bug, what is the actual behavior?
```
$ rg --debug -n --hidden --no-ignore --color=always --sort path every baz
DEBUG|rg::config|src/config.rs:40: /Users/harrisonmccullough/.ripgreprc: arguments loaded from config file: ["--smart-case"]
DEBUG|rg::args|src/args.rs:560: final argv: ["rg", "--smart-case", "--debug", "-n", "--hidden", "--no-ignore", "--color=always", "--sort", "path", "every", "baz"]
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(EVERY), Complete(eVERY), Complete(EvERY), Complete(evERY), Complete(EVeRY), Complete(eVeRY), Complete(EveRY), Complete(eveRY), Complete(EVErY), Complete(eVErY), Complete(EvErY), Complete(evErY), Complete(EVerY), Complete(eVerY), Complete(EverY), Complete(everY), Complete(EVERy), Complete(eVERy), Complete(EvERy), Complete(evERy), Complete(EVeRy), Complete(eVeRy), Complete(EveRy), Complete(eveRy), Complete(EVEry), Complete(eVEry), Complete(EvEry), Complete(evEry), Complete(EVery), Complete(eVery), Complete(Every), Complete(every)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
baz/file2.txt
1:Hello everyone
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/libcore/option.rs:347:21
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: rust_begin_unwind
   7: core::panicking::panic_fmt
   8: core::panicking::panic
   9: rg::try_main
  10: rg::main
  11: std::rt::lang_start::{{closure}}
  12: std::panicking::try::do_call
  13: __rust_maybe_catch_panic
  14: std::rt::lang_start_internal
  15: main
```

#### If this is a bug, what is the expected behavior?

Not panic.

---

_Comment by @HarrisonMc555 on 2019-09-25 22:10_

After further experimentation, it looks like it is the combination of the following:

  - The search path is a symbolic link to a directory
  - The `--no-ignore` flag is set
  - The `--sort` flag is set (with something other than `none`)

I used the `--no-config` flag and got the same results.

---

_Comment by @BurntSushi on 2019-09-25 22:32_

Please provide a complete reproduction, as the issue template requests. Please provide commands to setup an identical corpus, for example.

---

_Comment by @HarrisonMc555 on 2019-09-25 22:51_

```bash
mkdir mydir
ln -s mydir mylink
echo "text in file" >> mydir/file.txt
rg text --no-ignore --sort path # no search path is OK
rg text --no-ignore --sort path mydir # regulary directory is also OK
rg text --no-ignore --sort path mylink # symbolic link to directory is NOT OK
```

If you either remove all ripgrep configuration files or include the `--no-config` flag you get the same results.

---

_Renamed from "Panics for unknown reason" to "Combination of --no-ignore, --sort, and search path of a symbolic link to a directory causes panic" by @HarrisonMc555 on 2019-09-25 23:04_

---

_Comment by @nikofil on 2019-10-02 09:31_

Hey! I wanted to contribute to ripgrep as I use it a lot, so I made a PR to fix this issue. Hope that's ok!

This was caused by https://github.com/BurntSushi/ripgrep/blob/8892bf648cfec111e6e7ddd9f30e932b0371db68/ignore/src/walk.rs#L1036-L1040 where you get whether a file is a directory or not using `is_dir()`. When symlinks are not followed, however, this will return `false` even in the case of a symlink that points to a directory. Then, when ripgrep gets an event signaling that the directory listing is over https://github.com/BurntSushi/ripgrep/blob/8892bf648cfec111e6e7ddd9f30e932b0371db68/ignore/src/walk.rs#L949-L951 it tries to get the parent `Ignore` object which doesn't exist in this case, causing a panic.

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
