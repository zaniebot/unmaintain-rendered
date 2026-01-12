```yaml
number: 1126
title: Is there a way to provide patterns for the path being searched?
type: issue
state: closed
author: jaepetto
labels: []
assignees: []
created_at: 2018-11-28T09:51:30Z
updated_at: 2018-11-28T12:01:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1126
synced_at: 2026-01-12T16:13:23Z
```

# Is there a way to provide patterns for the path being searched?

---

_@jaepetto_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)

#### How did you install ripgrep?

debian package

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

Is there a way to provide patterns for the path being searched?

Typically, I am looking for HTML files that contain a specific pattern:
`sudo rg --no-ignore 'id="login" class="login-form"' /src`

The trick is that the `/src` folder contains code for multiple sites. Therefore, I'd like to use patterns in the path to search only in the folders I am interested in. It would be something around the lines of:
`/src/*client_a*` (using globbing) or `/src/.*client_a.*` (using regex).



---

_Comment by @BurntSushi on 2018-11-28 10:15_

Please consider reading the documentation before filling issue requests. The guide specifically describes how to do this type of filtering: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs --- Although, you might just consider using standard shell globbing. For example, `rg foo src/*client_a*`.

---

_Closed by @BurntSushi on 2018-11-28 10:15_

---

_Comment by @jaepetto on 2018-11-28 10:27_

Sorry about that. I didn’t find the correct part of the documentation / man.
I opened the issue request because the shell globbing was failing:

```
$ sudo rg "I am" /src/*unibas*
/src/*unibas*: No such file or directory (os error 2)
```




---

_Comment by @BurntSushi on 2018-11-28 10:38_

That's not a problem with ripgrep though. The error is telling you the problem. That glob doesn't expand to any known path. Change your glob to a path that exists. For example, what is the output of `ls /src/*unibas*`?

---

_Comment by @jaepetto on 2018-11-28 10:50_

I don't want to waste your time.

The `ls` command is indeed failing:
```
ls: cannot access '/src/*unibas*': No such file or directory
```

This is related to the fact that any `*unibas*` folder is not directly under `src`.

The aim is to look into all folders starting with `src` where the full path contains `unibas`.

---

_Comment by @BurntSushi on 2018-11-28 12:00_

Yeah, this issue tracker is really more geared to ripgrep specific problems. If you need help figuring out how to use your shell's globbing facilities, you might have better luck with tutorials like this one: https://linuxhint.com/bash_globbing_tutorial/

As to your specific issue, if I were to take a stab in the dark, I'd say that if you have a `/home/jaepetto/foo/src` directory and you're searching from within `/home/jaepetto/foo`, then using `/src` doesn't make sense because `/src` is an absolute path. You instead probably want `ls ./src/*unibas*` where `./src` is a path relative to your current working directory.

> The aim is to look into all folders starting with src where the full path contains unibas.

Then use `rg foo src/**/*unibas*` or possibly `rg foo -g '*unibas*'`.

---
