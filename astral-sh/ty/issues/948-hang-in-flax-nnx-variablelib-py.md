```yaml
number: 948
title: Hang in flax/nnx/variablelib.py
type: issue
state: closed
author: marcelroed
labels:
  - needs-info
  - fatal
  - memory
assignees: []
created_at: 2025-08-06T17:18:49Z
updated_at: 2025-08-09T00:41:37Z
url: https://github.com/astral-sh/ty/issues/948
synced_at: 2026-01-10T02:06:24Z
```

# Hang in flax/nnx/variablelib.py

---

_Issue opened by @marcelroed on 2025-08-06 17:18_

### Summary

<img width="1426" height="397" alt="Image" src="https://github.com/user-attachments/assets/cb251375-4e77-4db5-8866-8389dfec12ce" />

Hi! I just updated to the latest ty version and updated my VSCode plugin, resulting in a huge memory leak and my home server going down. [Minimal repro](https://gist.github.com/dcreager/fc53c59b30d7ce71d478dcb2c1c56444) (thanks @dcreager).

The memory keeps ticking up until it takes 100% of system memory and the OS tries to kill it. This kill also failed, however, so maybe there should be a safety check to limit `ty`'s memory usage to something reasonable.

I'm on Ubuntu 24.04 and running Python 3.13. The issue can also be reproduced on macOS.

### Version

ty 0.0.1-alpha.17 (1712284 2025-08-06)

---

_Comment by @carljm on 2025-08-06 17:27_

If you run the latest version of ty from the CLI on that codebase, does the issue repro?

---

_Label `needs-info` added by @carljm on 2025-08-06 17:27_

---

_Label `hang` added by @carljm on 2025-08-06 17:27_

---

_Label `hang` removed by @MichaReiser on 2025-08-06 17:29_

---

_Label `fatal` added by @MichaReiser on 2025-08-06 17:29_

---

_Label `memory` added by @MichaReiser on 2025-08-06 17:29_

---

_Comment by @marcelroed on 2025-08-06 17:50_

Yes, it also does, but only on Linux. I was able to get some more information:

https://github.com/erfanzar/EasyDeL on current main (da583c435c529c1f98d6a0324d40d7d609ccba4d) produces this issue consistently for me on Ubuntu 24.04, but not on MacOS.

Running with verbose, I see that it hangs (and increases in memory usage) at least with this file/command:

```sh
RAYON_NUM_THREADS=1 uvx ty check -vvvvv easydel/layers/quantization/linear_nf4.py
```

Here is the last of the trace:

```
  2025-08-06T17:49:28.992919Z DEBUG ty_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:327 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-08-06T17:49:28.993065Z DEBUG ty_python_semantic::module_resolver::resolver: Adding editable installation to module resolution path /home/marcel/projects/levanter/src
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:404 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-08-06T17:49:29.035212Z DEBUG ty_python_semantic::module_resolver::resolver: Module `jaxlib._jax` not found in search paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:74 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check
```

---

_Comment by @MichaReiser on 2025-08-06 17:51_

Wow, nice find. Any chance you could share the content of that file (or a smaller repro of it)? Are there any special types you're using (are they recursive)? You might also get better results with a debug build of ty, because we strip some traces from production builds (maybe we shouldn't?)

I know, it's a lot to ask, I'm sorry.

---

_Comment by @marcelroed on 2025-08-06 18:00_

Did a little more digging, and it seems like the issue is with an import from Flax: https://github.com/google/flax (reproduced on commit 220910f83201427d49ffe9954277ae1aed90921d). Running in that repo gives the exact same issue.

```sh
marcel@fred:~/projects/flax$ RAYON_NUM_THREADS=1 uvx -p 3.13 ty check -vvvvv
```

Here's the smallest repro I currently have in the Flax repo:
```sh
RAYON_NUM_THREADS=1 uvx -p 3.13 ty check -vvvvv tests/nnx/transforms_test.py
```

Removing `from flax import nnx` from that file prevents the issue from happening, so I suspect the issue is somewhere in there.

I can try this with a debug build of `ty` tonight.

---

_Comment by @MichaReiser on 2025-08-06 18:04_

That's a nice find. Regarding linux/mac os. I wonder if that is because we default sys.platform to the hosts platform (I hope that's it. A platform specific bug would e very annoying to narrow down 🥲)

We can pick the investigation up from here but I also don't want to stop you. 

---

_Comment by @marcelroed on 2025-08-06 18:07_

Yeah, I need to tap out for time reasons, but hopefully this is helpful! I'll check back on this later today and help out a bit if it's still unsolved by then.

---

_Comment by @marcelroed on 2025-08-06 18:19_

Nevermind, it was super quick to build and run the debug version of `ty`.

Here's what I'm seeing when I get the loop:

```
  2025-08-06T18:19:02.518095Z TRACE ty_project::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_expression_type(Id(1952)) } }
    at crates/ty_project/src/db.rs:57 on ThreadId(2)
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(194e) range=24114..24124 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=24106..24111 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(194f) range=24132..24157 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1947) range=23863..23873 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=23855..23860 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(194b) range=23966..23986 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1940) range=23611..23621 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=23603..23608 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1941) range=23629..23655 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1939) range=23350..23360 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=23342..23347 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(193a) range=23368..23397 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1932) range=23086..23096 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=23078..23083 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1933) range=23104..23133 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(192b) range=22831..22841 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=22823..22828 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(192c) range=22849..22875 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1924) range=22579..22589 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=22571..22576 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1925) range=22597..22623 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(191d) range=22312..22322 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=22304..22309 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(191e) range=22330..22361 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1916) range=22043..22053 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=22035..22040 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1917) range=22061..22091 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(190f) range=21778..21788 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=21770..21775 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1910) range=21796..21825 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1908) range=21523..21533 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=21515..21520 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1909) range=21541..21567 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1901) range=21271..21281 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=21263..21268 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1902) range=21289..21315 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(18fa) range=21019..21029 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=21011..21016 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(18fb) range=21037..21063 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(18c1) range=16711..16722 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=16703..16708 file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::infer::infer_scope_types with scope=Id(102c) file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_python_semantic::types::check_types with file=File(System("/home/marcel/projects/flax/flax/nnx/variablelib.py"))
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check
```

It seems like the source of the issues is `flax/nnx/variablelib.py`. New minimal repro is:
```sh
uvx ty check flax/nnx/variablelib.py
```
within https://github.com/google/flax.

Edit: This also happens on macOS, so I think the platform-dependency was a false alarm. The reason why it seemingly was different on different platforms was because I hadn't installed the dependencies when running my macOS test.

---

_Comment by @carljm on 2025-08-06 20:48_

> until it takes 100% of system memory and the OS tries to kill it. This kill also failed, however

@MichaReiser is there anything actionable in this part of the report? Shouldn't we still be responsive to signals if in a deep Salsa query explosion?

---

_Renamed from "Large memory leak/infinite loop" to "Hang in flax/nnx/variablelib.py" by @carljm on 2025-08-06 21:12_

---

_Comment by @marcelroed on 2025-08-07 04:40_

@carljm I'm guessing one of the threads got stuck during I/O? That's the only case where I've seen no response to OS signals in the past. Not sure why I/O would be happening in this infinite loop, though.

---

_Comment by @MichaReiser on 2025-08-07 07:55_

> @MichaReiser is there anything actionable in this part of the report? Shouldn't we still be responsive to signals if in a deep Salsa query explosion?


Technically yes. 

We have a separate thread that listens for CTRL+C

https://github.com/astral-sh/ruff/blob/6516db78353c8abd2c030ec6dbc0c2adfb37e07c/crates/ty/src/lib.rs#L140-L146

Which the main loop handles here

https://github.com/astral-sh/ruff/blob/6516db78353c8abd2c030ec6dbc0c2adfb37e07c/crates/ty/src/lib.rs#L378-L382

Now, there can be multiple reasons why ty wouldn't exit:

* The main loop is stuck, e.g. because it is currently rendering diagnostics (that's something we should fix but I don't think it's what's happening here)
* `db.trigger_cancellation();` Sets the salsa cancellation token and it waits for each thread holding a db (all check file operations) to complete (or abort with a panic that we gracefully handle). Now, this only works if the threading code checks if the cancellation token was set. This should happen automatically whenever fetching or validating a salsa query. However, ty will not respond if the stuck code never fetches any salsa query



---

_Comment by @MichaReiser on 2025-08-07 08:11_

The expression mentioned at the top of the tracing stack is: 

https://github.com/google/flax/blob/220910f83201427d49ffe9954277ae1aed90921d/flax/nnx/variablelib.py#L807-L815

Specifically

https://github.com/google/flax/blob/220910f83201427d49ffe9954277ae1aed90921d/flax/nnx/variablelib.py#L808-L809



---

_Comment by @MichaReiser on 2025-08-07 08:25_

I bisected this to https://github.com/astral-sh/ruff/pull/19604 CC: @dcreager 

---

_Comment by @dcreager on 2025-08-08 14:29_

This looks like a combinatorial explosion. I was able to create a [minimal repro](https://gist.github.com/dcreager/fc53c59b30d7ce71d478dcb2c1c56444). That file has 13 different in-place dunder methods, all with the same form. If I remove most of them (it doesn't matter which ones), `ty` completes just fine. But once we get to a certain point, running time and memory usage blows up:

```
  1    0.035s
  2    0.048s
  3    0.202s
  4    2.10s
  5   24.37s
  6  [hangs]
```

---

_Comment by @MichaReiser on 2025-08-08 14:30_

Could this be related to the combinatory explosion we're seeing with overload expansion

---

_Comment by @dcreager on 2025-08-08 15:08_

> Could this be related to the combinatory explosion we're seeing with overload expansion

I _think_ it's unrelated, but I will keep that in mind as I dig into it!

---

_Comment by @carljm on 2025-08-08 17:33_

This looks likely to be an explosion due to all the bindings of `self.value` that ultimately depend on the type of `self.value`, which will probably lead to a massive cycle in inferring the type of `self.value`. Not sure if the explosion is inherently related to the typevars, or if we could construct a similar example without typevars (using e.g. `self: Self` instead) that would similarly explode even before astral-sh/ruff#19604.

---

_Comment by @erictraut on 2025-08-08 18:02_

I ran into the same problem in pyright. In rare cases, I've seen thousands of assignments to a single (unannotated) variable. I try not to be too judgmental about code that pyright users include in bug reports, but these cases were among some of the worst code I've ever seen. :) It should come as no surprise that inference gets very expensive in this case — especially when there are complex and expensive dependencies between the assigned expressions. 

The solution I adopted was a bit of a hack. I added [a threshold](https://github.com/microsoft/pyright/blob/8b02aa84ec916697e76d5ee69299a89d555ec98c/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L556) beyond which pyright gives up attempting to infer the type. It falls back to `Unknown` in that case. 

---

_Comment by @dcreager on 2025-08-08 18:10_

> This looks likely to be an explosion due to all the bindings of `self.value` that ultimately depend on the type of `self.value`, which will probably lead to a massive cycle in inferring the type of `self.value`.

This does seem to be the root cause, though in this particular case the typevars do exacerbate the issue, since we need to infer the signature of the containing function to determine which generic context binds the `V` typevar, which is part of the implicit attribute's type.

It seems like it's enough to add an additional cycle handler around `implicit_attribute`. (At least for this case!)

> I've seen thousands of assignments to a single (unannotated) variable.

@erictraut, do you happen to have the pyright issue handy? If the code is public I'd love to test out my fix on it to see at what point this approach stops working.




---

_Closed by @dcreager on 2025-08-08 21:01_

---

_Comment by @dcreager on 2025-08-08 21:02_

I confirmed that with the fix from https://github.com/astral-sh/ruff/pull/19833, we can analyze flax quickly again.

---

_Comment by @erictraut on 2025-08-09 00:41_

> do you happen to have the pyright issue handy

Sorry, I can't find the particular examples. It was many years ago. If I remember correctly, it was something like this:

```python
class Test:
    def __init__(self):
        self.x = 1

    def method(self, c: bool):
        if cond:
            self.x += 1
        if cond:
            self.x += 1
        if cond:
            self.x += 1
        if cond:
            self.x += 1
        if cond:
            self.x += 1
        ... # Copy another several hundred times
```

---
