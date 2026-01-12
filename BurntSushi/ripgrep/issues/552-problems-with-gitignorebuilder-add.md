```yaml
number: 552
title: "Problems with GitignoreBuilder::add()"
type: issue
state: closed
author: behnam
labels: []
assignees: []
created_at: 2017-07-12T22:31:14Z
updated_at: 2017-10-21T22:26:55Z
url: https://github.com/BurntSushi/ripgrep/issues/552
synced_at: 2026-01-12T16:13:22Z
```

# Problems with GitignoreBuilder::add()

---

_@behnam_

The builder method `GitignoreBuilder::add()` has a poor API and I think it better be updated in a future major release.

First, it violates API Guidelines rule [C-BUILDER](https://github.com/brson/rust-api-guidelines#builders-enable-construction-of-complex-values-c-builder), where it says:

> The builder should offer a suite of convenient methods for configuration, including setting up compound inputs (like slices) incrementally. These methods should return `self` to allow chaining.

By not returning `self`, `GitignoreBuilder::add()` enforces having three statements for a simple build, instead of one.

In this repo, `GlobSetBuilder::add()`, and possibly other builder methods, already return `self`.

Next, it returns `Option<Error>`, which is something very hard to deal with. If the method is not returning `self` because it needs early error handling (I think that's questionable for itself), it should return `Result<(), Error>`, so an `unwrap()`/`expect`/`try!()`/`?` can be used to handle the error. With the current return type, two statements are needed to handle the error.

FYI: I didn't see anything regarding these cases in the API Guideline, so I've filed an issue suggesting to document it: <https://github.com/brson/rust-api-guidelines/issues/102>.

---

_Comment by @BurntSushi on 2017-07-12 22:54_

I like API guidelines, but just because one does not follow them in a specific instance does not mean the API is poor. API guidelines are called *guidelines* for a reason. :-)

In this particular case, `add` returns an `Option<Error>` because it indicates *partial failure*. The problem with a `Result<(), Error>` return type is that it becomes too easy to completely bail on the creation of a `Gitignore` just because a single glob failed to parse. This is actually important in practice because `git` is completely silent on errors when parsing `.gitignore` files, so it's easy to see how invalid globs might appear. There is even a particularly notable case where globs like `a**b` are permitted by `git`, but are rejected by both the specification written in `man gitignore` and ripgrep's implementation of said spec. It would be disastrous in this case to reject the entire `gitignore`.

I would be OK with adding another method that returns a `Result<&mut Gitignore, Error>`.

> By not returning self, GitignoreBuilder::add() enforces having three statements for a simple build, instead of one.

Indeed, and this is the desired outcome. If you only need to parse a single `.gitignore` file, then you can use the [`Gitignore::new`](https://docs.rs/ignore/0.2.0/ignore/gitignore/struct.Gitignore.html#method.new) convenience constructor. Although, this also returns a `(Gitignore, Option<Error>)` for the same reasons as above. We could add another constructor that returns a `Result<Gitignore, Error>` (complete failure even if only a single glob is invalid), but I'm not sure what I'd call it.

> Next, it returns Option<Error>, which is something very hard to deal with.

I will submit to you that it is not the type that is hard to deal with, but that partial success is the thing that is hard to deal with. :-)

---

_Comment by @BurntSushi on 2017-07-12 22:55_

Note that the documentation explains some of this. Could you help me write better documentation so that others aren't baffled by the type signature?

---

_Comment by @behnam on 2017-07-15 21:32_

Sorry, my wording was poor here. What I meant was this: although the API works for how it's used at the moment, it's not easy to work with in other cases; and since it's a main entry to this public API, I think it can be improved.

I should also note that I'm still learning common practices in Rust and some assumptions I have about APIs do not apply here. But, also, as a user, I don't expected to get surprising results, and when it happens, I try to start a discussion to see if we can improve the API or documents.

> I like API guidelines, but just because one does not follow them in a specific instance does not mean the API is poor. API guidelines are called guidelines for a reason. :-)

Agreed. I shouldn't have used *violate*. I meant it's not following a *common practice*.

I also agree with the point that the API needs to be able to allow ignoring bad glob rules in gitignore files, which is what git does by default.

To step back a bit, the way I got into this problem was this: I renamed the test gitignore file in my diff, where I was using the Builder to set up my test case. After the rename, all my matches started to fail and I didn't know why. It looked like my gitignore file is loading, but it's not matching the paths I have. After some debug, I realized that I'm throwing away the returned optional-error value of `.add()`, and it's something that can fail.

Now, there's one thing here that was very surprising to me: the main input passed into the `add()` method, the file path, was invalid and I was not aware that it will fail silently. I think this error is a different kind of error than glob errors.

With the assumption that each step of a builder can fail or throw *optional-errors*, I would argue that `add()` needs to distinguish between them: the file not existing is a hard error, which should be returned by `Result<_, Error>`. The glob-pattern errors may be returned in `Result<Option<Vec<GlobError>>, _>`.

I have not worked with many Builder patterns in Rust yet, but from other backgrounds, I would say I prefer a solution that's more *declarative*, and while being flexible and allowing user to easily set to ignore some errors, doesn't do so by default.

Agreed that it is expected for the class with default configs (`Gitignore`) to have default git-like behavior, but the builder class (`GitignoreBuilder`) can maintain compatibility without surprises.

In this case, we can allow user to define the behavior for *optional-errors* in the builder set up. The options would be: 1) ignore glob errors, 2) fail on glob errors, or 3) set up a user-defined handler on glob errors (so application can show warnings and such). This can be done with a builder method `on_glob_errors()` that accepts an error-handling strategy enum.

With that, we can also go back to returning `self` from all builder methods, and returning the final result from `build()`, with success or error.

---

_Comment by @BurntSushi on 2017-07-16 03:02_

@behnam I'm much much more receptive to this style of argument. :-) Namely, we don't really have guidelines that apply to partial success, so using the guidelines here doesn't really make a lot of sense. I'm not even sure whether we can really establish guidelines at all, since partial success isn't something that I see that much, and therefore, we don't have much data.

I feel like I agree with you more than I disagree, but it's not completely clear cut to me. Namely, I agree with the abstract idea that not all errors should necessarily be treated equally. A nit though:

> `Result<Option<Vec<GlobError>>, _>`

We don't actually need to return a `Vec<_>` here since the [`Error` type already encapsulates the possibility for multiple errors with its `Partial` variant](https://docs.rs/ignore/0.2.1/ignore/enum.Error.html). This representation is useful because it permits callers to both treat the error as a black box that can be logged and subsequently ignored, but also permits them to inspect the error more carefully if necessary.

I *think* I agree with your more important point that we should perhaps use a `Result` here in principle and yield an error for more surprising circumstances such as the one you ran into where the `.gitignore` file doesn't exist. The problem with this approach is that actual tools *probably* don't care whether a `.gitignore` file exists or not, since its existence is pretty much always optional. Namely, if the API returned a `Result`, then tools like ripgrep would want to log and ignore an error regardless of whether it was from the `Ok` or `Err` variant of the `Result` you propose. Invariably, the claim I'm making here is that the API, as exposed today, is probably exactly what you want in most use cases.

I am sympathetic to the failure mode you ran into. I can see how it would be annoying. Adding an `on_glob_errors` method feels like it might complicate the API a bit too much. Are there use cases for this level of control outside of tests where you ran into this problem?

---

_Comment by @BurntSushi on 2017-10-21 22:26_

I'm going to close this. While I don't necessarily believe the current API is great, I do think it is fine compared to alternatives. Partial success of an operation invariably leads to code that is more complex than code with total success or total failure. I would be willing to revisit this if someone came up with concrete alternatives that addressed the use case here.

---

_Closed by @BurntSushi on 2017-10-21 22:26_

---
