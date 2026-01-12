```yaml
number: 124
title: "Feature request: add a command-line option to force case-sensitive search"
type: issue
state: closed
author: jeberger
labels: []
assignees: []
created_at: 2016-09-28T07:30:22Z
updated_at: 2020-02-13T17:30:09Z
url: https://github.com/BurntSushi/ripgrep/issues/124
synced_at: 2026-01-12T16:13:21Z
```

# Feature request: add a command-line option to force case-sensitive search

---

_@jeberger_

Rationale: I would prefer smart case to be the default. I can get that by creating an alias like `alias rg="rg -S"` but then I need some way to force case-sensitive search when I need it e.g. `rg --case-sensitive`, which given the alias would become `rg -S --case-sensitive`, the last given flag would then override previous flags.


---

_Comment by @BurntSushi on 2016-09-28 09:30_

Should it also override the `-i` flag?

On Sep 28, 2016 3:30 AM, "jeberger" notifications@github.com wrote:

> Rationale: I would prefer smart case to be the default. I can get that by
> creating an alias like alias rg="rg -S" but then I need some way to force
> case-sensitive search when I need it e.g. rg --case-sensitive, which
> given the alias would become rg -S --case-sensitive, the last given flag
> would then override previous flags.
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/124, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34golU18VZzSkNELICyf0zP2fxOJgks5quheOgaJpZM4KIf8v
> .


---

_Comment by @kaushalmodi on 2016-09-28 13:39_

> Should it also override the `-i` flag?

Is it possible to have the last flag win? The idea is to allow a user to override whatever is in the alias.
- `rg -S --case-sensitive` ≈ `rg --case-sensitive`
- `rg --case-sensitive -S` ≈ `rg -S`
- `rg -S -i` ≈ `rg -i`
- `rg -i --case-sensitive` ≈ `rg --case-sensitive`
- and so on.. (you get the idea :))


---

_Comment by @BurntSushi on 2016-09-28 13:41_

Sadly, no, although I agree that might be more intuitive. We'd probably have to switch arg parsing libraries. Docopt doesn't expose that kind of information.


---

_Comment by @lespea on 2016-09-28 15:11_

Just a thought... most of the time this won't happen so if you detect both flags as being set you could just loop through argv and manually take the last one?  Not very clean but it would work without requiring a new library.


---

_Comment by @BurntSushi on 2016-09-28 15:30_

Yeah, I don't really want to add that implementation complexity. We should either do it right (switch to a different arg parsing library, which I don't really want to do) or just specify a precedence.

Let's punt on "last flag wins for now" and decide on precedence. I feel like the precedence should be:
- `--case-sensitive`
- `--case-insensitive`
- `--smart-case`

where flags earlier in the list override flags later in the list.


---

_Comment by @kaushalmodi on 2016-09-28 16:10_

That precedence will work for my use case too where I have `--smart-case` in the alias.

On a different note. Would you also please add `-s` as an alias to `--case-sensitive`?

**Update**:

_because I like tables :)_

```
--case-sensitive > --case-insensitive (-i) > --smart-case (-S)

|---------------------+--------------------+---------------------|
| Alias               | Extra arg to alias | Effective           |
|---------------------+--------------------+---------------------|
| rg -S               | --case-sensitive   | rg --case-sensitive |
| rg -S               | -i                 | rg -i               |
| rg -i               | --case-sensitive   | rg --case-senstive  |
| rg -i               | -S                 | rg -i               |
| rg --case-sensitive | -i                 | rg --case-sensitive |
| rg --case-sensitive | -S                 | rg --case-sensitive |
|---------------------+--------------------+---------------------|
```


---

_Comment by @jeberger on 2016-09-28 16:20_

Since we can't have "last flag wins", that precedence would be fine. And +1 for the short alias for `--case-sensitive`.


---

_Closed by @BurntSushi on 2016-09-28 20:32_

---

_Comment by @BurntSushi on 2016-09-28 20:32_

This is done and will be in the next release. I implemented the precedence rules discussed here and made `-s` alias `--case-sensitive`.


---

_Comment by @gomesalexandre on 2020-02-13 17:30_

Thank you! Made case-insensitive my default `:Rg` binding, this is really helpful when having to case-sensitive search ❤️  

---
