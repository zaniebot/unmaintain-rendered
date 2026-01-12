```yaml
number: 1283
title: unicode pattern match difference between grep and ripgrep?
type: issue
state: closed
author: JensTimmerman
labels:
  - question
assignees: []
created_at: 2019-05-16T15:27:00Z
updated_at: 2019-05-17T08:01:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1283
synced_at: 2026-01-12T16:13:23Z
```

# unicode pattern match difference between grep and ripgrep?

---

_@JensTimmerman_

#### What version of ripgrep are you using?

ripgrep 0.9.0
-SIMD -AVX

#### How did you install ripgrep?

dnf install ripgrep on fedora 29

#### What operating system are you using ripgrep on?
fedora 29
#### Describe your question, feature request, or bug.
I'm trying to grep trought files containing unicode characters and I find some peculiar differences between grep and ripgrep, I'm wondering if this is a bug in grep, ripgrep or a consequence of the better unicode support in ripgrep.:

```
 cat test1  | grep -i '[^a-z]uresdnd([^y-z][^/]*)?$'
 cat test1  | grep -i '[^a-z]uresdnd([^y][^/]*)?$'
uresdndɎᚶİᛘ.png
 cat test1  | grep -i '[^a-z]uresdnd([^z][^/]*)?$'
uresdndɎᚶİᛘ.png
 cat test1  | rg -i '[^a-z]uresdnd([^y-z][^/]*)?$'
uresdndɎᚶİᛘ.png
```

As you can see, on my system grep thinks `Ɏ` lies between y and z, rg disagrees.
I am wondering where this difference comes from, and if I should consider this a known limitation in grep or a feature in ripgrep?

---

_Comment by @BurntSushi on 2019-05-16 15:57_

Interesting question. I did have trouble following your example though. Here's a much simpler reproduction:

```
$ cat test
Ɏ
$ LC_ALL=en_US.UTF-8 grep '[Y-Z]' test
Ɏ
$ LC_ALL=C grep '[Y-Z]' test
$ rg '[Y-Z]' test
$
```

The `Ɏ` character is `LATIN CAPITAL LETTER Y WITH STROKE`.

It's hard to say which one is "better." I typically find GNU grep's behavior here more surprising, but the root cause is that GNU grep [implements bracketed expressions using your locale's collation semantics](https://www.gnu.org/software/grep/manual/html_node/Character-Classes-and-Bracket-Expressions.html). In particular:

> Within a bracket expression, a range expression consists of two characters separated by a hyphen. It matches any single character that sorts between the two characters, inclusive. In the default C locale, the sorting sequence is the native character order; for example, ‘[a-d]’ is equivalent to ‘[abcd]’. In other locales, the sorting sequence is not specified, and ‘[a-d]’ might be equivalent to ‘[abcd]’ or to ‘[aBbCcDd]’, or it might fail to match any character, or the set of characters that it matches might even be erratic. To obtain the traditional interpretation of bracket expressions, you can use the ‘C’ locale by setting the LC_ALL environment variable to the value ‘C’. 

As far as I can tell, this behavior is part of the [POSIX standard for regular expressions](http://pubs.opengroup.org/onlinepubs/009696899/basedefs/xbd_chap09.html):

> In the POSIX locale, a range expression represents the set of collating elements that fall between two elements in the collation sequence, inclusive.

This is interesting phrasing, since the POSIX locale and the C locale are the same. But this entire section of POSIX talks in terms of collation elements, so GNU grep's interpretation seems right with respect to POSIX.

As far as ripgrep goes, it does not and will never support the POSIX standard. The ordering of characters inside character classes for ripgrep is simply the ordering of codepoints according to the assigned codepoint number.

---

_Closed by @BurntSushi on 2019-05-16 15:57_

---

_Label `question` added by @BurntSushi on 2019-05-16 15:57_

---

_Comment by @JensTimmerman on 2019-05-17 08:00_

Thank you for this clear and extensive answer!
I agree that I was surprised by gnu grep and find ripgreps result more intuitive.

---
