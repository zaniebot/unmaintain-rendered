```yaml
number: 477
title: "[WIP] Support for mercurial"
type: pull_request
state: closed
author: kalekseev
labels: []
assignees: []
base: master
head: hgignore
created_at: 2017-05-09T13:09:38Z
updated_at: 2017-08-23T21:53:26Z
url: https://github.com/BurntSushi/ripgrep/pull/477
synced_at: 2026-01-12T18:23:13Z
```

# [WIP] Support for mercurial

---

_@kalekseev_

Support for mercurial https://github.com/BurntSushi/ripgrep/issues/6
Very basic support is implemented for now, although it covers my own usecases:
  - patterns from repo's `.hgignore` and from `ignore` control in ui part of `$HOME/.hgrc`
  - `syntax: glob` rules (mostly using git logic)
  - `syntax: regexp` rules (they are directly translated into `Glob::re`)

I'm planning to translate mercurial's tests suit for hgignore files as my next step.

@BurntSushi, I want to start a discussion about support for `subinclude` directives:

    subinclude:path/to/subignorefile reads patterns specifically for paths in the subdirectory

To support this the process of building Ignore object has to depend on the information from .hgignore file near the .hg directory. Therefore we have to guaranty that dirs under mercurial are processed after the parsing of a repo's .hgignore (and all the `subinclude`, `include` files). In the parallel run that looks hard until we lock all the worker's threads (hardly the option).

The other they may be to update retroactively all Ignore objects with the content of the related subignore file. 

---

_Review comment by @BurntSushi on `globset/src/glob.rs`:198 on 2017-05-09 13:17_

I'm not sure this is quite the right way to go about this. In particular, you wind up storing a regex in a field called `glob`, which doesn't seem good to me. Additionally, the `from_regex` option isn't really compatible with the `case_insensitive` or `literal_separator` flags, which also makes the builder strange. I think I'd rather see `Glob` itself be an enum and support a `from_regex` constructor. In that approach, `GlobBuilder` doesn't need to be modified at all.

---

_Review comment by @BurntSushi on `ignore/src/hgignore.rs`:499 on 2017-05-09 13:20_

I think you actually want [`env::home_dir`](https://doc.rust-lang.org/std/env/fn.home_dir.html) instead.

---

_Review comment by @BurntSushi on `ignore/src/hgignore.rs`:323 on 2017-05-09 13:21_

There's a lot in common with this method and a similar method in `GitignoreBuilder`. Are the semantics really this identical? And if so, can we split the logic out into a function that both builders can use?

---

_@BurntSushi reviewed on 2017-05-09 13:27_

Thanks for starting this work. :-) I realize it's a hefty undertaking, so I appreciate you tackling it. I've left some comments based on a cursory review, but I'll wait to do a deeper dive until you think I should.

Some more comments:

> To support this the process of building Ignore object has to depend on the information from .hgignore file near the .hg directory. Therefore we have to guaranty that dirs under mercurial are processed after the parsing of a repo's .hgignore (and all the subinclude, include files). In the parallel run that looks hard until we lock all the worker's threads (hardly the option).

Hmm. It seems like this should be part of building the hgignore matcher itself. I don't think it requires directory traversal, right? You just follow the include paths instead? Maybe you could say more about the semantics because I feel like I'm missing something.

If it's really more complicated than I'm making it out to be, then perhaps inspecting how mercurial itself handles this would be fruitful.

> syntax: regexp rules (they are directly translated into Glob::re)

Surely the regex syntax supported by mercurial is not in precise correspondence with Rust's regex engine. Is the plan to just emit an error message and suffer wontfix bug reports for eternity? (I say this with some snark as the maintainer, but I suppose there isn't a good alternative.)

---

_@kalekseev reviewed on 2017-05-09 22:35_

---

_Review comment by @kalekseev on `globset/src/glob.rs`:198 on 2017-05-09 22:35_

> I'd rather see Glob itself be an enum

Could you provide a draft definition for that enum that will suit the project best?
I've tried to replace the Glob structure with the
```
pub enum Pattern {
    Glob { glob: String, re: String, opts: GlobOptions, tokens: Tokens },
    Regex(String),
}
```
but it seems that enums don't have constructors in rust so the private GlobOptions and Tokens types leak.

---

_@BurntSushi reviewed on 2017-05-09 22:38_

---

_Review comment by @BurntSushi on `globset/src/glob.rs`:198 on 2017-05-09 22:38_

Ah, right, yeah you'll have to do something like this:

```rust
pub struct Glob(GlobInner);

enum GlobInner {
    Glob { ... },
    Regex(String),
}
```

The other interesting bit here is that by offering a constructor that accepts a regex, we've essentially tied to the implementation itself to regexes as well. It's mildly unfortunate, but I think it's the right path forward for now.

---

_Comment by @BurntSushi on 2017-08-23 21:53_

@kalekseev I'm trying to clean up the PR queue, so I think I'm going to close this for now due to inactivity, but please feel free to re-open another PR if you want to continue to attack this!

---

_Closed by @BurntSushi on 2017-08-23 21:53_

---
