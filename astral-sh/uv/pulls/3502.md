```yaml
number: 3502
title: "feat: add custom middleware"
type: pull_request
state: closed
author: tdejager
labels:
  - rustlib
assignees: []
base: main
head: feat/custom-middleware
created_at: 2024-05-10T08:49:58Z
updated_at: 2026-01-04T19:38:03Z
url: https://github.com/astral-sh/uv/pull/3502
synced_at: 2026-01-10T05:49:14Z
```

# feat: add custom middleware

---

_Pull request opened by @tdejager on 2024-05-10 08:49_


## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This adds the ability to the `RegistryClient` and `BaseClient` for the consumer of the library to add custom middleware. The problem is that we have different authentication methods/requirements for pypi registries and would like to use the middleware provided by https://github.com/mamba-org/rattler

I've decided that if you do this, it overrides the normal middleware. This makes the `retries` option, and maybe some others, not really do anything anymore. Which is a bit weird, but couldn't directly think of a better way to do it, as you might end up with 'duplicated' middleware maybe.

This should close the issue/question I made: https://github.com/astral-sh/uv/issues/3400

I'll make it a draft for now because I do not know how you feel about such a change and maybe there is a different design you have in mid.


## Test Plan

Once, I can update to the latest uv version, I'll try and test things out directly in: https://github.com/prefix-dev/pixi. I'm happy to add rust test here if you know a good place for it.

<!-- How was it tested? -->


---

_Label `rustlib` added by @zanieb on 2024-05-10 12:43_

---

_Comment by @konstin on 2024-05-13 17:35_

Sorry for not replying to the initial issue! Yes we can support. What do you think about the branch in https://github.com/astral-sh/uv/compare/konsti/custom-middleware ? It's similar to yours but encapsulates the stack selection into `MiddlewareStack`.

Am i correct to assume you don't care about middleware for the offline case?

---

_Comment by @tdejager on 2024-05-14 06:57_

Hi Konsti, that looks a lot better actually! Good question about the offline, our middleware does not account for it yet, but if you feel it's better to add it to the offline case as well, that seems totally valid!

---

_Review requested from @zanieb by @konstin on 2024-05-14 09:43_

---

_Comment by @konstin on 2024-05-14 09:47_

(force-pushing because i had to rebase on main, your initial commit should be preserved)

---

_@konstin reviewed on 2024-05-14 09:49_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/compile.rs`:120 on 2024-05-14 09:49_

Not sure about where to put the retries constant

---

_Comment by @charliermarsh on 2024-05-22 02:14_

@zanieb - do you want to review this?

---

_Assigned to @zanieb by @zanieb on 2024-05-22 02:48_

---

_Comment by @zanieb on 2024-05-22 02:48_

Sure

---

_@zanieb reviewed on 2024-05-23 14:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-05-23 14:57_

Hey @tdejager sorry I've been slow to get to this.

I don't love this new API. Could we instead:

- Only create the retry middleware if retries is non-zero
- Replace the `Keyring` option with `Option<AuthMiddleware>`
- Add a `extra_middleware: Vec<Arc<dyn Middleware>>` option

I think this should get you the same functionality, right?

---

_@tdejager reviewed on 2024-05-23 16:17_

---

_Review comment by @tdejager on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-05-23 16:17_

Sounds like a good improvement to me, I can also make the changes early next week if you are busy @konstin :)

---

_@tdejager reviewed on 2024-05-23 16:17_

---

_Review comment by @tdejager on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-05-23 16:17_

Also @zanieb and @konstin thanks again for taking a look 🙂

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-05-23 16:27_

I'll be out next week fyi.

I missed that @konstin introduced the `MiddlewareStack` — let's make sure he's onboard with my proposal :)

---

_@zanieb reviewed on 2024-05-23 16:27_

---

_@tdejager reviewed on 2024-05-23 18:07_

---

_Review comment by @tdejager on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-05-23 18:07_

Yeah maybe having the AuthMiddleware in the stack is enough. I thought you might want to be more explicit about enabling/disabling the auth middleware. Let see what konsti thinks :)

---

_@zanieb reviewed on 2024-05-23 18:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-05-23 18:18_

I think we can just have the `AuthMiddleware` populated by default and ya'll can pass `None` to explicitly remove it.

---

_@konstin reviewed on 2024-06-03 11:01_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/compile.rs`:294 on 2024-06-03 11:01_

I changed the API so it's now `.with` methods and checks for > 0 retries, but if we don't take a keyring it becomes even more verbose because we have a number of callsites, and also `KeyringProviderType` lives in `uv-configuration` so it can't be `Into<AuthMiddleware>`.

![image](https://github.com/astral-sh/uv/assets/6826232/b88c3a77-3245-4a72-b5d8-00cb42f0f837)


---

_Comment by @olivier-lacroix on 2024-07-09 05:46_

Hi all, just wondering if you guys are happy with the current design, and if rebasing is the only requirement for this PR to be merged?

---

_Review requested from @zanieb by @konstin on 2024-07-10 07:35_

---

_Comment by @zanieb on 2024-07-19 17:02_

Sorry, I need to review the latest implementation but am prioritizing things that impact more users right now.

---

_Comment by @olivier-lacroix on 2024-07-20 00:15_

 No worries. Thanks @zanieb !

---

_Comment by @zanieb on 2024-09-16 22:12_

Is this still needed?

---

_Comment by @tdejager on 2024-09-17 08:16_

Would still be very useful for us, however, as reqwest-middleware is now a forked version, I'm unsure regarding the compatibilities with the open-source version. However, we could re-vendor the middleware if needed :)

---

_Comment by @zanieb on 2024-10-01 17:10_

It looks like @konstin added `AuthIntegration::NoAuthMiddleware` allowing the auth middleware to be disabled so maybe we just need the ability to attach extra middleware now?

---

_Comment by @tdejager on 2024-10-01 18:39_

Sounds good, is there something you like me to change other than a rebase?

---

_Comment by @zanieb on 2024-10-01 18:41_

Can we drop all the `MiddlewareStack` stuff and just add a `extra_middleware: Vec<Arc<dyn Middleware>>` option? Or is there more that you need?

---

_Comment by @tdejager on 2024-10-01 18:59_

Not at all computer currently but sounds good to me, let me give it a go at the end of the week!

---

_Closed by @tdejager on 2024-11-04 08:36_

---

_Branch deleted on 2024-11-04 08:38_

---

_Comment by @tdejager on 2024-11-04 09:17_

Closing this as there were so many changes it make sense to make a different PR. Will reference this when I get to it :)

---

_Branch restored on 2024-11-04 09:18_

---
