```yaml
number: 2210
title: Combination of --file and --replace with capture group does not work as expected
type: issue
state: closed
author: salsifis
labels:
  - wontfix
assignees: []
created_at: 2022-05-12T07:18:07Z
updated_at: 2024-05-18T00:02:09Z
url: https://github.com/BurntSushi/ripgrep/issues/2210
synced_at: 2026-01-12T16:13:24Z
```

# Combination of --file and --replace with capture group does not work as expected

---

_@salsifis_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Windows 10 21H2 (build 19044)

#### Describe your bug.

It is not possible to use `--file` in combination with `--replace` where there are capture groups.

`rg --file <pattern_file> --replace "replacement_with_$capturegroup" --only-matching`

#### What are the steps to reproduce the behavior?

Suppose I would like to extract names of streets in different languages from text files.
I constitute the following pattern file:

```
\d+,?\s*rue (.+)
(\S+?)strasse\s*\d+
\d+,?\s*(.+?)\s+street
```
note: I have tried to use `\r`, `\n` and `\r\n` as line endings for the pattern file

Now, I try to invoke ripgrep:

`rg --file pattern_file --glob *.txt --only-matching --replace "$1"`

with the following input (sorry if my German or English are broken) :

```
La maison est au 3 rue Victor Hugo.
Du lebst am Hauptstrasse 39.
19, Raffles street is the address.
```

#### What is the actual behavior?

The actual behavior is that `$1` in my replace string corresponds only tho the capture group of the first pattern.

The output is:

```
test.txt
1:Victor Hugo
2:
3:
```

Replacing with `$1$2$3` works but this workaround could work only with a small and known number of pattern lines.

#### What is the expected behavior?

I think that the following output is desirable:

```
text.txt
1:Victor Hugo.
2:Haupt
3:Raffles
```

#### Notes
It is not possible to use a pattern file in which all patterns have a named capture group, with the same name.

For example this is not supported, and an error complains that a named capture group is declared twice:

```
\d+,?\s*rue (?P<street>.+)
(?P<street>\S+?)strasse\s*\d+
\d+,?\s*(?P<street>.+?)\s+street
```

---

_Comment by @BurntSushi on 2022-05-12 10:53_

This is because the pattern file is compiled down into a single regex. I don't see any easy way of making what you want work. Sorry.

---

_Closed by @BurntSushi on 2022-05-12 10:53_

---

_Label `wontfix` added by @BurntSushi on 2022-05-12 10:53_

---

_Comment by @salsifis on 2022-05-12 11:21_

> This is because the pattern file is compiled down into a single regex. I don't see any easy way of making what you want work. Sorry.

Yes, I was a bit afraid that this would be too complicated because it would imply much refactoring.

Maybe it would be worth mentioning in the documentation that capture groups and `-f` do not mix well.

---

_Comment by @pamburus on 2024-05-17 23:53_

How about using this?
https://docs.rs/regex-automata/latest/regex_automata/meta/struct.Regex.html#method.new_many

It supports multiple regular expressions with repeated groups.

---

_Comment by @BurntSushi on 2024-05-18 00:02_

Yes, I wrote that. I do have vague ambitions to eventually migrate ripgrep to those APIs, but there's a lot of engineering effort involved there. I believe it also currently inhibits some key optimizations. And there may be semantic differences that I can't think of off the top of my head that makes it difficult to do.

---
