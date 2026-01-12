```yaml
number: 1595
title: rg crashes on every use. 
type: issue
state: closed
author: WaterSibilantFalling
labels:
  - invalid
assignees: []
created_at: 2020-05-25T01:13:15Z
updated_at: 2020-06-08T06:06:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1595
synced_at: 2026-01-12T16:13:23Z
```

# rg crashes on every use. 

---

_@WaterSibilantFalling_

#### What version of ripgrep are you using?

prompt> rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

via Debian aptitude

#### What operating system are you using ripgrep on?

WSL on Windows 10

Linux DESKTOP-6PNTD6B 4.4.0-17763-Microsoft #1217-Microsoft Mon May 05 16:09:00 PST 2020 x86_64 x86_64 x86_64 GNU/Linux

Ubuntu focal

#### Describe your bug.

rg crashes whenever it runs 

#### What are the steps to reproduce the behavior?

Run rg with any search pattern, with or without regex wildcards, character classes . . .

#### What is the actual behavior?

```
rg -i --debug ConfigurationFragment *

DEBUG|grep_regex::literal|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/grep-regex-0.1.5/src/literal.rs:105: required literals found: [Cut(CONFI), Cut(cONFI), Cut(CoNFI), Cut(coNFI), Cut(COnFI), Cut(cOnFI), Cut(ConFI), Cut(conFI), Cut(CONfI), Cut(cONfI), Cut(CoNfI), Cut(coNfI), Cut(COnfI), Cut(cOnfI), Cut(ConfI), Cut(confI), Cut(CONFi), Cut(cONFi), Cut(CoNFi), Cut(coNFi), Cut(COnFi), Cut(cOnFi), Cut(ConFi), Cut(conFi), Cut(CONfi), Cut(cONfi), Cut(CoNfi), Cut(coNfi), Cut(COnfi), Cut(cOnfi), Cut(Confi), Cut(confi)]
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 13 basenames, 7 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; thread '0<unnamed> literals, ' panicked at '0assertion failed: `(left == right)`
  left: `22`,
 right: `4` basenames, ', 11src/libstd/sys/unix/thread.rs extensions, thread ':0<unnamed>172 prefixes, ' panicked at ':0assertion failed: `(left == right)`
  left: `22`,
 right: `4`21 suffixes, ',
0src/libstd/sys/unix/thread.rsnote: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
 required extensions, :0172 regexes:
21
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:430: glob converted to regex: Glob { glob: "**/hs_err_pid*", re: "(?-u)^(?:/?|.*/)hs_err_pid[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('h'), Literal(thread ''<unnamed>s' panicked at ''assertion failed: `(left == right)`
  left: `22`,
 right: `4`)', , src/libstd/sys/unix/thread.rsLiteral:(172':21
_'), Literal('e'), Literal('r'), Literal('r'), Literal('_'), Literal('p'), Literal('i'), Literal('d'), ZeroOrMore]) }
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 2 literals, 5 basenames, 10 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 1 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, thread '0<unnamed> basenames, ' panicked at '11assertion failed: `(left == right)`
  left: `22`,
 right: `4` extensions, 0',  prefixes, src/libstd/sys/unix/thread.rs0: suffixes, 172:021 required extensions,
0 regexes
thread '<unnamed>' panicked at 'assertion failed: `(left == right)`
  left: `22`,
 right: `4`', src/libstd/sys/unix/thread.rs:172:DEBUG21|
globset|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/globset-0.4.4/src/lib.rs:435: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/ignore-0.4.10/src/walk.rs:1639: ignoring app/.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/ignore-0.4.10/src/walk.rs:1639: ignoring app/app.iml: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/mnt/c/sdm/SI App source/application.whitelabel.android/.gthread 'i<unnamed>t' panicked at 'iassertion failed: `(left == right)`
  left: `22`,
 right: `4`g', nsrc/libstd/sys/unix/thread.rso:r172e:"21)
, original: "*.iml", actual: "**/*.iml", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/ignore-0.4.10/src/walk.rs:1639: ignoring gradle/wrapper/gradle-wrapper.jar: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/mnt/c/sdm/SI App source/application.whitelabel.android/.gitignore"), original: "*.jar", actual: "**/*.jar", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|/usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/ignore-0.4.10/src/walk.rs:1639: ignoring app/build: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("app/.gitignore"), original: "/build", actual: "buildthread '"<unnamed>, ' panicked at 'is_whitelist: assertion failed: `(left == right)`
  left: `22`,
 right: `4`false', , src/libstd/sys/unix/thread.rsis_only_dir:: 172false: }21)
))

```
at this point rg freezes, computer CPU rises until one CPU is at 99%


#### What is the expected behavior?

* Found the string seached for
* not spit out errors
* not crashed & hung





---

_Comment by @BurntSushi on 2020-05-25 01:39_

This looks like a problem with your WSL environment. I'm not a Windows user, but IIRC, there is some problem with spawning threads in WSL1. ripgrep can't fix a broken environment.

You might be able to work around this by using `-j1`, which will disable multi-threading. Searches will be slower.

---

_Closed by @BurntSushi on 2020-05-25 01:39_

---

_Label `invalid` added by @BurntSushi on 2020-05-25 01:39_

---

_Comment by @WaterSibilantFalling on 2020-06-08 06:06_

BTW, adding ``-j1`` is the solution.  

ripgrep / rg now works perfectly on Windows 10

---
