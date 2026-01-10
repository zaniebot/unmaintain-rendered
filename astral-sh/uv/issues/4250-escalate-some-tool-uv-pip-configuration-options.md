```yaml
number: 4250
title: "Escalate some `tool.uv.pip` configuration options to the project API"
type: issue
state: closed
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
created_at: 2024-06-11T19:41:40Z
updated_at: 2024-06-14T00:56:39Z
url: https://github.com/astral-sh/uv/issues/4250
synced_at: 2026-01-10T05:31:37Z
```

# Escalate some `tool.uv.pip` configuration options to the project API

---

_Issue opened by @charliermarsh on 2024-06-11 19:41_

Right now, we don't read anything from `tool.uv.pip` in `uv lock`, `uv sync`, or `uv run`.

We either need to read from `tool.uv.pip`, _or_ read from `tool.uv.pip` but prune it down, _or_ escalate some options to `tool.uv`.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-11 19:41_

---

_Label `configuration` added by @charliermarsh on 2024-06-11 19:41_

---

_Label `preview` added by @charliermarsh on 2024-06-11 19:41_

---

_Comment by @charliermarsh on 2024-06-12 15:08_

There are a few options here with different considerations, like:

- We may want to evolve the configuration schema for the Project API over time in a way that doesn't fit the pip API (e.g., we might want to change the arguments to `only-binary`, or the way that indexes are managed, but it's nice to have those match pip's interface for `uv pip`).
- It could be confusing if `uv sync` and `uv pip install` behaved differently (e.g., if they read from different indexes).

I genuinely have no idea what to do here because these create conflicting goals: we _might_ want to change the schema in the future for the Project API, but if we do that, then by definition we lose unified configuration across the Project and pip APIs.

Some options...

### Duplicate a subset of the `tool.uv.pip` arguments under `tool.uv.project`.

This would give us the flexibility to evolve the schemas entirely independently. The downside is that you'd need to repeat any configuration between the two, if you have a workflow that requires use of both APIs.

If we choose to add new configuration to the Project API in the future, then we go through a normal deprecation pattern: deprecate (e.g.) `tool.uv.project.only-binary`, and add a new thing.

### Read the `tool.uv.pip` arguments from the Project API.

Then, `pip.index-url` would "just work" for all invocations.

If we decide to evolve the `only-binary` schema in the future, we could add new configuration options under `tool.uv.project` and deprecate reading from `tool.uv.pip` (e.g., ignore `pip.only-binary` if you have configuration defined under `project`). But then we have divergent configuration that isn't respected across the two commands, so it just puts us back in the same position as above... However, it's forwards-compatible: we can add new settings in the future once we've worked out the APIs we want.

### Escalate a subset of the `tool.uv.pip` arguments to `tool.uv`.

This also has the nice property that you can define your configuration once. But it has the downside that if we later add some configuration that is specific to the project API, it's not clear from the schema that those APIs _don't_ apply to the pip API.


---

_Comment by @charliermarsh on 2024-06-12 15:15_

If it's likely that configuration will deviate between these two APIs, then we should do the first thing or even the second thing for now (and then deprecate reading from `tool.uv.pip` in the future).

If it's _unlikely_, and we want to commit to maintaining compatibility and unified configuration between the Project and pip APIs, then I think we do the third thing.


---

_Comment by @zanieb on 2024-06-12 15:34_

Yeah, I think `only-binary` is a great example of something I don't want to support in the way that `pip` does and a very compelling reason to do something different, e.g. I think we should use `no-build-package`/ `no-build` instead which matches our other options. Index configuration is similar, in that pip's options are very ambiguous but different in that we don't have a clear design for a better interface (and creating one will take time) so we probably need to support `index-url` and `extra-index-url` then discourage use in the future.

I feel quite strongly that reading from `tool.uv.pip` outside of `uv pip` would be very confusing. It blurs the boundary of the namespace. How would we frame this to users? "uv supports reading pip-compatible options from the `uv.pip` section"?

I think a mix of the first and third options makes the most sense to me.

- Options that are needed in the project API and overlap with the pip API today should be placed in the top-level namespace. These options can use different syntax than supported by pip. We'll continue to support the `uv.pip`-level declarations if we change the syntax but those will only affect `uv pip`. If we keep the same syntax, should we deprecate the `uv.pip` option?
- Options that are needed in the project API but should not be used in the pip API and could be construed to apply to the pip API should probably go in the `uv.project` namespace. If it's pretty obvious that they don't apply to the pip API, I feel like they could go in the top-level. I don't have a good example of this yet.
- Options that are needed in the project API _and_ another new API like the tool API should probably go in the top-level regardless of their relevant to the pip API. There may be some confusion if they don't apply to the pip API but duplicating things across `uv.project` and `uv.tool` seems even worse?

A compelling case for me here is: if you configure `uv.pip` at the user-level then the workspace-level `uv` top-level options should clearly take precedence for all the new uv commands. I'd be really confused if `uv sync` used `uv.pip` options in this scenario.


---

_Comment by @charliermarsh on 2024-06-12 15:57_

Ok, for now, how about this:

- We move anything that's needed globally up to the top-level. For now, it would be roughly these:

```
index_url: Option<IndexUrl>,
extra_index_url: Option<Vec<IndexUrl>>,
no_index: Option<bool>,
find_links: Option<Vec<FlatIndexLocation>>,
index_strategy: Option<IndexStrategy>,
keyring_provider: Option<KeyringProviderType>,
resolution: Option<ResolutionMode>,
prerelease: Option<PreReleaseMode>,
config_settings: Option<ConfigSettings>,
exclude_newer: Option<ExcludeNewer>,
link_mode: Option<LinkMode>,
compile_bytecode: Option<bool>,
```

- When we move these to the top-level, we will deprecate the variants in `tool.uv.pip`. We might just remove them entirely and bump to `v0.3.0`. Or we'll add a deprecation warning for now.
 
- If we need project-specific configuration, we will introduce `tool.uv.project`. We don't have an example of this yet though. Maybe like: "The Python requirement for locking that's separate from my declared `Requires-Python`" would be one.

- We'll add `no_build` and `no_binary` settings to the top-level that match the APIs we want. We'll continue to support the variants under `tool.uv.pip` for now although we may deprecate them as persistent configuration.


---

_Comment by @zanieb on 2024-06-12 16:04_

> We might just remove them entirely and bump to v0.3.0. Or we'll add a deprecation warning for now.

I'd avoid a deprecation-free breaking change since people are very likely to be dependent on these options in production.

> Maybe like: "The Python requirement for locking that's separate from my declared Requires-Python" would be one.

Yeah great example.

The proposed list looks great, my only concern is around the index options — I think these are pretty likely to change compared to the rest. I wonder if we should isolate them into a `uv.network` / `uv.index` / `uv.package-index` section? Would that be weird? Similarly, should `keyring_provider` be moved to `uv.authentication`? I guess this generally raises the question of whether or not we should be introducing setting categories while we're moving things to avoid a future deprecation cycle.


---

_Comment by @charliermarsh on 2024-06-12 16:06_

Maybe to help inform our answer: what happens in the future if we change the index API? Do we move `index-url` back under `tool.uv.pip`?

---

_Comment by @zanieb on 2024-06-12 16:14_

I find it challenging to say because I'm not sure how the `index-url` API would change. Do you have a realistic example? I usually imagine it changing in the project `sources` instead. I find it hard to imagine moving anything _back_ to the `tool.uv.pip` interface but if we see that as a feasible situation then instead of

> When we move these to the top-level, we will deprecate the variants in `tool.uv.pip`.

I would consider just leaving the `tool.uv.pip` variants in perpetuity and they'll take precedence over `tool.uv` when using the `uv pip` interface. I feel like moving something out then back into `tool.uv.pip` would be too much whiplash. We might want to leave these variants around forever so we have the freedom to change our top-level configuration without worrying about this. I think it's a good goal to have the `uv pip` interface be very stable and let the top-level interface move quickly.



---

_Comment by @charliermarsh on 2024-06-12 16:16_

> I usually imagine it changing in the project sources instead.

I am imagining something whereby you define named indexes in one section (like `tool.uv.index`), and then can point to them by name in `tool.uv.sources` to pin a package to an index. But you could also define a given index in `tool.uv.index` as, e.g., the primary index.


---

_Comment by @charliermarsh on 2024-06-12 16:17_

Ok, maybe the thinking is something like this, then:

- We leave all those things in `tool.uv.pip`.
- We escalate some of the global settings to `tool.uv` (or perhaps `tool.uv.index` and subsections).
- The pip API respects the global settings but prefers the `tool.uv.pip` settings.

---

_Comment by @zanieb on 2024-06-12 16:18_

Ah okay that's really helpful, so I guess I see that story going something like this:

- Support `tool.uv.index-url` and `tool.uv.pip.index-url`
- Add `tool.uv.indexes` for some new named index declaration
- Deprecate `tool.uv.index-url` and eventually remove
- Support `tool.uv.pip.index-url` forever

---

_Comment by @charliermarsh on 2024-06-12 16:19_

That... seems right.

---

_Closed by @charliermarsh on 2024-06-14 00:56_

---
