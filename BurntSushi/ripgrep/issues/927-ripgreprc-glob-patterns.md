```yaml
number: 927
title: ".ripgreprc & glob patterns"
type: issue
state: closed
author: ahmedelgabri
labels: []
assignees: []
created_at: 2018-05-24T21:24:22Z
updated_at: 2018-05-24T23:09:22Z
url: https://github.com/BurntSushi/ripgrep/issues/927
synced_at: 2026-01-12T16:13:22Z
```

# .ripgreprc & glob patterns

---

_@ahmedelgabri_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1
-SIMD -AVX
```

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.13.4

#### Describe your question, feature request, or bug.

I'm trying to add a global `.ripgreprc` file with the following but I can't seem to make it work with `--glob`

```
--hidden
--smart-case
--follow
--glob "!.git/*"
--glob "!node_modules/*"
```

This returns

```
error: Found argument '--glob "!.git/*"' which wasn't expected, or isn't valid in this context
        Did you mean --glob?

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list

For more information try --help
```

I also tried 

```
--hidden
--smart-case
--follow
--glob 
"!.git/*"
--glob 
"!node_modules/*"
```

this returns 

```
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

So how can I do this?


---

_Comment by @okdana on 2018-05-24 21:47_

Each line in the `ripgreprc` is an individual argument, and there is no special handling of quoting or escaping or anything else. (If you're familiar with C, each line is basically an element of the `argv` array to `execve()` or similar.) The only special cases are (1) leading and trailing white space is trimmed from each line, and (2) lines with a leading `#` are ignored as comments.

So you'd want to write your glob lines like either

```
--glob=!node_modules/*
```
or
```
--glob 
!node_modules/*
```

---

_Comment by @ahmedelgabri on 2018-05-24 22:22_

I assumed I need to keep the quotes, thanks very much @okdana 

---

_Closed by @ahmedelgabri on 2018-05-24 22:22_

---

_Comment by @BurntSushi on 2018-05-24 22:29_

Thanks @okdana!

@ahmedelgabri Note that the format is document in both the [man page](https://github.com/BurntSushi/ripgrep/blob/c21b9b20cf0d4d25bd2765f39a2d19a782c4b254/doc/rg.1.txt.tpl#L73-L116) and in the [guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file).

---

_Comment by @ahmedelgabri on 2018-05-24 22:35_

@BurntSushi I did read the help but I didn't see any examples besides simple flags

> Every line is a shell argument, after trimming ASCII whitespace.

When I read this I interpreted it as `--glob "!git/*"` not as `--glob=!git/*`

---

_Comment by @BurntSushi on 2018-05-24 22:40_

@ahmedelgabri Does the `--type-add` example in the guide clarify things? If you think an explicit example with globs would help, then that sounds like a nice addition too.

---

_Comment by @ahmedelgabri on 2018-05-24 22:45_

It’s useful indeed but I honestly I only checked the `manpage` & searched for anything related to `RIPGREP_CONFIG_PATH`

But I’d add an example for glob too & have the same examples in the manpage & the guide. I can open a PR tomorrow for this if you want.

Sent from my iPhone
________________________________
From: Andrew Gallant <notifications@github.com>
Sent: Friday, May 25, 2018 12:40:40 AM
To: BurntSushi/ripgrep
Cc: Ahmed El Gabri; Mention
Subject: Re: [BurntSushi/ripgrep] .ripgreprc & glob patterns (#927)


@ahmedelgabri<https://github.com/ahmedelgabri> Does the --type-add example in the guide clarify things? If you think an explicit example with globs would help, then that sounds like a nice addition too.

—
You are receiving this because you were mentioned.
Reply to this email directly, view it on GitHub<https://github.com/BurntSushi/ripgrep/issues/927#issuecomment-391887752>, or mute the thread<https://github.com/notifications/unsubscribe-auth/AAD5hMhR9P8a2_Wfu9-aIqs1VqAEzK-hks5t1zbogaJpZM4UNBV2>.


---

_Comment by @BurntSushi on 2018-05-24 23:09_

@ahmedelgabri SGTM! Much appreciated.

---
