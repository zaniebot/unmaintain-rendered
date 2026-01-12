```yaml
number: 673
title: Why was .rgignore deprecated in favor of .ignore?
type: issue
state: closed
author: rlue
labels: []
assignees: []
created_at: 2017-11-11T15:21:03Z
updated_at: 2017-11-11T16:47:35Z
url: https://github.com/BurntSushi/ripgrep/issues/673
synced_at: 2026-01-12T16:13:22Z
```

# Why was .rgignore deprecated in favor of .ignore?

---

_@rlue_

Without presuming to ask for a reversal of this deprecation, I'd like to get some perspective on why `.rgignore` was abandoned in favor of `.ignore`. There are many programs that can be configured with ignore files — git, syncthing, and GNU stow all come to mind. All of these programs follow a practice of namespacing configuration files to avoid potential collisions (`.gitignore`, `.stignore`, and `.stow-local-ignore`, respectively) — in fact, I have a directory that contains all three (my `~/.config` dir). 

I understand that both _ag_ and _rg_ previously supported separate `.agignore` and `.rgignore` files, and I'm also under the impression that both have been deprecated in favor of a unified `.ignore` file. In a [currently-open issue with _fd_](https://github.com/sharkdp/fd/issues/156)_,_ a question arose as to whether it made sense for the program to honor `.ignore` by default. I don't think it does, as the use cases for the programs are different, but I also think that the fact that `.ignore` actually belongs to _rg_/_ag_ (which cannot be inferred or intuited from the name of the file itself) obscures the nature of the question. 

So if I may ask, why have the developers of _rg_ and _ag_ chosen to designate a common configuration file which, in a manner of speaking, pollutes the global config-file namespace? and would you consider switching back to a namespaced alternative, like `.grepignore`? 

(And either way, might you also consider supporting a _non-hidden_ XDG-compatible ignore file — _i.e.,_ `~/.config/ignore`, rather than `~/.config/.ignore`?)

---

_Comment by @BurntSushi on 2017-11-11 15:33_

> I'd like to get some perspective on why `.rgignore` was abandoned in favor of `.ignore`

It's simple: so that multiple tools can use the same set of ignore rules. The change was made here: #41 and the conversation that sparked it is here: https://news.ycombinator.com/item?id=12567328

I have *zero* interest in discussing the particular choice of `.ignore` further. I think that conversation covers the motivation, and my intent is to *never* remove support for it. Therefore, it doesn't make sense to dig into that any more than we already have. (Sorry if I'm coming across as curt here, but I have so little time and the last thing I want to do is spend it re-litigating things, especially on things that aren't going to change because it will **break end user uses of ripgrep**. I also don't want to invite the maintenance and confusion overhead of a deprecation, so that's off the table as well.)

---

With that said, I am open to potentially supporting something like a `.grepignore` that has higher precedence than `.ignore` (which means rules in `.grepignore` would override `.ignore`, just like `.ignore` overrides `.gitignore`). We could implement this by making the name configurable at the level of the `ignore` crate, which should let ripgrep use `.grepignore` and tools like `fd` use `.fdignore` (for example), while also continuing to support `.ignore`.

Am I understanding the issue here correctly? Do you agree that the above would solve this specific problem?

So to make this happen, I think these are the steps:

1. Decide whether we should actually do this. Adding an extra ignore-like file means an additional stat call for every directory we search. We should attempt to carefully benchmark this and make sure we're OK with the performance. ripgrep does use a parallel iterator by default, so I'm hoping this is negligible.
2. Decide on a name for the file. Every name I've ever come up with has problems, including `.ignore` and `.grepignore`.
3. Actually implement it in the `ignore` crate.

---

_Comment by @BurntSushi on 2017-11-11 15:36_

> (And either way, might you also consider supporting a non-hidden XDG-compatible ignore file — i.e., `~/.config/ignore`, rather than `~/.config/.ignore`?)

You can simulate this today with the `--ignore-file` flag, and embed that into an alias. When ripgrep gets persistent config files (which will likely use XDG), then you can add `--ignore-file` flags to that config.

(If you think that even given the above we should still have a separate XDG ignore file, please file a separate issue.)

---

_Comment by @rlue on 2017-11-11 15:59_

> Sorry if I'm coming across as curt here

Not at all. The issue has clearly been discussed at length, and I missed the memo. Thanks very much for taking the time to point me over to it. 

> Am I understanding the issue here correctly? Do you agree that the above would solve this specific problem?

I am beginning to appreciate the volume of opinion and decisionmaking that has gone into getting things the way they are thus far, so rather than rocking the boat with more undocumented functionality, I might restate the issue as a request for better documentation (both in the README and the manpage).

Specifically, I think a section clarifying 1) the usage of the `.ignore` file, and 2) a statement of its intended/proposed scope would go a long way in preempting questions like mine. 

I'd offer to submit a PR for these additions to the docs myself, but clearly, I have no idea what I'm talking about. Nevertheless, if you'd like me to take a stab at it (as I'm sure you have bigger fish to fry), I'll gladly oblige.

---

_Closed by @rlue on 2017-11-11 15:59_

---

_Comment by @BurntSushi on 2017-11-11 16:12_

Hmm but it sounds like the cause of this issue is an actual problem that might be worth solving. Separate from how we got here, do you still see a use case for more specific ignore files or does .ignore on its own solve the problem and I'm misunderstanding?

---

_Comment by @rlue on 2017-11-11 16:44_

The aforementioned issue on _fd_ was simply that the @sharkdp was trying to decide whether it made sense to honor `.ignore`. The issue I raised here is that the name `.ignore` made that decision harder than it should be — _find-like_ ignore and _grep-like_ ignore are definitely distinct use cases, and IMO they simply shouldn't be lumped together by default.

(And if you concocted, in a single file, a way to specify lines that applied to grep and lines that applied to find and lines that applied to both, my opinion is that it would unduly increase the documentation burden and violate the principle of least surprise.)

_fd's_ problems can be reasonably addressed by simply disregarding `.ignore` altogether (or honoring both `.ignore` and `.fdignore` but allowing the latter to override the former, if it's present). I don't know if there is a great need among find-like utilities to share a unified ignore-file (the way that ag, ripgrep, and sift do), but until the authors of those utilities come knocking on your doors to work together on a shared config format, I think it's fine to let this sleeping dog lie.

(I definitely still advocate for documentation on `.ignore`, though!)

---
