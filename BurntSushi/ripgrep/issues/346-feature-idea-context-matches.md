```yaml
number: 346
title: "Feature idea: \"context matches\""
type: issue
state: closed
author: quodlibetor
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2017-02-02T16:58:04Z
updated_at: 2023-02-20T04:38:14Z
url: https://github.com/BurntSushi/ripgrep/issues/346
synced_at: 2026-01-12T16:13:21Z
```

# Feature idea: "context matches"

---

_@quodlibetor_

I "often" find myself doing something like:

```
$ rg -A5 something | grep -B5 something-else
```

it would be kind of awesome if `rg` took a `--context-matches` regex (that requires one of the other context specifiers (`-A -B -C`)) that does this automatically for me.

```
$ rg -A5 -B2 --context-matches 'some.regex' 'main.search'
...
```

```
$ rg --context-matches 'some.regex' 'main.search'
error: --context-matches requires one of --before, --after, or --context
```

---

_Comment by @BurntSushi on 2017-02-02 17:16_

I'm not against this on principle, but it would be a very difficult feature to add in the current implementation.

---

_Label `enhancement` added by @BurntSushi on 2017-02-02 17:16_

---

_Label `libripgrep` added by @BurntSushi on 2017-02-02 17:16_

---

_Comment by @quodlibetor on 2017-02-02 19:19_

Ah, that makes sense. I was hoping that it would be as simple as just caching the output for each chunk for a bit and running a regex over that cached chunk.

---

_Comment by @quodlibetor on 2017-02-02 19:20_

Which is obviously less efficient than some kind of fancy one-pass analysis.

---

_Comment by @timotheecour on 2017-02-17 18:26_

related to: https://github.com/BurntSushi/ripgrep/issues/360

---

_Comment by @BurntSushi on 2017-02-18 17:52_

> I was hoping that it would be as simple as just caching the output for each chunk for a bit and running a regex over that cached chunk.

The current implementation already does that. It *has* to, because it doesn't know which lines to print until a match occurs.

You're welcome to go look at the code and marvel at just how hard an addition like this would be. The context handling is complicated, which is why I've tagged this with `libripgrep`. Namely, we should think about this use case while refactoring the search code into a reusable library.

---

_Comment by @Hywan on 2017-03-03 08:29_

Hello,

It might sound stupid, but what would be the difference between:

```
$ rg --context-matches 'foo' 'bar'
```

and:

```
$ rg 'foo.*bar'
```

---

_Comment by @BurntSushi on 2017-03-03 10:33_

ripgrep doesn't do multiline search.

On Mar 3, 2017 3:29 AM, "Ivan Enderlin" <notifications@github.com> wrote:

> Hello,
>
> It might sound stupid, but what would be the difference between:
>
> $ rg --context-matches 'foo' 'bar'
>
> and:
>
> $ rg 'foo.*bar'
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/346#issuecomment-283895669>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34laVxe-2mr2JkmfV74h-QJmsWW2iks5rh893gaJpZM4L1XYf>
> .
>


---

_Comment by @BurntSushi on 2017-05-08 22:36_

I'm going to close this not only because the implementation complexity of this scares me a little, but it opens the door to a whole new world of feature bloat that I'd rather avoid. I'd kindly suggest that if someone wants a search this sophisticated, they should just write some code or figure out a clever work-around using the existing context flags. :-)

---

_Closed by @BurntSushi on 2017-05-08 22:36_

---

_Comment by @jdtsmith on 2023-02-20 04:37_

A related idea that might be much easier to implement: a _context state_ match.  Rather than printing lines before or after the match, a context match would print the most recent matching line of context, if any.  Any given context match is only printed (once) when the main match hits.  So with a file like:

```
* foo
** bar
*** baz
something
*boo
something else
```

and `rg --context-match '^\*+' something` would produce:

```
*** baz
something
* boo
something else
```

---
