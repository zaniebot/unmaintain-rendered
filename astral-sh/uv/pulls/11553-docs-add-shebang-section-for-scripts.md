```yaml
number: 11553
title: "[docs] Add shebang section for scripts"
type: pull_request
state: merged
author: sa-
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-02-16T10:55:08Z
updated_at: 2025-04-16T16:43:10Z
url: https://github.com/astral-sh/uv/pull/11553
synced_at: 2026-01-10T11:10:38Z
```

# [docs] Add shebang section for scripts

---

_Pull request opened by @sa- on 2025-02-16 10:55_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Documentation only. Adds a section in scripts.md about running uv scripts with a shebang line

## Test Plan

n/a


---

_Renamed from "[docs] Add shebang sectiong for scripts" to "[docs] Add shebang section for scripts" by @sa- on 2025-02-16 10:55_

---

_Assigned to @zanieb by @zanieb on 2025-02-16 16:36_

---

_Comment by @sa- on 2025-02-17 16:29_

CI now passes :)

---

_Comment by @c14n on 2025-02-19 13:54_

While these changes are now formally correct, they are unfortunately factually incorrect.

The proper shebang line should be

```
#!/usr/bin/env -S uv run --script
```

The use of the `-S` flag for `env` is required due to the way spaces are handled in shebang lines. The example given in the patch will simply not work. For details, refer to

1. https://www.gnu.org/software/coreutils/env
2. «Use in shell-scripts» on https://man.freebsd.org/cgi/man.cgi?env 

The use of the `--script` flag for `uv` is required to prevent infinite recursion, as seen in #11220.

Moreover, if the latter flag is used, the extension of the script can be chosen arbitrarily, or even omitted; it does not have to be `.py`.


---

_Comment by @sa- on 2025-02-20 05:39_

@c14n Thanks for taking the time, I've incorporated your feedback

---

_Comment by @c14n on 2025-02-20 09:51_

Notice that I am not a maintainer, so the following are just the observations of some random passer-by.

---

There are still some issues with this pull request, chief of which being that the changes do not mesh well with the surrounding material.

This is due to the fact that you do not adhere to the [style guide](https://github.com/astral-sh/uv/blob/main/STYLE.md), and do not try to match the structure of the surrounding material.

Notice how every section on this page of the documentation adheres to this pattern:

    This is an brief introductory paragraph that explains what you can expect
    to accomplish by following the advice in this section. After the colon 
    terminating this sentence, we give you example code:
    
    ```python title="example.py"
    # Here is the code that is absolutely necessary to illustrate the point, 
    # and no more. 
    ```
    
    ```console
    $ command-to-run-the-example-must-have-dollar-sign-before
    Output. Does not start with a dollar sign.
    ```

Do you see how focused and concise that approach is?

My advice: Read the [style guide](https://github.com/astral-sh/uv/blob/main/STYLE.md) and try to match the form of the existing material. (This also applies to commit messages.)

---

There are also some issues with the contents of the patch. I'll add inline comments to point those out.

---

_@c14n reviewed on 2025-02-20 09:52_

---

_Review comment by @c14n on `docs/guides/scripts.md`:223 on 2025-02-20 09:52_

An executable script with a shebang can be run from anywhere, if you provide the full path to it.

---

_@c14n reviewed on 2025-02-20 09:53_

---

_Review comment by @c14n on `docs/guides/scripts.md`:228 on 2025-02-20 09:53_

This is the important part of the example. Why do you distract from this with inline script dependencies?

A plain `print()` statement should probably be enough.

---

_@c14n reviewed on 2025-02-20 09:57_

---

_Review comment by @c14n on `docs/guides/scripts.md`:244 on 2025-02-20 09:57_

Does this need to be explained this explicitly? According to the style guide, guides

> May assume basic knowledge of the domain.

so it should be enough to mention that a script should have its execute bit set in the introduction.

---

_@c14n reviewed on 2025-02-20 10:03_

---

_Review comment by @c14n on `docs/guides/scripts.md`:244 on 2025-02-20 10:03_

How about this for an introductory paragraph:

    On Unix-like systems, you can turn your script into a standalone executable by setting its execute
    bit with `chmod +x standalone.py` and adding a shebang interpreter directive at the start of your
    script:
    
    ```python title="standalone.py"
    #!/usr/bin/env -S uv run --script
   
    print("Hello, standalone!")
    ```


---

_Comment by @EliteTK on 2025-02-28 13:39_

`env -S` is not portable. Please consider at least mentioning this in the documentation.

---

_Comment by @c14n on 2025-02-28 16:00_

Seeing that FreeBSD, coreutils, and OS X have supported for `env -S` for at least five years, I'd also argue that it is portable enough for all practical purposes.

Moreover, the [style guide](https://github.com/astral-sh/uv/blob/main/STYLE.md#guides) indicates that guides

> 8. Should not enumerate all possibilities.

and in particular

> 10. May generally ignore platform-specific behavior.

So I don't think this needs to be mentioned. 

---

_Comment by @EliteTK on 2025-02-28 16:17_

I think those points in the style guide don't seem entirely relevant here given the entire point it using env is portability between systems.

If anything, `env -S` is the platform specific behavior.

While I am glad `env -S` works for your limited set of purposes, there are many places where `env -S` doesn't work. The following is a list of cases that I know of:

* Busybox (common in containers)
* Toybox (used on android but can be used just like busybox)
* OpenBSD (where I most commonly encounter this problem)
* NetBSD (ditto)
* Solaris (and OpenIndiana)

I think it would be best to consider some addition to uv which makes it possible to not need -S. (e.g. a uv-run symlink)

In the meanwhile I feel like the docs should, when recommending an approach to solving a portability problem (which this is), at least mention in passing that the solution is itself non-portable.

---

_Comment by @zanieb on 2025-02-28 17:59_

It seems dubious that we'll introduce another entry point to uv specifically for shebangs.

I think it's reasonable for the guide to focus on major platforms, i.e., Ubuntu, macOS, Windows. It's also fine to say that before the example. Is there a viable alternative on those distributions? 

Long-term, we need to split some of the scripts guide into a concept document so we can cover things like this in-depth without adding complexity for beginners.

---

_Comment by @EliteTK on 2025-02-28 21:46_

Regarding alternatives for different operating systems and linux environments that don't use GNU coreutils and would also work on systems with coreutils:

You can put a script in your path, called `uv-run` for lack of a better name, with the contents:

```sh
#!/bin/sh
exec uv run --script "$@"
```

Then you can use this from a shebang like `#!/usr/bin/env uv-run`. The downside is that now alongside with installing UV, you need to install this wrapper on any system where you want this style of "works out of the box" python scripts to work. (This is why, if uv wants to lean into this "easy shebang solution to all your python woes", a dedicated uv-run would be beneficial.) 

I would also agree that this alternative requires too much explanation to be documented by these docs. But, if the argument is that these kinds of details are too much for these "beginner" oriented docs then I question if it's even appropriate to document the solution in this PR either.

The selling point of this style of script which has been made when presenting uv as useful in this context (as something which can be executed from the shebang) is that you can put this at the top of a shell script and distribute the shell script across various people or contexts and that it will just work. But the reality is that there are caveats like I outlined above. You can say that OpenBSD, NetBSD, and Solaris are niche use-cases and I agree for the most part but a lot of people do use containers and are likely to run into the `env -S` issue in those cases. (Although let me be clear, I really like this use-case and that's why I would love to see it supported explicitly by uv without needing a wrapper script for it to be portable.)

And at the end of the day, regardless of what direction the project wants to take on this topic or this PR, all I was hoping for with my first comment was some note either at the top or the bottom to say: "Note: `-S` is a GNU extension, and this may introduce portability concerns for your use-cases." or something to that effect. I wasn't trying to suggest that there needs to be some long-form discussion of `-S` and alternatives. There is already information on that topic on the internet (and if there isn't, I would put it on my blog rather than asking for such unrelated information to be included in the uv docs).

---

_Comment by @zanieb on 2025-02-28 22:02_

Thanks for the thorough response, appreciate it.

---

_Comment by @c14n on 2025-03-02 23:30_

I choose to believe that users can generally be trusted to consult the page «Using uv in Docker» rather than «Running scripts» when attempting to containerize their software. Therefore, I think it's appropriate to restrict the guidance provided on the latter page to the use cases that do not involve containerization.

I also think that the official docs should only provide guidance on how to use the software for platforms for which they also provide installation instructions. At time of writing, the Unix-likes that are supported in that sense are only macOS and Linux, both of which support `env -S` in the most common setups.

The introduction of a wrapper script to facilitate running scripts on platforms that fail to implement `env -S` should probably be discussed in a separate feature request. We should probably wait with this PR until a definite decision on that topic has been made.

Personally, I doubt that the introduction of such a wrapper is particularily useful for containerization, seeing that adding one to your image is a four-liner:

```DOCKERFILE
COPY --chmod=755 <<EOF /usr/bin/envess
#!/bin/sh
exec uv run --script "$@"
EOF
```

Finally, guides should strive to provide solutions. Even if it were a good idea to include documentation for unsupported platforms, I would object to a note that reads

> Note: -S is a GNU extension, and this may introduce portability concerns for your use-cases.

How does that help the reader of the guide accomplish their goal, should they be on a system that does not support that flag? (Also FreeBSD introduced `-S` over a decade before coreutils, hence the support in macOS. So why single GNU out?)

---

_Comment by @EliteTK on 2025-03-03 19:51_

> I choose to believe that users can generally be trusted to consult the page «Using uv in Docker» rather than «Running scripts» when attempting to containerize their software. Therefore, I think it's appropriate to restrict the guidance provided on the latter page to the use cases that do not involve containerization.

Containerising software is not the situation I was thinking of when I suggested people would run into this limitation of `-S`. One of the primary goals of using `#!/usr/bin/env` is to have working one-off scripts without needing to modify the shebang for every environment. This goal is partially defeated when needing to use `-S` since it means your one-off scripts now stop working in some environments. This problem isn't solved by reading the documentation on containerisation.

> I also think that the official docs should only provide guidance on how to use the software for platforms for which they also provide installation instructions. At time of writing, the Unix-likes that are supported in that sense are only macOS and Linux, both of which support `env -S` in the most common setups.

I came across the docs without ever seeing or reading the installation section. I think it's likely people will come across the page on using uv in scripts directly from search engines (which is how I came across the documentation for `uv run` in the first place). From what I can tell, the docs so far would for the most part work on any system on which you've successfully managed to install uv (which is likely any system with a working pip and rust installation). If this one page is going to rely on this assumption that you've installed it on a system where env supports -S then I don't see a good reason why it's harmful to just mention that in passing rather than saying "unix like" which is a very broad term and is inaccurate.

> The introduction of a wrapper script to facilitate running scripts on platforms that fail to implement `env -S` should probably be discussed in a separate feature request. We should probably wait with this PR until a definite decision on that topic has been made.
>
> Personally, I doubt that the introduction of such a wrapper is particularily useful for containerization, seeing that adding one to your image is a four-liner:
> 
> ```dockerfile
> COPY --chmod=755 <<EOF /usr/bin/envess
> #!/bin/sh
> exec uv run --script "$@"
> EOF
> ```

I was answering a question, I explicitly said I wasn't suggesting it be included in any part of these docs. But worth clarifying that I wasn't thinking of dockerfiles as the main case where people would encounter this issue. More-so any situation where one might either enter the container to run a one-off script. The obvious workaround is to just do `uv run --script <script>` but presumably one of the goals of the shebang was to avoid that.

> Finally, guides should strive to provide solutions. Even if it were a good idea to include documentation for unsupported platforms, I would object to a note that reads
> 
> > Note: -S is a GNU extension, and this may introduce portability concerns for your use-cases.
> 
> How does that help the reader of the guide accomplish their goal, should they be on a system that does not support that flag? (Also FreeBSD introduced `-S` over a decade before coreutils, hence the support in macOS. So why single GNU out?)

I was mainly trying to keep it short, and I didn't realise it originated in FreeBSD but I can see that you are in fact right. So maybe it's best to just say it's a common extension which is not supported on all environments.

Either way, yes, guides should guide people to solutions but it's also important for guides to be up-front about the limitations of the solutions. In this case it's easy to assume, especially given the surrounding wording, that if you use `#!/usr/bin/env -S uv run --script` that you will get a script which can be ran on any environment where `uv` is installed. I think it would be helpful to dispel this potential misunderstanding with some note, the exact wording isn't important to me.

---

_Comment by @zanieb on 2025-03-17 23:30_

I'll try to get this in this week.

---

_Comment by @hyperknot on 2025-03-19 14:12_

I also come here to say that please embrace the uv shebang. This is currently the best way to work with Python scripts, yet it's totally unofficial / you have to ask on Discord level hidden.

I recommend the following:
1. Please agree on an official uv shebang line. Just in this thread there are quite a few alternatives. I'm personally using 
```
#!/usr/bin/env -S uv run
```

2. Advertise it everywhere, in the FAQ, in the uv init scripts, etc.

3. Modify ruff EXE003 rule to allow this.

---

Also, please don't limit the 99.999% of developers's use case, just because there is an incompatibility with Busybox, Toybox, OpenBSD, NetBSD or Solaris. Those users with exotic environments can use any other shebang line, the important thing would be to make development experience smooth for the majority of users.








---

_Comment by @sa- on 2025-03-19 14:14_

CI failing looks unrelated

---

_Comment by @sa- on 2025-04-05 06:38_

Could someone please retrigger the CI pipeline? My PR has nothing to do with the GraalPy error

---

_Comment by @zanieb on 2025-04-16 15:10_

Thanks for your patience here!

---

_Label `documentation` added by @zanieb on 2025-04-16 15:11_

---

_Merged by @zanieb on 2025-04-16 15:20_

---

_Closed by @zanieb on 2025-04-16 15:20_

---

_Comment by @hyperknot on 2025-04-16 15:40_

Thanks for merging this. I just wanted to ask one Q: the other day I run into that if I make a project with `uv init`, I need to have the shebang without --script, or otherwise it cannot find the local lib folders.

I mean the ones where I put __init__.py and utility functions, etc. I use this in pyproj:

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["lib"]
```

So for the script files **in projects** it has to be without --script. For the script files outside of projects, like self-contained ones with 
```
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "requests",
# ]
# ///
```

these scripts absolutely need `--script`.

Either this needs to be fixed in uv, or the documentation should mention this. Of course, it'd be best if we could get rid of --script altogether.

---

_Comment by @zanieb on 2025-04-16 15:46_

`--script` is needed because without it we use `.py` to sniff if `uv run` is given a script. The documentation uses `--script`, what do you want to change here?

---

_Comment by @zanieb on 2025-04-16 15:46_

Ah I see

> So for the script files in projects it has to be without --script. 

I can add a note about that.

---

_Comment by @zanieb on 2025-04-16 15:49_

I don't know if this is true? in a project which has `httpx` as a dependency...

```
❯ cat example
#!/usr/bin/env -S uv run --script

import httpx

print(httpx.get("https://example.com"))
❯ uv run --script example
Installed 8 packages in 8ms
<Response [200 OK]>
```

---

_Comment by @hyperknot on 2025-04-16 16:43_

I mean importing from my own utility library. But I tried it today and I cannot reproduce it, but yesterday I run into it.
Once I run into it again, I'll try to make a minimal repro case and post it here.


---
