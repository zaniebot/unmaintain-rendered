```yaml
number: 323
title: Support colorized output to standard error
type: pull_request
state: closed
author: pkgw
labels: []
assignees: []
base: master
head: pr-termcolor-stderr
created_at: 2017-01-15T23:24:32Z
updated_at: 2017-01-19T16:05:15Z
url: https://github.com/BurntSushi/ripgrep/pull/323
synced_at: 2026-01-12T18:23:12Z
```

# Support colorized output to standard error

---

_@pkgw_

This pull request adds support for colorized output to standard error on both Unix and Windows platforms. It preserves compatibility with the existing API at the expense of being a bit verbose in some of its data structure names.

Virtually all of the changes are re-castings of the previous code to be generic over the `io::Stdout` and `io::Stderr` types. This involves the creation of a few new traits that need to be exposed in the API.

---

_Comment by @BurntSushi on 2017-01-16 00:15_

Thanks!

So... A change like this that significantly impacts the public API of a crate really needs to have discussion before launching into patches. For example, I am pretty solidly against adding more traits. I would rather add more concrete types.

I don't quite have time to do a thorough review right now, but judging the code from a quick perusal, I suspect major changes will be required before I'd be willing to land this PR.

And to be clear, I do agree with the motivation for this change. We should be able to emit colors to stderr. What I'm not sold on however is adding a generic API. Maybe you could talk more about that?

I will also add a CONTRIBUTING.md file that will hopefully avoid PRs like this without prior discussion.

---

_Comment by @BurntSushi on 2017-01-16 00:19_

Yes. A generic API is absolutely not something we can do because it doesn't work on Windows. Moreover, I don't like the generic API for the complexity it introduces.

---

_Comment by @pkgw on 2017-01-16 00:20_

Hmm, perhaps I didn't explain it well. The generic bits are just to avoid internal code duplication — it's not intended to be something that users of the crate will ever care about. The motivation for is the fact that `io::Stdout` and `io::Stderr` are completely unrelated types in Rust. So, if you want to support colorized output to both of them, you either need to duplicate a lot of code, or you need to add an internal layer that lets you abstract between the two.

There isn't an expectation that the generic API will ever be implemented for other types. But I made the generic traits public because I *think* you have to ... but I actually just read something that made me think that maybe you don't.

---

_Comment by @BurntSushi on 2017-01-16 00:31_

Generics that are strictly internal are something I could get on board with. But none of your PR is really internal. Everything is public, and for example, StandardStream is generic which is not an accurate reflection of its capabilities.

Code duplication can be solved in other ways, like macros or truly internal generics.

Sorry I don't have time to into more details at the moment. :-( My current suggestion would be to open a new issue for a proposal that includes public API changes and an implementation plan.

---

_Comment by @pkgw on 2017-01-16 00:31_

Ok, yeah, in principle you can keep all of the traits private by changing the newtypes from things ilke this:

```
pub type Stdout = StandardStream<io::Stdout>;
```

to structs like this:

```
pub struct Stdout(StandardStream<io::Stdout>);
```

I don't think that actually buys you much, though. You have to write wrappers for the exposed functions and you remove the ability for users of the crate to genericize their code over the stdout/stderr choice. There are some more traits exposed but again, the typical user can completely ignore them — as far as most of them are concerned, there's just a new `Stderr` struct to go along with the preexisting `Stdout`.

---

_Comment by @BurntSushi on 2017-01-16 00:40_

No. I'm not on board with exposing those. As I've said, they aren't an accurate reflection of all supported platform capabilities. On top of that, I am **very firmly** opposed to just exposing things in the public API and justifying it by saying that most users can ignore it. If it's part of the public API, then there should be cohesion and stability guarantees. A larger API means more maintenance and potentially less flexibility moving forward.

---

_Comment by @pkgw on 2017-01-16 00:43_

OK, [issue #324](https://github.com/BurntSushi/ripgrep/issues/324)

---

_Comment by @pkgw on 2017-01-19 16:05_

Closing as it seems unlikely that this PR will move forward.

---

_Closed by @pkgw on 2017-01-19 16:05_

---
