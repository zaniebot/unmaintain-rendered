```yaml
number: 176
title: support searching across multiple lines
type: issue
state: closed
author: isobit
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2016-10-13T21:31:20Z
updated_at: 2021-12-01T14:11:33Z
url: https://github.com/BurntSushi/ripgrep/issues/176
synced_at: 2026-01-12T16:13:21Z
```

# support searching across multiple lines

---

_@isobit_

Say for example I'm trying to find instances of `click` that reside in a `listeners` block, like so:

```
listeners: {
    foo: ...
    click: ....
}
```

According to the Rust regex docs, I should be able to do: `rg '(?s)listeners.+click'`, but this doesn't seem to work. Does ripgrep not support multiline regex?


---

_Comment by @BurntSushi on 2016-10-13 22:06_

> Does ripgrep not support multiline regex?

Correct. Not even the `s` flag will help, because `ripgrep` explicitly instructs the regex automaton to never match `\n`. Like `grep`, `ripgrep` is a _line oriented search tool_.

`ripgrep` can perform a search in two different ways. One of them reads a chunk of bytes at a time and searches it. The other memory maps the file and searches that all at once. The former has a number of advantages, including being faster when searching a large number of small files in parallel and being able to search streams in constant memory. The latter has the advantage of being faster for single files (sometimes) and much simpler to implement.

The former _only works_ because search is line oriented. A multiline regex can technically match, say, 2GB of data, which is completely incompatible with searching small chunks at a time.

The latter could be made to work with multiline search, but memory maps can't search stdin for example. So a multiline search on stdin would have to block and read all of stdin into memory before searching. (There exists a way around even this, but it requires changing the regex engine to be capable of incremental search, which is an even bigger change, but theoretically possible.)

multiline searching therefore comes with significant implementation complexity, and IMO is a pretty niche use case. I can also imagine it having a pretty big impact on the printing code. This fact alone is a good reason why it may never be in `ripgrep` proper, but perhaps once #162 is done, others can take a crack at it.

This is a good example of a feature that The Silver Searcher has that `ripgrep` may either never have or won't have for a long time.


---

_Closed by @BurntSushi on 2016-10-13 22:06_

---

_Comment by @isobit on 2016-10-13 23:50_

Gotcha, thanks for the explanation. I really like ripgrep as a tool, just was hoping to use it for this case too 😉 . 


---

_Comment by @BurntSushi on 2016-10-13 23:53_

@joshglendenning Yeah, I admit, it would be nice, and if it were easy, I'd have no problems with it. While I do consider it niche, I have no doubts that it would be quite useful!

Once I split out most of the pieces of `ripgrep` to library form, perhaps there will be interest in building other tools for more niche use cases! I will keep this case in mind as I do that though.


---

_Comment by @maxbrunsfeld on 2017-01-09 19:46_

This is a really cool tool, but I might suggest including this as a caveat in the README, alongside the comparisons to `ag`, since `ag` does support multi-line patterns.

---

_Comment by @BurntSushi on 2017-01-10 00:57_

@maxbrunsfeld I've been meaning to add an "anti pitch" section to the README like the one in my blog post. [That's now done.](https://github.com/BurntSushi/ripgrep#why-shouldnt-i-use-ripgrep) Thanks for the reminder!

---

_Reopened by @BurntSushi on 2017-03-17 01:08_

---

_Comment by @BurntSushi on 2017-03-17 01:18_

I'm going to re-open this, because it's one of the most highly requested features.

Nothing has changed about the [problems I outlined above](https://github.com/BurntSushi/ripgrep/issues/176#issuecomment-253652811). However, multiline search needn't be the default. If we provide it as a flag, then we can do what we need to do to support multiline search only when that flag is provided. The critical thing that multiline search needs is a *complete* sequence of bytes in memory to search. Memory maps can provide this, but failing that, we would need to read the entire file into memory before starting a search.

Other than using heap space proportional to the file being searched, the fundamental issue with this flag is when it's used in conjunction with searching stdin. Namely, ripgrep will need to block until EOF is read on stdin before a search can even start. Alternatively, multiline search simply wouldn't be allowed on stdin. The silver searcher will in fact do this silently when searching stdin:

```
/* TODO: this will only match single lines. multi-line regexes silently don't match */
void search_stream(FILE *stream, const char *path) {
    // ...
}
```

I don't like the "silent" idea, but stopping ripgrep with an error is certainly something I'd be open to. Neither seem like good choices to me, but I don't think it should block this feature altogether.

N.B. This is a significant feature and it would have to be part of the libripgrep effort.

---

_Comment by @BurntSushi on 2017-03-17 01:22_

The other thing I forgot to mention is that multiline search will negate inner literal optimizations. Normal prefix and, in special cases, suffix, literal optimizations will still be performed as part of the regex engine. (I've long thought about making inner literal optimizations work on arbitrary strings, but it's hard.)

---

_Label `libripgrep` added by @BurntSushi on 2017-03-17 01:22_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-03-17 01:23_

---

_Comment by @gulshan on 2017-03-17 01:35_

A naive question/suggestion. Assuming single lines are being loaded for search now, can that be changed to n lines, n set to 10 or 20 or something like that? While a line gets in, another gets out of the load in FIFO fashion? This will not be technically correct for all cases, but may be enough for most cases.

---

_Comment by @d-akara on 2017-03-17 01:36_

How significant are the trade-offs to the user experience?  
If doing multiline is more expensive, I'm fine with that as long as single line performance is not impacted.

Would you actually need a special flag to ripgrep? or can you reliably determine from the expression itself?

---

_Comment by @BurntSushi on 2017-03-17 01:49_

Great questions! Keep'em coming.

> Assuming single lines are being loaded for search now

They are not. If they were, ripgrep would be very slow. The reasons for this are a bit subtle, but basically, "it's faster to search a huge chunk than it is to break it into little pieces and then search each piece." "Huge chunk" in this case might be the size of some internal buffer, perhaps, 8KB.

If you're curious about how a fast grep tool works in more detail, check out this section in my blog post on ripgrep: http://blog.burntsushi.net/ripgrep/#anatomy-of-a-grep

> can that be changed to n lines, n set to 10 or 20 or something like that? While a line gets in, another gets out of the load in FIFO fashion? This will not be technically correct for all cases, but may be enough for most cases.

If you have a regex like `a\s+b`, then it's not possible to determine the length of the match up front. You have three choices:

1. You use a regex engine that supports incremental search. (This is somewhat at odds with performance if "incremental" means "byte at a time." So for something like this, you'd need an incremental engine that can process chunks at a time.) ripgrep's regex engine doesn't support this.
2. You feed the regex engine every byte you got. (The Plan.)
3. You arbitrarily cap the size of the match. This will invariably get things wrong and there's no way to escape.

I still actually strongly believe that multiline search is a very niche feature, but it is one that can be quite useful when the situation calls for it. (A text editor is perhaps one such situation, but ripgrep is first and foremost a command line tool where multiline search feels a lot less common.) Therefore, taking approach (3) doesn't seem worth it. In the common case, memory maps will work just fine and your OS will manage the memory for you. It's only the corner cases that are sub-optimal: when memory maps can't be used (e.g., on virtual files or stdin).

> How significant are the trade-offs to the user experience? If doing multiline is more expensive, I'm fine with that as long as single line performance is not impacted.

If `--multiline` is behind a flag, then I'm pretty confident that the standard UX of ripgrep won't be impacted. Including performance.

> Would you actually need a special flag to ripgrep? or can you reliably determine from the expression itself?

A flag is 100% necessary. A regex like `a\s+b` shouldn't match across multiple lines by default, because that's what we've all come to expect from line oriented searchers. But it is totally plausible that you might *want* it to. That's when you'd pass a flag.

---

_Comment by @d-akara on 2017-03-17 02:57_

> I still actually strongly believe that multiline search is a very niche feature, but it is one that can be quite useful when the situation calls for it.

I would agree use is actually niche, but desire to use is not.
1.  It is a bit non intuitive how to properly write a multiline expression.  Especially if the engine doesn't support the `. dotAll` matching and even worse if you want to constrain to a range like next N lines.
1.  Due to 1, many use incomplete results although not always knowingly.  Most coding languages can have line breaks almost anywhere.

I would say if you are searching for 2 terms and completeness is important then using multiline would often be your default.  However, writing an expression to find termA followed by termB within 5 or less lines is likely not something that rolls off of the fingertips of someone who occasionally uses regular expressions although I think many would find it useful and use such expressions if more intuitive to write.

---

_Comment by @BurntSushi on 2017-03-17 10:58_

@dakaraphi Good points. I'd like to use your comment to *constrain* this feature, namely, that multiline search is the ability to apply a regex whose matches may span an arbitrary number of lines.

With that said:

1. It would be plausible to make `.` match `\n` by default if multiline mode is enabled.
2. The use case of "where do `A` and `B` co-occur within N lines of each other" is definitely something I agree can be useful. It's possible to some extent to do this with a regex, e.g., `A([^\n]*\n){0,5}[^\n]*B|B([^\n]*\n){0,5}[^\n]*A`, but that is a little painful. Extending this to three terms would probably be horrifying.

I think (2) is something that's enabled by multiline search, although, today, you can do something similar with contexts: `rg B -C5 | rg A -C5` for example works to some extent. Regardless, it might be wiser to categorize this into a separate feature whose UX can be more thoughtfully designed. Others have requested similarish things, as in #346 and #360. [`sift`](https://github.com/svent/sift) is a tool that has support for this kind of matching, so we may be able to crib ideas from them.

With all that said, we must be careful not to get too far away from what ripgrep is supposed to be good at doing: _searching lines_. :-) I say this because there *has* to be a point at which "write code for your specialized search" becomes a valid thing to say. The key is figuring out where that point is.

---

_Comment by @d-akara on 2017-03-17 12:47_

> multiline search is the ability to apply a regex whose matches may span an arbitrary number of lines

Just to make sure I understand the intention, could you state that as what you see ripgrep would not do that possibly other regex engines do when searching multiline?

---

_Comment by @BurntSushi on 2017-03-17 12:54_

@dakaraphi Sorry, the intention of me saying that was to push UX concerns like "how do I find co-occurring terms, A and B, within a fixed number of lines" out of multiline support. i.e., I don't think that particular UX should be addressed as part of standard multiline support, but should instead be considered as a separate feature (that may or may not happen). :-)

I don't think there's anything ripgrep would do differently in terms of UX with respect to the silver searcher, other than 1) not doing it by default and 2) probably not doing silent things.

---

_Comment by @BurntSushi on 2017-03-17 12:54_

Are there are other tools that support multiline search other than the silver searcher?

---

_Comment by @d-akara on 2017-03-17 14:22_

I'm not sure about command line tools.  Prior to using VS Code I was using Brackets which supported multiline file search.  I believe other editors like Sublime, Notepad++ etc also support multiline.

---

_Comment by @d-akara on 2017-03-17 14:25_

> I don't think that particular UX should be addressed as part of standard multiline support, but should instead be considered as a separate feature (that may or may not happen). :-)

ok right.  Yes I'm not sure if that really should be part of something like ripgrep or not.  For example, I've been thinking about maybe writing some extension for VS Code like a regex helper or such that would take something like common patterns or templates and you just plugin the values for such use cases and it would generate the regex.

---

_Comment by @BurntSushi on 2017-03-17 14:38_

@dakaraphi Great! I think we're on the same page now. :-) Thanks for poking!

---

_Label `enhancement` added by @BurntSushi on 2017-04-09 13:13_

---

_Comment by @rshpeley on 2017-04-30 05:34_

@dakaraphi directed me here from Microsoft/vscode #13155

It looks like one of the most common requests for searching across multiple lines is related to text editors. At the moment, my needs are very simple. If I can get a match across multiple files in a project for a multiline selection -- even if it's fully literal -- I could work with it. For most text editors, the menu option to search across multiple lines is separate than a simple search, and so a ripgrep flag, as @BurntSushi suggested, would naturally fit this use case.

I'm still making it through @BurntSushi's _anatomy of a grep_ link, but it appears to me that a multiline search for text editors mostly requires a literal search with some multiple literals (white space, line endings) and therefore the search won't even make it to the regex engine for these cases. 

Isn't the multiple line selection just a contiguous sequence of bytes (in the fully literal case) to be matched in a buffer? Or am I missing something related to optimisation here?

I'm sure people will come up with cases where a regex in a multiline search/replace would be mighty handy, but I think support for the simpler multiple literal multiline case would be a good start to give some text editors (such as vscode and atom) missing functionality.

btw, a most excellent ripgrep article @BurntSushi!

---

_Comment by @priyadarshan on 2017-04-30 06:30_

Multi-line searching would be a boon to many. See for example [this use case](https://github.com/alphapapa/helm-org-rifle/issues/15#issuecomment-293707038).

---

_Comment by @BurntSushi on 2017-04-30 11:57_

@rshpeley I'm having a hard time understanding your comment. Do you have a specific question?

---

_Comment by @rshpeley on 2017-04-30 13:42_

@BurntSushi I'm having a hard time understanding why you don't understand -- in particular your now edited comment where you previously said,

_(Implementation suggestions are welcome, but I strongly advise reading the existing implementation before making them.)_

If I had a specific question, other than the topic of this one, I would have started a new issue. Perhaps it would be better for you to state what you don't understand from my previous post. 

---

_Comment by @BurntSushi on 2017-04-30 16:13_

@rshpeley Are you making a suggestion on the next step? Or are you saying, "this is an important feature."? My question is: what are you trying to achieve? Are there specific questions I can help answer for you?

---

_Comment by @waldyrious on 2017-04-30 19:58_

Sorry to interject, but although I agree @rshpeley's comment was perhaps a bit longer (or less structured) than ideal, I believe it does contain sufficient elements to provide the relevant information, particularly:

> Are you making a suggestion on the next step?
>
>> (...) I think support for the simpler multiple literal multiline case would be a good start (...)

and

> Are there specific questions I can help answer for you?
>
>> Isn't the multiple line selection just a contiguous sequence of bytes (in the fully literal case) to be matched in a buffer? Or am I missing something related to optimisation here?



---

_Comment by @BurntSushi on 2017-04-30 20:20_

> I think support for the simpler multiple literal multiline case would be a good start 

I disagree. I'm not particularly interested in half baked solutions. Moreover, I don't think "simpler" actually carries through to the implementation.

> Isn't the multiple line selection just a contiguous sequence of bytes (in the fully literal case) to be matched in a buffer? Or am I missing something related to optimisation here?

All searching happens through the `grep` crate, and the interface is a regex. Literal optimizations are an implementation detail.

I would be happy to answer more specific questions about the code.

---

_Comment by @kasperschnack on 2018-03-26 10:09_

I was looking for this feature and found a hacky way of matching only files that contained both(all) patterns

`rg -C 9999 pattern1 | rg pattern2`. I know, it isn't pretty. But it gets the job done :)

---

_Comment by @BurntSushi on 2018-06-26 00:19_

I am making decent progress on multi line mode in ripgrep. In the course of implementing it, I came across a question that doesn't have an obvious answer (to me anyway) and I was wondering if anyone had any thoughts on it.

In particular, when multi line mode is enabled (which is an opt-in), what should the semantics of `.` be? In most regex engines, a `.` will match any character except for `\n`. In ripgrep today, we obviously don't have to worry about this because every match is restricted to a single line, so a `.` can't match through a `\n`. But in multi line mode, it's not clear to me what the **default** behavior should be.

* We could make `.` match `\n` by default when multi line mode is enabled, which seems somewhat natural, since multi line mode had to be explicitly enabled. However, I worry that folks will find this surprising since I kind of think that most people have come to expect `.` to not match `\n`.
* We could make `.` not match `\n` by default when multi line mode is enabled, but here I feel like this might also be surprising since folks might be expecting this sort of thing to "just work" since they had to opt-in to multi line search.

Of course, in either case, the user can always twiddle the meaning by using `(?s:.)` to force `.` to match `\n` and `(?-s:.)` to force `.` to not match `\n`.

What do people think the default behavior should be?

---

_Comment by @varkor on 2018-06-26 00:51_

Personally, I've always found it frustrating that `.` does not match newlines, as when I want to match any character, I mean any character, and having to special case `\n` each time is a pain.

I think though people might have come to expect `.` doesn't match `\n`, that's more often than not an inconvenience rather than a feature. I think, as you say, having opted into multi-line mode is a sufficient reason to override this behaviour.

---

_Comment by @BurntSushi on 2018-06-26 13:36_

Interestingly, the silver searcher by default does not permit `.` to match new lines:

```
$ cat testing
a
b
$ ag '...' testing
$ ag '(?s)...' testing
1:a
2:b
```

Not to imply that we should definitely follow the silver searcher's lead, but it's an interesting data point.

---

_Comment by @d-akara on 2018-06-26 14:00_

Most implementations have a 'dotall' flag.  Typically 's'

For example:
PCRE uses 's' http://php.net/manual/en/reference.pcre.pattern.modifiers.php
JavaScript will use 's' https://github.com/tc39/proposal-regexp-dotall-flag
Python uses 's' https://docs.python.org/2/library/re.html#re.S

---

_Comment by @BurntSushi on 2018-06-26 14:02_

@dakaraphi Could you say more? How does that help us determine how ripgrep should behave by default?

(I both mentioned and used the "dotall" flag in my comments above.)

---

_Comment by @d-akara on 2018-06-26 14:24_

I guess I'm stating convention is already well established that you must opt-in to dotall.
Although convenient, I'm not sure if enabling multi-line should change the meaning of `.`


---

_Comment by @ssokolow on 2018-06-26 14:29_

@dakaraphi PCRE, JavaScript, and Python all have regex engines that operate in multi-line-capable mode by default. ripgrep would require an option to opt into that mode.

The question is whether "enable multi-line matching, but keep `.` in non-dotall mode" is a common enough case to justify "enable multi-line matching" not implying dotall. (Keeping in mind that it's always possible to turn dotall back off using `(?-s:.)`.)


---

_Comment by @poke on 2018-06-26 14:29_

I would agree that having dotall enabled by default would appear more sensible. After all, the user opted in to search over multiple lines, so a multi-line behavior is already desired.

---

_Comment by @d-akara on 2018-06-26 14:37_

I was considering if one were switching regex implementations how likely they might need to alter their existing regex to work with ripgrep.
Seems either way you are likely that you would have to alter the existing regex being that multiline is already default from other implementations.

In such case, there doesn't seem to be much gained from keeping the original precedent.  I agree that `.` should default as dotall 

---

_Comment by @okdana on 2018-06-26 14:58_

I don't think i've ever seen a regular-expression implementation that automatically enables `DOTALL` (or equivalent) in 'multi-line' mode. (To say 'multi-line' here is a bit confusing, because in PCRE and, i'm pretty sure, the Rust engine as well, patterns are *always* matched across lines, and the `MULTILINE` option just controls whether `^` and `$` match only against the full string or against each line in it.)

One major example of an end-user application of regex searching i can think of is Sublime Text, whose search function *does* operate across lines by default, but, again, it does *not* enable the `DOTALL` behaviour.

Sounds like most of the people who've commented so far are in favour of defaulting to `DOTALL`, but idk, might be worth considering that it will be different from most other search tools. I personally don't feel super strongly either way, it's easy enough to change if i ever need that functionality.

Whichever way you decide, if you're worried about the UX, you could always offer an additional option in `rg` to control the default flags. Like a `--regex-modes=-s` or whatever that people could put in their alias/config.

---

_Comment by @BurntSushi on 2018-06-26 15:01_

A point of clarity: regex engines often expose both "multiline" flags and "dotall" flags. They do different but somewhat related things. Multiline flags typically impact the behavior of `^` and `$`. Namely, when multiline mode is disabled, `^` will only match at the beginning of a haystack where as if mulitline mode is enabled, then `^` can also match the position immediately following the line terminator. This proceeds analogously for `$`. (Regex engines also often expose `\A` and `\z` as variants of `^` and `$` that always behave as if multiline mode is disabled.)

As far as I know, most regex engines have both of these modes disabled by default. That is, `^/$` only match at the beginning/end of a haystack and `.` never matches `\n`.

Today, ripgrep always enables multiline mode for the regex engine (such that `^/$` maybe after/before line terminators). The question of whether `.` matches a line terminator is moot because ripgrep specifically forbids this.

In a world where "multiline search" is enabled (completely unrelated to the "mulitline mode" mentioned above for regexes), it's less clear what the defaults for these should be. I'm inclined to continue to keep multiline mode in the regex itself enabled by default. The dotall flag is where I'm kind of hung up.

@okdana A `--regex-modes` flag is interesting. I'd be sad if we had to resort to that, but yeah, I can kind of see that happening. (And sorry, I typed out most of this comment before I saw yours, so apologies for re-explaining some things!)

---

_Comment by @ssokolow on 2018-06-26 15:05_

I should probably also have clarified that I was contrasting the behaviour of PCRE, JavaScript, and Python's regex engines with the behaviour ripgrep currently imposes on Rust's regex engine rather than the bare capabilities of the `regex` crate.

---

_Comment by @cosmicexplorer on 2018-07-04 14:08_

If I'm understanding it correctly, having the dotall flag enabled by default would make something like `name1.*name2` potentially match two consecutive lines `name1\nname2`, as well as `name1name2`? I don't know if I'm missing a use case, but I find myself very often using `<pattern1>.*<pattern2>` when I use ripgrep from the command line, much more than I would expect to find `.` matching a newline as useful. I can see dotall being on by default as requiring a lot of typing to avoid accidentally matching something much too large across multiple lines (not a perf problem, but could cover up many intended matches in a single big match depending on greediness). Anecdotally, when using `isearch` in emacs interactively, I have found it tolerable to type out `\(.\|\n\)` to express a match across lines (because I'm far less often interested in matching that) and since ripgrep is PCRE (and searching more than one file at a time) that's 3 fewer backslashes, so it would be even more tolerable.

I also think the conjunction of multiline matching mode on by default, and dotall mode off by default, makes sense -- if you were to do `rg -l '^a.*b' | xargs sed -rie 's/^a.*b//g'` (which I've done), you currently can expect `.` to match similarly for `rg` and `sed`. I don't think it's necessary to follow the lead of other tools' interfaces if that's not the best, but I also think that conjunction of options is a highly sensible default, and getting that parity with other tools seems like a useful bonus, again unless that parity requires other compromises.

All of the above is clearly anecdotal, I just wanted to comment because I feel like I have (anecdotally) definitely run into the problem of accidentally matching too much when running interactive searches when `.` matches a newline (when I have set up my search flags to do that, in other search tools). I think either approach would work -- but I really do think the frequency of searches intended to span multiple lines will be a lot lower (and typing out `(?s:.)` will then be more tolerable), and for someone who's not familiar with the default, accidentally matching their whole file (in a pathological case) could be potentially very confusing, even if the reasoning is in the help page.

---

_Comment by @BurntSushi on 2018-07-04 14:16_

> `name1.*name2` potentially match two consecutive lines `name1\nname2`, as well as `name1name2`

That's correct, yes.

> I don't know if I'm missing a use case, but I find myself very often using `<pattern1>.*<pattern2>` when I use ripgrep from the command line, much more than I would expect to find multiline search useful.

Multi line search will emphatically be opt-in (as noted in my comments above) and not be enabled by default. Therefore, the question here is only about what the defaults are when multi line search is enabled. The defaults when multi line search is not enabled (which is itself ripgrep's default) will not change.

---

_Comment by @cosmicexplorer on 2018-07-04 14:17_

Yes, I'm sorry, I had just edited it to reflect that I meant "the ability to search across multiple lines with `.` alone", not "multi-line search".

---

_Comment by @BurntSushi on 2018-07-04 14:22_

@cosmicexplorer Does that change the rest of your calculus at all? That is, in order to use multi line search, you already need to explicitly opt into it, so the thinking I had is that if `.` matches `\n` by default in multi line search, then that would be OK since you're already explicitly opting into multi line search.

As a point of clarity, note that enabling multi line search itself by default for ripgrep proper is not up for discussion IMO. That will never happen.

---

_Comment by @cosmicexplorer on 2018-07-04 14:35_

> As a point of clarify, note that enabling multi line search itself by default for ripgrep proper is not up for discussion IMO. That will never happen.

Understood!

> ...if `.` matches `\n` by default in multi line search, then that would be OK since you're already explicitly opting into multi line search.

I was thinking about this and this logic is pretty reasonable, and could definitely justify the decision. I guess my concern is that if the default with multi-line search is that `.` matches `\n`, then I think the `^` and `$` are only going to be useful when you need to match the end of a line and have that also be the end of a string, which, if you're trying to match a pattern across multiple lines, seems like it becomes less likely to be what you want (especially since e.g. `$` in this case can be simulated anyway with `\n?\Z` I think?). That's of course unless the default pattern expands to allow `^`/`$` in the *middle* of patterns -- but that's not at all a serious suggestion.

---

_Comment by @BurntSushi on 2018-07-04 14:50_

@cosmicexplorer Hmm, that is an interesting one to noodle on. The problem is that the usefulness of `^/$` is a bit strange in general when multi line search is enabled. Namely, all we're discussing here are the semantics of `.`, but there are other commonly used character classes, such as `\s` or `[[:space:]]`, that will also be able to match through a `\n` when multi line search is enabled, which will make the interaction between `^/$` and `\s` similarly confusing as `^/$` with `(?s:.)`. But, `\s` matches much less than `.`, so the confusion may not manifest nearly as frequently.

I think we might just need to pick one here, and then probably provide a flag for enabling/disabling DOTALL semantics so that folks can pick their own default via an alias or ripgreprc. :-/

---

_Comment by @cosmicexplorer on 2018-07-04 14:57_

I will always advocate for "just picking one", and I definitely hadn't considered the potential dilemma for character classes besides `.` that you just mentioned -- especially in the cli context (which is what we're discussing). I think if all of this is behind a non-default flag anyway it does seem to make sense to not necessarily be super concerned about the default semantics of DOTALL behind a non-default flag. And thinking again about the "searching a few different patterns on the command line" context I can see `.` matching newline being pretty nifty (the example in the OP in particular).

---

_Comment by @joshtriplett on 2018-07-04 18:53_

For my part, I'd personally find it confusing and inconvenient if `.` matched newlines by default, even after opting into searches spanning lines, but that's primarily because no other regex engine does that.

However, there's an interesting related issue: negated character classes do include newlines even when `.` doesn't. Even with DOTALL disabled, regex engines normally let `[^x]` match *any* character other than x, including newline. This can *also* be confusing and inconvenient, but can also sometimes be exactly what you want.

I think I'd describe the distinction this way: sometimes when doing a multiline search, you still want to match something line-oriented *inside* that search, and even after opting into multiline search, when you write the line-oriented sub-search you might get surprised if the components of that sub-search match across lines.

So, I'd suggest the following:

- Separate the options to enable multiline and dotall.
- In addition to the in-regex options like `(?:m)`, have a command-line option that enables multiline for the whole regex, and another that enables dotall. (But perhaps have a single combined option for what seems to be a common case; for instance, `-m` for multiline and `-M` for multline-with-dotall.)
- Encourage people, when trying to match line-oriented things inside a multiline regex, to turn multiline and dotall *off* for the line-oriented match, to avoid surprises. (And perhaps have a convenient shortcut for turning both off at once.)

---

_Comment by @thijsvandien on 2018-07-05 00:38_

Coming back to the question whether there are other tools, I've been using `pcregrep -M` when I need multiline matching. There, a `.` does not match a `\n` automatically, whereas `\s` does. In fact, I wasn't able to find any option to change that.

---

_Comment by @BurntSushi on 2018-07-05 10:48_

@thijsvandien As mentioned above, most regex engines support inline flags to control `DOTALL` behavior (among other things). PCRE is no exception:

```
$ cat sherlock
For the Doctor Watsons of this world, as opposed to the Sherlock
Holmeses, success in the province of detective work must always
be, to a very large extent, the result of luck. Sherlock Holmes
can extract a clew from a wisp of straw or a flake of cigar ash;
but Doctor Watson has to have it taken out for him and dusted,
and exhibited clearly, with a label attached.

$ pcregrep 'Sherlock.Holmeses' sherlock
$ pcregrep -M 'Sherlock.Holmeses' sherlock
$ pcregrep -M 'Sherlock(?s:.)Holmeses' sherlock
For the Doctor Watsons of this world, as opposed to the Sherlock
Holmeses, success in the province of detective work must always
```

---

_Comment by @thijsvandien on 2018-07-05 13:47_

@BurntSushi Ah, thanks for the heads up. In any case, not the default. Therefore, at least in my experience, even when doing a multiline search, `DOTALL` should additionally be enabled when desired.

---

_Comment by @mateon1 on 2018-07-19 18:41_

I may be a bit late to comment on this issue, since the general consensus so far is to ignore the behavior  of existing regex engines and set DOTALL by default with multiline, but I'll say this anyways.

My personal experience with writing multiline regexes makes me expect `.` to NOT match newlines by default. In cases where I want to match anything I usually use `[\s\S]` or a similar construct.
I don't use DOTALL pretty much at all, since I work with many different regex engines, some of which don't support it (e.g. Javascript regexes), so I prefer to always use "portable" regex syntax.

If DOTALL-by-default ends up being implemented, I'd like it to be possible to disable it via a command-line flag, so it'd be possible to make a shell alias that disables DOTALL without an inline flag.

---

_Comment by @BurntSushi on 2018-07-21 01:01_

@mateon1 I'm currently leaning in the direction of not enabling `DOTALL` by default, such that `.` will not match new lines. My suspicion is that we'll want a flag to twiddle `DOTALL` either way, but I'll probably start without it.

---

_Comment by @Kroc on 2018-08-01 01:31_

I think PCRE supports `\A` & `\Z` for matching the start and end of input independent of `^` (start of line) & `$` end of line. I am personally in favour of including newlines by default, because it's mind-numbing to write a complex 'catch-all' pattern every time you want to skip over some white-space.

---

_Comment by @BurntSushi on 2018-08-01 01:34_

@Kroc I'm having a hard time understanding your comment. Have you read the conversation above? It sounds like you're confusing things, but I can't tell for sure. If you aren't sure how to untangle your comment, then I'd suggest providing some small examples.

---

_Comment by @BurntSushi on 2018-08-06 15:29_

All righty, so, the [`ag/libripgrep-freeze-1`](https://github.com/BurntSushi/ripgrep/tree/ag/libripgrep-freeze-1) branch has multiline powered ripgrep on it. It passes the integration test suite on Unix (I still have a bit of work to do for Windows), but if folks want to try it out, they can build it from source now. I won't force push to that branch (`ag/libripgrep` is my dev branch and i do frequently force push to it).

Right now, you get multiline search using the `--multiline` flag. The "dot all" option is not enabled by default, so you need to use `(?s)` to make it possible for `.` to match `\n` in your regex.

I've been using it a little bit myself, and here's my rough experience (with the added caveat that I still don't quite consider myself the target audience for this feature, so I don't really trust my experience yet):

* Nearly every time I've run a multiline search, I've used `.*?` as a way to match through multiple lines. As a result, I've basically always needed to add `(?s)` to my regex.
* The `--multiline` flag is kind of painful to type and too long. It feels like this flag is deserving of a short switch.
* I found myself "overshooting" my search quite a bit, where I would wind up with matches that spanned many more lines than I intended. It seems like a dual to `--max-columns` might be desirable here, such that matches longer than a particular number of lines are omitted.

With respect to finding a short switch to enable multiline mode, unfortunately, both `-m` and `-M` are taken. While ripgrep has generally reserved the right to make breaking changes in accordance with semver, I am in practice pretty conservative about it, and would rather not re-purpose existing flags. Therefore, if we want a short switch for multiline mode, then we'll need to pick from the available switches. Here are the switches already in use:

```
0 a A b B c C e E f F g h H
i j l L m M n N o p q r s S
t T u v V w x z
```

Alphabetic uppercase switches available:

```
D G I J K O P Q R U W X Y Z
```

Alphabetic lowercase switches available:

```
d k y
```

Wow. That's a little horrifying to look at! We're almost out of real estate.

---

_Comment by @BurntSushi on 2018-08-06 15:42_

Some random thoughts on switches to get the ball rolling, in particular, looking at how these switches are used by other tools:

* `-d`/`-D` are used in GNU grep to control the behavior when a "directory"/"device" is encountered. It's conceivable ripgrep could support this one day, but I doubt it deserves a short switch.
* `-G` is used in GNU grep to indicate the use of "basic" regexes, which ripgrep will never support.
* `-I` is related to changing how binary detection. Probably doesn't deserve a short switch.
* `-J` isn't used by GNU grep at all.
* `-K` isn't used by GNU grep at all.
* `-O` isn't used by GNU grep at all.
* `-P` is out. I have another use in mind for this.
* `-Q` is commonly used as "literal" search in other tools (ack, ag).
* `-R` means "recursive search, but follow symlinks," which doesn't really make sense in ripgrep. Plus, we already have `-L/--follow` for following symlinks.
* `-U`, like `-I`, is also related to binary detection. Probably also doesn't deserve a short switch. This is also the second letter in `multiline`. `-U` is also used in the silver searcher to relax its filtering, but ripgrep generally uses `-u` for that. Hmm.
* `-W` isn't used by GNU grep at all.
* `-X` isn't used by GNU grep at all.
* `-Y` isn't used by GNU grep at all.
* `-Z` is used in GNU grep, but ripgrep uses `-0/--null` for the same functionality.

`-k` isn't used by GNU grep, although it is used in `ack`, where I believe the ripgrep equivalent would be `-tall`.

`-y` appears to be an obsolete synonym for `-i` in GNU grep. TIL. Neat.

`ag` itself uses the `--multiline` flag, but of course, multiline is the default there.

I'm currently liking `-U` the best, but the choices aren't great.

---

_Comment by @okdana on 2018-08-06 15:49_

You could use `-1` (the digit), as a slightly oblique reference to 'single-line' (hence `(?s)`) mode

Edit: Oh, i misread; i thought you said `(?s)` was enabled by default. Maybe that doesn't make sense then. idk, `-U` seems fine

---

_Comment by @d-akara on 2018-08-06 16:06_

So what I've done in the past is to limit lines within the regex itself using something like
`line(.*\n.*){0,2}word`

Where I'm looking for `line` and `word` within 2 lines of each other

---

_Comment by @roblourens on 2018-08-07 18:40_

I played with this a bit and it works great! With the current output format, vscode won't be able to use it because it can't distinguish between one match across two lines, and two one-line matches. But once we have the parseable output which seems to also be in progress, we will be ready to use it!

re: short switch, I like your reasoning for `-U`, I can't come up with a better justification for any of the other letters. `-Y` for multyline, `-K` for search akross lines...

---

_Comment by @thijsvandien on 2018-08-07 18:51_

Although I much appreciate backward compatibility, personally I'd say that the ability for multiline searches is in itself a big enough change (heck, it was something `rg` would never do), that it would justify giving it its well deserved `-M`.

---

_Comment by @BurntSushi on 2018-08-07 19:45_

@roblourens Thank you so much for trying it out! I will hook up the JSON output format soon and update this thread when that's done.

@thijsvandien I'd like to avoid the backward compatibility discussion if possible please. While I have no concrete way of knowing this, my feeling from talking to various people about how they use ripgrep is that `-m` and `-M` are not infrequently used. `-M` in particular was added almost a year and a half ago and `-m` was added before even that. Reversing course on that kind of thing is not good for users. **The hardest part about backwards compatibility is coming to terms with the fact that your software will have warts.** If I could do it all again, I would have found a different short flag for `--max-columns` and used `-M` for multiline. Alas, we shall live with the wart. It is not the first and will most certainly not be the last.

---

_Comment by @thijsvandien on 2018-08-07 19:54_

Alright then. From the remaining options I like `-U` (for reasons given) and `-X` (for "a cross") the best.

---

_Comment by @BurntSushi on 2018-08-07 20:02_

I'm probably going to regret this, but let's see if Twitter can be useful for something: https://twitter.com/burntsushi5/status/1026921457237078019

---

_Comment by @waldyrious on 2018-08-07 21:53_

> twitter.com/burntsushi5/status/1026921457237078019

The `-X` for "a cross", mentioned in the tweet, actually has a neat alternative (and more direct) interpretation, considering that `x` is the informal symbol for multiplication ("multiple lines", 3x, etc.).

---

_Comment by @gibfahn on 2018-08-08 09:24_

>Nearly every time I've run a multiline search, I've used .*? as a way to match through multiple lines. As a result, I've basically always needed to add (?s) to my regex.

Am I right in saying that if `DOTALL` is on by default, you can just use `[^\n]` instead of `.` to not use it, whereas if `DOTALL` is off by default, you can use `[.\n]` to get the same result?

---

_Comment by @BurntSushi on 2018-08-08 09:30_

Yes, I believe that's correct. 

---

_Comment by @okdana on 2018-08-08 09:40_

`[.\n]` would match either a literal `.` or a new-line; if you wanted to match any character across lines in a pattern with `DOTALL` disabled by default i think you'd probably want to use either `[\s\S]` or `(?s:.)`

---

_Comment by @BurntSushi on 2018-08-08 09:47_

Oof. Thanks @okdana. I shouldn't be responding to email right after I wake up. :)

---

_Comment by @thijsvandien on 2018-08-08 12:01_

If we continue thinking metaphorically, `-Z` also makes sense, because you're searching in a (big) Z-pattern then (reach the end of one line, go to the beginning of the next, and on, and on).

---

_Comment by @lnicola on 2018-08-08 12:18_

How about `-2`, because you're looking for more than one thing?

Anyway, it probably doesn't matter.

---

_Comment by @BurntSushi on 2018-08-08 13:44_

Another way to utter `(?s:.)` is `\p{any}` (new as of ripgrep 0.9.0 IIRC), although I'm not sure that it is actually better.

---

_Comment by @BurntSushi on 2018-08-08 20:41_

And Twitter says... Both `-U` and `-X`! Hah. A tie. I'm just going to go with `-U`, which is what I was originally leaning towards anyway.

---

_Comment by @thijsvandien on 2018-08-08 21:54_

Being the one who suggested `-X`, I am happy to accept `-U` as the outcome.

---

_Comment by @BurntSushi on 2018-08-16 16:07_

All righty, here is what I have for docs for multiline mode. Do folks mind giving them a quick skim to make sure I haven't missed anything? Are there any obvious unanswered questions that the docs could cover?

```
-U, --multiline
    Enable matching across multiple lines.

    When multiline mode is enabled, ripgrep will lift the restriction that a match
    cannot include a line terminator. For example, when multiline mode is not
    codepoint other than \n. Similarly, the regex \n is explicitly forbidden, and if
    you try to use it, ripgrep will return an error. However, when multiline and
    regexes like \n are permitted.

    An important caveat here is that multiline mode does not change the match
    semantics of .. Namely, in most regex matchers, a .  will by default match any
    character other than \n, and this is true in ripgrep as well. In order to make .
    match \n, you must enable the "dot all" flag inside the regex. For example, both
    (?s).  and (?s:.)  have the same semantics, where .  will match any character,
    including \n. Alternatively, the --multiline-dotall flag may be passed to make
    the "dot all" behavior the default. This flag only applies when mulitline search
    is enabled.

    There is no limit on the number of the lines that a single match can span.

    WARNING: Because of how the underlying regex engine works, multiline searches may
    be slower than normal line oriented searches, and they may also use more memory.
    In particular, when multiline mode is enabled, ripgrep requires that each file it
    searches appear as if it exists contiguously in memory (either by reading it on
    to the heap or memory mapping it). Things that cannot be memory mapped (such as
    stdin) will be consumed until EOF before searching can begin. In general, ripgrep
    will only do these things when necessary. That is, even if you use the
    --multiline flag but your regex cannot match over multiple lines, then ripgrep
    won’t consume unnecessary resources. Nevertheless, if you only care about matches
    spanning at most one line, then it is always better to disable multiline mode.

    This flag can be disabled with --no-multiline.

--multiline-dotall
    This flag causes .  to match new lines when multiline searching is enabled. This
    flag has no effect if multiline searching isn’t enabled.

    Normally, a .  will match any character except for newlines. While this behavior
    typically isn’t relevant for line oriented matching (since matches can span at
    most one line), this can be useful when searching with the -U/--multiline flag.
```

---

_Comment by @roblourens on 2018-08-16 17:25_

```
For example, when multiline mode is not
codepoint other than \n.

...

However, when multiline and
regexes like \n are permitted.
```

Are those sentences missing a word or something?

---

_Comment by @BurntSushi on 2018-08-16 17:29_

Weird. Looks like I botched the copy & paste! Let's try again. Here's `-U/--multiline`:

```
Enable matching across multiple lines.

When multiline mode is enabled, ripgrep will lift the restriction that a match
cannot include a line terminator. For example, when multiline mode is not
enabled (the default), then the regex '\\p{any}' will match any Unicode
codepoint other than '\\n'. Similarly, the regex '\\n' is explicitly forbidden,
and if you try to use it, ripgrep will return an error. However, when multiline
mode is enabled, '\\p{any}' will match any Unicode codepoint including '\\n'
and regexes like '\\n' are permitted.

An important caveat here is that multiline mode does not change the match
semantics of '.'. Namely, in most regex matchers, a '.' will by default match
any character other than '\\n', and this is true in ripgrep as well. In order
to make '.' match '\\n', you must enable the \"dot all\" flag inside the regex.
For example, both '(?s).' and '(?s:.)' have the same semantics, where '.' will
match any character, including '\\n'. Alternatively, the '--multiline-dotall'
flag may be passed to make the \"dot all\" behavior the default. This flag only
applies when mulitline search is enabled.

There is no limit on the number of the lines that a single match can span.

**WARNING**: Because of how the underlying regex engine works, multiline
searches may be slower than normal line oriented searches, and they may also
use more memory. In particular, when multiline mode is enabled, ripgrep
requires that each file it searches appear as if it exists contiguously in
memory (either by reading it on to the heap or by memory mapping it). Things
that cannot be memory mapped (such as stdin) will be consumed until EOF before
searching can begin. In general, ripgrep will only do these things when
necessary. That is, even if you use the --multiline flag but your regex cannot
match over multiple lines, then ripgrep won't consume unnecessary resources.
Nevertheless, if you only care about matches spanning at most one line, then it
is always better to disable multiline mode.

This flag can be disabled with --no-multiline.
```

And `--multiline-dotall`:

```
This flag causes '.' to match new lines when multiline searching is enabled.

This flag has no effect if multiline searching isn't enabled.

Normally, a '.' will match any character except for newlines. While this
behavior typically isn't relevant for line oriented matching (since matches
can span at most one line), this can be useful when searching with the
-U/--multiline flag.
```

---

_Comment by @waldyrious on 2018-08-16 17:51_

As an occasional regex user, I think this is pretty clear. Some minor stylistic suggestions:

> cannot include a line terminator. For example, when multiline mode is not

Instead of "For example", I'd use "In particular", "namely", "specifically", or some other equivalent expression.

> However, when multiline
mode is enabled, '\\p{any}' will match any Unicode codepoint including '\\n'
and regexes like '\\n' are permitted.

I'd use commas around "including '\\n'", which IMO makes the sentence structure (and intended reading flow) slightly more explicit.

> An important caveat here

I don't think the "here" is necessary. Or in other words, IMO it is not specific enough to be useful.

> applies when mulitline search is enabled.

Typo: "mulitline" --> "multiline".

> slower than normal line oriented searches

I'd hyphenate "line oriented searches" --> "line-oriented searches".

> either by reading it on to the heap

"onto"?

> or by memory mapping it). Things that cannot be memory mapped

Suggestion: "memory-mapping" and "memory-mapped".

> even if you use the --multiline flag but your regex cannot
match over multiple lines, then ripgrep won't consume unnecessary resources.

I'm not sure this sentence's structure conveys the intended message clearly. Do you think you could rephrase it somehow? I think what's confusing me is the "even if" / "but" / "then" structure.

> any character except for newlines.

"any character except newlines." -- simpler and has the same meaning.

> line oriented matching

"line-oriented matching", as suggested for similar expressions above.

> this can be useful when searching with the
-U/--multiline flag.

Just for completeness, I'd add a note at the end explicitly mentioning that multiline mode by default assumes a false dotall flag.

---

_Comment by @BurntSushi on 2018-08-16 18:03_

@waldyrious Awesome! I think I took all of your suggestions except for the first. Here's paragraph containing the portion you requested to have rewritten. What do you think?

```
**WARNING**: Because of how the underlying regex engine works, multiline
searches may be slower than normal line-oriented searches, and they may also
use more memory. In particular, when multiline mode is enabled, ripgrep
requires that each file it searches appear as if it exists contiguously in
memory (either by reading it onto the heap or by memory-mapping it). Things
that cannot be memory-mapped (such as stdin) will be consumed until EOF before
searching can begin. In general, ripgrep will only do these things when
necessary. Specifically, if the --multiline flag is provided by the regex
cannot match over multiple lines, then ripgrep won't read each file into memory
before searching it. Nevertheless, if you only care about matches spanning at
most one line, then it is always better to disable multiline mode.
```

---

_Comment by @thijsvandien on 2018-08-16 23:39_

Sorry to bring this topic up again after it was closed. This is the first time I hear about `--multiline-dotall`. If we’re going to have that “stronger version”, wouldn’t it make sense to use a short switch that is available both in lower case (`--multiline`) and upper case (`--multiline-dotall`), like `-z` and `-Z`? That would be a good reason to have a different switch than most other tools, because none offer both options, as far as I’m aware.

---

_Comment by @BurntSushi on 2018-08-16 23:44_

@thijsvandien Nah. The `--multiline-dotall` flag is probably intended to be something that goes in your config file (or an alias) as something that's always set, if those are the semantics you prefer by default.

---

_Comment by @thijsvandien on 2018-08-17 00:37_

Ah, I see now that it is meant as a modifier rather than an alternative with different semantics.

---

_Comment by @mateon1 on 2018-08-17 03:00_

Some nits:
```
This flag causes '.' to match new lines
```
Should be `newlines` for consistency reasons
```
requires that each file it searches appear as if it exists contiguously in
```
appear**s**, but perhaps this section could be worded differently.
Maybe: `... ripgrep requires that the searched file is laid out/mapped/allocated contiguously in memory`
I'm unsure which wording is the best (I prefer `laid out`, but maybe that's not appropriate for documentation), but all three sound better to me than the existing version.
```
Specifically, if the --multiline flag is provided by the regex
cannot match over multiple lines
```
`s/by/but/`

---

_Comment by @waldyrious on 2018-08-17 08:59_

@BurntSushi I'm glad you agree with the suggestions! The reworded sentence is indeed much clearer, after fixing the typo pointed out by @mateon1.

Here's the diff of that sentence, for future reference/convenience:

```diff
-That is, even if you use the --multiline flag but your regex cannot
-match over multiple lines, then ripgrep won't consume unnecessary resources.
+Specifically, if the --multiline flag is provided but the regex
+cannot match over multiple lines, then ripgrep won't read each file into memory
+before searching it.
```

Now that I re-read that, I'm not sure "cannot match" is the best choice of words, since it can imply both a neutral statement or an imperative enforcement. (Not sure I'm being clear myself; let me know if I should rephrase!)

I suppose you're referring to the case where the regex does not contain any patterns that would match newlines, or it contains `.` without the dotall flag being activated. Is that correct?

---

_Comment by @BurntSushi on 2018-08-17 10:54_

@mateon1 Thanks! I took your advice, and chose "laid out."

@waldyrious 

> I suppose you're referring to the case where the regex does not contain any patterns that would match newlines, or it contains . without the dotall flag being activated. Is that correct?

Yes. Whether dotall is enabled or not is mostly orthogonal; what matters is whether a `\n` exists in any of the possible matches of a regex. Enabling dotall and uttering `.` is one way to achieve that, but a literal `\n`, `\s`, `\p{any}` and so on also achieve that.

It is possible I should just remove this part of the docs. I'm not sure. I put it there as a way of saying that even if you enable multiline mode but don't make use it, you generally won't pay (much) for it. But maybe that's not that important.

---

_Comment by @waldyrious on 2018-08-17 14:07_

I think it wouldn't be a problem if it were removed, but it is useful information so I'd have a slight preference to keep it.

IMO changing that sentence to something like this:

> "Specifically, if the --multiline flag is provided, but the regex ~~cannot match over multiple lines~~ *does not contain patterns that would match \n characters*, then ripgrep ~~won't read~~ *will automatically avoid reading* each file into memory before searching it."

...would make it sufficiently unambiguous.

---

_Comment by @BurntSushi on 2018-08-17 14:19_

@waldyrious I like it. Much better. Thanks! :)

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

_Comment by @myfairsyer on 2018-08-23 21:25_

Will `\n` only match `\n` / `0x0A` or any common single line break (`\r?\n`) (or if you take the classic MacOS and BBC into account [`((\n\r?)|(\r\n?))`](https://regex101.com/r/T5dkAH/1/))

(I do know that both styles exist among regex engines but couldn't tell which is which)

Sry if there is an answer to that somewhere.

---

_Comment by @BurntSushi on 2018-08-23 21:52_

`\n` only matches `\n`.

Current master has a `--crlf` option that causes `$` to match `\r\n` line breaks in addition to `\n`.

I'm not aware of any regex engines that permit a literal `\n` to match `\r\n`. Some regex engines certainly allow for a looser definition of what "line terminator" actually means when necessary, e.g., when matching the `^` or `$` anchors. If you know of a regex engine that permits a literal `\n` to match `\r\n` then I'd like to have a link to that so I can investigate!

---

_Comment by @roblourens on 2018-08-23 23:36_

VS Code matches `\r\n` on `\n` when ctrl+f searching in a single file, it's useful in an editor but I wouldn't use that as inspiration for ripgrep.

---

_Comment by @BurntSushi on 2018-08-23 23:43_

Oh interesting. Is that something VS code layers on, or is it part of JS
regexes?

On Thu, Aug 23, 2018, 19:36 Rob Lourens <notifications@github.com> wrote:

> VS Code matches \r\n on \n when ctrl+f searching in a single file, it's
> useful in an editor but I wouldn't use that as inspiration.
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/176#issuecomment-415606098>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34nyP5Q-P7DWfp_i4QLvDJ-p0RCLDks5uTzx3gaJpZM4KWZCK>
> .
>


---

_Comment by @roblourens on 2018-08-23 23:46_

No, it's just something vscode does.

---

_Comment by @myfairsyer on 2018-08-24 06:38_

> If you know of a regex engine that permits a literal \n to match \r\n then I'd like to have a link to that so I can investigate!

@BurntSushi Most probably I only encountered it in text editors like VSCode.

> it's useful in an editor but I wouldn't use that as inspiration for ripgrep.

@roblourens Would you mind to elaborate?
And does that mean that VSCode will behave differently inside an editor and when searching across files?

---

_Comment by @roblourens on 2018-08-24 15:18_

Personally I don't prefer "magic" like that, but yeah I'll have to see whether we can rewrite `\n` to `\r?\n` so that search across files works the same as search inside files.

---

_Comment by @myfairsyer on 2018-08-24 16:11_

> it's useful in an editor but I wouldn't use that as inspiration for ripgrep.

> @roblourens Would you mind to elaborate?

> Personally I don't prefer "magic" like that

@roblourens 
I was rather driving at the distinction between text editor and ripgrep.
I couldn't quite follow.
Is it b/c you consider ripgrep as a command line tool having a more advanced audience which demands more control and less _magic_ than a graphical text editor?

@BurntSushi 
I don't want to derail or hijack this therad for irrelevant discussions.
You said you'd like to know more and investigate and found VSCode's behavior interesting.
If you don't anymore tell me.

---

_Comment by @wmww on 2018-10-31 20:41_

Currently, if you try to make a multiline search without the `-U/--multiline` option, ripgrep errors with `the literal '"\n"' is not allowed in a regex`. Would it make sense to mention the existence of a multiline enabling option here?

---

_Comment by @BurntSushi on 2018-10-31 21:08_

@wmww That should already be done on master. See: https://github.com/BurntSushi/ripgrep/issues/1055

Also, please file new issues for new requests.

---

_Comment by @unphased on 2020-11-25 22:33_

~Hi, I'm curious if there is a way to make multiline dot non-greedy? I tried `(?s).*?` and `.*?` under `--multiline-dotall` and neither worked. It seems with multiline mode, the `.*?` fails to become non-greedy. Is there an underlying reason for this?~

~`[^>]` and such work under multiline mode, though, which is the less general way to do sort of non-greedy stuff.~

---

_Comment by @BurntSushi on 2020-11-25 22:39_

Please file a new issue. And please don't use phrases like "does not work" without actually _showing_ what you mean by it. Please fill out the complete bug template.

---

_Comment by @unphased on 2020-11-26 00:22_

You're right, sorry for trying to resurrect and derail an old issue. I did more testing on this and I think I neglected to consider something with my test. it seems all to work as expected. 

---

_Comment by @amosbird on 2021-12-01 04:10_

Do we have an option to limit the max number of lines each match can have?

---

_Comment by @BurntSushi on 2021-12-01 13:07_

No. And I don't see any obvious way to implement that either. You can usually build such limits into your regex instead.

---

_Comment by @amosbird on 2021-12-01 13:13_

> No. And I don't see any obvious way to implement that either. You can usually build such limits into your regex instead.

Can we build a regex to match the following?  `foo.*[at most three new lines]bar.*[at most three new lines]baz.*` (all together at most three new lines)

---

_Comment by @BurntSushi on 2021-12-01 13:39_

@amosbird Sure? Why not? `foo(.*\n?){0,3}bar(.*\n?){0,3}`, or something like that anyway.

In the future, I'd really prefer you open new tickets for support questions. Bumping old issues doesn't make these discussions easy to find. There is even a [Q&A forum](https://github.com/BurntSushi/ripgrep/discussions) designed for this purpose. Please use it.

---

_Comment by @amosbird on 2021-12-01 14:11_

> In the future, I'd really prefer you open new tickets for support questions. Bumping old issues doesn't make these discussions easy to find. There is even a Q&A forum designed for this purpose. Please use it.

Sure. Will continue the discussion in the Q&A forum.

---
