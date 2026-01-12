```yaml
number: 1355
title: Outputting capture groups instead of the whole match
type: issue
state: closed
author: Bios-Marcel
labels: []
assignees: []
created_at: 2019-08-28T08:27:24Z
updated_at: 2019-08-28T11:06:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1355
synced_at: 2026-01-12T16:13:23Z
```

# Outputting capture groups instead of the whole match

---

_@Bios-Marcel_

#### What version of ripgrep are you using?

11.0.2

#### How did you install ripgrep?

scoop

#### What operating system are you using ripgrep on?

windows 10

#### Question

I was trying to generate a little code-snippet. I have the following code in 3 different files, just with a different string at `SOMETHING` each time:

```java
//More code above
public enum SOMETHINGOrder {
}
//More code below
```

My goal is now, to produce a list of only the name of the enum, for example `SOMETHINGOrder`. So when in my file-hiearchy it finds `AOrder`, `BOrder` and `COrder`, I want to retrieve all of those.

Meaning I expect an output of something like this:

```
$ user@system: rg ...
AOrder
BOrder
COrder
```

By reading the docs I couldn't really find out how to do this. The first thought that occured to me was capture groups. Maybe this is also something I shouldn't really be doing with ripgrep but multiple tools together?

Either way, I have used this now `rg "enum (.+Order)"` which will correctly capture the lines and put the relevant thing in a capture group. Now is there a way to only output the group instead of the whole line?

In case this isn't possible, you could see this issue as a feature request instead 😄 

---

_Comment by @okdana on 2019-08-28 08:44_

If you didn't need to match text outside of the capture group, you could just use `-o`

But since you need to match the `enum` part too, you can use the replace feature, like this:

```
% rg -r '$1' '.*enum (.+Order).*' <<< 'public enum SOMETHINGOrder {'
SOMETHINGOrder
```

(Though a more accurate pattern might be e.g. `.*\benum\s+(\w+Order)\b.*`)

---

_Comment by @Bios-Marcel on 2019-08-28 08:53_

Cool, thanks @okdana 

I am doing this ` rg -r '$1' '.+enum (.+Order).+implements.+' --no-line-number --no-filename` in the root of my target file hierarchy now. Works like a charme :D

---

_Comment by @Bios-Marcel on 2019-08-28 09:08_

I'll be closing this then, since what I wanted to do is possible, even if not immediatly obvious.

---

_Closed by @Bios-Marcel on 2019-08-28 09:08_

---

_Comment by @BurntSushi on 2019-08-28 11:06_

This technique is documented in the guide: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#replacements

As the guide shows, it might be even simpler to combine the replace flag with the only-matching flag.

Glad you got it resolved though! Thanks @okdana!

---
