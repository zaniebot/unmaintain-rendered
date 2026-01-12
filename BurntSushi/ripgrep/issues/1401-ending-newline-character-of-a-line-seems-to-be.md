```yaml
number: 1401
title: Ending newline character of a line seems to be exposed to the regex engine with -P option
type: issue
state: closed
author: learnbyexample
labels:
  - bug
  - rollup
assignees: []
created_at: 2019-10-10T05:49:44Z
updated_at: 2021-06-01T01:51:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1401
synced_at: 2026-01-12T16:13:23Z
```

# Ending newline character of a line seems to be exposed to the regex engine with -P option

---

_@learnbyexample_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Using `ripgrep_11.0.2_amd64.deb` 

#### What operating system are you using ripgrep on?

Ubuntu LTS 16.04

#### Describe your question, feature request, or bug.

When `-P` option is used, `\s` seems to take ending newline character of input line into consideration.

#### If this is a bug, what are the steps to reproduce the behavior?

Consider this sample input file:

```bash
$ printf 'foo 42\nxoyz\ncat\tdog\n' > ip.txt
$ cat ip.txt
foo 42
xoyz
cat	dog
```

Here's a minimal example showing the issue (I needed lookarounds, hence the need for `-P` switch)

```bash
$ # extract till last 'o' in the line, if there are no more whitespaces after 'o'
$ # the issue is that characters after 'o' are also displayed despite the -o switch
$ rg -NoP '.*o(?!.*\s)' ip.txt
xoyz
cat	dog

$ # works if I replace \s with space/tab character class or use GNU grep
$ # using \s(?!$) instead of \s is another workaround that works
$ rg -NoP '.*o(?!.*[ \t])' ip.txt
xo
cat	do
$ grep -oP '.*o(?!.*\s)' ip.txt
xo
cat	do
```

Another example shown below:

```
$ rg -No '.*\s' ip.txt
foo 
cat	

$ rg -NoP '.*\s' ip.txt
foo 42
cat	dog
```


---

_Comment by @BurntSushi on 2019-10-10 10:41_

Interesting, yeah, I think I might see why this happens. With the default regex engine, all character classes containing `\n` (including `\s`) are _rewritten_ to _not_ contain `\n`. But doing this isn't possible with PCRE2. Of course, this is why ripgrep then uses PCRE2 on a line-by-line basis instead of searching through many lines at once. My guess is that it is including the the `\n` at the end of each line, and it probably shouldn't be. Although, I haven't thought through the full consequences of that. I will attempt a fix for this at some point, but it is possible that this will end up being a `wontfix` bug.

Thanks for filing this!

---

_Label `bug` added by @BurntSushi on 2019-10-10 10:41_

---

_Comment by @maage on 2020-08-19 14:04_

Some extended testing.
Tests ran under Fedora 32 x86_64.
```
$ rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
$ pcre2grep --version
pcre2grep version 10.35 2020-05-09
$ perl --version

This is perl 5, version 30, subversion 3 (v5.30.3) built for x86_64-linux-thread-multi
(with 75 registered patches, see perl -V for more detail)
...
$ grep --version
grep (GNU grep) 3.3
...
```

Test data:
```
$ printf 'foo 42\nxoyz\ncat\tdog\nfoo' > ip.txt
$ cat ip.txt; echo "<EOF>" 
foo 42
xoyz
cat	dog
foo<EOF>
```

Just to make comparison complete:
```
$ pcre2grep -o '.*o(?!.*\s)' ip.txt 
xo
cat	do
foo
$ perl -ne 'chomp;print "$&\n" if /.*o(?!.*\s)/' ip.txt 
xo
cat	do
foo
$ grep -oP '.*o(?!.*\s)' ip.txt
xo
cat	do
foo
$ perl -ne 'print "$&\n" if /.*o(?!.*\s)/' ip.txt 
foo
```
In perl line contains \n at the end when you go over line by line. Seems pcre2grep does do that. Grep colors all matches correctly. Others do not do coloring.

With rg:
```
$ rg -NoP '.*o(?!.*\s)' ip.txt
xoyz
cat	dog
foo
```
Only "foo" is colored as match. Somehow first two lines end up being printed fully even if they are not marked as matches.

---

_Label `rollup` added by @BurntSushi on 2021-05-31 23:02_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
