```yaml
number: 542
title: Support Basic Regular Expression (BRE) syntax common to vim, gnu grep, and git grep
type: issue
state: closed
author: RedBeard0531
labels:
  - enhancement
  - question
  - icebox
assignees: []
created_at: 2017-07-06T19:14:00Z
updated_at: 2017-10-22T01:24:45Z
url: https://github.com/BurntSushi/ripgrep/issues/542
synced_at: 2026-01-12T16:13:22Z
```

# Support Basic Regular Expression (BRE) syntax common to vim, gnu grep, and git grep

---

_@RedBeard0531_

Part of what prevents `rg` from feeling like a drop in replacement for gnu grep or git grep is that some parts of the regex syntax behave differently. These are the ones that seem to bite me the most. The first two could probably be always available, but the last would require a flag to activate "basic" mode. 

1. Support `$` as a literal in the middle of the pattern and only give it special meaning at the end of a sub-pattern. With today's syntax I don't think the pattern `$cmd` will ever match any input so it is probably not what the user wants. Ditto for `^` anywhere other than the beginning, but that is less annoying because it is used much less often in code.
2. Support `\<` and `\>` zero-width assertions of begin and end of word. This is the pattern that vim uses when you search for current word by hitting `*` or `#`. These can probably just be made synonyms of `\b`, which is what [ack.vim](https://github.com/mileszs/ack.vim/blob/6ef28a1/autoload/ack.vim#L70-L71) does. 
3. Add a mode where every character except `.*[\` and `^$` at start and end of pattern is considered a literal, and require `\` to activate the special meaning.  I know that the basic syntax is more slash-heavy when writing complex regexes, but I find myself wanting to use a literal `(` or `{` much more often than using the regex meanings.

If you'd prefer, I can split this up and make 3 separate tickets.

---

_Comment by @BurntSushi on 2017-07-06 23:23_

@RedBeard0531 Thanks for filing this issue. Unfortunately, my short answer here is probably: no to everything.

The longer answer has three prongs to it.

Philosophically, I very much dislike BREs. They feel like a legacy to me, and from my own personal experience and from hearing about the experience of others, have led to a lot of confusion. At least conceptually speaking, I'd rather support one syntax instead of one syntax and one legacy syntax that is almost-but-not-quite-like the other syntax.

Motivation wise, ripgrep was never, is never, and will never be a drop-in replacement for any of the standard Unix tools like grep or sed. While there's a lot to be said for keeping behavior similarish because it corresponds to what people know, I'm generally not in favor of adding things to ripgrep *just because* that's how some other tools does it. I think the features need to stand on their own. With that said, I do sympathize with your annoyances and can see how BREs can be more convenient in some cases. In my experience, the `-F` flag (which just interprets the pattern as a literal with no regexes at all) goes a long way to fixing most of them. The only cases it doesn't cover is when you do want to sprinkle a bit of regex with your literal. The problem with BREs is that you need to keep a bunch of special rules in your head; some meta characters need to be escaped while others don't. So it's kind of annoyance either way, although I can appreciate that folks have different preferences.

Finally, and perhaps most importantly, the features you've requested here basically have two possible implementation paths. Either they get added to the upstream [`regex-syntax`](https://github.com/rust-lang/regex) parser (which I myself maintain) or ripgrep grows its own regex parser or ripgrep somehow hacks these features by doing a translation step at the level of concrete syntax (which is roughly equivalent to writing a parser). Any of these feels like a lot of work to me.

Adding these features to `regex-syntax` proper feels a bit out of place since having support for BREs at the code level has a much less compelling argument, since you're less likely to be typing ad hoc patterns on the fly like you would with `grep`. Adding this on top of `regex-syntax` inside ripgrep isn't really possible, because the `regex-syntax` parser doesn't actually produce a faithful AST. Instead, it produces something akin to a high level intermediate representation that can be compiled to opcodes easily. So things like "a literal `$` was used" end up getting lost in translation (a `$` can yield either a `EndLine` or a `EndText`, depending on whether the `m`/"multi line" flag is set).

Some features (like `$` should be a literal `$` unless it is at the end up a sub-pattern) really require a full parser to implement, which means there's no way to hack that on top of the `regex-syntax` parser without writing your own parser. And I will say this: there's **no way** ripgrep is getting its own regex parser. Never. Never. Never. :-)

-----

With all that said, I am in the process of rewriting the `regex-syntax` crate to provide a more faithful AST representation of a regex pattern, and then a separate explicit translation step to something like the HIR that we have now. If we had a faithful AST, then I believe one could write a transformation on top of that that implements all of the features you wanted here. (There would be tricky parts, for example, unwinding a `(re)+` expression to a `(re)\+`, but it seems possible.) But, this rewrite has already been months in the making. I've made a ton of progress, but it will take a while yet before it's ready for us.

So... I will say this. If the new `regex-syntax` parser does indeed make these transformations easy and it is easy to maintain, then this is probably something I'd be willing to add to ripgrep. But if this somehow adds an explosion of implementation complexity that I'm not able to see right now, then the outlook will not look so rosy. :-)

---

_Label `enhancement` added by @BurntSushi on 2017-07-06 23:23_

---

_Label `icebox` added by @BurntSushi on 2017-07-06 23:23_

---

_Label `question` added by @BurntSushi on 2017-07-06 23:23_

---

_Comment by @BurntSushi on 2017-10-22 01:24_

I'm going to close this. I think my conclusion in my previous comment still stands, but I don't see any reason to track this. If an industrious individual wants to do this after the new regex parser lands (which is still a ways away), then we can revisit this ticket.

---

_Closed by @BurntSushi on 2017-10-22 01:24_

---
