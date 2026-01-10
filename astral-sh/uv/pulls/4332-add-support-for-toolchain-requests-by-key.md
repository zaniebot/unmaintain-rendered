```yaml
number: 4332
title: Add support for toolchain requests by key
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-request-key
created_at: 2024-06-14T20:38:29Z
updated_at: 2024-06-17T18:13:30Z
url: https://github.com/astral-sh/uv/pull/4332
synced_at: 2026-01-10T13:54:02Z
```

# Add support for toolchain requests by key

---

_Pull request opened by @zanieb on 2024-06-14 20:38_

Adds support for toolchain keys e.g. `cpython-3.11.2-macos` allowing you to download toolchains for specific architectures and operating systems using the format we use to uniquely identify a toolchain.

---

_Label `enhancement` added by @zanieb on 2024-06-14 20:38_

---

_Label `preview` added by @zanieb on 2024-06-14 20:38_

---

_Review comment by @konstin on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 08:21_

The prefix differentiates it from the implementation version

---

_Review comment by @konstin on `crates/uv-toolchain/src/toolchain.rs`:209 on 2024-06-17 08:25_

Would this currently panic if i had graalpython installed?

---

_Review comment by @konstin on `crates/uv-toolchain/src/toolchain.rs`:293 on 2024-06-17 08:27_

Are these `with_` methods used?

---

_@konstin approved on 2024-06-17 08:28_

---

_@zanieb reviewed on 2024-06-17 14:37_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 14:37_

Ah that's good to know but we don't expose the `implementation_version` at the top level and I found this confusing because `python_version` meant the "marker" in some cases which didn't have the full version tuple. I'll think about this a bit harder though (and perhaps change it in a dedicated pull request for discussion purposes).

---

_@zanieb reviewed on 2024-06-17 14:38_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/toolchain.rs`:209 on 2024-06-17 14:38_

Probably. I need to address this before merge.

---

_@zanieb reviewed on 2024-06-17 14:38_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/toolchain.rs`:293 on 2024-06-17 14:38_

Probably not, they're copied from the `PythonVersionRequest`. I'll drop them if not. Thanks!

---

_@zanieb reviewed on 2024-06-17 14:43_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 14:43_

Why do we have a `Interpreter::python_full_version` method there too? It seems like that just captures the string?

---

_@konstin reviewed on 2024-06-17 15:40_

---

_Review comment by @konstin on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 15:40_

iirc that the method were named after PEP 508 environments, hence the `python_version` and `python_full_version` keys

---

_@zanieb reviewed on 2024-06-17 15:50_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 15:50_

But they're the same here and different elsewhere. 🤷‍♀️ 

---

_@zanieb reviewed on 2024-06-17 15:50_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 15:50_

Reverting this change for now.

---

_@konstin reviewed on 2024-06-17 16:11_

---

_Review comment by @konstin on `crates/uv-toolchain/src/interpreter.rs`:237 on 2024-06-17 16:11_

Might totally be outdated and needs to change

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:625 on 2024-06-17 16:44_

Maybe... `result.map_or(true, |(_, interpreter)| request.satisfied_by_interpreter(interpreter))`?

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/downloads.rs`:331 on 2024-06-17 16:46_

This is nice. I like it.

---

_@BurntSushi approved on 2024-06-17 16:46_

I like this feature!

---

_@zanieb reviewed on 2024-06-17 16:50_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:625 on 2024-06-17 16:50_

This feels familiar :) will do

---

_@zanieb reviewed on 2024-06-17 16:50_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:331 on 2024-06-17 16:50_

Thanks!

---

_@zanieb reviewed on 2024-06-17 16:52_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:625 on 2024-06-17 16:52_

This seems... less clear sometimes?

```diff
diff --git a/crates/uv-toolchain/src/discovery.rs b/crates/uv-toolchain/src/discovery.rs
index 8dbec984..d6bb224b 100644
--- a/crates/uv-toolchain/src/discovery.rs
+++ b/crates/uv-toolchain/src/discovery.rs
@@ -619,9 +619,10 @@ pub fn find_toolchains<'a>(
             debug!("Searching for {request} in {sources}");
             python_interpreters(request.version(), request.implementation(), sources, cache)
                 .filter(move |result| result_satisfies_system_python(result, system))
-                .filter(|result| match result {
-                    Err(_) => true,
-                    Ok((_source, interpreter)) => request.satisfied_by_interpreter(interpreter),
+                .filter(|result| {
+                    result.as_ref().map_or(true, |(_, interpreter)| {
+                        request.satisfied_by_interpreter(interpreter)
+                    })
                 })
                 .map(|result| result.map(Toolchain::from_tuple).map(ToolchainResult::Ok))
         }),
 ```
 
 Somehow it feels more verbose.

---

_@zanieb reviewed on 2024-06-17 16:53_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/implementation.rs`:33 on 2024-06-17 16:53_

Is there a standard pattern or name for this? @BurntSushi 

---

_@zanieb reviewed on 2024-06-17 16:54_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/implementation.rs`:33 on 2024-06-17 16:54_

I guess the capitalized version is the "display name" but much more often want the lowercase string, should I be doing lowercase casts all over the place instead?

---

_Review requested from @BurntSushi by @zanieb on 2024-06-17 16:58_

---

_@BurntSushi reviewed on 2024-06-17 17:00_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:625 on 2024-06-17 17:00_

I think I'd still go `map_or` but honestly it's dealer's choice. I'm not _too much_ of a combinator snob.

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/implementation.rs`:33 on 2024-06-17 17:05_

Hmmmmmmmmm.

Usually you'd have a `Display` impl here somewhere, but given that you can return a `&'static str`, that's usually a bit nicer to work with.

If there were only one sensible string representation, then I think I'd call this `as_str`.

If there is a sensible "default" representation that should be used in almost all cases and another representation that is used less frequently, then I think I'd call the default `as_str` and the other something more descriptive, like, `as_str_capitalized` or something.

If there is no obvious default, I think I'd just be descriptive with both names. Like, `as_str_lowercase` and `as_str_capitalized`. Or something like that.

With that said, I don't mind `pretty` either.

Something bothers me about having one form be a concrete method and another form be a `From` impl. I can't quite articulate it, but it seems a little confusing. Like, it would be easy to see one but not the other. You could add concrete methods for both, and then add a `From` impl with whatever the "default" is I guess.

I think overall the right thing to do here depends a lot on how these are used and whether there's a clear default or whatever.

---

_@BurntSushi approved on 2024-06-17 17:06_

---

_@zanieb reviewed on 2024-06-17 17:15_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/implementation.rs`:33 on 2024-06-17 17:15_

Thanks for all the context! We do have a `Display` representation which is the lowercase name right now. I used to have `as_str` for the capitalized name but changed it. Maybe I'll change it back... I think it's probably better to consider outside this pull request though.

---

_Merged by @zanieb on 2024-06-17 18:11_

---

_Closed by @zanieb on 2024-06-17 18:11_

---

_Branch deleted on 2024-06-17 18:11_

---

_@ibraheemdev reviewed on 2024-06-17 18:13_

---

_Review comment by @ibraheemdev on `crates/uv-toolchain/src/discovery.rs`:625 on 2024-06-17 18:13_

I'd prefer `.map(..).unwrap_or(true)` to `map_or`, tends to be clearer, but the `match` might end up being less verbose.

---
