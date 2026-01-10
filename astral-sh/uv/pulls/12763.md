```yaml
number: 12763
title: "Support `--with-requirements script.py` and `-r script.py` to include inline dependency metadata from another script"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: uvx-with-requirements-scripts
created_at: 2025-04-08T21:08:47Z
updated_at: 2025-09-05T16:46:02Z
url: https://github.com/astral-sh/uv/pull/12763
synced_at: 2026-01-10T06:44:32Z
```

# Support `--with-requirements script.py` and `-r script.py` to include inline dependency metadata from another script

---

_Pull request opened by @blueraft on 2025-04-08 21:08_

## Summary

Closes #6542 

## Test Plan

`cargo test`


---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4384 on 2025-04-08 21:46_

I'm not sure how we'll want to reframe the documentation here.

Maybe we want something like..

> Run with all packages listed in the given requirements files.
>
> The following formats are accepted:
>
> - `requirements.txt`-formatted files
> - `pyproject.toml`
> - `setup.cfg` and `setup.py`
> - `.py` files with PEP 723 metadata

I'm not sure if we want to enumerate those everywhere? Are they all supported everywhere?

---

_@zanieb reviewed on 2025-04-08 21:46_

---

_@zanieb reviewed on 2025-04-08 21:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:91 on 2025-04-08 21:47_

We'll probably want to support PEP 723 scripts as requirements sources everywhere if we support them somewhere. It's probably reasonable to start with `uv run` and `uv tool run`? but we'll probably want to consider other use-cases too. I'd be surprised if it was just supported in `uv tool run`.



---

_@zanieb reviewed on 2025-04-08 21:50_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4386 on 2025-04-08 21:50_

Separately from the documentation, we need to make sure we want `--with-requirements` to support this format. It means we won't be able to read requirements from a PEP 723 script without a `.py` extension (which is more common than I expected) unless we want to start sniffing for the metadata block? Maybe that's reasonable?

Another option is like `--with-script`, which only really applies to `uv run` / `uv tool run` (i.e., we'd need something else in `uv pip install` like `--script <path>` instead of `-r <path>`).

---

_Comment by @zanieb on 2025-04-08 21:51_

That was more straight-forward than I assumed! I think we'll need to align on the interface.

---

_@zanieb reviewed on 2025-04-08 21:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:91 on 2025-04-08 21:55_

Bringing `uv run` into the picture does complicate things a little, like `uv run --with-script foo.py $EDITOR` — I guess we are supposed to layer on top of the project environment there but then we can't use the stable script environment path?

@sharkdp I would be curious how you expect this to behave / what your dream interface is.

---

_Comment by @T-256 on 2025-04-09 00:55_

- Also consider it for `uv run`: https://github.com/astral-sh/uv/issues/7032

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:4386 on 2025-04-09 12:53_

I like the `--with-script` option. The `.py` extension issue would be gone and  makes it easier to add similar options to other commands later. Better than having `--with-requirements` act slightly differently in different places.

---

_@blueraft reviewed on 2025-04-09 12:53_

---

_@blueraft reviewed on 2025-04-09 12:55_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:4384 on 2025-04-09 12:55_

This is the help text for `uv pip install -r`, where `pyproject.toml` is also supported

```bash
  -r, --requirements <REQUIREMENTS>            Install all packages listed in the given `requirements.txt` files
```

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4384 on 2025-04-09 13:32_

Looks outdated :) we can probably update those holistically separately if you prefer.

---

_@zanieb reviewed on 2025-04-09 13:32_

---

_@jtfmumm reviewed on 2025-04-09 13:41_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4386 on 2025-04-09 13:41_

Would `--with-script <script>` mean we're just using the requirements from `<script>` or that we're running it? For the former, I think something like `--with-script-requirements` would be less confusing (I'd read `uv run --with-script` to mean we're running that script).

---

_@Gankra reviewed on 2025-04-09 13:45_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4386 on 2025-04-09 13:45_

Given every other format has a specific filename(?) we could just have a rule that if none match we assume it's a pep723 script. I like that at least `--with-requirements` seems to more imply the script is being invoked, so if we want a separate argument it would be nice to preserve that (`--script` or `--with-script` is further from that).

---

_@blueraft reviewed on 2025-04-09 13:46_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:4384 on 2025-04-09 13:46_

yeah I can make a separate PR for that!

---

_@zanieb reviewed on 2025-04-09 13:56_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4386 on 2025-04-09 13:56_

> Would --with-script <script> mean we're just using the requirements from <script> or that we're running it?

It'd mean we're using the requirements for it. I suggested `--with-script-requirements` in another thread but worry it's too verbose :/ that's why I leaned towards the option of overloading `--with-requirements`.

> Given every other format has a specific filename(?)

We actually already fall-through like this for the `requirements.txt` format, since those names are frequently arbitrary.

>  I like that at least --with-requirements seems to more imply the script is being invoked,

The script isn't being invoked though.

Note we already have `--script` for `uv run`. It takes no additional arguments, but specifies that the "command" we receive should be parsed as a script path and executed as such.

---

_@sharkdp reviewed on 2025-04-09 19:17_

---

_Review comment by @sharkdp on `crates/uv/src/commands/tool/run.rs`:91 on 2025-04-09 19:17_

What I wanted to do originally was to run an (external) *tool*, with the virtual env. (including the requirements defined by a PEP 723 script) "activated". So exactly what you indicated above for `$EDITOR`, just with arbitrary tools (like a type checker that needs to be aware of all script dependencies in order to type check it correctly):
```bash
uv run --with-script foo.py <tool>
```

I think I do *not* want `uv tool run --with-script foo.py …`, which, in my limited understanding, would do something different: I guess it would allow me to run a tool *from* the requirements of `foo.py`, but without having the virtual env. of the project itself "activated".

I'm afraid I don't know enough about uv's CLI design goals to give any advise on how the command line option should be called. But given how often I wanted this option, I'd personally be in favor of something shorter. `--with-script` seems very reasonable to me. If it becomes `--with-requirements script.py`, I'd also be happy with that, but I think I might have had more difficulty discovering it in the `--help` text, as I would have intuitively searched for something with "script" in the name.

---

_@sharkdp reviewed on 2025-04-09 19:23_

---

_Review comment by @sharkdp on `crates/uv/src/commands/tool/run.rs`:91 on 2025-04-09 19:23_

I think this new option would be more generally useful if I could not just replace a "script runner" (as indicated in [this comment](https://github.com/astral-sh/uv/issues/6542#issuecomment-2307709992)), but control the full command line of the tool that is being called, even if that means that I have to repeat `foo.py` twice. So `uv run --with-script foo.py tool --option` would simply call `tool --option` without implicitly passing `foo.py` as a positional argument (`tool --option foo.py`).

---

_@Gankra reviewed on 2025-04-09 19:56_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4386 on 2025-04-09 19:56_

(oh whoops typo: meant to say `--with-requirements` implies the script *isn't* being invoked)

---

_@zanieb reviewed on 2025-04-09 20:11_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:91 on 2025-04-09 20:11_

> So uv run --with-script foo.py tool --option would simply call tool --option without implicitly passing foo.py as a positional argument (tool --option foo.py).

Totally agree we shouldn't do this — that was an original use-case that I think is too narrow.

> I think I do not want uv tool run --with-script foo.py …, which, in my limited understanding, would do something different: I guess it would allow me to run a tool from the requirements of foo.py, but without having the virtual env. of the project itself "activated".

If we're being consistent with todays semantics:

`uv tool run --with-script foo.py <tool>` would: install your script dependencies with the `<tool>` and its dependencies, invoke `<tool>`. The environment would be isolated from the project (today, that's how `uv tool run` works — though we could change it to layer on top of the project environment in the future so, e.g., `uv tool run mypy` works)

`uv run --with-script foo.py <tool>` would: install the script dependencies into a virtual environment, layer that virtual environment on top of the project environment (if in a project), then invoke `<tool>`. You could use `--no-project` to ignore the project dependencies.



---

_@sharkdp reviewed on 2025-04-10 17:39_

---

_Review comment by @sharkdp on `crates/uv/src/commands/tool/run.rs`:91 on 2025-04-10 17:39_

That sounds great

---

_Comment by @zanieb on 2025-05-15 14:06_

@blueraft I wonder if we can push this forward? I think support in `--with-requirements` might make sense still, given we read a bunch of other file types there. I wonder if we can just sniff for a PEP 723 inline metadata entry in the script if there's no file extension? 

---

_Comment by @blueraft on 2025-05-15 15:52_

> I wonder if we can just sniff for a PEP 723 inline metadata entry in the script if there's no file extension?

Yeah, I think we could do that. Let me give that a try! 

---

_Review requested from @zanieb by @blueraft on 2025-05-19 14:01_

---

_@gesslerpd reviewed on 2025-06-25 16:46_

---

_Review comment by @gesslerpd on `crates/uv/src/commands/tool/run.rs`:91 on 2025-06-25 16:46_

This is great, stumbled upon this while trying to use `uv export` to implement a similar setup.

> 
> `uv run --with-script foo.py <tool>` would: install the script dependencies into a virtual environment, layer that virtual environment on top of the project environment (if in a project), then invoke `<tool>`. You could use `--no-project` to ignore the project dependencies.
> 
> 

Not sure a good syntax or extra flag for this but it would be nice if there was a way to opt into the `--with-script` arg being passed last but then specifying a `<tool>` earlier so this feature can be used directly in shebangs w/o wrappers.

E.g.

```
#!/usr/bin/env -S uv run --tool pytest --with-script
```

Effectively all "with script" args are passed as positional args to the tool in this mode.
 
So resulting command is equivalent to `uv run --with-script <script> <tool> <script>`

This might be difficult if extra args are required to be passed to `<tool>` but thought I'd throw idea out there.



---

_Assigned to @zanieb by @zanieb on 2025-07-28 13:16_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1178 on 2025-08-15 13:35_

I'm sorry if this is something we've talked about before (trying to pick this back up now), but why are we doing all this here instead of in `RequirementsSource::from_requirements_file`? It feels weird to parse these eagerly then filter them from the subsequent call?

---

_@zanieb reviewed on 2025-08-15 13:35_

---

_@blueraft reviewed on 2025-08-15 14:01_

---

_Review comment by @blueraft on `crates/uv/src/lib.rs`:1178 on 2025-08-15 14:01_

As far as I can tell, `RequirementsSource::from_requirements_file` is used in a bunch of places, but we may not want to support reading requirements from a PEP723 script in all of those instances. I guess we could pass a flag to that function whether to accept PEP723 scripts or not but no strong preference there

---

_@zanieb reviewed on 2025-08-15 14:04_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1178 on 2025-08-15 14:04_

I see. I think I probably would expect it to support PEP 723 scripts everywhere, e.g., `--with-requirements <path>` being equivalent to `pip install -r <path>` and `pip compile <path>`. I wonder if there are edge-cases where we _wouldn't_ want it? Like `-r script.py` inside a `requirements.txt` file? Do we already allow `-r pyproject.toml` inside `requirements.txt` files? If so, I guess this would be fine?

---

_@blueraft reviewed on 2025-08-15 14:27_

---

_Review comment by @blueraft on `crates/uv/src/lib.rs`:1178 on 2025-08-15 14:27_

> `--with-requirements <path>` being equivalent to `pip install -r <path>`

Does this mean we would also support `uv pip install -r script.py`?

> Do we already allow -r pyproject.toml inside requirements.txt files?

This doesn't seem to be supported

```
❯ cat requirements.txt
-r pyproject.toml
flask
❯ uv pip install -r requirements.txt
error: Error parsing included file in `requirements.txt` at position 0
  Caused by: Unexpected '[', expected '-c', '-e', '-r' or the start of a requirement at pyproject.toml:1:1
```

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1178 on 2025-08-15 14:37_

> Does this mean we would also support uv pip install -r script.py?

Yeah, I think so. I think it'd be inconsistent otherwise, and I'm not sure why we shouldn't support it.

> This doesn't seem to be supported

Thank goodness :) though I'm confused by that error message 🤔 

---

_@zanieb reviewed on 2025-08-15 14:37_

---

_Converted to draft by @blueraft on 2025-08-15 16:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:125 on 2025-08-18 17:58_

```suggestion
                bail!("Adding requirements from a PEP 723 script is not supported in `uv add`");
```

---

_@zanieb reviewed on 2025-08-18 17:58_

---

_@zanieb reviewed on 2025-08-18 17:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:125 on 2025-08-18 17:59_

 We probably could support this fwiw — it seems less confusing than importing from another project. It seems good to wait until there's a clear use-case though.

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:190 on 2025-08-18 18:00_

I'd probably omit the `run ... to initialize` hint. That belongs outside the error message. The dependencies would be empty too, so it'd have no meaningful effect for this kind of usage.

---

_@zanieb reviewed on 2025-08-18 18:00_

---

_@zanieb reviewed on 2025-08-18 18:00_

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:199 on 2025-08-18 18:00_

Similarly, I'd omit the `run ...` here.

---

_Review comment by @zanieb on `crates/uv-requirements/src/sources.rs`:16 on 2025-08-18 18:06_

We should stylize this consistently
```suggestion
    /// Dependencies were provided via a PEP 723 script.
```

---

_@zanieb reviewed on 2025-08-18 18:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2716 on 2025-08-18 18:07_

I wonder if we should just say "script metadata tag"? I generally don't want the specification to be a user-facing concept and prefer to refer to "inline script metadata".

---

_@zanieb reviewed on 2025-08-18 18:07_

---

_@blueraft reviewed on 2025-08-18 19:36_

---

_Review comment by @blueraft on `crates/uv-requirements/src/specification.rs`:190 on 2025-08-18 19:36_

Done

---

_Renamed from "Support `--with-requirements script.py` to include inline dependency metadata from another script" to "Support `--with-requirements script.py` and `-r script.py` to include inline dependency metadata from another script" by @blueraft on 2025-08-18 19:38_

---

_Marked ready for review by @blueraft on 2025-08-18 19:38_

---

_@zanieb reviewed on 2025-08-18 21:32_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1803 on 2025-08-18 21:32_

We don't wrap PEP 723 in backticks

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:190 on 2025-08-18 21:33_

```suggestion
                            "`{}` does not contain inline script metadata",
```
Just bike-shedding the message, but note we don't end with a period for single sentence errors..

---

_@zanieb reviewed on 2025-08-18 21:33_

---

_@blueraft reviewed on 2025-08-19 08:17_

---

_Review comment by @blueraft on `crates/uv-requirements/src/specification.rs`:190 on 2025-08-19 08:17_

Done

---

_@zanieb approved on 2025-09-05 16:45_

---

_Merged by @zanieb on 2025-09-05 16:45_

---

_Closed by @zanieb on 2025-09-05 16:45_

---

_Comment by @zanieb on 2025-09-05 16:45_

Thank you so much for your patience here!

---

_Label `enhancement` added by @zanieb on 2025-09-05 16:46_

---
