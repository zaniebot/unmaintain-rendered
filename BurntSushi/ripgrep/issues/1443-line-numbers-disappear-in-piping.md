```yaml
number: 1443
title: Line numbers disappear in piping
type: issue
state: closed
author: peterbe
labels:
  - wontfix
assignees: []
created_at: 2019-12-06T21:22:28Z
updated_at: 2019-12-06T21:47:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1443
synced_at: 2026-01-12T16:13:23Z
```

# Line numbers disappear in piping

---

_@peterbe_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install ripgrep` I think. 

#### What operating system are you using ripgrep on?

macOS

#### Describe your question, feature request, or bug.

```
▶ rg quoteattr
kuma/wiki/content.py
4:from xml.sax.saxutils import quoteattr
152:                sample = pq(src).find('[id=%s]' % quoteattr(name))
```

In this particular example, there's just 2 results. But suppose it's 100 and I realize, I need to pipe it to some more filtering. E.g. 

```
▶ rg quoteattr | rg sample
kuma/wiki/content.py:                sample = pq(src).find('[id=%s]' % quoteattr(name))
```
Now, I list the line number. I wanted that. I need that. 

You *can't* do this:
```
▶ rg quoteattr | rg -n sample
2:kuma/wiki/content.py:                sample = pq(src).find('[id=%s]' % quoteattr(name))
```
I have no idea what that `2` is. 

The reason **this is a feature request** is because there *is* a work-around:

```
▶ rg -n quoteattr | rg sample
kuma/wiki/content.py:152:                sample = pq(src).find('[id=%s]' % quoteattr(name))
```

I think that's counter-intuitive. 

> What do you think ripgrep should have done?

I don't know if it's possible but ideally, I'd like that appending ` | rg morefiltering` should be the equivalent of doing an `AND` operation which I don't know how to do. 


---

_Comment by @BurntSushi on 2019-12-06 21:47_

This is intended and correct behavior. When ripgrep detects that it isn't writing to a tty, then it not only removes line numbers but also reverts tk the traditional grep format instead of the ack format. If you want line numbers, then use the `-n` flag.

> I have no idea what that 2 is.

It's the line number of the output from the first rg process.

> The reason this is a feature request is because there is a work-around:

It's not a work around. It's exactly the right solution. You want line numbers from the first process.

> I don't know if it's possible but ideally,

It's not. The first rg process doesn't know that its output is being consumed by another rg process.

---

_Closed by @BurntSushi on 2019-12-06 21:47_

---

_Label `wontfix` added by @BurntSushi on 2019-12-06 21:47_

---
