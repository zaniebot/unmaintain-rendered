```yaml
number: 1299
title: "Confusing behavior: rg stops without warning when encountering binary data, even after printing matches"
type: issue
state: closed
author: mateon1
labels:
  - duplicate
assignees: []
created_at: 2019-06-13T05:26:08Z
updated_at: 2019-06-13T11:21:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1299
synced_at: 2026-01-12T16:13:23Z
```

# Confusing behavior: rg stops without warning when encountering binary data, even after printing matches

---

_@mateon1_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`ripgrep-0.10.0` via nixpkgs.

#### What operating system are you using ripgrep on?

NixOS 18.03, with unstable nixpkgs channel, `uname -a`:
`Linux ceres 4.14.90 #1-NixOS SMP Fri Dec 21 13:13:19 UTC 2018 x86_64 GNU/Linux`

#### Describe your question, feature request, or bug.

ripgrep stops reading data (without warning!) on the first unprintable character it encounters, even if it has already printed matches.

I encountered this issue when trying to use ripgrep to process a large corpus of text, which includes some incorrectly processed data, such as ASCII control codes and unprintable unicode codepoints.
I was piping the output of unzip into rg, and was wondering why rg was finishing within only a few seconds, as the corpus is hundreds of gigabytes in size.
rg was printing matches up until a certain point, where it exited with zero warning.
The workaround is to pass the `-a` flag to ripgrep, which allows rg to process the whole corpus.

#### If this is a bug, what are the steps to reproduce the behavior?

Unknown, the corpus is too large to share easily.
I could not produce a reduced testcase manually, as rg seems to stop at null bytes despite the `-a` flag (so a testcase with null bytes isn't equivalent), and I don't know how to make echo in bash produce invalid codepoints otherwise.

#### If this is a bug, what is the actual behavior?

`$ unzip -p Corpus.zip | pv | rg -i 'a peak' > ctx/a_peak.txt`

Output is too large to upload in its entirety

The only DEBUG information is printed at the start:
```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:100: required literals found: [Cut(A PEA), Cut(a PEA), Cut(A pEA), Cut(a pEA), Cut(A PeA), Cut(a PeA), Cut(A peA), Cut(a peA), Cut(A PEa), Cut(a PEa), Cut(A pEa), Cut(a pEa), Cut(A Pea), Cut(a Pea), Cut(A pea), Cut(a pea)]
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
Nothing is printed as rg exits

#### If this is a bug, what is the expected behavior?

I believe that either:
- rg should switch to text mode `-a` after printing it's first match if reading from stdin, or
- rg should exit with a warning (on stderr) if stdin contains binary data and no relevant flags were passed

---

_Comment by @mateon1 on 2019-06-13 05:40_

Sorry, closing this issue as this is fixed in ripgrep 11.*, I did not notice my version of ripgrep was outdated, since I thought I did a full system update about a week ago.

---

_Closed by @mateon1 on 2019-06-13 05:40_

---

_Label `duplicate` added by @BurntSushi on 2019-06-13 11:21_

---
