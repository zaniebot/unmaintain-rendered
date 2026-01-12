```yaml
number: 1424
title: "Feature suggestion: Search for UTF-16 encoded version of ascii text"
type: issue
state: closed
author: nico
labels:
  - wontfix
assignees: []
created_at: 2019-11-10T07:50:19Z
updated_at: 2019-11-10T13:58:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1424
synced_at: 2026-01-12T16:13:23Z
```

# Feature suggestion: Search for UTF-16 encoded version of ascii text

---

_@nico_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)

#### How did you install ripgrep?

Binary off github.

#### What operating system are you using ripgrep on?

Windows 10.

#### Describe your question, feature request, or bug.

I've been trying to find where some program stores some setting. To do this, I set the setting to "someuniquestring" and then ran `rg someuniquestring likelydirectory`.

The search came back empty -- but it was because the string was stored UTF-16 encoded in some binary file. I didn't think of that for a few minutes, even though this is common on Windows.

It'd be nice if ripgrep had a way to search for the UTF-16 encoding of an ascii string, either behind a flag, or maybe even by default (maybe only when running on Windows because UTF-16 is so common here).

If this sounds like something you'd accept, I could try to write a patch.

---

_Comment by @okdana on 2019-11-10 08:10_

I guess you can't use `-E` here because the file is not actually UTF-16, just the sub-string is?

If that's so, i think the work-around on UNIX systems would be something like this:

```
printf %s someuniquestring | iconv -t utf-16be | rg -af-
```

(You wouldn't be able to do `rg -a "$( iconv ... )"` because you can't pass `NUL` bytes in argument lists on UNIX)

Not sure if `iconv` is available for Windows, but maybe something to consider for now

---

_Comment by @BurntSushi on 2019-11-10 12:28_

> It'd be nice if ripgrep had a way to search for the UTF-16 encoding of an ascii string, either behind a flag, or maybe even by default (maybe only when running on Windows because UTF-16 is so common here).

ripgrep _should_ already handle the common case already. If ripgrep sees a UTF-16 BOM at the beginning of a file, then it will automatically transcode the contents of the file to UTF-8, which should allow your search to succeed.

If your case is a "mostly ASCII/UTF-8/binary file, with some parts in UTF-16" then I'd say that's pretty uncommon. In that case, I imagine you could still try `rg foo -E utf-16`. Otherwise, I think @okdana's example is probably the right path to go.

In general, I don't think ripgrep can solve the issue of mixed encodings for you automatically.

---

_Closed by @BurntSushi on 2019-11-10 12:28_

---

_Label `wontfix` added by @BurntSushi on 2019-11-10 12:29_

---

_Comment by @nico on 2019-11-10 12:51_

The data is somewhere in a binary file, not in a text file with a BOM. This is pretty common on Windows (for example, Compound File Binary Format [1], PDB files, .res files, etc). These files won't decode as UTF-16, so I don't think `-E` helps.

Sure, it's possible to do this. Windows has neither printf nor iconv preinstalled, and one has to think to do it, but just manually putting 0 bytes in the search string isn't that hard to do manually either. But it's also easy to implement.

If Windows isn't a priority, that's certainly understandable.

1:https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-cfb/53989ce4-7b05-4f8d-829b-d08d6148375b?redirectedfrom=MSDN

---

_Comment by @BurntSushi on 2019-11-10 13:58_

> These files won't decode as UTF-16, so I don't think -E helps.

Have you tried it?

> If Windows isn't a priority, that's certainly understandable.

There are several aspects of ripgrep that make it clear that Windows _is_ a priority. Otherwise, I wouldn't have spent the considerable effort required to get colors working correctly, supporting UTF-16 automatically and making sure everything single PR passes Windows CI builds before coming in.

> But it's also easy to implement.

It's only easy to implement when you're searching for a simple fixed string literal. In the general case---for an arbitrary regex---it is not easy to implement, and this is one of the reasons why this feature doesn't exist. Moreover, it wouldn't be turned on by default automatically in _any_ situation, since the only thing it would be good for is a mixed encoding use case. It would _also_ mean that any corresponding ASCII results would not be found, so you'd wind up trading one problem for another.

No matter which way you slice it, if you need to search mixed encoding files, then you'll need to take some kind of action to make the search behave as you want.

---
