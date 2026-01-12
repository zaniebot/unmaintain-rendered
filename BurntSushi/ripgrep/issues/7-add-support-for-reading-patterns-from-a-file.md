```yaml
number: 7
title: add support for reading patterns from a file
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-17T20:59:03Z
updated_at: 2016-11-15T23:15:55Z
url: https://github.com/BurntSushi/ripgrep/issues/7
synced_at: 2026-01-12T18:23:11Z
```

# add support for reading patterns from a file

---

_@BurntSushi_

It would be cool to support `grep`'s `-f/--file` option, and it should be relatively easy for us to do so. The implementation strategy I have in mind is to just join all of the patterns/literals using a `|` and hand it off to the regex engine.

It _might_ be faster to hand build an Aho-Corasick automaton (using the `aho-corasick` crate) if you know we have a bunch of literals. In fact, this is almost assuredly more memory efficient if the number of literals being searched is very large. Unfortunately, this is a harder thing to add, since it would require plumbing an Aho-Corasick automaton through all of the searching code so that either a `Regex` or an `AcAutomaton` could be used. Doable, but not straight-forward.


---

_Label `enhancement` added by @BurntSushi on 2016-09-17 20:59_

---

_Comment by @BurntSushi on 2016-09-22 11:39_

> Unfortunately, this is a harder thing to add, since it would require plumbing an Aho-Corasick automaton through all of the searching code so that either a Regex or an AcAutomaton could be used. Doable, but not straight-forward.

Thinking about this just a touch more, I think it might actually be easier than I let on. The search code doesn't actually know anything about regexes---all it cares about is using the relatively simple interface of `grep::Grep`. If we put that interface behind a trait and then impl it with Aho-Corasick, we should be pretty much good to go.

It might be nice if the trait and Aho-Corasick implementation were in the `grep` sub-crate. And maybe the `Grep` type should be renamed to `Regex`, that way, the trait could be called `Grep`. Hmm, yes, I quite like that!


---

_Comment by @seamusabshere on 2016-09-23 16:27_

for context, here's the man page entry from grep:

```
 -f file, --file=file
    Read one or more newline separated patterns from file.  Empty pattern
    lines match every input line.  Newlines are not considered part of a
    pattern.  If file is empty, nothing is matched.
```

🌞 note that neither `ag` or `ack` have this, so it would be another differentiator!


---

_Comment by @emk on 2016-10-11 21:29_

I might have some time next week to take a look at this. Would you be interested in a PR following your outline above?


---

_Comment by @BurntSushi on 2016-10-11 21:34_

@emk That'd be great! Although re-reading it, I think I'd like to keep the trait private inside of `ripgrep` for now. I'd eventually like to move more of the searching code to the `grep` crate, so its API is going to need to be re-worked and thought out anyway.


---

_Comment by @emk on 2016-10-28 22:02_

Happy news! My top card in Trello at work says we need support for fast `-f`-style searching, so I have time to start on this on Monday. I'd love to prepare you a pull request to look at. Beyond what you wrote above, do you have any further advice on how I should tackle this to make the integration process as easy as possible?


---

_Comment by @BurntSushi on 2016-10-28 22:14_

> My top card in Trello at work says we need support for fast -f-style searching

Do you have any requirements more specific than this? Specifically, the number of strings you need to search for?

> I'd love to prepare you a pull request to look at. Beyond what you wrote above, do you have any further advice on how I should tackle this to make the integration process as easy as possible?

I think it's probably pretty important to support use of Aho-Corasick when the `-F` flag is given, which should bypass the regex engine entirely. Specifically, Aho-Corasick should scale quite a bit better to thousands/millions of literal strings than the regex engine will. A key problem you might face with this is the fact that Aho-Corasick doesn't actually preserve leftmost-first match semantics, and I'm not quite sure how to deal with that.

In any case, I suspect that the easiest first thing you can do here is to forget about the Aho-Corasick optimization and just join all the strings together with `|` and feed it to the regex engine. This may not meet your performance requirements however.


---

_Comment by @emk on 2016-10-28 22:27_

Thank for the encouragement! (And for the great tool, which I already use dozens of times a day.)

As for the implementation, let's start with the simple case as a warm-up exercise and go from there. :-)

For our purposes, I could easily imagine a couple hundred strings. As far as I know, we're unlikely need to search for thousands of strings, but @seamusabshere might know otherwise. I'm still happy to try to implement the general case, because I agree that the PR just wouldn't be up to ripgrep's high standards otherwise.

In the longer run, our use case might benefit from a `ripgrep` API which allowed us to call the searching engine from another binary. But for our first in-house experiments, we'll have a shell script that uses `curl` to download a list of strings from a URL, and which then passes them to `rg` for a filesystem-wide search. We might also benefit from a basic implementation of #1 that can at least detect UTF-16 files automatically, even if it can't detect more exotic encodings, so that we don't miss data in files created on Windows.


---

_Comment by @BurntSushi on 2016-10-28 22:56_

> As for the implementation, let's start with the simple case as a warm-up exercise and go from there. :-)

To be clear, I'd be happy with a PR that did the simplest thing first! I imagine it should work fine with hundreds of strings. I don't know what it will look like at thousands or hundreds of thousands though. (I do know that my Aho-Corasick implementation can handle a million pretty easily.)

> In the longer run, our use case might benefit from a ripgrep API which allowed us to call the searching engine from another binary. But for our first in-house experiments, we'll have a shell script that uses curl to download a list of strings from a URL, and which then passes them to rg for a filesystem-wide search.

Yup. #162 is the tracking issue for this. I'm almost ready to submit a PR that takes another step in that direction.

> We might also benefit from a basic implementation of #1 that can at least detect UTF-16 files automatically, even if it can't detect more exotic encodings, so that we don't miss data in files created on Windows.

I honestly expect #1 to be quite hard to implement. At a high level, you just need something that takes a `Read` and transcodes the raw input to UTF-8, which in turn also implements `Read`. I think that alone is enough to work with the current implementation. The problem then comes with the output though. Do you output in the same encoding as the input? If so, that sounds pretty hairy. (Possibly even intractable.)

I am kind of hoping to delay #1 until the search code gets pushed into its own library. That way we'll have an API to look at and design from, instead of what we have today, which is a bit convoluted.


---

_Comment by @seamusabshere on 2016-10-29 23:14_

examples of what i want to search for:
- specific uuids
- strings that look like addresses `/\d+ [a-z]+ (?:st|dr)/i`
- `/^[0-9A-F]{8}-[0-9A-F]{4}-[4][0-9A-F]{3}-[89AB][0-9A-F]{3}-[0-9A-F]{12}$/i`

in case that's helpful :)


---

_Comment by @BurntSushi on 2016-10-29 23:16_

@seamusabshere I think that's all part of the plan. Most of the discussion at this point is just implementation details. :-)

Note that ripgrep probably won't ever support "I want to match hundreds of thousands of regexes all at once." If you're at that point, you'll probably need something more specialized. (Hundreds of thousands of _literals_ should be fine though, even if it's not practical in the first rev.)


---

_Comment by @BurntSushi on 2016-11-06 17:27_

Note that if this supports `-f-` for reading from stdin, then it should satisfy #180 (which I'm now closing as a dupe).


---

_Comment by @emk on 2016-11-06 17:33_

Ah, nice! OK, I have some work in progress on this issue, and I'll be sure to support that case as well. That's a great feature.


---

_Comment by @emk on 2016-11-07 23:42_

The trickiest part of this so far is testing `-f-`, which can't be done using the existing `WorkDir::stdout` and `WorkDir::output` APIs, because we need to pipe some standard input to the process. This requires using something like `spawn`, as in [this example](http://rustbyexample.com/std_misc/process/pipe.html). Do you have any preferences about how I should implement this?


---

_Comment by @BurntSushi on 2016-11-07 23:50_

@emk If there's a way to add a convenience method or two to `WorkDir` to test it, then I think that would be fine. That way, we could add other tests that depend on stdin.

However, since there aren't actually any existing tests for stdin, I wouldn't be offended if you opted to skip testing `-f-`. :-)


---

_Comment by @emk on 2016-11-09 11:23_

The first piece of this is available in #227. Thank you for getting me started in the right direction, and please feel free to suggest improvements!


---

_Comment by @BurntSushi on 2016-11-15 23:15_

Closed by #227 


---

_Closed by @BurntSushi on 2016-11-15 23:15_

---
