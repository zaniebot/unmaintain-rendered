```yaml
number: 302
title: Broken .toml file
type: issue
state: closed
author: mcepl
labels: []
assignees: []
created_at: 2017-01-03T21:35:51Z
updated_at: 2017-01-03T23:02:58Z
url: https://github.com/BurntSushi/ripgrep/issues/302
synced_at: 2026-01-12T16:13:21Z
```

# Broken .toml file

---

_@mcepl_

```
matej@xerus:~/Build/ripgrep$ cargo build --release
failed to parse manifest at `/home/matej/Build/ripgrep/Cargo.toml`

Caused by:
  could not parse input as TOML
Cargo.toml:42:9 expected a key but found an empty string
Cargo.toml:42:9-42:10 expected `.`, but found `'`

matej@xerus:~/Build/ripgrep$ 
```

---

_Comment by @BurntSushi on 2017-01-03 21:38_

What version of rust/cargo do you have installed?

On Jan 3, 2017 4:35 PM, "Matěj Cepl" <notifications@github.com> wrote:

> matej@xerus:~/Build/ripgrep$ cargo build --release
> failed to parse manifest at `/home/matej/Build/ripgrep/Cargo.toml`
>
> Caused by:
>   could not parse input as TOML
> Cargo.toml:42:9 expected a key but found an empty string
> Cargo.toml:42:9-42:10 expected `.`, but found `'`
>
> matej@xerus:~/Build/ripgrep$
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/302>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34n8MMcW3NOVrPAUNijugzG1NWg-eks5rOr83gaJpZM4LaDeO>
> .
>


---

_Comment by @mcepl on 2017-01-03 21:42_

What's in Ubuntu 16.04, rustc 1.7.0 and cargo 0.8.0.

---

_Comment by @mcepl on 2017-01-03 21:49_

OK, so rust is still too unstable for normal use. I'll get the staticly build file.

---

_Closed by @mcepl on 2017-01-03 21:49_

---

_Comment by @BurntSushi on 2017-01-03 22:01_

That's wrong. Rust is quite stable. ripgrep simply requires a newer version of Rust than what you have installed.

---

_Comment by @mcepl on 2017-01-03 22:53_

Sorry, I didn't mean to be rude. However, then probably it would be good to reconsider the required compatibility. I am not personally invested in Ubuntu (being the Red Hat employee, I use Fedora/RHEL for all my needs, and we have the latest rust even for RHEL-7/EPEL), but 16.04 is LTS, so I guess it could be wise to have it supported.

---

_Reopened by @mcepl on 2017-01-03 22:53_

---

_Comment by @BurntSushi on 2017-01-03 23:02_

No, it's not happening, sorry. I can't afford to do that kind of maintenance. We need to move forward.

---

_Closed by @BurntSushi on 2017-01-03 23:02_

---
