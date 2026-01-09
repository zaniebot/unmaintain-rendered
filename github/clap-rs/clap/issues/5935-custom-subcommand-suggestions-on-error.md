---
number: 5935
title: Custom subcommand suggestions on error
type: issue
state: closed
author: ilyagr
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2025-03-02T23:06:53Z
updated_at: 2025-03-20T02:02:46Z
url: https://github.com/clap-rs/clap/issues/5935
synced_at: 2026-01-07T13:12:20-06:00
---

# Custom subcommand suggestions on error

---

_Issue opened by @ilyagr on 2025-03-02 23:06_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

(I looked, but it wasn't easy to search; I could have missed something)

### Clap Version

4.5.31

### Describe your use case

In https://github.com/jj-vcs/jj/pull/5845 (**Update:** now merged as https://github.com/jj-vcs/jj/commit/d446a85c4a884b603bacf6e8a88dfe6da47d09c3, see also the discussion at https://github.com/jj-vcs/jj/pull/5845#discussion_r1976561273), I needed to modify the suggested command of a `clap::Error`.

The motivation is that `jj` does not have a `jj clone` command, which brand new users experimented with `jj` are likely to try. Currently, it uselessly suggests `jj config` as an alternative. I'd like it to point the users to the correct `jj git clone` command.

Less importantly, I'd love to remove the "Usage" message that does not seem useful.

In other words, we currently have:

```
 error: unrecognized subcommand 'clone'

      tip: a similar subcommand exists: 'config'

    Usage: jj [OPTIONS] <COMMAND>

    For more information, try '--help'.
```

I'd like to modify the error so that it says:

```
 error: unrecognized subcommand 'clone'

      tip: a similar subcommand exists: 'git clone'

    For more information, try '--help'.
```

(Aside: either way, the command would be followed by a hint like """Hint: You can configure `aliases.clone=["git", "clone"]` if you want `jj clone` to default to the Git backend.""", but that's done independently of `clap`)

### Describe the solution you'd like

I'd like something like the following to work, improving on https://docs.rs/clap/latest/clap/error/struct.Error.html#method.insert:

```rust
// Currently, `original_err` is Arc<clap::Error> for us
// since `clap::Error` is not `Clone`. So, the following
// does *not* work.
let new_clap_err = original_err.clone();
// `remove_all` does not currently exist
new_err.remove_all(ContextKind::SuggestedSubcommand);
new_err.remove_all(ContextKind::Usage);
new_err.insert(  // Already exists
        ContextKind::SuggestedSubcommand,
        ContextValue::String("git clone".to_string()),
);
return CommandError::from(new_clap_err);
```

In other words, I'd like `clap::Error` to be `Clone` (perhaps by wrapping the non-clonable parts of it in an `Arc`) and to be able to remove error contexts.

Moreover, I'd like the above to result in an error that gets formatted nicely, with color and the text above.

### Alternatives, if applicable

#### Alternative 1

Currently, ~~https://github.com/jj-vcs/jj/pull/5845~~ https://github.com/jj-vcs/jj/commit/d446a85c4a884b603bacf6e8a88dfe6da47d09c3 can only add an hint to the confusingly verbose message. The result is:

```
    error: unrecognized subcommand 'clone'

      tip: a similar subcommand exists: 'config'

    Usage: jj [OPTIONS] <COMMAND>

    For more information, try '--help'.
    Hint: You can configure `aliases.clone=["git", "clone"]` if
    you want `jj clone` to work and always use the Git backend.
```

Screenshot of a slightly older version:

![Image](https://github.com/user-attachments/assets/ca0970e8-2c22-4931-8c2d-12ed93a99ba9)

#### Alternative 2

`clap` could provide a facility for "when the user types in command A, suggest command B", extending the recent "do not suggest these commands" functionality. 



#### Alternative 3

I also tried to recreate the error manually, but the result lost formatting, and I'm afraid to forget to copy something important.

```rust
            let mut new_err = clap::Error::new(err.kind());
            new_err.insert(
                ContextKind::InvalidSubcommand,
                ContextValue::String(format!("{cmd}")),
            );
            new_err.insert(
                ContextKind::SuggestedSubcommand,
                ContextValue::String(format!("git {cmd}")),
            );
            return CommandError::from(new_err);
```

resulted in losing color in the output :

<img width="432" alt="Image" src="https://github.com/user-attachments/assets/99ba5233-4eec-48f7-8ec6-afcff066d974" />

Still, if the above worked, it'd be an (imperfect) alternative,

---

_Label `C-enhancement` added by @ilyagr on 2025-03-02 23:06_

---

_Referenced in [jj-vcs/jj#5845](../../jj-vcs/jj/pulls/5845.md) on 2025-03-02 23:10_

---

_Renamed from "FR: Make `clap::Error` `Clone`, make it easier to add/remove its properties" to "FR: Make it easier to edit `clap::Error`s: make it `Clone`, allow removing its properties" by @ilyagr on 2025-03-02 23:30_

---

_Label `A-help` added by @epage on 2025-03-03 15:13_

---

_Renamed from "FR: Make it easier to edit `clap::Error`s: make it `Clone`, allow removing its properties" to "Custom subcommand suggestions on error" by @epage on 2025-03-03 15:36_

---

_Comment by @epage on 2025-03-03 15:38_

This jumps immediately to a solution, particularly one that requires advanced clap use to implement.

Let's step back and focus on the core of the problem

> The motivation is that jj does not have a jj clone command, which brand new users experimented with jj are likely to try. Currently, it uselessly suggests jj config as an alternative. I'd like it to point the users to the correct jj git clone command.

With our `ValueParsers`, we added support for [UnknownArgumentValueParser](https://docs.rs/clap/latest/clap/builder/struct.UnknownArgumentValueParser.html) so you can create custom suggestions for arguments users assume to be present but aren't.  Cargo makes use of this.  I would love for our dynamic completions to be able to pull this out and complete a known bad argument as the proper one.

It would be great to offer something similar for subcommands.

Current workarounds
- Use `external_subcommands(true)` and manually implement it
- Mung the `Error` to get what you want

I don't see `ValueParser`s having an analogue in subcommands.

I have considered the idea of an Action for subcommands and for custom Actions.  I've not done custom `ArgAction`s because I wanted to see how the interface works out and its been fairly broad and evolving, making it hard to lock that down (and its tied to implementation details).

The challenge with subcommands is how to handle them.  `help` is a pre-parse action.  Some people have wanted a post-parse action.  It sounds like this would also be a pre-parse action.

See also
- #5815
- #5251

In designing something, we may want to consider how it interacts with #2222.

> Less importantly, I'd love to remove the "Usage" message that does not seem useful.

We've not developed a good set of principles for when to show `Usage` and when not to.  I am open to someone exploring this.  I'd recommend creating a dedicated issue, organizing it as a table (type of error, whether usage is shown), writing out principles, and then updating the table with the proposals based on those principles.

---

_Comment by @epage on 2025-03-03 15:43_

So all of that will benefit you and all future clap developers.  However, working through some of that would also be a lot of work.  Extending `Error` seems reasonable even if other changes are made so we can probably move forward with that.  I would prefer you at least focus on the `Usage` part in clap proper, rather than working around that.

If you could list out more precisely what new APIs you are wanting, that'd be a big help.  Also, could you clarify why you need `Clone`?

---

_Comment by @ilyagr on 2025-03-03 23:08_

Thank you very much for thinking about this and the many interesting points!

I'm not sure whether you saw the edits I made to the question after the original version.

> Let's step back and focus on the core of the problem

We should consider what I called "Alternative 2" above, which would be customized for my particular problem. However, there would still be some use for editing `Error`s even if that's implemented, and I'm not sure whether that's worth a purpose-made function.

More generally, this FR could be split into many, and I'm not sure which of the possible features that could be implemented are most feasible and make the most sense. I described my main suggestion, and I'll try to motivate it again below (in another comment), but whether that's the right approach is still a question in my mind.

------------------------

For the rest of your first comment above, it's full of interesting ideas, but I don't think they quite address the part of my problem that I can't solve (though many of them might be better ways to address the part of the problem that we already have a workaround for). I could easily have misunderstood something.

> With our ValueParsers, we added support for `UnknownArgumentValueParser`... It would be great to offer something similar for subcommands.

Something similar to `UnknownArgumentValueParser` but for subcommands is an interesting idea. I think it is relevant, but not quite necessary or sufficient, since it would still require either manual creation or editing an `Error` object (we still probably want to generate an error for `jj clone`). As I described in "Alternative 3" above, I don't know how to create a good `clap::Error` manually; my attempts result in an error without formatting.

But if the problem with creating native-looking `clap::Error`s was solved (or if it is already solved, and I just didn't find the solution), your suggestion could be a more flexible version of the "Alternative 2" I suggested above.

Except for creating/modifying the `clap::Error`s, there is already a solution that more or less work. It might be sub-optimal, but it is available today, and `jj` uses it. Instead of creating a custom trigger for `clone` subcommand, we take a `clap::Error` and check whether `err.get(ContextKind::InvalidSubcommand)` matches `ContextValue::String("clone")`. It's in the last commit of the PR I linked (I don't have a permalink yet, since the PR isn't yet merged).

> Use external_subcommands(true) and manually implement it

I am not very familiar with `external_subcommands`, and do not quite understand at the moment whether that would help beyond the approach of post-processing errors.

> We've not developed a good set of principles for when to show Usage and when not to.

This is a bit tangential to most of this issue; being able to manually remove "Usage" from an error would be nice, but it doesn't make or break a solution to this problem. Still, I think it's a good idea to look further into this.

At the moment, my understanding of when "Usage:" should be shown is minimal, all I know is that "I saw one case where `Usage:` wasn't very useful".

-------------

I will write more, hopefully shortly, but this comment is getting long.

---

_Comment by @ilyagr on 2025-03-04 01:20_

> If you could list out more precisely what new APIs you are wanting, that'd be a big help. 

TLDR: Making `clap::Error` `Clone` would go a long way; I'd also like `clap::Error::remove` to extend `clap::Error::insert`.

## Prelude

The approach that seems most direct to me would be to provide a way to create a `clap::Error` for "invalid command" or "invalid subcommand' that's just like the normal one `clap` creates, but with a different suggested command.

This could either be a new error created from nothing or it could be a way to take the error created by `clap` and modify it. The latter seems easier seems `clap` seems to treat `clap::Error` as an opaque structure which can change between releases (e.g. if a new `clap` release wants to keep track of more metadata), so if I create the "correct" `clap::Error` today it might starts diverging from normal `clap::Error`s `clap` creates in the future. Still, a better API from creating errors is a viable alternative.

(In theory, creating `clap::Error`s from nothing is already possible, but as I explained in "Alternative 3" above, it does not end up with the same formatting.)

>  Also, could you clarify why you need Clone?

Very naively, if we allow editing `clap::Error`s (as `clap::Error::insert` already does), it seems natural to want to copy and preserve the original error and modify a copy of it.

More technically, the immediate reason I need `clap::Error` to be `Clone` is that `jj` wraps `clap::Error` in `jj_lib::CommandError`, which needs to be `Clone`. Currently, this is done by storing `Arc<clap::Error>` (which is `Clone` even thought `clap::Error` isn't) inside `jj_lib::CommandError`. AFAIK, while it's trivial to get a `&clap::Error` from `Arc<clap::Error>`, `clone()` would be the only way to get the `&mut clap::Error` that I'd need to use `clap::Error::insert`.

I'm not 100% sure that `jj_lib` couldn't be refactored to not require `jj_lib::CommandError` to be `Clone`, but it would be at least a pain; I couldn't do it in 5 minutes. I'm not an expert on Rust design patterns for non-Clone types, though.

There's certainly no terrible performance cost to cloning `clap::Error`s.

----------

## Suggestion

Actually, I think making `clap::Error` `Clone` (possibly with the same trick of storing non-`Clone`-able parts inside an `Arc`) would be more or less sufficient to make what I want possible.

It is not clearly documented (**Update:** https://github.com/clap-rs/clap/pull/5938, unless you think it's obvious), but after looking more carefully at [`clap::Error::insert`](https://docs.rs/clap/latest/clap/error/struct.Error.html#method.insert), I expect that if `err: clap::Error`, `err.insert(ContextKind::SuggestedSubcommand, "fred")` will *replace* the suggested subcommand inside `err` rather than insert an additional one. I expected otherwise since I can imagine an error including more than one suggested subcommand.

---------

Less importantly, I'd still want a `clap::Error::remove(&mut self, kind: ContextKind) -> Option<ContextValue>` command that could be used to remove a `SuggestedSubcommand` with no replacement. This could also be used to remove "Usage" and any other part of the error I don't find useful in some particular case.

Or, if we're OK with breaking changes, we could change the signature of `clap::Error::insert` to take an `Option<ContextValue>` instead of a `ContextValue`.

Either way, we don't need `clap::Error::remove_all` that I suggested previously. This would only be needed if an error's context was conceptually a map `ContextKind -> Vec<ContextValue>`.

---

_Comment by @ilyagr on 2025-03-04 02:16_

Here's a naive patch that makes `clap::Error` become `Clone`. If it looks OK, I can make it into a PR (I haven't read the guide yet, I'm guessing it's mainly missing a changelog entry).

```diff
diff --git a/clap_builder/src/error/format.rs b/clap_builder/src/error/format.rs
index 4c22bc2d9d..db6bed9391 100644
--- a/clap_builder/src/error/format.rs
+++ b/clap_builder/src/error/format.rs
@@ -33,6 +33,7 @@
 ///
 /// </div>
 #[non_exhaustive]
+#[derive(Clone)]
 pub struct KindFormatter;
 
 impl ErrorFormatter for KindFormatter {
@@ -59,6 +60,7 @@
 /// This follows the [rustc diagnostic style guide](https://rustc-dev-guide.rust-lang.org/diagnostics.html#suggestion-style-guide).
 #[non_exhaustive]
 #[cfg(feature = "error-context")]
+#[derive(Clone)]
 pub struct RichFormatter;
 
 #[cfg(feature = "error-context")]
diff --git a/clap_builder/src/error/mod.rs b/clap_builder/src/error/mod.rs
index 7bd627375c..bf822e9e2d 100644
--- a/clap_builder/src/error/mod.rs
+++ b/clap_builder/src/error/mod.rs
@@ -14,6 +14,7 @@
     fmt::{self, Debug, Display, Formatter},
     io,
     result::Result as StdResult,
+    sync::Arc,
 };
 
 // Internal
@@ -57,18 +58,19 @@
 /// See [`Command::error`] to create an error.
 ///
 /// [`Command::error`]: crate::Command::error
+#[derive(Clone)]
 pub struct Error<F: ErrorFormatter = DefaultFormatter> {
     inner: Box<ErrorInner>,
     phantom: std::marker::PhantomData<F>,
 }
 
-#[derive(Debug)]
+#[derive(Debug, Clone)]
 struct ErrorInner {
     kind: ErrorKind,
     #[cfg(feature = "error-context")]
     context: FlatMap<ContextKind, ContextValue>,
     message: Option<Message>,
-    source: Option<Box<dyn error::Error + Send + Sync>>,
+    source: Option<Arc<dyn error::Error + Send + Sync>>,
     help_flag: Option<Cow<'static, str>>,
     styles: Styles,
     color_when: ColorChoice,
@@ -300,7 +302,7 @@
     }
 
     pub(crate) fn set_source(mut self, source: Box<dyn error::Error + Send + Sync>) -> Self {
-        self.inner.source = Some(source);
+        self.inner.source = Some(source.into());
         self
     }
 
@@ -892,7 +894,7 @@
 }
 
 #[cfg(feature = "debug")]
-#[derive(Debug)]
+#[derive(Debug, Clone)]
 struct Backtrace(backtrace::Backtrace);
 
 #[cfg(feature = "debug")]
@@ -911,7 +913,7 @@
 }
 
 #[cfg(not(feature = "debug"))]
-#[derive(Debug)]
+#[derive(Debug, Clone)]
 struct Backtrace;
 
 #[cfg(not(feature = "debug"))]
@@ -930,5 +932,5 @@
 
 #[test]
 fn check_auto_traits() {
-    static_assertions::assert_impl_all!(Error: Send, Sync, Unpin);
+    static_assertions::assert_impl_all!(Error: Send, Sync, Unpin, Clone);
 }
```

**Update:** Maybe I'd also require implementors of `ErrorFormatter` to be `Clone`, so that every version of `clap::Error` is guaranteed to be `Clone`. (I'm guessing, but not sure, that `PhantomData<F>` is not `Clone` if `F` isn't `Clone`).

---

_Comment by @ilyagr on 2025-03-04 04:00_

...and here is what I can get to work with that patch. 


https://github.com/ilyagr/jj/commit/clapclonerr

<img width="1075" alt="Image" src="https://github.com/user-attachments/assets/8b9f1001-ae76-4b13-bc76-2aa30fcf52ff" />

(Compare this to "Attempt 1" from my original comment)

If I also added `clap::Error::remove`, I'd get rid of the `Usage`.

---

_Comment by @epage on 2025-03-04 14:41_

> Something similar to UnknownArgumentValueParser but for subcommands is an interesting idea. I think it is relevant, but not quite necessary or sufficient, since it would still require either manual creation or editing an Error object (we still probably want to generate an error for jj clone). As I described in "Alternative 3" above, I don't know how to create a good clap::Error manually; my attempts result in an error without formatting.

Why would you still need to manually create an error for `jj clone`, rather than having a `.subcommand(Command::new("clone").hide(true).action(UnknownSubcommand::new().suggest("git clone")))`?

> Or, if we're OK with breaking changes, we could change the signature of clap::Error::insert to take an Option<ContextValue> instead of a ContextValue.

`Error` is meant to be a map-like interface, so `insert(kind, None)` isn't right but `remove(kind)` is.

---

_Comment by @ilyagr on 2025-03-07 00:25_

> Why would you still need to manually create an error for `jj clone`, rather than having a `.subcommand(Command::new("clone").hide(true).action(UnknownSubcommand::new().suggest("git clone")))`?

That would work[^caveat], I think it's a good idea. I'm not sure whether this is related to `UnknownArgumentValueParser`, but in any case I like the `UnknownSubcommand` action better.

It still sounds to me that making `Clone` and `remove` work would be easier in the short term. I think it only makes sense for a type that represents an opaque object (not a factory) and has methods for editing it, but is hard to manually recreate, to provide a `clone` interface. 

So, unless you tell me otherwise, I'm planning to turn the patches I suggested above into PRs.

[^caveat]: There is a caveat that this functionality would have to be carefully designed to work well with `ignore_errors`, so that `jj`'s aliases support still works and it's possible to override `jj clone` with an alias. It does a complicated dance with parsing the command-line with `ignore_errors` first, substituting aliases, and then parsing again.

---

_Comment by @epage on 2025-03-07 19:47_

> So, unless you tell me otherwise, I'm planning to turn the patches I suggested above into PRs.

I'm fine with remove.

`Clone` feels a bit odd, specialized to your requirements without going into why those requirements exist.  That said, I've been in situations where that has been a problem and I wished they were more regularly cloneable (caching, particularly when you can't cache only on success, looking at you `LazyLock`).  The downsides to locking are relatively low (limited in which fields, already effectively immutable).  I'd say its ok to move forward with it.

> There is a caveat that this functionality would have to be carefully designed to work well with ignore_errors, so that jj's aliases support still works and it's possible to override jj clone with an alias. It does a complicated dance with parsing the command-line with ignore_errors first, substituting aliases, and then parsing again

`ignore_errors` is a trap.  There is no clearly defined behavior of what it *should* do and is easily broken with any minor change and we can't exhaustively test for every potential way it can be broken.  I'd be interested in finding alternative solutions and deprecating it.

---

_Comment by @ilyagr on 2025-03-08 00:09_

> I'd say its ok to move forward with it.

Thanks! I understand a perspective from which it can feel like a cludge, but I think more people than just me will find it useful.

> Clone feels a bit odd, specialized to your requirements without going into why those requirements exist. 

> ignore_errors is a trap. ... I'd be interested in finding alternative solutions and deprecating it.

Both of these points run into the fact that, at least at the moment, I'm not the best expert on why `jj`'s CLI code is organized the way it is, and what options there are for reorganizing it without losing useful functionality. I shared what I do understand as best I could :).

For example, I'd love to have `ignore_errors` replaced with something cleaner, but I'm not sure what would work nor exactly what our requirements are[^known]. For that discussion, perhaps @yuja, @torquestomp, @zummenix (who originally added the function I edited in https://github.com/jj-vcs/jj/commit/d446a85c4a884b603bacf6e8a88dfe6da47d09c3, AFAICT, for the `--templates` feature I described in the footnote), @elasticdog, or other people from https://github.com/jj-vcs/jj/blame/29e2ccf4aeae24fa031e561211750d7e6e93f0b0/cli/src/cli_util.rs#L3384 might have some thoughts.

[^known]: Well, two relevant features `jj` has are [aliases](https://jj-vcs.github.io/jj/prerelease/config/#aliases), the [default command](https://jj-vcs.github.io/jj/prerelease/config/#default-command), and dynamic suggestions for configured templates if the user enters a `--template` arugment without a value. This is in addition to the feature I discussed in this issue (which is actually easier to change if we have to, since we don't have to worry about breaking changes as much for `jj`'s behavior that happens only when errors occur). One reason I pinged all the people above is that I may have forgotten some other important feature that's relevant here, and that I'm not sure whether `ignore_errors` is relevant to all of these or not.

-----

P.S. The PR I was discussing in the first message in this issue is now merged, so I can provide a better permalink to the commit I was talking about: https://github.com/jj-vcs/jj/commit/d446a85c4a884b603bacf6e8a88dfe6da47d09c3. I also edited the first message to include it.

---

_Comment by @yuja on 2025-03-08 03:02_

fwiw, `Clone` wouldn't be needed for jj's purpose. We can mutate `clap::Error` before wrapping it in `Arc<dyn Error>`.

---

_Referenced in [clap-rs/clap#5941](../../clap-rs/clap/pulls/5941.md) on 2025-03-08 05:08_

---

_Referenced in [clap-rs/clap#5942](../../clap-rs/clap/pulls/5942.md) on 2025-03-08 05:17_

---

_Comment by @epage on 2025-03-10 15:57_

From https://github.com/clap-rs/clap/pull/5942#issue-2904541056

> Some reasons to make `clap::Error` be `Clone`:
> 
>   * As you (@epage) suggested, `clap::Error`s could be cached
> 
>   * `Clone` types are convenient for new Rust users who might not be experienced in solving ownership problems
> 
>   * Since `clap::Error`s can be edited, one might want to edit them in a fallible way. It should be possible to save a backup copy in case the edit fails.
> 
>   * More generally, all else being equal, it's nice to not require people to refactor their programs. In this case, one could argue that the refactor would improve their programs, but the improvement of avoiding a clone in the error path would be quite minor and might not be worth their time.
> 
>   * There's little to no cost to doing so

(justifying changes should happen in issues, PRs should focus on the implementation)

---

_Comment by @epage on 2025-03-10 15:58_

> Clone types are convenient for new Rust users who might not be experienced in solving ownership problems

Its a mixed bag of whether users have errors that can clone and new users shouldn't be dealing with clap's Error.

Overall, I lean towards use-case driven development and since the use case has gone away, I'll pass on this.

---

_Comment by @ilyagr on 2025-03-10 21:43_

Sure, no problem. Thanks for reviewing this and the interesting discussion.

---

_Closed by @epage on 2025-03-19 21:00_

---

_Comment by @ilyagr on 2025-03-19 21:53_

I think this was closed by 4451c0f84e850e1846450ac9ba4ddaf1e014a728, not by https://github.com/clap-rs/clap/commit/2e13847533d458991e3353b373e679ab65f1c8c3. (That commit closed #5949)

---
