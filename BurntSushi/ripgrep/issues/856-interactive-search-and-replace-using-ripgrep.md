```yaml
number: 856
title: Interactive search and replace using ripgrep crates
type: issue
state: closed
author: felipesere
labels:
  - question
assignees: []
created_at: 2018-03-12T23:47:28Z
updated_at: 2018-04-20T07:19:54Z
url: https://github.com/BurntSushi/ripgrep/issues/856
synced_at: 2026-01-12T16:13:22Z
```

# Interactive search and replace using ripgrep crates

---

_@felipesere_

I am thinking of writing a rust tool that allows me to search and replace terms (simple ones at least) interactively across a tree a files.

When I say interactive, I mean in the same way `git add -p` allows you decide whether to add (or skip, or quit etc) each hunk individually.

These are the options git offers:
<img width="509" alt="screen shot 2018-03-12 at 23 41 24" src="https://user-images.githubusercontent.com/1850188/37315057-7fb23700-264f-11e8-8156-e3d77d8664e3.png">
  
I am wondering if I could use some the brilliant crates you have built :)

---

_Comment by @BurntSushi on 2018-03-13 00:08_

Maybe? The question you're asking is *really* broad, and there are probably a billion and one ways of solving it. Can you add more details? Could you also please peruse the crates docs? That might help give more answers or lead you to more specific questions.

---

_Label `question` added by @BurntSushi on 2018-03-13 02:19_

---

_Comment by @felipesere on 2018-03-13 13:35_

Maybe its better if I describe what I would like to be able to do... and how I think it could be setup? I was a little intimidated by your brilliant blog post explaining ripgrep 😺 

Given a folder containing the source code for some project, I'd like to run
```
riplace foo bar
```
And it would look for _all_ instances of `foo` and ask me if I want to replace it with `bar` _in place_.
The way it would ask me would be one by one, allowing me to say "yes, yes, yes, ...no not that one, yes to that one, no, yes, yes, no" .

The difficulty I see running the search "interactively", meaning riplace should stop when it finds a hit and then allow me to change the file in the precise location.

To be really similar to git, it would also be nice to be able to skip/"leave undecided" individual replacements but also entire files. That would be like saying "Actually skip any hit you find in this file from here forward".

I assume I'd need the regex, grep, glob, and walkdir crates?

---

_Comment by @BurntSushi on 2018-03-13 18:55_

@felipesere Thanks for writing that out more clearly, I think I have a better idea of what you're trying to do.

Fist, I can say some things with near certainty:

* If you need to do regex search, then yeah, you'll definitely want the `regex` crate.
* Since you want to recursively traverse a directory, then `walkdir` is definitely the crate to use for that. **However**, if you want to copy ripgrep's approach of respecting `.gitignore` and the like, then you should probably use the `ignore` crate, which comes with its own directory walker (built on `walkdir`) that automatically filters results based on `.gitignore`. There's also a parallel walker too, although I imagine it would be simpler to use the single threaded walker when combining that with interactive replace.
* The `globset` crate (**not** `glob`) is probably not something you need to use directly unless you want to roll your own filtering and avoid using the `ignore` crate.
* The `grep` crate might help you, but be warned, it will be rewritten at some point and might not help you as much as you hope. It depends. If you only use memory maps to search files, then the `grep` crate might work OK for you. You might consider trying to implement your program without the `grep` crate first to get a better handle on what you need.

A key problem you need to solve is the actual file mutation itself. You need to make sure you *never* corrupt user's files, for example. Usually this is done by writing to a temporary file first, and then doing an atomic `mv tmp-file real-file`. ripgrep and its code won't really help you with this part, since it explicitly will never mutate your files.

I'm happy to continue answering questions!

---

_Comment by @felipesere on 2018-03-14 16:33_

My current thinking is to split "searching for and collecting hits" from "modifying the file".
I see though, that the `grep` crates gives me the line that matched but not the line number. :(

---

_Comment by @BurntSushi on 2018-03-14 18:03_

> I see though, that the grep crates gives me the line that matched but not the line number. :(

Right. As I said, if you're using memory maps or are OK loading the entire file into memory, then it is pretty easy to track line numbers yourself.

The `grep` crate is pretty low level. That makes it flexible, but yes, if you want to build the tool you have in mind, you will need to get your hands dirty.

At some point, `grep` will become more like "use ripgrep as a library," but that is a long way off.

---

_Comment by @felipesere on 2018-03-25 11:11_

I looked through `grep` but I couldn't figure out how to track the line number of any matches. 
If I interpret it correctly, it searches across a `[u8]`, meaning I'd have to look for `\n` myself? (assuming linux/mac for a second).

Actually, I've read the source some more. Is the following intuition right?
Grep will search in a buffer and return byte-offsets to the start and end,
the streaming_search then takes the `start_offset` and uses `bytecount:count(.., eol)` to look for end-of-lines _up to_ start_offset? It optimises the counting by remembering the count so far and narrowing down the slice for `bytecount:count` down to `last_line` to `upto`?

---

_Comment by @BurntSushi on 2018-03-25 11:37_

Yes, you need to teach them yourself. Like I said, grep is a low level building block right now, and tracking lines has costs associated with it.

I'd suggest looking at ripgrep's memory map searcher as the simplest use of the grep crate that tracks line numbers. It is in `src/search_buffer.rs`.

---

_Comment by @felipesere on 2018-04-20 07:19_

This has been solved by facebooks `fastmod` https://github.com/facebookincubator/fastmod

---

_Closed by @felipesere on 2018-04-20 07:19_

---
