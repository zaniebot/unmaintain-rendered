```yaml
number: 434
title: "Unrecognized escape sequence: '\\/'"
type: issue
state: closed
author: tekumara
labels: []
assignees: []
created_at: 2017-04-02T02:49:19Z
updated_at: 2018-07-25T21:53:48Z
url: https://github.com/BurntSushi/ripgrep/issues/434
synced_at: 2026-01-12T16:13:22Z
```

# Unrecognized escape sequence: '\/'

---

_@tekumara_

This works

```
$ echo 31/03 | grep '[0-9\/]'
31/03
```

but this fails

```
$ echo 31/03 | rg '[0-9\/]'
Error parsing regex near '[0-9\/]' at character offset 5: Unrecognized escape sequence: '\/'.
$ rg --verison
ripgrep 0.4.0
```

---

_Comment by @DoumanAsh on 2017-04-02 11:49_

I suspect it is because `/` does not require actual escaping as it is not special character.
But it is most likely issue of [regex](https://github.com/rust-lang/regex), rather than ripgrep

---

_Comment by @tekumara on 2017-04-02 13:04_

Ah, looks like you are right.

```
$ echo 31/03 | grep '[0-9/]'
31/03
```

```
$ echo 31/03 | rg '[0-9/]'
1:31/03
```

---

_Closed by @tekumara on 2017-04-02 13:04_

---

_Comment by @BurntSushi on 2017-04-02 17:34_

In case anyone else lands on this: the underlying regex library is indeed to blame for this. The regex library is paranoid about which escape sequences it accepts so that additional escape sequences can be added in the future in a backwards compatible way.

With that said, permitting escape sequences like `\/` or similar seems harmless, although it seems better to write regexes without escapes if possible.

---

_Comment by @blindside85 on 2018-07-25 21:44_

I'm mixed on this thread. On one hand I enjoy not needing to escape things while doing regex with this beloved tool... but on the other, it'd make doing silly things like this...

```rg 'unescaped regex' --files-with-matches | tr '\n' '\0' | xargs -0 sed -i -E 's/escaped regex find/escaped regex replace/g'```

...able to use the same regex in both `rg` and `sed` without modifications, so that'd be the only real 'pros' point I can come up with. Either way, your creation has saved hours of my life, and I'm still finding new uses for it weekly, so *thank you* and please keep it going!

---

_Comment by @BurntSushi on 2018-07-25 21:53_

@blindside85 You may find the `-F/--fixed-strings` flag useful.

---
