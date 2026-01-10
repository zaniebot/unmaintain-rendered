---
number: 8639
title: Variant metadata prototype ideas
type: issue
state: open
author: msarahan
labels: []
assignees: []
created_at: 2024-10-28T16:37:06Z
updated_at: 2024-10-31T17:35:44Z
url: https://github.com/astral-sh/uv/issues/8639
synced_at: 2026-01-10T01:24:31Z
---

# Variant metadata prototype ideas

---

_Issue opened by @msarahan on 2024-10-28 16:37_

Greetings Astral team! I'm looking to build a prototype of the variant metadata idea proposed in https://discuss.python.org/t/selecting-variant-wheels-according-to-a-semi-static-specification/53446/111. I'm doing so with `uv` to start with because the existing "first match" index priority strategy is necessary for this plan.

A rough diagram of the parts is here:

![variant_concept_august_2024 drawio](https://github.com/user-attachments/assets/6fdc6c93-3039-4c9d-bd4b-d7bd697c4a4d)

From this diagram, uv's part is the lower-right. The installer has 3 new "jobs" in this scheme:

1. Loading a pre-computed local file that represents the desired variants
2. For each user-configured remote index, try to fetch a list of variants that the index has available. This is probably a per-package list of variants, rather than a whole-index list of variants.
3. Preserving the order of the lists, intersect them. The intersection represents the ordered set of variants that match the user's preferences. Each item in the set is a URL that matches PyPI's PEP 503 "details" page, listing the distribution files that are specific to a particular variant.

The end result is basically that each index URL will be "exploded" into several index URLs - one per variant that matches both the user's configured variants and the variants present on the server.

It seems like this is maybe the right location to add this: https://github.com/astral-sh/uv/blob/main/crates/uv-distribution-types/src/index_url.rs#L249 - does that seem right? Maybe it needs to be deeper, if the variant list is per-package rather than global.

I'm concerned about additional HTTP requests being an issue, but otherwise the splitting of one index into many saves the need for changes to the resolver algorithms. I welcome suggestions and input on any of the ideas here. I can be available for a chat or video call if a higher-bandwidth discussion would be helpful.

---

_Comment by @charliermarsh on 2024-10-28 16:51_

Thanks @msarahan. I’m happy to be your point-of-contact on this. Will review and reply later today / tonight.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-28 16:51_

---

_Comment by @charliermarsh on 2024-10-28 19:52_

Ok, I think I understand the overall flow here. I would have much better answers / guidance if I attempted to implement it myself, but I'll do my best.

Assuming that you want to make a separate variant request per package per index, I think I'd suggest confining the logic to the registry client:

https://github.com/astral-sh/uv/blob/7948441121b1d54655caa927b3eaadaf90717327/crates/uv-client/src/registry_client.rs#L205-L210

In there, we could make a request to each index as we visit them in the `for index in it` loop.

If we localize it to the registry client, then we just have to ensure that the registry client itself has access to the user-defined variant manifest (and we can skip these steps entirely if it's not defined). So we need to thread it through to the `RegistryClientBuilder`, like we do for `index_urls`. The use of the builder (and making this optional) will also make it easier for you to get this compiling, since you can add support for (e.g.) a single command (like `uv pip install`) at a time.

To do that threading, you want to get that manifest to be passed in here:

https://github.com/astral-sh/uv/blob/7948441121b1d54655caa927b3eaadaf90717327/crates/uv/src/commands/pip/install.rs#L299-L306

As an aside, can we store the desired variants in a `pyproject.toml` file? Or do you intend for it to be a separate, standalone file? For the prototype at least, it'd be easiest to store them in the `pyproject.toml` file.


---

_Comment by @msarahan on 2024-10-28 23:43_

Thanks for the guidance! I'll clarify that I'll be thrilled if you want to work on a prototype. I didn't want to lay that on you, though. We have an org where we're trying to centralize the many wheel-enhancing efforts: https://github.com/wheel-next

So far it's only NVIDIA people, but we're hoping that others will join as time goes on. If I end up making the uv prototype (that is, if you don't), it'll be at that org so we can demo it, then PR it over here when we all think it's ready.

I'd be happy to chat more about where variants should live. There are 2 or 3 different contexts:
1. How variants get selected and encoded at build time
2. How the variants encoded in the package get read by the hosting index
3. How the end-user client specifies which variant

I'm not clear what you mean by your request that variants live in a pyproject.toml file. I think that might fall under the first context. I haven't really fleshed out that part yet. Our plan is to punt on the build-time questions until the hosting and installation prototypes are shown to work (or not).

If you don't mean the build context, you probably mean the client context. I see variants as a per-environment setting. I think they live more in lockfiles or inputs to lockfiles, like https://peps.python.org/pep-0751/.  We're punting on that until a little later too, but it is definitely an essential part of the overall plan.

---

_Comment by @charliermarsh on 2024-10-29 00:20_

Ah yeah sorry -- I was referring to (3). I think I was assuming that the user would be encoding the variants they want / expect, and then we'd write those to the lockfile after resolving -- so was trying to figure out how they would be provided / specified by the user. But I'm a little fuzzy on that part of the workflow.

---

_Comment by @charliermarsh on 2024-10-31 16:58_

I'm happy to prototype this in uv if you have a server / example registry that implements the spec.

---

_Comment by @msarahan on 2024-10-31 17:35_

Early days, but we have this mockup: https://github.com/wheel-next/mockhouse/

---
