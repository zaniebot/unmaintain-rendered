```yaml
number: 1499
title: add engine option to easily switch between regexp engines
type: pull_request
state: closed
author: pierreN
labels: []
assignees: []
base: master
head: master
created_at: 2020-02-27T11:42:03Z
updated_at: 2020-02-27T15:34:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1499
synced_at: 2026-01-12T18:23:13Z
```

# add engine option to easily switch between regexp engines

---

_@pierreN_

This is the commits corresponding to https://github.com/BurntSushi/ripgrep/issues/1488 

I tried to not change any default behavior of the options.

If I've done things correctly, the only thing that should change is when using the "auto-hybrid-regex" option without pcre2 feature enabled, an additional error message stating that pcre2 is not enabled should appear.

Thanks !

---

_Comment by @pierreN on 2020-02-27 12:26_

 Tests should be fine now ? Unsure if I'm missing something or.. ?

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1205 on 2020-02-27 12:35_

Could this be written in prose? i.e.,

> Accepted values are 'default', 'pcre2' or 'auto-hybrid'.

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1209 on 2020-02-27 12:35_

"as look-around or backreferences"

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1211 on 2020-02-27 12:36_

Should be `'--auto-hybrid-regex'`.

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1215 on 2020-02-27 12:36_

Also should be `'--auto-hybrid-regex'`.

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1222 on 2020-02-27 12:38_

Hmm, so I think this is not quite right. I think we want this to explicitly say that it overrides the `--pcre2` and `--auto-hybrid-regex` flags. (And vice versa for them.) Otherwise, something like `--pcre2 --engine default` would still use PCRE2 instead of what it should do: use the default engine.

---

_Comment by @BurntSushi on 2020-02-27 12:40_

Not sure what's going on with CI. Could you try deleting

https://github.com/BurntSushi/ripgrep/blob/ecec6147d117da638c933cf103a3ea14ce87102d/.github/workflows/ci.yml#L89-L90

and

https://github.com/BurntSushi/ripgrep/blob/ecec6147d117da638c933cf103a3ea14ce87102d/.github/workflows/ci.yml#L188-L189

And see if that helps.

---

_Review comment by @BurntSushi on `crates/core/args.rs`:588 on 2020-02-27 12:41_

Could you create errors in a way that is consistent with the surrounding code? e.g., `From::from(format!("..."))`.

---

_Review comment by @BurntSushi on `crates/core/args.rs`:623 on 2020-02-27 12:41_

Looks like this will fit on one line?

---

_Review comment by @BurntSushi on `crates/core/args.rs`:658 on 2020-02-27 12:46_

It looks like this should fit on one line?

---

_Review comment by @BurntSushi on `crates/core/args.rs`:588 on 2020-02-27 12:48_

Actually, now that I think about it, you should just be able to call `unwrap()` here since `engine` has a default value.

---

_Review comment by @BurntSushi on `crates/core/args.rs`:660 on 2020-02-27 12:49_

Could you please squash this commit back into the pertinent commits? I try to keep ripgrep's commit history clean, readable and linear, with tests passing on all commits. I don't always succeed, but I'd like to try. There isn't too much code here, so I think it's not too big of a pain to split these changes and squash them back into appropriate commits.

Although, I guess now that I say this, the commit hygiene in this PR is a bit off. It probably would have been better to start by refactoring how matchers are built and have that in one commit. And then a separate commit that actually adds the new `--engine` flag, which would include all relevant changes (including completions and docs and tests).

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1215 on 2020-02-27 12:50_

Changes like this should always be folded back into their corresponding commits.

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:189 on 2020-02-27 12:50_

It looks like this fixed CI. Not sure why, but fine to leave this as is. (And this should be in a separate commit.)

---

_@BurntSushi requested changes on 2020-02-27 12:51_

Thanks very much! I left a few comments. I think most of them are minor, but the most major one is probably the commit hygiene in this PR. Fixing the small problems (squashing formatting and typo commits) is probably fairly easy. Fixing the bigger problem (intermingling a refactor with a feature addition) is probably nastier. I'd be happy to do the latter since I'm trying to be strict about keeping a clean history that's easy to read. But it might take me longer to merge this PR if so.

Also, please add integration tests (in top-level `tests` directory) covering this new flag. Tests should also cover flags overriding each other, e.g., `--pcre2 --engine default` should use the default engine, not `--pcre2`.

I'll likely just deprecate the `--auto-hybrid-regex` flag in favor of `--engine`, but I'll do that separately and it shouldn't impact anything in this PR.

---

_@pierreN reviewed on 2020-02-27 12:58_

---

_Review comment by @pierreN on `crates/core/args.rs`:660 on 2020-02-27 12:58_

Ok yes thanks, I see what you meant by "commit hygiene", seeing modifications that way does make more sense.

Seeing all the changes needed to be done, is it OK if we just close/throw away this PR and make a new one ? Thanks. 

---

_@pierreN reviewed on 2020-02-27 13:04_

---

_Review comment by @pierreN on `crates/core/args.rs`:660 on 2020-02-27 13:04_

Maybe throw it away, do a first PR for the CI build, then do a second PR for the refactor+engine option ? 

I'm actually impressed by the speed of your answers ! I will wait for your confirmation before doing that though :)

---

_@BurntSushi reviewed on 2020-02-27 13:09_

---

_Review comment by @BurntSushi on `crates/core/args.rs`:660 on 2020-02-27 13:09_

Up to you. I don't have a strong opinion. I'm happy to have you force push stuff here, but if a separate PR is easier for you, then go for it.

As for the CI fix, sure, a separate PR seems fine for that. Thanks. :-)

---

_Review comment by @BurntSushi on `crates/core/args.rs`:660 on 2020-02-27 13:09_

> I'm actually impressed by the speed of your answers 

Thanks. I'm not always this fast though. You just happened to catch me at a good time. :)

---

_@BurntSushi reviewed on 2020-02-27 13:09_

---

_Comment by @pierreN on 2020-02-27 13:19_

Thanks, then yes I'll do this as a separate PR.

---

_Closed by @pierreN on 2020-02-27 13:19_

---

_Review comment by @pierreN on `crates/core/app.rs`:1222 on 2020-02-27 15:09_

OK, fair point. 

While testing now I was unsure to add the `engine` override for the `no-pcre2` and `no-auto-hybrid-regexp` since intuitively the option set `--engine=pcre2 --no-auto-hybrid-regex` should enable pcre2.

But since `--pcre2 --no-auto-hybrid-regex` doesn't enable `pcre2` neither, I guess it's just better to keep backward compatibility with default options and add overrides everywhere.

---

_@pierreN reviewed on 2020-02-27 15:09_

---

_@BurntSushi reviewed on 2020-02-27 15:17_

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1222 on 2020-02-27 15:17_

Yeah good point. Defer to backward compatibility for now. I'll take another pass over this before the next release to make sure it all makes sense.

---
