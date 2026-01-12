```yaml
number: 267
title: Weird non-matching error with ignore-case and certain strings containing non-ascii characters?
type: issue
state: closed
author: crumblingstatue
labels:
  - duplicate
assignees: []
created_at: 2016-12-04T23:19:12Z
updated_at: 2016-12-07T16:42:28Z
url: https://github.com/BurntSushi/ripgrep/issues/267
synced_at: 2026-01-12T16:13:21Z
```

# Weird non-matching error with ignore-case and certain strings containing non-ascii characters?

---

_@crumblingstatue_

I really don't know what to make of this.

I tried to search my project for "magnézium", ignoring case, but I didn't get any matches, even though I know the project has several instances of it.
So I did some test cases:

```
echo 'Néz' | rg -i néz
1:Néz
```
Good

```
echo 'Agnéz' | rg -i agnéz
1:Agnéz
```
Still good

```
echo 'Magnéz' | rg -i magnéz
```
What? This suddenly doesn't match, even though I only added a `M` letter?

```
echo 'Magnez' | rg -i magnez
1:Magnez
```
But wait... If I replace the non-ascii `é` with `e`, it matches again.

Using `ripgrep 0.3.1`. The non-matching case works correctly using `grep` or `git grep`.

---

_Comment by @BurntSushi on 2016-12-05 02:23_

I suspect this is a dupe of #251, which is fixed on master. Namely, this works:

```
$ echo 'Magnéz' | rg -i magnéz
1:Magnéz
```

I meant to get a release out this weekend but got tied up with other things. I think CI is giving me trouble too. I'll try harder to get one out soon.

---

_Label `duplicate` added by @BurntSushi on 2016-12-05 02:23_

---

_Comment by @ngirard on 2016-12-07 13:23_

I'm suspecting another dupe of #251 here. Tested under ripgrep 0.3.1:

`echo "touré kunda" | rg 'touré.kunda'`

returns nothing.
Note that ignore-case is not enabled in this case.

If it is already fixed in master, a new release would be highly appreciated.
Cheers, and many thanks for your great work!

---

_Comment by @BurntSushi on 2016-12-07 14:50_

> If it is already fixed in master, a new release would be highly appreciated.

I'm trying, but one of the tools I use to build releases is broken. People are working on fixing it. (I am specifically using the nightly version of said tool, so I get what I deserve, but nightlies are required to build executables with explicit support for SIMD.)

---

_Comment by @ngirard on 2016-12-07 16:14_

Alright, then, I'll keep waiting :-)

---

_Comment by @BurntSushi on 2016-12-07 16:27_

Awesome. Looks like whatever was broken has been fixed. A new release is popping out now! https://github.com/BurntSushi/ripgrep/releases --- Still waiting on Mac/Windows.

---

_Closed by @BurntSushi on 2016-12-07 16:27_

---

_Comment by @ngirard on 2016-12-07 16:32_

Great !

---

_Comment by @crumblingstatue on 2016-12-07 16:42_

Alright, tested 0.3.2, and it indeed returns the same number of matches for "magnézium" as git-grep, so I'm going to trust this issue is fixed.

---
