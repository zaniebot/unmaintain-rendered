```yaml
number: 664
title: "Glob: support for extended fnmatch-style glob patterns"
type: issue
state: closed
author: bpasero
labels:
  - enhancement
  - question
assignees: []
created_at: 2017-11-06T10:18:54Z
updated_at: 2018-01-31T02:31:04Z
url: https://github.com/BurntSushi/ripgrep/issues/664
synced_at: 2026-01-12T16:13:22Z
```

# Glob: support for extended fnmatch-style glob patterns

---

_@bpasero_

I wonder if extended glob patterns are on the list for the future. Specifically I am referring to:

Source: http://man7.org/linux/man-pages/man3/fnmatch.3.html

> '?(pattern-list)'
>       The pattern matches if zero or one occurrences of any of the
>       patterns in the pattern-list match the input string.
> 
> '*(pattern-list)'
>       The pattern matches if zero or more occurrences of any of the
>       patterns in the pattern-list match the input string.
> 
> '+(pattern-list)'
>       The pattern matches if one or more occurrences of any of the
>       patterns in the pattern-list match the input string.
> 
> '@(pattern-list)'
>       The pattern matches if exactly one occurrence of any of the
>       patterns in the pattern-list match the input string.
> 
> '!(pattern-list)'
>       The pattern matches if the input string cannot be matched with
>       any of the patterns in the pattern-list.

I am mainly interested in support for negation (`!`) within a pattern. 

---

_Renamed from "Glob: support for extended glob patterns" to "Glob: support for extended fnmatch-style glob patterns" by @bpasero on 2017-11-06 10:19_

---

_Comment by @BurntSushi on 2017-11-06 10:32_

Could you please elaborate on your use case for these? I'm no spring chicken, and in all my years I have never seen anyone try to use these glob extensions. If it's because they don't enjoy wide support, then it seems like no big deal for ripgrep to elide them.

Moreover, of the extensions proposed, negation is by far the hardest to implement.

---

_Comment by @bpasero on 2017-11-07 06:35_

@BurntSushi I am with the VS Code team and we are using glob patterns in various places (e.g. to control which files should be hidden in the files explorer). Currently VS Code is not supporting negations within the pattern but we have people asking for it (e.g. to be able to hide the contents of a folder except for specific entries). 

Now, we pass on the glob patterns to RipGrep (to define where to search in or what to exclude) and of course this means our glob syntax is limited with what RipGrep is supporting. We would not make our glob more powerful if RipGrep does not support the syntax. Otherwise we would have to exclude the pattern in RipGrep and do a post-process of the results using our glob matcher implementation which seems wrong. 

/cc @roblourens

---

_Comment by @BurntSushi on 2017-11-07 13:20_

Yeah, I mean, I hear ya, but I don't think this is going to happen. I think I *might* be able to get on board with this (sans negation) if you twisted my arm, but asking me to support negation is basically asking me to write and maintain an entirely new glob implementation.

To elaborate, ripgrep's glob matcher is provided by the [`globset`](https://docs.rs/globset/0.2.1/globset/) crate, which works by translating the glob to a regular expression, and does various literal optimizations on top of that. This in particular enables the ability to compile an entire set of globs (from, say the CLI or a `.gitignore` file) into a single finite state machine and execute it over a file path once to discover every matching glob.

All of the extensions you propose (again, except for negation) seem like they translate well to regular expressions, so supporting them is mostly a matter of adding support to the glob parser and the translation to regex. It will require a rewrite of the parser (the parser currently doesn't support arbitrarily nested globs), but that's as far as it goes. Negation, on the other hand, isn't supported by the regex engine at all. Not because it's impossible, but because it's hard. So to add negation, I'd need to do one of

1. Add support for negation to the regex engine
2. Write a completely different glob matcher implementation.
3. Find a pre-existing glob matcher implementation that supports it.

Even if I agreed to do (1), the time scale on that is measured in *years*. (2) is something I personally won't do because that isn't how I want to spend my time. And finally, (3) (and also (2)) suffer the problem where you have one particular type of glob that can't be compiled with the rest of the globs, which will create all manner of special cases in the code. That doesn't sound like fun to maintain.

So ya, there ya have it. You might consider that running your own glob matcher after-the-fact if a user is using an extension to be the least bad solution. :-)

---

_Comment by @bpasero on 2017-11-08 06:38_

Thanks for the summary, I wasn't thinking this was an easy task, it also means a bit of work to support this in our glob implementation in VS Code ;)

---

_Comment by @BurntSushi on 2018-01-31 02:30_

Based on my previous comment here, I'm going to close this out. While I appreciate that some folks may find these glob extensions useful, I think the bang isn't worth the buck in this instance.

---

_Closed by @BurntSushi on 2018-01-31 02:30_

---

_Label `enhancement` added by @BurntSushi on 2018-01-31 02:31_

---

_Label `question` added by @BurntSushi on 2018-01-31 02:31_

---
