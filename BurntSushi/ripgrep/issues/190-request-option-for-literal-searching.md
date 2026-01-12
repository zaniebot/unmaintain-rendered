```yaml
number: 190
title: "request: option for literal searching"
type: issue
state: closed
author: lilyball
labels: []
assignees: []
created_at: 2016-10-21T18:23:27Z
updated_at: 2020-01-27T09:19:22Z
url: https://github.com/BurntSushi/ripgrep/issues/190
synced_at: 2026-01-12T16:13:21Z
```

# request: option for literal searching

---

_@lilyball_

I use `ag -Q` (or `ag --literal`) fairly often to do a literal search across files, since it's so fast and colorizes the output. I'd love to be able to use `rg` this way, except `rg` doesn't seem to have any support for literal searching.


---

_Comment by @BurntSushi on 2016-10-21 19:25_

Indeed it does. It uses the same flag as grep does:

```
    -F, --fixed-strings        Treat the pattern as a literal string instead of
                               a regular expression.
```

Note that `-F` should _never_ be used for performance reasons. Its only use (at the moment) is as a convenience to avoid the need to escape regex meta characters. Eventually, I'd like to support what grep does, which is match any one of a number of literals separated by new lines (which should emerge fairly naturally out of #7).


---

_Closed by @BurntSushi on 2016-10-21 19:25_

---

_Comment by @lilyball on 2016-10-22 06:52_

Oh geeze. When I saw -Q didn't work, I looked at the list of flags, but apparently I only looked at the "Less common options" section and ignored the top, thinking that the top was a summary and the longer descriptions beneath had all the flags. But now that I've actually read the section header it's obvious that it's not describing the most common options.


---

_Comment by @BurntSushi on 2016-10-22 10:07_

Yeah, if you have any suggestions on how to make the docs clearer, I would be happy to hear them. I don't think --help is that great today, and I mostly intended the top section to be options that i thought were either more common or somehow more important, but the categorization is something i came up with willy nilly. Maybe it should have more sections? Or one big one?


---

_Comment by @lilyball on 2016-10-24 20:57_

Honestly, I think the best solution would be to have a manpage. The problem with `rg --help` is that it's not paged, so you have to scroll up to find what you want, which leads to the problem I had of not even reading the top. But manpages are paged by default, so you start with the top, which should make the most common options much more visible.


---

_Comment by @BurntSushi on 2016-10-24 21:07_

There is a man page. :-)

In #189, it was suggested to show less on `--help`, but that has downsides too.


---

_Comment by @lilyball on 2016-10-24 22:23_

There is? Does cargo just not know how to install it?


---

_Comment by @BurntSushi on 2016-10-24 22:40_

Correct.


---

_Comment by @BurntSushi on 2016-10-25 18:18_

Yes it does. It uses the standard `-F` flag that grep uses.

Note that this should never be used for performance reasons. Its only use
is for convenience since you don't need to worry about escaping regex meta
characters.

On Oct 21, 2016 14:23, "Kevin Ballard" notifications@github.com wrote:

> I use ag -Q (or ag --literal) fairly often to do a literal search across
> files, since it's so fast and colorizes the output. I'd love to be able to
> use rg this way, except rg doesn't seem to have any support for literal
> searching.
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/190, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34rbg_mrtnusqX_ufRBpryFdc41sgks5q2QMggaJpZM4Kdd_s
> .


---

_Comment by @duane on 2017-01-28 01:22_

You should probably stop advertising any compatibility with ag/ack if you don't make an effort to match their arguments.... Why copy from grep when you advertise copying from ag/ack? It makes no sense, and ripgrep is harder to use because of it.

It's also very difficult to find in the man because you don't use 'literal', 'quote', 'exact', or any other terminology people are likely to use. "Fixed strings" is a terrible phrase that can mean anything. What is a non fixed string?

---

_Comment by @BurntSushi on 2017-01-28 01:39_

@duane Can you please point me to the place where ripgrep advertises "compatibility with ag/ack" so that I can fix it? No such promise is intended. No such promise for grep is intended either. ripgrep does take inspiration from those tools though.

Documentation can always be improved. I'm not perfect.

> It's also very difficult to find in the man because you don't use 'literal', 'quote', 'exact', or any other terminology people are likely to use.

"literal" is there: https://github.com/BurntSushi/ripgrep/blob/master/doc/rg.1#L61 --- It's also in the `--help` output.

FWIW, the `-F/--fixed-strings` flag is from grep. I'd say people are likely to just try `-F` and hope it works.

---

_Comment by @offero on 2019-11-15 16:06_

The help is pretty large. I spent a minute reading through it and then just searched the internet and came here.

---

_Comment by @duane on 2020-01-27 08:12_

The "literal" reference in --help refers to a regular expression, not a literal. Edited to remove snark. The documentation does not seem accurate to me.

Re: ack and ag, it's not clear why this is undesirable—they have a much more approachable interface, and I would be happy to explain why if you indicate interest.

---

_Comment by @okdana on 2020-01-27 09:19_

> The "literal" reference in --help refers to a regular expression, not a literal.... The documentation does not seem accurate to me.

What is inaccurate about it...?

```
% rg --help | rg -C2 literal
...
--
             
    -F, --fixed-strings                     
            Treat the pattern as a literal string instead of a regular expression. When
            this flag is used, special regular expression meta characters such as .(){}*+
            do not need to be escaped.
--
...
```

---
