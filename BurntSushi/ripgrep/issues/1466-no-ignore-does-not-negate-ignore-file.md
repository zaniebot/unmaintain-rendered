```yaml
number: 1466
title: "--no-ignore does not negate --ignore-file"
type: issue
state: closed
author: zachriggle
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2020-01-22T02:37:04Z
updated_at: 2020-03-15T17:19:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1466
synced_at: 2026-01-12T16:13:23Z
```

# --no-ignore does not negate --ignore-file

---

_@zachriggle_

When using `ripgrep`, the `--no-ignore` flag does not negate a previous `--ignore-file` argument.  There does not seem to be a way to "ignore" a manually-added `--ignore-file`.

```
$ cat .gitignore
*

$ cat rg-ignore-file
*Test*

$ rg --no-ignore Foo
FileA.c
1:Foo

FileTestA.c
1:Foo

$ rg --ignore-file=rg-ignore-file --no-ignore Foo
FileA.c
1:Foo
```

---

_Comment by @BurntSushi on 2020-01-22 02:40_

`--no-ignore` isn't documented to ignore `--ignore-file` flags. I'm not sure that's desirable behavior, since ignore files are explicitly provided.

Could you please elaborate on your use case for why you want this?

---

_Comment by @zachriggle on 2020-01-22 09:12_

The general use case is the same as having `--ignore-case` override a preceding `--case-sensitive`.  I do a lot of source code spelunking in my work, and have an ever-expanding ignore file for my ripgrep wrapper -- which generally ignores test cases and other undesirable files.

Occasionally, I want to search everything again, and have all ignore-files be... ignored.  I can't share my wrappers for IP / employer reasons (and can probably work around this in shell wrappers) but it seems that `--no-ignore` should take precedence over preceding operators and general VCS ignores files.

---

_Label `enhancement` added by @BurntSushi on 2020-01-22 10:26_

---

_Comment by @okdana on 2020-03-06 20:32_

I ran into this again today as well.

I use `--ignore-file` in my `rg` alias to ensure that whenever i run `rg` it respects my global ignore patterns, per #45. Occasionally i need to override that, and i've just been either running as `\rg` to bypass the alias (obv losing all of my other settings in the process) or adding `-g\*` to white-list everything. The latter is easy enough and doesn't seem to have any side-effects in the general case, but there's an intuition/reflex issue i suppose; it's never the first thing that comes to mind.

I'm not sure about the UX implications of making `--no-ignore` itself override `--ignore-file` though. It does seem conceivable that users would want an easy way to bypass only implicit ignore files (as the option does now), or even the other way around. Also, intuitively, if you made `--no-ignore` work that way, you'd probably also want to make `-u` work that way, which is another thing that could have serious implications for existing aliases and config files.

I'm not sure what the best thing would be. Maybe, to start with, a `--no-ignore-file` (or whatever) that cancels the effect of any previous `--ignore-file`s on the command line? That shouldn't impact anything else. (I'm not too upset if i just have to keep using `-g\*` tho)

---

_Comment by @zachriggle on 2020-03-09 00:24_

Yes, my original intent is that it should override preceding flags, not all
flags.

On Fri, Mar 6, 2020 at 2:32 PM dana <notifications@github.com> wrote:

> I ran into this again today as well.
>
> I use --ignore-file in my rg alias to ensure that whenever i run rg it
> respects my global ignore patterns, per #45
> <https://github.com/BurntSushi/ripgrep/issues/45>. Occasionally i need to
> override that, and i've just been either running as \rg to bypass the
> alias (obv losing all of my other settings in the process) or adding -g\*
> to white-list everything. The latter is easy enough and doesn't seem to
> have any side-effects in the general case, but there's an intuition/reflex
> issue i suppose; it's never the first thing that comes to mind.
>
> I'm not sure about the UX implications of making --no-ignore itself
> override --ignore-file though. It does seem conceivable that users would
> want an easy way to bypass only implicit ignore files (as the option does
> now), or even the other way around. Also, intuitively, if you made
> --no-ignore work that way, you'd probably also want to make -u work that
> way, which is another thing that could have serious implications for
> existing aliases and config files.
>
> I'm not sure what the best thing would be. Maybe, to start with, a
> --no-ignore-file (or whatever) that cancels the effect of any previous
> --ignore-files on the command line? That shouldn't impact anything else.
> (I'm not too upset if i just have to keep using -g\* tho)
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1466?email_source=notifications&email_token=AAA3IGFQTM47O4VSDJQRMFLRGFMXRA5CNFSM4KJ6XMUKYY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGOEOCYPRI#issuecomment-595953605>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAA3IGH6DEV6E4GJU557T3TRGFMXRANCNFSM4KJ6XMUA>
> .
>
-- 

*Zach Riggle*


---

_Comment by @BurntSushi on 2020-03-15 15:14_

OK, so this is pretty tricky.

I initially started by adding a new `--no-ignore-files` flag that basically does what you want here: it causes ripgrep to ignore all `--ignore-file` flags (before or after `--no-ignore-files`). I _also_ made it so the `-u/--no-ignore` flag implied `--no-ignore-files`. To me that "makes sense" in that it's consistent with `-u` implying every other `--no-ignore-*` flag.

The main problem I have with doing that is that it's a breaking change, and it's kind of a severe one. If I were starting over, then yeah, I'd probably make `-u` imply `--no-ignore-files`.

I'm generally okay with making breaking changes, but there really needs to be an easy way to capture the old workflow. i.e., It should be possible to do, say, `rg -u --ignore-files <blah blah>` in order to cause ripgrep to stop respecting all ignore rules _except_ for `--ignore-file` flags, which is the status quo today. But the problem with doing that is that `rg -u --ignore-files -u <blah blah>` doesn't work like you'd expect it to: the second `-u` will _not_ override `--ignore-files`. This is partially because ripgrep's argv parser doesn't support doing this in a simple way, partially because the other `--ignore-*` flags don't work like this and partially because it just makes ripgrep's flags overall more complicated.

So... it's not a great solution, but it's something. I might suggest `alias rgu="rg -u --no-ignore-files"` to suppress all ignore related logic.

I am open to doing a breaking change in the future, but I'd like to see a simpler way of re-capturing existing behavior. Perhaps that needs to wait until our argv handling can become more robust with respect to flag overrides. (This particular issue about overrides comes up quite a bit. See #809 for example, which actually motivated [clap to add new APIs for that purpose](https://docs.rs/clap/2.33.0/clap/struct.ArgMatches.html#method.indices_of). But I just haven't gotten around to building an abstraction that's easy to understand that uses them yet.)

---

_Label `rollup` added by @BurntSushi on 2020-03-15 15:15_

---

_Closed by @BurntSushi on 2020-03-15 17:19_

---
