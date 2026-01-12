```yaml
number: 14937
title: "Allow templating the `UV_PROJECT_ENVIRONMENT` path"
type: pull_request
state: open
author: zanieb
labels:
  - configuration
  - preview
assignees: []
base: main
head: zb/template-project-env
created_at: 2025-07-28T12:56:04Z
updated_at: 2025-09-15T22:55:16Z
url: https://github.com/astral-sh/uv/pull/14937
synced_at: 2026-01-12T16:11:30Z
```

# Allow templating the `UV_PROJECT_ENVIRONMENT` path

---

_@zanieb_

I'm looking at supporting centralized environments, and this feels like a nice incremental step towards that. Basically we can re-use `UV_PROJECT_ENVIRONMENT` to empower centralized storage without adding any more settings. Later, we'd add a toggle that enables centralized storage in the cache using our "canonical" recommended naming scheme. At that point, we'd probably explore things like empowering editor discovery via `.venv` pointers and such.

The "canonical" usage would be something like:

```
UV_PROJECT_ENVIRONMENT=$HOME/.local/share/uv/envs/{project_name}-{python_implementation}{python_version_minor}-{project_path_hash}
```

Unfortunately, templating the Python version (which is a common request) is a little annoying because we determine the path to the environment _before_ discovering a Python interpreter, but I got it working.

The only thing that's missing here is documentation, which I can add if there's consensus this is a good path forward.

---

_Label `configuration` added by @zanieb on 2025-07-28 12:56_

---

_Comment by @zanieb on 2025-07-28 16:19_

e.g.,

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv sync
warning: Templating the `UV_PROJECT_ENVIRONMENT` setting is in preview and may change without warning; use `--preview-features template-project-environment` to disable this warning
Using CPython 3.13.2
Creating virtual environment at: /Users/zb/.local/share/uv/envs/example-cpython3.13-cff78d0b34cfcb51
Resolved 1 package in 30ms
Audited in 0.02ms
```

---

_Marked ready for review by @zanieb on 2025-07-29 02:20_

---

_Review requested from @charliermarsh by @zanieb on 2025-07-29 17:14_

---

_Review requested from @konstin by @zanieb on 2025-07-29 17:14_

---

_Comment by @PhilipVinc on 2025-07-29 20:34_

Is there a binary I can test?

---

_Comment by @zanieb on 2025-07-29 20:41_

There are debug binaries attached to the bottom of the CI summary at https://github.com/astral-sh/uv/actions/runs/16572262146?pr=14937

---

_Comment by @charliermarsh on 2025-07-30 01:14_

I think this is pretty neat. I'm supportive of it, if you want to add docs etc.

---

_Comment by @PhilipVinc on 2025-07-30 08:15_

Tested on 3 different HPC setups, and works great, thanks!

The only thing that I feel is missing (but probably it's just because I am not aware of how to do it) is a simple command to programmatically resolve the path to the current environment, something like `uv venv --path` or something...

`uv run python -c 'print(sys.exec_prefix)'` works, but would be handy to have something more 'straightforward.


---

_Comment by @konstin on 2025-07-30 08:43_

Environment variables are already complex and have interactions with e.g. the user's shell and their workspace/deploy path layout, could we skip over the templating the whole path and go directly to centralized env management the way e.g. poetry does it? This avoids exposing this complexity to users. Specifically, the complexity of templating the whole path (not just the leaf directory).

---

_Comment by @zanieb on 2025-07-30 11:49_

> Environment variables are already complex and have interactions with e.g. the user's shell and their workspace/deploy path layout, could we skip over the templating the whole path and go directly to centralized env management the way e.g. poetry does it? This avoids exposing this complexity to users. Specifically, the complexity of templating the whole path (not just the leaf directory).

The goal is to only expose this feature to motivated power users to start, so I don't mind some complexity here while we figure out how to make centralized environments a first-class feature. Do you have an example of what you're concerned about? Would it be resolved by adding a cache bucket and a `{env_cache}` template variable?

---

_Comment by @konstin on 2025-07-30 12:03_

We're introducing a new templating syntax that is only used in this one env var, we're adding a new concept just for this one place. If we want to experiment with centralized storage, why not start with a setting for a centralized root and e.g. a path hash and expand from that?

If users want to use this for global configuration, we should primarily expose this in `uv.toml` over an env var.

---

_Comment by @charliermarsh on 2025-07-30 12:26_

I guess one question is: if we do ship a centralized environment setting, would we continue to support this? If so, why? If not, maybe we just rip it on a setting. (I suspect it’s easy to implement, even easier than this.)

---

_Comment by @zanieb on 2025-07-30 12:50_

> We're introducing a new templating syntax that is only used in this one env var, we're adding a new concept just for this one place

I think this is a fair concern, though I'm not sure it's a big deal to support templating here. I'm not sure where else templating would be needed in uv so far, so it makes sense it doesn't exist as a concept today.

> if we do ship a centralized environment setting, would we continue to support this? If so, why? 

Yeah, I think so. The discussion at https://github.com/astral-sh/uv/pull/14628 made it clear people want control over the path. I initially considered that out of scope for a first pass at the feature, but we probably would want it eventually.

> If users want to use this for global configuration, we should primarily expose this in uv.toml over an env var.

I'm explicitly trying _not_ to add new settings here. I'm not even sure what the top-level setting would look like, I guess some hybrid boolean path option? I think being able to control the root path is critical. We could certainly start with a boolean though.

> If not, maybe we just rip it on a setting. (I suspect it’s easy to implement, even easier than this.)

A setting implies first-class support, which is what I'm trying to avoid getting into here. e.g., how does an editor discover the environment? how do I inspect or clean up environments? etc.

The motivation for using a low-level environment variable is to avoid answering those questions while delivering _some_ functionality now.

---

_Comment by @charliermarsh on 2025-07-30 14:10_

> I'm explicitly trying not to add new settings here. I'm not even sure what the top-level setting would look like, I guess some hybrid boolean path option? I think being able to control the root path is critical. We could certainly start with a boolean though.

(I guess the minimal thing would be an env var and `uv.toml` setting that controls the root path, and then if enabled, use some deterministic path within the root?)

> Yeah, I think so. The discussion at https://github.com/astral-sh/uv/pull/14628 made it clear people want control over the path. I initially considered that out of scope for a first pass at the feature, but we probably would want it eventually.

(I don't find the templating part to be _that_ compelling based on that conversation, but I'm not strictly against it.)

> A setting implies first-class support, which is what I'm trying to avoid getting into here. e.g., how does an editor discover the environment? how do I inspect or clean up environments? etc.

I totally understand the motivation. Is there some risk, though, that by shipping this without solving these problems, we actually make it harder for us to solve them in the future? For example, if the paths are ~arbitrary (user-specified), how could an editor ever discover them? (How does that discovery work in Poetry, etc.?) By shipping this, we impose some constraints on ourselves that we either need to roll back or workaround later.


---

_Comment by @zanieb on 2025-07-30 14:29_

> (I guess the minimal thing would be an env var and uv.toml setting that controls the root path, and then if enabled, use some deterministic path within the root?)

Yeah, but I think we'd also want the root path to have a sensible default? Otherwise, it's kind of annoying since we don't do templating you'll have to hard-code an absolute path that's brittle?

> Is there some risk, though, that by shipping this without solving these problems, we actually make it harder for us to solve them in the future? For example, if the paths are ~arbitrary (user-specified), how could an editor ever discover them? (How does that discovery work in Poetry, etc.?) By shipping this, we impose some constraints on ourselves that we either need to roll back or workaround later.

I'm not sure I mind if users that opt in to arbitrarily complicated paths also need to configure their editor to find them.

I think discovery works in Poetry via editor specific integrations. e.g.,

- https://github.com/microsoft/vscode-python/wiki/Support-poetry-virtual-environments
- https://github.com/microsoft/vscode-python/blob/main/src/client/pythonEnvironments/base/locators/lowLevel/poetryLocator.ts
- https://github.com/microsoft/vscode-python/blob/main/src/client/pythonEnvironments/common/environmentManagers/poetry.ts#L202

which comes down to parsing `poetry env list --full-path` output.

---

_Comment by @zanieb on 2025-07-30 15:08_

I guess I'll defer this and prototype an alternative approach later, my goal here was to pitch a quick idea that I thought might unblock people, but it's not clear we have consensus this is worth it.

---

_Comment by @rsyring on 2025-07-30 15:47_

> The only thing that I feel is missing (but probably it's just because I am not aware of how to do
> it) is a simple command to programmatically resolve the path to the current environment, something
> like uv venv --path or something...

FWIW, I suggested `uv venv --dir` in #14628 b/c it seemed "dir" was the abbr. used in current CLI
options.

> If users want to use this for global configuration, we should primarily expose this in uv.toml
> over an env var.

I've never felt like environment variables are any more or less complex than settings.  Just a
matter of which method makes more sense for setting them.

As a power user who is already hacking together an environment based solution with Mise, either
option works for me.  The only reason I do it in mise w/ an environment variable is because
I need mise to help calculate the project path name.  Obviously, with this PR, all that goes away.
:)

> The motivation for using a low-level environment variable is to avoid answering those questions
> while delivering some functionality now.

Yes, please!  :)

> I guess I'll defer this and prototype an alternative approach later, my goal here was to pitch a
> quick idea that I thought might unblock people, but it's not clear we have consensus this is worth
> it.

I can't speak to the implementation, but I like the proposal and would love to have it sooner
rather than later.  Anything to avoid the janky stuff I do right now with mise to get this working:
https://github.com/jdx/mise/discussions/4301#discussioncomment-13758809

You've marked it as a preview.  Does that give you any more confidence in shipping something like
this?  A more agile approach where you can make decisions based on real-world use instead of 
having to make educated guesses about all of it?

FWIW, mise has an "experimental" flag and puts some functionality behind that so that features that
need usage to get right can still get shipped.  IMO, you wouldn't have to "guard" this features as
it's obviously opt-in by whether or not use use template variables.  I'm just pointing out that
mise is also a really popular project and a lot of times real-world usage and feedback is needed to
get things right.

---

_Label `preview` added by @zanieb on 2025-07-30 15:49_

---

_Comment by @DanCardin on 2025-07-30 19:34_

i cant find the issue but there were discussions about templating auth secrets for private indexes at some point. Whether or not you want to do that idk, but that's at least another theoretical usecase.

What I like about this is that it's completely orthogonal to a more dedicated feature/setting, and I could imagine a future setting being implemented essentially in terms of this feature.

e.g. you might eventually have some
```toml
# settings.toml
managed_venv = true
managed_venv_root = "{venv_cache}" # default setting, say
```
that is essentially like setting a fallback `UV_PROJECT_ENVIRONMENT='{managed_venv_root}/{project_name}-{project_hash}'` or whatever default you want.

And makes straightforward sense that in the presence of a user defined `UV_PROJECT_ENVIRONMENT`, you get what the user asked for, not what the setting says.

Not that this all couldnt be done similarly with a static setting without templating at all, but it **seems** like a lot more thought has to go into ensuring you're making everyone happy with a setting with fixed semantics. versus this, where everyone can, for sure, make the setting do what they want.

---

As far as making this consumable, i feel like `uv venv --print` is probably clearer than --dir/--path. but tools probably also want the ability to list all venvs, so perhaps a subcommand like `info` gives more room to put other options.

---

_Comment by @rsyring on 2025-08-11 14:24_

> I guess I'll defer this and prototype an alternative approach later, my goal here was to pitch a quick idea that I thought might unblock people, but it's not clear we have consensus this is worth it.

Is this dead then?  Should it be closed?

---

_Comment by @marcelroed on 2025-08-11 19:16_

While this doesn't solve editor/tool environment discovery, my understanding is that discovery is already broken by setting `UV_PROJECT_ENVIRONMENT`. This solution seems to me to be a very unobtrusive way to implement centralized environment functionality through an interface that has already been stabilized.

Centralized envs are a highly desired feature by anyone using a cluster of machines where project source is stored on a network drive, which is all of HPC right now, and I think using an environment variable is totally fine for this use case. In the future, if there's a central configuration variable for a centralized environment path, it seems fine to me to override that with whatever is set in `UV_PROJECT_ENVIRONMENT`.

Are there any other potential downsides to this templating approach? To me, this appears to be pretty elegant and solves the issue without breaking anything that's not already broken by `UV_PROJECT_ENVIRONMENT`.

---

_Comment by @PhilipVinc on 2025-08-26 13:02_

I would like to reiterate that this PR by @zanieb was solving thery well all the issues raised in #1495 and it would be a shame not to see it or an alternative implemented in uv...

---

_Comment by @marcelroed on 2025-09-15 22:55_

Any updates on this direction? I still think this is a great non-disruptive way of solving this somewhat urgent issue.

---
