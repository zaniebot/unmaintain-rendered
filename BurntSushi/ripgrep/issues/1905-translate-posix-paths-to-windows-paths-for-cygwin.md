```yaml
number: 1905
title: Translate posix paths to windows paths for cygwin.
type: issue
state: closed
author: account-login
labels:
  - wontfix
assignees: []
created_at: 2021-06-20T13:47:48Z
updated_at: 2023-11-24T20:18:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1905
synced_at: 2026-01-12T16:13:24Z
```

# Translate posix paths to windows paths for cygwin.

---

_@account-login_

Hello, as https://github.com/BurntSushi/ripgrep/issues/269 and https://github.com/BurntSushi/ripgrep/issues/275 points out, the experience of using rg on cygwin is not great.

I propose adding the `--cygwin-path` flag to rg. When enabled, paths in command line will be translated to windows paths using the `cygpath` command.
For example, `rg --cygwin-path xxx /usr /cygdrive/z` will be translated to something like `rg xxx C:/cygwin/usr Z:/`.
Here is a proof of concept implementation: https://github.com/account-login/ripgrep/commit/9686e721762d9b9b94b783ade7084c8729123f74

This flag, combined with the `--path-separator=/` flag, can improve the usability on cygwin platform.


---

_Comment by @BurntSushi on 2021-06-20 19:21_

This seems like a duplicate of #269 (which you linked), and in particular, I don't think you've addressed why I marked this as `wontfix`. See my comments https://github.com/BurntSushi/ripgrep/issues/269#issuecomment-383983479 and https://github.com/BurntSushi/ripgrep/issues/269#issuecomment-439734719.

It sounds like you're asking me reconsider a particular implementation path without actually addressing the concerns I raised.

Maybe I need to change my perspective here and just add this flag that lets folks work around some problems and declare that it might result in other things not behaving as one might expect. But this seems... sloppy to be honest.

---

_Comment by @account-login on 2021-06-21 14:04_

Hi, the flag I proposed just performs a workaround on behave of the user.
To use rg on cygwin, one has to perform path translation anyway, the difference is who is performing the workaround.
So the edge cases you are concerned about (if I understand correctly) are not newly introduced, but existing problems.

Of course, workarounds like this do not fix all the problems, but afaik, there is no way for a non-cygwin app to behave like cygwin app.
So I think it is worthwhile to add some simple workarounds (like the --path-separator before) for common use cases, workarounds like this make problems less, not more.


---

_Comment by @BurntSushi on 2023-11-24 20:18_

I think this is out of scope for ripgrep.

---

_Closed by @BurntSushi on 2023-11-24 20:18_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:18_

---
