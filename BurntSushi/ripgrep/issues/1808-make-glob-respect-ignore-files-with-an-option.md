```yaml
number: 1808
title: Make --glob respect ignore files with an option (feature request)
type: issue
state: open
author: ilyagr
labels:
  - question
assignees: []
created_at: 2021-02-28T00:08:57Z
updated_at: 2021-03-21T04:31:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1808
synced_at: 2026-01-12T16:13:24Z
```

# Make --glob respect ignore files with an option (feature request)

---

_@ilyagr_

I think the `--glob` and `--type` options would be more useful if they respected ignored files. If possible, I would like there to be an option to make them behave that way. 

Currently, `rg --glob '*text*'` is equivalent to `rg --glob '*text*' -uuu` in every way (as far as I can tell).  This is annoying, for example, in the following example (simplified a little from what I encountered):

```bash
$ ls -a
./  ../  .git/  .gitignore  README-debian.txt  README.md  README.md~
$ cat .gitignore
*~
$ rg --files  # Looks good
README.md
README-debian.txt
$ rg --files --glob 'README*'
README.md
README.md~
README-debian.txt
```

I'd like there to be an option `--weakglobs` that would make inclusive globs (see below) and `--type` have a lesser priority than ignore files. I would put it into my config file. It would work as follows:

```bash
$ rg --weakglobs --files --glob 'README*' 
README.md
README-debian.txt
$ rg --weakglobs --files --glob 'README*' -u
README.md
README.md~
README-debian.txt
```

I think this behavior is strictly more capable than the current behavior, since `rg --weakglobs --files --glob 'README*' -uuu` is equivalent to `rg  --files --glob 'README*'`. 

There is a caveat: the excluding globs (the ones with `!`) already work as they should (for instance, `-u` flags are not useless in the presence of `-g !something`). Their behavior should not change. In other words, they should continue to have priority over ignore files: if a gitignore file has a line like `!file`, it should still be possible to exclude it from ripgrep with `-g !file`.

So, the documentation would be something like:

> `--weakglobs`
>
>Make searches with `--glob` and `--type` skip hidden files and respect all of the ignore logic. This has no effect on globs that start with `!`. Ignore logic can still be partially or fully disabled with `-u` and similar flags.

P.S. Thank you for such a wonderful piece of software!

---

_Comment by @ilyagr on 2021-02-28 07:42_

If you can see yourself accepting a pull request for this, I'd probably enjoy implementing it.

---

_Comment by @BurntSushi on 2021-03-01 12:59_

I've given this some thought. First, some details to clarify.

Using the `-g/--glob` flag does not automatically imply `-uuu`. For example:

```
$ tree
.
├── bar
│   ├── README-debian.txt
│   ├── README.md
│   └── README.md~
├── foo
│   ├── README-debian.txt
│   ├── README.md
│   └── README.md~
├── README-debian.txt
├── README.md
└── README.md~

2 directories, 9 files

$ rg --files
foo/README-debian.txt
foo/README.md
README-debian.txt
README.md

$ rg --files -g 'README*'
foo/README-debian.txt
foo/README.md
foo/README.md~
README-debian.txt
README.md
README.md~

$ rg --files -g 'README*' -u
README.md
README-debian.txt
README.md~
foo/README-debian.txt
foo/README.md
foo/README.md~
bar/README.md~
bar/README-debian.txt
bar/README.md
```

Notice here that `bar` remains absent from both listings, but once I add `-u`, `bar` appears. This is because the interaction between `-g/--glob` and gitignore is a bit subtle. Namely, that globs given on the CLI are semantically a gitignore file themselves (with whitelist/blacklist semantics inverted) and then given the highest precedence. So when you use, `README*`, that is in effect whitelisting every file path starting with `README`. Since it has the highest precedence, it overrides any conflicting gitignore rules in, say, a `.gitignore` file, like in your example. But since `README*` does not match `bar`, there is no conflict in rules and `bar` is still ignored.

When I encoded this logic, my thinking was that if you specified a glob at the CLI, then that should be given the highest priority.  But even this appears inconsistent. Namely, `rg --files -g 'bar/*'` returns no results, which seems to contradict my claim that globs given on the CLI have the highest precedent. The reason is that `bar/*` doesn't actually match `bar`, so you actually need `{bar,bar/*}`, which is a bit weird.

I suspect when I encoded this logic I was _also_ thinking about blacklist globs. Namely, if we moved all CLI globs down to the lowest priority, then _any_ whitelist glob in a gitignore file is going to override that and prevent the CLI blacklist glob from working as one might expect. For example, let's say I add a `README-debian.txt~` file and tweak my `.gitignore`:

```
$ cat .gitignore
*~
!*-debian.txt~
/bar
```

That is, ignore all files ending in `~`, except for files ending in `-debian.txt~`. Listing files with ripgrep and using blacklist globs works as one might expect:

```
$ rg --files
foo/README-debian.txt
foo/README.md
README-debian.txt
README.md
README-debian.txt~

$ rg --files -g '!*~'
foo/README-debian.txt
foo/README.md
README-debian.txt
README.md
```

If we make all CLI globs lower precedence, then in this example, the whitelist glob in the `.gitignore` file would actually override the glob given at the CLI and `README-debian.txt~` would be listed.

So it does seem like your insight here is spot on: whitelist globs should get lower precedence, but blacklist globs should get higher precedence.

I _think_ I agree with this request. And in particular, I'd like to see this fixed rather than worked around with another flag like `--weakglob`. IMO, supporting both behaviors makes an already complex filter logic a lot worse, and I do not want to maintain both modes. Moreover, I like the idea of this behavior being the default because it makes the interaction between ignore files and CLI globs more sensible, and moreover, makes it more intuitive to use the various `-u` flags in conjunction with the `-g` flag. That is, this way of doing things can be more intuitively described as, "CLI globs are the last filter to apply, after all ignore logic has been applied." (I think.)

It would be useful to think about other cases that might be changed by this behavior.

Implementing this could also be tricky. I don't think the code is really setup to give different precedence to whitelist and blacklist globs. But maybe it's as simple as partitioning them... But if you do that, you now lose the fact that whitelist and blacklist globs can interleave with each other. Namely, `-g 'README*' -g '!README*~' -g 'README-debian.txt~'` should work today and really should continue to work as one might expect. But having different precedence for whitelist and blacklist globs may make this impractical.

So I'm not quite sure what to do here. Sorry.

---

_Label `question` added by @BurntSushi on 2021-03-01 12:59_

---

_Comment by @ilyagr on 2021-03-06 08:23_

Sorry it took me a while to respond, and thank you very much for such a nice reply!

> Using the `-g/--glob` flag does not automatically imply `-uuu`.

Yes, you are right, that's an excellent example. You forgot to mention that `bar` is in the `.gitignore` (in case somebody reading this later doesn't figure it out immediately; perhaps it's best to edit your comment and remove this sentence).

> Namely, `rg --files -g 'bar/*'` returns no results

This is a bit weird indeed. I think you mentioned in https://github.com/BurntSushi/ripgrep/issues/1654 (or elsewhere?) that it's a performance optimization. It is a good advertisement for my proposal, since this particular weirdness would be gone with the weak glob behavior. I just hope I haven't missed some new inconsistency that may appear because of weaker globs.

> If we make all CLI globs lower precedence, then in this example, the whitelist glob in the `.gitignore` file would actually override the glob given at the CLI and `README-debian.txt~` would be listed.

Yes, that's exactly what I meant to avoid.

> So it does seem like your insight here is spot on: whitelist globs should get lower precedence, but blacklist globs should get higher precedence.
>
> I _think_ I agree with this request. 

😊 🐶 😺

> And in particular, I'd like to see this fixed rather than worked around with another flag like `--weakglob`.

The purist in me is overjoyed to hear it, though on my own I'd be hesitant to go that far immediately. Do you think there doesn't even need to be a transition period? 

> Moreover, I like the idea of this behavior being the default because it makes the interaction between ignore files and CLI globs more sensible, and moreover, makes it more intuitive to use the various `-u` flags in conjunction with the `-g` flag.

:)

> That is, this way of doing things can be more intuitively described as, "CLI globs are the last filter to apply, after all ignore logic has been applied." (I think.)

I think that description is right. For the cases I thought of so far, the patch below seems like its behavior should match this as well.

> It would be useful to think about other cases that might be changed by this behavior.

Yes, and I don't claim to have thought it through exhaustively yet. I tried the patch below, and was surprised that it didn't seem to break any tests immediately. I'm not sure if I ran them correctly. The next thing I'd do is to try to write some tests it does change. It might also be nice to go through various glob-related issues in this repository, and see whether they'd become better or worse.

I'm not sure exactly when I'll be able to do it, though. My free time is somewhat unpredictable at this point.

> Implementing this could also be tricky.

I'm not sure I've accounted for everything, but when I looked, it seemed to me that the code is set up perfectly for this change. Ignoring tests, documentation, and any command-line flag, the change may be just a line [here](https://github.com/BurntSushi/ripgrep/blob/c5ea5a13df8de5b7823e5ecad00bad1c4c4c854d/crates/ignore/src/dir.rs#L365-L374):

```diff
diff --git a/crates/ignore/src/dir.rs b/crates/ignore/src/dir.rs
index 296c803..c6d2064 100644
--- a/crates/ignore/src/dir.rs
+++ b/crates/ignore/src/dir.rs
@@ -365,13 +365,13 @@ impl Ignore {
         if !self.0.overrides.is_empty() {
             let mat = self
                 .0
                 .overrides
                 .matched(path, is_dir)
                 .map(IgnoreMatch::overrides);
-            if !mat.is_none() {
+            if mat.is_ignore() {
                 return mat;
             }
         }
```

I think the overrides here are the list of all the globs, positive and negative. If a negative globs matched the file in question, `mat` will be `Ignore`, and we abort. As a [comment for `Override.matched` describes](https://github.com/BurntSushi/ripgrep/blob/c5ea5a13df8de5b7823e5ecad00bad1c4c4c854d/crates/ignore/src/overrides.rs#L80-L84), if we are dealing with a file and there are any positive globs in the list, `mat` should never be `None`. If any positive globs match, we move on to checking whether we should reject the file based on patterns from ignore files and hidden files. If no positive globs match, `mat` should be `Ignore` and we abort.

TODO: I haven't yet thought through whether this does the right thing for directories.

TODO 2: (**Later edit**: I don't think this matters very much. The question of whether *everything* one would want can be accomplished with a one-line diff is not that important, after all.) Now that I look at it again, a week later, I'm not sure when [line #390](https://github.com/BurntSushi/ripgrep/blob/c5ea5a13df8de5b7823e5ecad00bad1c4c4c854d/crates/ignore/src/dir.rs#L390) may be hit (is it ever?), and whether that still works correctly. I'm also not exactly sure where the hidden files are checked. But then it's getting late, and I'm getting tired.
 
> Namely, `-g 'README*' -g '!README*~' -g 'README-debian.txt~'` should work today and really should continue to work as one might expect. But having different precedence for whitelist and blacklist globs may make this impractical.

I need to write some tests. I think the diff I have above may already work correctly in this case. As far as I can tell, all the globs (positive and negative) are checked together, as a group, at the `dir.rs` lines I pointed out above. The outcome of that process is recorded in `mat`.  We just have to make sure this process (completely internal to that if statement) checks globs in the correct order, and it probably already does.

---

_Comment by @ilyagr on 2021-03-06 22:38_

Here is a way to think of the weak globs behavior that makes it conceptually simpler than the current behavior. Hopefully, it will also make it easier to reason about whether weak globs are a good idea. 

## Conceptual framework

With weakglobs, there is only **one** mechanism in `rg` that increases the set of files that is to be searched: `rg` goes through every filename and directory given to it. If none were given, it defaults to `.`.

There are at least **two** ~~ignore engines~~ **independent filters** that reduce the set of files that is to be searched.  (~~I can't quite come up with a better word for "ignore engine", but there should be one~~ **Update:** Filters! They are called filters! I may later replace "ignore engines" with "filters" in the rest of this comment)

- Ignore files and hidden files.
- `--glob`s, both positive and negative

Each of these filters has some complex internal logic (ignores with `!` can make hidden files visible, the order in which you specify globs matters, etc), but since each of them can **only reduce** the set of files to be searched, they are conceptually independent of each other. In particular, it doesn't matter which of these ~~engines~~ filters is applied first. 

This also explains why, I think, the code is set up perfectly for this: the ignore engines are already there. They just have three outcomes (whitelist, ignore, neutral) instead of two (neutral, ignore). This is useful to support their internal logic, but it shouldn't be hard to make sure that 'whitelist' and 'neutral' behave the same way in the function that I referenced in my previous comment.

### Unresolved issues

At the moment, I can only think of one: what to do with `--type` and `--type-not`. I can think of two possibilities:

1. Make it a third ignore engine. This seems to be close to how it's currently implemented.
2. (My current preference) Make it part of the `--glob` engine. So, `--type yaml` is exactly equivalent `-g '*.yaml' -g '*.yml'`in the same position in the argument list. `--type-not yaml` is equivalent to `-g '!*.yaml' -g '!*.yml'`.

At the moment, I don't know whether the current behavior matches either of these and, if not, whether that's desirable.

Can you think of any other `ripgrep` functionality that wouldn't fit into this framework?

## Use-cases
I've thought of a couple of use-cases. Let's take the file structure in the beginning of your comment https://github.com/BurntSushi/ripgrep/issues/1808#issuecomment-787930233, and let's say `*~` and `bar` are in the `.gitignore`. 

### 1. Mostly happy example
Let's say I want to find all the `*.md` files, even inside `bar`. I have two good options that would work just as they work today: 1) Boring old `-u` or 2) `rg --files --glob '*.md' . bar/`.

With current globs, there is also the option of `rg-no-weakglob --files --glob '*.md' --glob 'bar'`; this would no longer work. I think the other options are better already, but if anyone has gotten used to typing this, they would have to learn the new way. It might be a good riddance, considering your example of `rg-no-weakglob --files -g 'bar/*'` having surprising behavior.

### 2. Less happy
With weak-globs, it becomes difficult to reproduce your example of `rg-no-weakglob --files -g 'README*'`. One key question is whether there's an uncontrived situation when someone might expect that behavior (as opposed to the normal behavior, or the behavior with `-uuu`) in real life. 

### 3. Very happy example
(For completeness) my original example of wanting to find all the `README*`files, but skip the ignored ones.


-----

I think it's now pretty straightforward for me to reason about what the weak glob behavior can and cannot do. I still plan to try to learn a little about how people use `--glob` and when people miss not having `--include` by looking through the issues a bit.


---

_Comment by @ilyagr on 2021-03-10 04:06_

## Resolving the `--type` "unresolved issue"
TLDR: Regarding `--type`, I think we should just keep the way it behaves now. There should be a separate proposal to change its behavior (~~I might file another issue~~ **Update:** This is not exactly the same, but would include most of https://github.com/BurntSushi/ripgrep/issues/809).

For related reasons, we should also rename this proposal to something like "globs respect ignores" instead of "weak globs".

----

I realized that, currently, `ripgrep` does not follow either of the options for the behavior of `--type` that I described in the above comment. Instead, `-g` always has priority over `--type`.

So, there are now three potential behaviors on the table when both `-g` and `--type` are specified: 

1. `--type` is a separate ~~ignore engine~~ filter (see my comment above), so it can only *remove* files from consideration. I think this would be very straightforward to implement, but I now also think it's not the most useful behavior.
2. "Order matters": `--type yaml` is exactly equivalent `-g '*.yaml' -g '*.yml'`in the same position in the argument list. `--type-not yaml` is equivalent to `-g '!*.yaml' -g '!*.yml'`. 

    This might require changing how command-line arguments are parsed, so that we remember the order until after the point when all the `--type-add` and `--type-clear` are processed. 
3. **Current behavior:** globs have priority over `--type`. It should be quite easy to implement the rest of this proposal but keep this behavior for `--type`. There are some issues with it (see below), but ultimately we should probably stick with it.

    (**Update 2021-03-19:** after writing some tests, I realized that the *actual* current behavior is more complicated than I thought. Currently, `--type` does not treat hidden files and ignored files in the same way, while `--glob` does. I'll shortly link a PR that will fix that)

To illustrate what they mean, consider 4 invocations:

```
a. rg --files --type txt -g '!*.txt'
b. rg --files -g '!*.txt' --type txt 
c. rg --files --type-not txt -g '*.txt'
d. rg --files -g '*.txt' --type-not txt
```

They would work differently depending on which option we pick for `--type` behavior:
      
| #  | Behavior                     | Invocations that show text files | Implementation difficulty |
|----|------------------------------|----------------------------------|---------------------------|
| 1. | `--type` is a separate filter | None of them ~(not as useful)~     | Easy                      |
| 2. | Order matters                | `b.` and `c.`                    | Possibly hard             |
| 3. | Current behavior             | `c.` and `d.`                    | Easy                      |

The reason I call option 1 "~not as useful~" is that switching to that behavior ~would remove~ the possibility of using `--type lisp -g '!*.el'` to search lisp files that aren't Emacs-lisp. (**Update 2021-03-20**: I think this paragraph is wrong; it now seems to me that this example would work great with option 1. I'm no longer certain option 1 is less useful; I should think of more examples.)

### Thoughts on option #3 (current behavior)

I don't think #3 makes quite as much sense as #2, especially in light of the rest of the proposal. It will be harder to document and explain. Ultimately, however, it does make sense; `--type` and `--glob` would be parts of the same filter, just as in #2. Moreover, with option #3, I'm pretty sure the rest of the proposal can be implemented with all the significant changes limited to that one function in the `ignore` crate of `ripgrep`.

In this version of the larger proposal, globs remain stronger than `--type`. So, we need a better name for the proposal than "weak globs". The best I've come up with so far is  "globs respect ignores".
	
Still, I think behavior #2 is better in terms of being slightly more useful (e.g. `-g 'README*' --type-not md` would work; it doesn't now) and a lot easier to explain.  Regardless of whether the "globs respect ignores" proposal is implemented, at some point changing `--type` to behavior #2 would be an improvement, I think. 

---

_Comment by @aswild on 2021-03-17 23:16_

As a transitioning ag->rg user, I have a related feature request, and found this issue before opening a new one.

I'm trying to find the right invocation for "search \<type> files that also contain \<pattern> in their name."

~~For example, in ag, I can use `ag --rs -G test ...`~~ to search for Rust files that have `test` somewhere in the filename. I had hoped that `rg -t rs -g '*test*' ...` would be equivalent, but as detailed here, the `-g test` overrides all the type filtering logic and the results include non-rust files. (On top of that, it's clunkier to type the extra quotes and `*`s in `'*test*'` to make it a proper glob pattern than interpreting the argument as a regex like ag does)

Is this something that could be considered?

\* Edit: Combining file types with `-G` in `ag` is something I implemented in my own fork a few years ago and not a feature in the standard version. Oops, I've been patching ag so heavily that I sometimes forget which features are stock and which ones I've added myself, I didn't mean to misrepresent things here.

---

_Comment by @ilyagr on 2021-03-20 01:46_

@aswild I'm not the decider here, but your comment gave me some food for thought.

What you are asking for is how the "Behavior 1: `--type` is a separate filter" from my comment above would work. I declared this behavior useless in the comment, but I hadn't looked at how `ag` works, and now there's at least one person who thinks it would be a good thing. Perhaps I should give it a little more thought.

My feeling was that `-t rs -g '*test*'` should ultimately become equivalent to `-g '*.rs' -g '*test*'`, which would behave as it does now. (What would be different is that `-g` would no longer always override `--type-not`.) To find Rust files that also have test in them, one would use `-g '*test*.rs'`. This has the advantage of being conceptually simple; I also thought it was the most flexible option, but now I'll think again.

The behavior you suggest would be more important for something like `-t lisp`, since that covers several extensions. Without that behavior, achieving the effect you asked for would require `-g '*test*.el' -g '*test*.sc' <3 more of these>`.

---

_Comment by @aswild on 2021-03-20 06:04_

I accidentally lied about how `ag` works, filtering based on both file type and an additional regex is something I added in my fork, it doesn't work that way in the stock version (comment above edited).

In my mental model I tend to think of file types as a "baseline" that are OR'd together, and then additional manual filters like globs are AND'd on top of that. But the specific semantics definitely get complicated and there's cases where that doesn't really make sense.

I used rust as an example off the top of my head, but it's true this applies more for file types that have multiple extensions.

I don't really have a good holistic suggestion for how all these filter complexities should interact, so these are just my two cents; I appreciate the thought and effort being put in here.

---
