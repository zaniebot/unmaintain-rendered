```yaml
number: 3784
title: "\"Note that Ruff's pre-commit hook should run before Black, isort, and other formatting tools.\" ???"
type: issue
state: closed
author: brucearctor
labels: []
assignees: []
created_at: 2023-03-28T20:27:51Z
updated_at: 2023-07-30T18:14:23Z
url: https://github.com/astral-sh/ruff/issues/3784
synced_at: 2026-01-12T15:54:44Z
```

# "Note that Ruff's pre-commit hook should run before Black, isort, and other formatting tools." ???

---

_@brucearctor_

From:  https://github.com/charliermarsh/ruff/blob/main/docs/usage.md --> "Note that Ruff's pre-commit hook should run before Black, isort, and other formatting tools."

It is not clear why this is a 'should'.  Would anyone be willing to explain?  Thanks!

I don't see a dev/user mailing list, so imagine asking questions in issues is fair game.  If this project uses another method, happy to post there, please advise in that case.  

---

_Comment by @JonathanPlasse on 2023-03-28 20:36_

This is because ruff can remove and change the code which would require a second pass out black and isort.
That is why it must be run before.

There is also GitHub discussions that is available. I see discussions as non actionable questions.

---

_Comment by @brucearctor on 2023-03-28 20:41_

awesome -- marking to be closed.  🤦 ... not used to discussions being enabled in repos ... that's a much more appropriate place!  

---

_Closed by @brucearctor on 2023-03-28 20:41_

---

_Comment by @charliermarsh on 2023-03-28 20:50_

Yeah, if you don't run autofix in pre-commit, then it makes sense to run Ruff _last_. If you _do_ run autofix, you should run it first, otherwise Ruff might output code that Black then needs to reformat.

---

_Comment by @brucearctor on 2023-03-28 21:05_

That was my thinking, and why I was hung up on the directive of 'should'.  Still gaining confidence with ruff, so not allowing fix in precommit.  

---

_Comment by @failable on 2023-07-30 14:06_

If `ruff --fix` is run before `black`, `options = {"debug": True,}` will be 'fixed' to `options = {"debug": True}` before `black` formatting. But the comma is inserted here to expect `black` to format it into 

```
{
    "debug": True,
}
```

I encounter many scenarios like this: running `ruff` before `black` would results in quite different output from without `ruff`.

---

_Comment by @charliermarsh on 2023-07-30 15:51_

Do you have the trailing comma rules enabled (COM)? Ruff should not touch that trailing comma unless you’ve opted into those rules, which aren’t enabled by default.

---

_Comment by @brucearctor on 2023-07-30 18:14_

There are some interesting behaviors when black modifies line [ex line
length ], which in turn changes the ruff noqa [if adding those].  Also, if
memory serves, #noqa counts as part of line length.

On Sun, Jul 30, 2023, 8:51 AM Charlie Marsh ***@***.***>
wrote:

> Do you have the trailing comma rules enabled (COM)? Ruff should not touch
> that trailing comma unless you’ve opted into those rules, which aren’t
> enabled by default.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/3784#issuecomment-1657206211>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ABGMTJGG4PG3ATAEGPBGM73XSZ7HLANCNFSM6AAAAAAWLB2QDA>
> .
> You are receiving this because you modified the open/close state.Message
> ID: ***@***.***>
>


---
