```yaml
number: 2286
title: Certain files are ignored without visible reason
type: issue
state: closed
author: actionless
labels:
  - invalid
assignees: []
created_at: 2022-08-17T16:56:28Z
updated_at: 2022-08-17T17:42:46Z
url: https://github.com/BurntSushi/ripgrep/issues/2286
synced_at: 2026-01-12T16:13:24Z
```

# Certain files are ignored without visible reason

---

_@actionless_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
pacman -S ripgrep

#### What operating system are you using ripgrep on?

Arch current

#### Describe your bug.

Certain files are ignored without visible reason such as having gitignore or rgignore file or etc

#### What are the steps to reproduce the behavior?

```console
$ cd /tmp/
$ mkdir env
$ echo '#export GTK3_MODULES="${GTK3_MODULES:+$GTK3_MODULES:}/usr/lib/libplotinus.so"' > env/.profile
```

#### What is the actual behavior?

```console
$ command rg plotinus --no-ignore --no-ignore-dot --hidden env/ --debug
DEBUG|rg::config|crates/core/config.rs:40: /home/lie/.config/rg/ripgreprc: arguments loaded from config file: ["--follow", "--text", "--glob", "!*.png", "--glob", "!*.jpg", "--glob", "!*.jpeg", "--glob", "!*.so", "--glob", "!*.pyc", "--glob", "!.git/*", "--glob", "!env/*", "--smart-case"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--follow", "--text", "--glob", "!*.png", "--glob", "!*.jpg", "--glob", "!*.jpeg", "--glob", "!*.so", "--glob", "!*.pyc", "--glob", "!.git/*", "--glob", "!env/*", "--smart-case", "plotinus", "--no-ignore", "--no-ignore-dot", "--hidden", "env/", "--debug"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:109: required literals found: [Cut(PLOTI), Cut(pLOTI), Cut(PlOTI), Cut(plOTI), Cut(PLoTI), Cut(pLoTI), Cut(PloTI), Cut(ploTI), Cut(PLOtI), Cut(pLOtI), Cut(PlOtI), Cut(plOtI), Cut(PLotI), Cut(pLotI), Cut(PlotI), Cut(plotI), Cut(PLOTi), Cut(pLOTi), Cut(PlOTi), Cut(plOTi), Cut(PLoTi), Cut(pLoTi), Cut(PloTi), Cut(ploTi), Cut(PLOti), Cut(pLOti), Cut(PlOti), Cut(plOti), Cut(PLoti), Cut(pLoti), Cut(Ploti), Cut(ploti)]
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:PLOTI|pLOTI|PlOTI|plOTI|PLoTI|pLoTI|PloTI|ploTI|PLOtI|pLOtI|PlOtI|plOtI|PLotI|pLotI|PlotI|plotI|PLOTi|pLOTi|PlOTi|plOTi|PLoTi|pLoTi|PloTi|ploTi|PLOti|pLOti|PlOti|plOti|PLoti|pLoti|Ploti|ploti)
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: ".git/*", re: "(?-u)^\\.git/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "env/*", re: "(?-u)^env/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('e'), Literal('n'), Literal('v'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 5 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring env/.profile: Ignore(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "!env/*", actual: "env/*", is_whitelist: true, is_only_dir: false })))))
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What is the expected behavior?

```console
$ grep -R -i plotinus env/
env/.profile:#export GTK3_MODULES="${GTK3_MODULES:+$GTK3_MODULES:}/usr/lib/libplotinus.so"
```


---

_Comment by @actionless on 2022-08-17 16:59_

sorry, i found the problem in my `.config/rg/ripgreprc`

---

_Closed by @actionless on 2022-08-17 16:59_

---

_Closed by @actionless on 2022-08-17 17:00_

---

_Comment by @actionless on 2022-08-17 17:03_

i wonder though if there is any flag similar to `--no-ignore-got` to skip ignore patterns from the main config file

---

_Comment by @BurntSushi on 2022-08-17 17:42_

> i wonder though if there is any flag similar to `--no-ignore-got` to skip ignore patterns from the main config file

Nope. This would require specifically splicing out `-g` flags from your ripgreprc. Ain't going to happen.

---

_Label `invalid` added by @BurntSushi on 2022-08-17 17:42_

---
