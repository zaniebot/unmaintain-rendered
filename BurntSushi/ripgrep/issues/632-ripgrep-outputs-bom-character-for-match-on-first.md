```yaml
number: 632
title: Ripgrep outputs BOM character for match on first line of a file with a BOM
type: issue
state: closed
author: roblourens
labels: []
assignees: []
created_at: 2017-10-10T05:54:17Z
updated_at: 2017-10-21T01:26:56Z
url: https://github.com/BurntSushi/ripgrep/issues/632
synced_at: 2026-01-12T16:13:22Z
```

# Ripgrep outputs BOM character for match on first line of a file with a BOM

---

_@roblourens_

From https://github.com/Microsoft/vscode/issues/35633

As the title says, I noticed that ripgrep will output the BOM character when printing a match in the first line. Not really blocking anything, but I think it would make sense for ripgrep to strip this.

---

_Comment by @BurntSushi on 2017-10-10 10:51_

@roblourens That is interesting. I think my intuition was the opposite, but I could be completely wrong? For example, if you run `rg pattern file > results` and `file` has a BOM, shouldn't `results` also have a BOM?

---

_Comment by @FSMaxB on 2017-10-10 12:38_

But shouldn't the output encoding depend on the locale?

---

_Comment by @BurntSushi on 2017-10-10 12:41_

@FSMaxB ripgrep doesn't respect any locale settings and always uses UTF-8 for output. That seems orthogonal to the issue reported here.

---

_Comment by @FSMaxB on 2017-10-10 12:42_

Ok, sorry for the noise.

---

_Comment by @roblourens on 2017-10-10 15:06_

It seems to me like the BOM is metadata and not really part of the actual text of the file. But I'm not sure and haven't checked what other tools do.

---

_Comment by @okdana on 2017-10-10 15:57_

`rg`'s behaviour seems consistent with BSD `grep`, GNU `grep`, and `ag`:

```
% printf '\xef\xbb\xbftest\n' > bom.txt
% file bom.txt
bom.txt: UTF-8 Unicode (with BOM) text
% /usr/bin/grep . bom.txt | xxd -a
00000000: efbb bf74 6573 740a    ...test.
% /usr/local/bin/grep . bom.txt | xxd -a
00000000: efbb bf74 6573 740a    ...test.
% /usr/local/bin/ag --no-numbers . bom.txt | xxd -a
00000000: efbb bf74 6573 740a    ...test.
% /usr/local/bin/rg --no-line-number . bom.txt | xxd -a
00000000: efbb bf74 6573 740a    ...test.
```

I can see why someone might expect it to be stripped out tho

---

_Comment by @BurntSushi on 2017-10-21 01:26_

I don't quite know what the right answer is here. Given that both BSD and GNU grep leave the BOM in tact, I'm also inclined to take that path as well. In particular, in the absence of more concrete use cases where removing the BOM makes sense, I'd like to side with tradition on this one.

I'm going to close this for now, but if there is a more compelling argument to be made, please make it here and we can revisit this.

---

_Closed by @BurntSushi on 2017-10-21 01:26_

---
