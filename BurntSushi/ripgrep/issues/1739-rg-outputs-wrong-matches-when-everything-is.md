```yaml
number: 1739
title: rg outputs wrong matches when everything is matched and replaced with custom string
type: issue
state: closed
author: borestad
labels: []
assignees: []
created_at: 2020-11-22T03:10:09Z
updated_at: 2021-06-01T01:51:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1739
synced_at: 2026-01-12T16:13:24Z
```

# rg outputs wrong matches when everything is matched and replaced with custom string

---

_@borestad_

#### What version of ripgrep are you using?

ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?
brew install ripgrep

#### What operating system are you using ripgrep on?
Darwin 19.6.0 x86_64
zsh 5.8 (x86_64-apple-darwin19.6.0)


#### Describe your bug.

rg outputs wrong matches when everything is matched and replaced with custom string

#### What are the steps to reproduce the behavior?

**Preparation**
```
❯❯❯ cd $(mktemp -d)
❯❯❯ touch a b c d e
```

**Example1 - When it works**

```
❯❯❯ fd . | rg 'a.*' -r '$0.<NO_EXTENSION>'

a.<NO_EXTENSION>
```


**Example2 - When it fails**

```
❯❯❯ fd . | rg '.*' -r '$0.<NO_EXTENSION>'

a.<NO_EXTENSION>
.<NO_EXTENSION>
b.<NO_EXTENSION>
.<NO_EXTENSION>
c.<NO_EXTENSION>
.<NO_EXTENSION>
d.<NO_EXTENSION>
.<NO_EXTENSION>
e.<NO_EXTENSION>
.<NO_EXTENSION>

```

#### What is the expected behavior?

**Example2 - How it should behave**

```
❯❯❯ fd . | rg '.*' -r '$0.<NO_EXTENSION>'

a.<NO_EXTENSION>
b.<NO_EXTENSION>
c.<NO_EXTENSION>
d.<NO_EXTENSION>
e.<NO_EXTENSION>
```


#### What do you think ripgrep should have done?
`Removed the .<NO_EXTENSION> from the output`



---

_Comment by @learnbyexample on 2020-11-26 05:14_

I think this is because of how `*` can match **zero** or more times.

```bash
$ echo 'a' | rg '.*' -r '$0.<NO_EXTENSION>'
a.<NO_EXTENSION>
.<NO_EXTENSION>
$ echo 'a' | rg '.+' -r '$0.<NO_EXTENSION>'
a.<NO_EXTENSION>

$ echo 'a' | perl -lpe 's/.*/$&.<NO_EXTENSION>/g'
a.<NO_EXTENSION>.<NO_EXTENSION>
$ echo 'a' | perl -lpe 's/.+/$&.<NO_EXTENSION>/g'
a.<NO_EXTENSION>
```

See https://www.regular-expressions.info/zerolength.html for a detailed discussion on this topic.

---

_Comment by @BurntSushi on 2020-11-26 15:22_

The `.*` matching an empty string is almost certainly the issue here, but I'm not sure whether the end result is correct or not. In particular, these examples is interesting:

```
$ printf a | rg '.*' -r '${0}f'
af
$ printf 'a\n' | rg '.*' -r '${0}f'
af
f
```

My guess here is that something about the replacement code is erroneously matching the position immediately after the `\n`. I say "erroneous" here because, logically speaking, grep tools work by matching a line at a time, and that generally doesn't include the line terminator. (Without multiline mode for example, ripgrep literally forbids you from writing a regex that matches a line terminator.)

Moreover, it is _at least_ inconsistent:

```
$ printf 'a' | rg '.*' --json
{"type":"begin","data":{"path":{"text":"<stdin>"}}}
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"a"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"a"},"start":0,"end":1}]}}
{"type":"end","data":{"path":{"text":"<stdin>"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":123261,"human":"0.000123s"},"searches":1,"searches_with_match":1,"bytes_searched":1,"bytes_printed":217,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.006336s","nanos":6335770,"secs":0},"stats":{"bytes_printed":217,"bytes_searched":1,"elapsed":{"human":"0.000123s","nanos":123261,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}

$ printf 'a\n' | rg '.*' --json
{"type":"begin","data":{"path":{"text":"<stdin>"}}}
{"type":"match","data":{"path":{"text":"<stdin>"},"lines":{"text":"a\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"a"},"start":0,"end":1}]}}
{"type":"end","data":{"path":{"text":"<stdin>"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":165502,"human":"0.000166s"},"searches":1,"searches_with_match":1,"bytes_searched":2,"bytes_printed":219,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.005240s","nanos":5240405,"secs":0},"stats":{"bytes_printed":219,"bytes_searched":2,"elapsed":{"human":"0.000166s","nanos":165502,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

Notice how the second search reports exactly one match. Which is inconsistent with the fact that the replacement behaves as if there are two matches. So as far as I can tell, _one_ of these is wrong.

---

_Comment by @learnbyexample on 2020-11-27 03:49_

>Moreover, it is at least inconsistent:

Just remembered, I had filed a similar issue: https://github.com/BurntSushi/ripgrep/issues/1295

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
