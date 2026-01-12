```yaml
number: 1344
title: "--hidden doesn't work with --type all"
type: issue
state: closed
author: kurnevsky
labels:
  - doc
assignees: []
created_at: 2019-08-07T07:51:12Z
updated_at: 2020-02-17T22:16:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1344
synced_at: 2026-01-12T16:13:23Z
```

# --hidden doesn't work with --type all

---

_@kurnevsky_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Linux package.

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

`--hidden` doesn't work with `--type all`

#### If this is a bug, what are the steps to reproduce the behavior?

Search something contained in project's hidden files (starting with dot) with both `--hidden` and `--type all` flags.

#### If this is a bug, what is the actual behavior?

It won't be found.

#### If this is a bug, what is the expected behavior?

It should be found.


---

_Comment by @BurntSushi on 2019-08-07 12:36_

Please provide a reproduction. As the issue template requests:

> If possible, please include both your search patterns and the corpus on which
you are searching.

---

_Label `question` added by @BurntSushi on 2019-08-07 14:23_

---

_Comment by @kurnevsky on 2019-08-07 19:28_

```sh
mkdir /tmp/1
cd /tmp/1
echo qwerty > .env
rg --hidden qwerty
rg --hidden --type all qwerty
```

---

_Comment by @kurnevsky on 2019-08-07 19:31_

Hm... Just realized that it's filtered by extension `.env`. So even file `name.env` won't be shown. Is it expected to happen?

---

_Comment by @kurnevsky on 2019-08-07 19:38_

It seems `--type all` means all supported extensions - couldn't find it in the docs so was confused about it. Sorry for bothering.

---

_Closed by @kurnevsky on 2019-08-07 19:38_

---

_Label `question` removed by @BurntSushi on 2019-08-07 19:39_

---

_Label `doc` added by @BurntSushi on 2019-08-07 19:39_

---

_Comment by @BurntSushi on 2019-08-07 19:39_

Indeed, the special `all` type does not appear in the man page. That should be fixed, so I'm re-opening this. Thanks!

---

_Reopened by @BurntSushi on 2019-08-07 19:39_

---

_Comment by @telotortium on 2020-01-27 18:33_

Hi, could you add this to the documentation? I was confused about `--type all` wasn't searching all of my non-ignored files and couldn't find anything about it until now.

---

_Comment by @BurntSushi on 2020-01-27 18:37_

@telotortium Yes. This ticket is marked as `doc` bug.

---

_Comment by @BurntSushi on 2020-01-27 18:38_

@telotortium It would be helpful if you could write out the blurb in the docs that would have helped you! Thanks.

---

_Comment by @telotortium on 2020-01-27 18:54_

Hi @BurntSushi I gave it a shot in  PR #1472 - thanks for getting back to me!

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
