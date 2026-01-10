```yaml
number: 6318
title: Make it possible to lock dependencies in a script
type: issue
state: closed
author: blin
labels:
  - enhancement
  - help wanted
  - needs-design
assignees: []
created_at: 2024-08-21T11:32:12Z
updated_at: 2025-01-29T14:50:48Z
url: https://github.com/astral-sh/uv/issues/6318
synced_at: 2026-01-10T03:50:30Z
```

# Make it possible to lock dependencies in a script

---

_Issue opened by @blin on 2024-08-21 11:32_

Uv's script support is amazing for creating self-contained scripts that can be written once and then executed by anyone with uv months later, as long as the upper boundaries for dependencies were specified.

Being able to lock the dependencies for a script would be a huge boon in script preservation, all the same arguments that apply to `uv lock` and `pip-tools compile` apply here.

Preserving original requirement specifications would be beneficial, so after running `uv add --script example.py 'requests<3' 'rich'` and `uv lock --script example.py` the `example.py` will contain something like (which is basically the result of extracting dependencies into requirements.txt, running uv pip compile and pasting the result back into the script):

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "requests<3",
#     "rich",
# ]
# compiled_dependencies = [
#     "certifi==2024.7.4",
#     "charset-normalizer==3.3.2",
#     "idna==3.7",
#     "markdown-it-py==3.0.0",
#     "mdurl==0.1.2",
#     "pygments==2.18.0",
#     "requests==2.32.3",
#     "rich==13.7.1",
#     "urllib3==2.2.2",
# ]
# ///
```

---

_Label `enhancement` added by @charliermarsh on 2024-08-21 13:07_

---

_Comment by @charliermarsh on 2024-08-21 13:07_

This is really cool. We can write it under `tool.uv`.

---

_Label `help wanted` added by @charliermarsh on 2024-08-21 15:37_

---

_Comment by @charliermarsh on 2024-08-23 18:18_

I think this should be opt-in (something in the script's `[tool.uv]` section, maybe?), and we should probably write the lockfile at the bottom, since it could be large.

---

_Label `needs-design` added by @zanieb on 2024-09-05 00:25_

---

_Comment by @SummerGram on 2024-09-19 15:39_

Is anyone working on it?

---

_Comment by @zanieb on 2024-09-19 15:47_

No, we need to design it. Are you interested in helping with that?

---

_Comment by @SummerGram on 2024-09-20 16:53_

Hi, @zanieb

I do not see the information about the compiled_dependencies in https://packaging.python.org/en/latest/specifications/inline-script-metadata/#inline-script-metadata

Where can I search more information?

---

_Comment by @ketozhang on 2024-09-22 00:00_

PEP 723 has a sentence on this

> A script runner may support injecting of dependency resolution data for an embedded lock file (this is what Go’s gorun can do).
> --https://peps.python.org/pep-0723/#why-not-limit-tool-configuration

I do not interpret this as a recommendation, but it's definitely thought of when this was designed.

For me, this solution isn't as useful as I like it to be in the use case of "a repository of python scripts (not packaged)". Adopting inline script metadata is still very difficult as not many of my of my users use `uv` or a 723-compatible runner. External lock files would support a wider compatibility. `pip` not supporting 723 makes this problem worse.

---

_Comment by @marengaz on 2024-09-22 05:48_

https://peps.python.org/pep-0723/#how-to-teach-this

It also mentions that tool blocks are allowed. If we dumped the lock file contents into some tool.uv.xxx property there, I guess the benefit would be 2-fold; it gives non-uv-users a hint that their life would be easier with UV, gives us free reign to decide the name and number of properties we need to accomplish this

---

_Comment by @SummerGram on 2024-09-24 17:11_

I think `uv lock --script example.py` should provide an option to let users select the file to define dependencies. The default value should be the standard `pyproject.toml`. I am not sure if this command generate the `requirements.txt`.

https://docs.astral.sh/uv/pip/compile/

It mentions that uv allows dependencies to be locked in the `requirements.txt` format. 

I think it is confused for the users to manually modify the locked environments if uv creates both the `compiled_dependencies` field and `requirements.txt` file. What is the uv suggested method to modify the locked environments?

---

_Comment by @zanieb on 2024-09-24 17:17_

> should provide an option to let users select the file to define dependencies

The point is that the dependencies are defined in the script per PEP 723 — I don't think we'd support other things here.

---

_Comment by @zanieb on 2024-09-24 17:19_

> Adopting inline script metadata is still very difficult as not many of my of my users use uv or a 723-compatible runner. External lock files would support a wider compatibility.

I think using `uv pip compile` would be the recommendation then — not the `uv lock` format (until PEP 751 is done). I think `uv pip compile --script <path>` would be fine to support for that purpose.

---

_Comment by @zanieb on 2024-09-24 17:20_

> If we dumped the lock file contents into some tool.uv.xxx property there...

The only problem with this is that the lockfile is quite long, so we probably don't want it at the top of the script.

---

_Comment by @ketozhang on 2024-09-24 19:25_

> I think `uv pip compile --script <path>` would be fine to support for that purpose.

I am in support of this. How do you see it integrate with `uv run` to use the lock file? 

---

_Comment by @zanieb on 2024-09-24 19:27_

`uv run --with-requirements requirements.txt script.py` probably already works, though I'm honestly not sure what happens if there's PEP 723 metadata in there.

---

_Comment by @SummerGram on 2024-09-25 13:33_

> I think using `uv pip compile` would be the recommendation then — not the `uv lock` format (until PEP 751 is done). I think `uv pip compile --script <path>` would be fine to support for that purpose.

Which one will `uv pip compile --script example.py` compile the `requirements.in` file to? The bottom of the inline script metadata in the example.py or the lock file?

---

_Comment by @Halkcyon on 2024-11-20 21:17_

I just want to add a voice _against_ having _another_ file next to the script, since if you're going to do that, why not make it a `pyproject.toml` file, toss in a `uv.lock` and call it a project?

The whole benefit of PEP 723 is keeping the script self-contained, in my opinion.  I'll voice something that happens with other tools that embed metadata is it is _appended_.  What I'm thinking about here is signing of, e.g., PowerShell scripts where the signature is attached at the end.  I don't have knowledge of how `uv` is parsing code today to get the header in the first place to know whether it would be difficult to read a footer instead for locked dependencies.

To get this behavior today, I've been using `uv export` and dumping that into the `dependencies` value to get consistent resolutions.

---

_Comment by @zanieb on 2024-11-20 21:26_

I'm sort of all for just having a way to write this metadata at the bottom. We're sort of in a tough place w.r.t. the specification though. It says things like:

> When there are multiple comment blocks of the same TYPE defined, tools MUST produce an error.

> Tools MUST NOT read from metadata blocks with types that have not been standardized by this PEP or future ones.

These feel relatively prohibitive towards embedding the lockfile in the bottom, though I don't think it's a firm blocker.

I think there's some benefit to locking scripts in projects without embedding the metadata so we can improve resolve / execute times for scripts and have consistent dependencies — but that's sort of a separate idea.

---

_Comment by @Halkcyon on 2024-11-20 21:30_

Maybe `uv lock` could replace the `dependencies` block with resolutions, with varying levels of specificity (e.g., whether to include hashes or not, exactly pinned versions, et al.)?  I think there's some acknowledgement that we're editing these scripts with tools and they can fold the metadata header if the script needs to be changed.  This would fit within the spec, but could lead to confusing CLI experiences for people authoring the scripts, but the users of those scripts should at least be happy (which is my goal, working as a dev within a devops team).

---

_Comment by @hauntsaninja on 2024-11-20 22:41_

If you want like 90% of the benefits of a lock file in a PEP 723 script in a way that is also extremely concise, you can use https://docs.astral.sh/uv/guides/scripts/#improving-reproducibility

---

_Comment by @zanieb on 2024-11-20 22:57_

I think rewriting the `dependencies` section would be too detrimental to the user experience.

There are caveats to `exclude-newer`, like private indexes and proxies may not include publish times, but yeah that's a good note.

---

_Comment by @ketozhang on 2024-11-21 20:34_

> why not make it a pyproject.toml file, toss in a uv.lock and call it a project?

@Halkcyon You would have to make sure all scripts' dependencies are cross-compatible. That's a big sacrifice from PEP-723 & uv where I can run each script in its separate virtual environment.

---

_Comment by @charliermarsh on 2024-11-23 01:56_

@zanieb -- What about...

- `uv lock --script` to lock a script (and we write the lockfile to the bottom)
- `uv run --script` will read from and/or update the lockfile if it's present at the bottom (but won't create one if it doesn't exist)

---

_Comment by @zanieb on 2024-12-11 21:46_

I'm into that interface, yeah. What about `uv add --script` — won't create?

---

_Comment by @schrockn on 2024-12-21 14:25_

+1 to this feature! Was surprised it wasn't already there. 

I would recommend `.uv.script_name.lock` or `.script_name.uv.lock` as a naming convention for a lockfile for `script_name.py`.

As a convention so you 1) per-script lock specifications and 2) don't super long lock specifications inline within a file. If you want a single file I think https://docs.astral.sh/uv/guides/scripts/#improving-reproducibility is sufficient and the full lock files should be an advanced feature.



---

_Comment by @zanieb on 2024-12-21 16:58_

There's also a case for putting them in `<workspace-root>/.uv/locks` or `$(dirname <script>)/.uv/` so they don't clutter your directories. Then we could avoid the awkward leading `.` (which I think is much easier on the eyes for file names without extensions).

> If you want a single file I think https://docs.astral.sh/uv/guides/scripts/#improving-reproducibility is sufficient 

This is pretty fair.

---

_Comment by @schrockn on 2024-12-21 17:18_

I think what was wrong with my original suggestion was the leading `.`. These files should be checked in I think (just like vanilla `uv.lock` files). Could be in some directory too to hide the noise as your say.

And thank for all your work on this spectacular tool!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-25 19:18_

---

_Closed by @charliermarsh on 2025-01-08 18:36_

---

_Closed by @charliermarsh on 2025-01-08 18:36_

---

_Comment by @martimlobao on 2025-01-29 09:59_

It sounded like the conversation here was leaning towards including the lockfile at the bottom, but the current implementation produces a separate lockfile (with no option to generate the lockfile inline afaict). Is the goal still to also have the option/change the behavior to inject the lockfile at the bottom of the script or is this the current implementation final?

My two cents is that having a single file for a script would be very advantageous, and coupling this with a `#!/usr/bin/env -S uv run --script` [shebang](https://x.com/charliermarsh/status/1884448591617384688) would allow for fully executable, reproduceable, and portable Python scripts that only need `uv` to be installed.

---

_Comment by @zanieb on 2025-01-29 14:29_

It was easier to put it elsewhere — but I'm generally in favor of support an embedded lockfile too.

---

_Comment by @charliermarsh on 2025-01-29 14:43_

I personally think the embedded lockfile is somewhat impractical -- just way too big. But we can probably support it.

---

_Comment by @zanieb on 2025-01-29 14:50_

Let's discuss further in #11064 

---
