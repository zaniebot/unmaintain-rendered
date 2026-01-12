```yaml
number: 2293
title: Ripgrep panicks when run with --word-regexp and empty string
type: issue
state: closed
author: Emilv2
labels:
  - duplicate
assignees: []
created_at: 2022-08-28T20:59:57Z
updated_at: 2022-08-28T22:15:50Z
url: https://github.com/BurntSushi/ripgrep/issues/2293
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep panicks when run with --word-regexp and empty string

---

_@Emilv2_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

Arch linux package manager

#### What operating system are you using ripgrep on?

Arch linux

#### Describe your bug.
Ripgrep panicks when run with --word-regexp and empty string.

#### What are the steps to reproduce the behavior?

For example, run in the ripgrep repository:

`rg --word-regexp -- ''`

#### What is the actual behavior?

```
DEBUG|grep_regex::word|crates/regex/src/word.rs:52: word regex: "(?:(?m:^)|\\W)((?:z{0})*)(?:\\W|(?m:$))"                                                                                  
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "*", re: "(?-u)^[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash
_escape: true }, tokens: Tokens([ZeroOrMore]) }                                                                                                                                            
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes                               
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                               
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 6 literals, 6 basenames, 2 extensions, 0 prefixes, 0 suffixes, 3 required extensions, 0 regexes                               
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))                                                                                            
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.cargo: Ignore(IgnoreMatch(Hidden))                                                                                          
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1744: whitelisting ./.github: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "!/.github/", actual: ".github", is_wh
itelist: true, is_only_dir: true })))                                                                                                                                                      
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))                                                                                      
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.ignore: Ignore(IgnoreMatch(Hidden))                                                                                         
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9                                                    
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace                                                                                                              
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9                                                    
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9                                                    
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9                                                    
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                              
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9                                                    
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Any', crates/ignore/src/walk.rs:1302:31                                                                            
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /build/ripgrep/src/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
tests/data/sherlock.br

```

#### What is the expected behavior?

I'm not sure what the correct output would be since I'm not sure `--word-regexp` with an empty string makes sense, but certainly panicking isn't what I would expect.

---

_Comment by @BurntSushi on 2022-08-28 22:15_

Duplicate of #1891, #2096

---

_Closed by @BurntSushi on 2022-08-28 22:15_

---

_Label `duplicate` added by @BurntSushi on 2022-08-28 22:15_

---
