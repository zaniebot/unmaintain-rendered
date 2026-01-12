```yaml
number: 2311
title: No such file or directory (os error 2)
type: issue
state: closed
author: lukastillmann
labels:
  - invalid
assignees: []
created_at: 2022-09-22T18:37:22Z
updated_at: 2022-09-22T19:18:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2311
synced_at: 2026-01-12T16:13:24Z
```

# No such file or directory (os error 2)

---

_@lukastillmann_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

I also tried the two previous major releases and hat the same issue. 

#### How did you install ripgrep?

With the `.deb` binary file

#### What operating system are you using ripgrep on?

Windows WSL (Ubuntu 20.04.4 LTS)

#### Describe your bug.

No matter what I am searching for, I get the error message `No such file or directory (os error 2)`

#### What are the steps to reproduce the behavior?

```
echo test > testfile.txt
rg test testfile.txt
```

#### What is the actual behavior?

```
$ rg --debug test testfile.txt

DEBUG|rg::config|crates/core/config.rs:40: /home/lukas/.ripgreprc: arguments loaded from config file: ["--glob", "!git/*", "!node_modules/*\"", "--smart-case", "--max-columns=150", "--max-columns-preview"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--glob", "!git/*", "!node_modules/*\"", "--smart-case", "--max-columns=150", "--max-columns-preview", "--debug", "test", "testfile.txt"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:109: required literals found: [Cut(!NODE_M), Cut(!nODE_M), Cut(!NoDE_M), Cut(!noDE_M), Cut(!NOdE_M), Cut(!nOdE_M), Cut(!NodE_M), Cut(!nodE_M), Cut(!NODe_M), Cut(!nODe_M), Cut(!NoDe_M), Cut(!noDe_M), Cut(!NOde_M), Cut(!nOde_M), Cut(!Node_M), Cut(!node_M), Cut(!NODE_m), Cut(!nODE_m), Cut(!NoDE_m), Cut(!noDE_m), Cut(!NOdE_m), Cut(!nOdE_m), Cut(!NodE_m), Cut(!nodE_m), Cut(!NODe_m), Cut(!nODe_m), Cut(!NoDe_m), Cut(!noDe_m), Cut(!NOde_m), Cut(!nOde_m), Cut(!Node_m), Cut(!node_m)]
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:!NODE_M|!nODE_M|!NoDE_M|!noDE_M|!NOdE_M|!nOdE_M|!NodE_M|!nodE_M|!NODe_M|!nODe_M|!NoDe_M|!noDe_M|!NOde_M|!nOde_M|!Node_M|!node_M|!NODE_m|!nODE_m|!NoDE_m|!noDE_m|!NOdE_m|!nOdE_m|!NodE_m|!nodE_m|!NODe_m|!nODe_m|!NoDe_m|!noDe_m|!NOde_m|!nOde_m|!Node_m|!node_m)
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "git/*", re: "(?-u)^git/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
test: No such file or directory (os error 2)
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What is the expected behavior?

It should print the string `test`


---

_Comment by @BurntSushi on 2022-09-22 18:45_

This isn't a bug. The `--debug` logs give you your answer:

```
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--glob", "!git/*", "!node_modules/*\"", "--smart-case", "--max-columns=150", "--max-columns-preview", "--debug", "test", "testfile.txt"]
```

Notice that you have `--glob !git/*`, and then `!node_modules/*` without a corresponding `--glob`. Another hint is the regex that ripgrep found:

```
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:!NODE_M|!nODE_M|!NoDE_M|!noDE_M|!NOdE_M|!nOdE_M|!NodE_M|!nodE_M|!NODe_M|!nODe_M|!NoDe_M|!noDe_M|!NOde_M|!nOde_M|!Node_M|!node_M|!NODE_m|!nODE_m|!NoDE_m|!noDE_m|!NOdE_m|!nOdE_m|!NodE_m|!nodE_m|!NODe_m|!nODe_m|!NoDe_m|!noDe_m|!NOde_m|!nOde_m|!Node_m|!node_m)
```

It's important to make sure you don't have any aliases or config files throwing a wrench in things before filing a bug.

---

_Closed by @BurntSushi on 2022-09-22 18:45_

---

_Label `invalid` added by @BurntSushi on 2022-09-22 18:45_

---

_Comment by @BurntSushi on 2022-09-22 18:46_

It also looks like you have a stray quote in `"!node_modules/*\""`.

Be sure to read the docs for the config file format carefully.

---

_Comment by @lukastillmann on 2022-09-22 19:15_

Thank you for your answer. However, I do not have a config file set up anywhere. This is the native behavior after installing ripgrep.. 

Any ideas, where this wrong configuration might come from? 

---

_Comment by @lukastillmann on 2022-09-22 19:16_

Never mind.. I found it. Must have had it forever from a previous try to install ripgrep. 

Sorry for the inconvenience! 

---

_Comment by @BurntSushi on 2022-09-22 19:18_

Indeed. Not only does the file need to be explicitly created by you (ripgrep won't do it), but you *also* need to set an env var.

---
