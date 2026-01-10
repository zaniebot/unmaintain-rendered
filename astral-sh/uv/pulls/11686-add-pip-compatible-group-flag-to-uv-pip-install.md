```yaml
number: 11686
title: "add pip-compatible `--group` flag to `uv pip install` and `uv pip compile`"
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/pip-groups-again
created_at: 2025-02-21T05:02:32Z
updated_at: 2025-03-17T18:44:13Z
url: https://github.com/astral-sh/uv/pull/11686
synced_at: 2026-01-10T11:10:38Z
```

# add pip-compatible `--group` flag to `uv pip install` and `uv pip compile`

---

_Pull request opened by @Gankra on 2025-02-21 05:02_

This is a minimal redux of #10861 to be compatible with `uv pip`.

This implements the interface described in: https://github.com/pypa/pip/pull/13065#issuecomment-2544000876 for `uv pip install` and `uv pip compile`. Namely `--group <[path:]name>`, where `path` when not defined defaults to `pyproject.toml`.

In that interface they add `--group` to `pip install`, `pip download`, and `pip wheel`. Notably we do not define `uv pip download` and `uv pip wheel`, so for parity we only need to implement `uv pip install`. However, we also support `uv pip compile` which is not part of pip itself, and `--group` makes sense there too.

----

The behaviour of `--group` for `uv pip` commands makes sense for the cases upstream pip supports, but has confusing meanings in cases that only we support (because reading pyproject.tomls is New Tech to them but heavily supported by us). **Specifically case (h) below is a concerning footgun, and case (e) below may get complaints from people who aren't well-versed in dependency-groups-as-they-pertain-to-wheels.**


## Only Group Flags

Group flags on their own work reasonably and uncontroversially, except perhaps that they don't do very clever automatic project discovery.

a) `uv pip install --group path/to/pyproject.toml:mygroup` pulls up `path/to/project.toml` and installs all the packages listed by its `mygroup` dependency-group (essentially treating it like another kind of requirements.txt). In this regard it functions similarly to `--only-group` in the rest of uv's interface.

b) `uv pip install --group mygroup` is just sugar for `uv pip install --group pyproject.toml:mygroup` (**note that no project discovery occurs**, upstream pip simply hardcodes the path "pyproject.toml" here and we reproduce that.)

c) `uv pip install --group a/pyproject.toml:groupx --group b/pyproject.toml:groupy`, and any other instance of multiple `--group` flags, can be understood as completely independent requests for the given groups at the given files.


## Groups With Named Packages

Groups being mixed with named packages also work in a fairly unsurprising way, especially if you understand that things like dependency-groups are not really supposed to exist on pypi, they're just for local development.

d) `uv pip install mypackage --group path/to/pyproject.toml:mygroup` much like multiple instances of `--group` the two requests here are essentially completely independent: pleases install `mypackage`, and please also install `path/to/pyproject.toml:mygroup`.

e) `uv pip install mypackage --group mygroup` is exactly the same, but this is where it becomes possible for someone to be a little confused, as you might think `mygroup` is supposed to refer to `mypackage` in some way (it can't). But no, it's sourcing `pyproject.toml:mygroup` from the current working directory.


## Groups With Requirements/Sourcetrees/Editables

Requirements and sourcetrees are where I expect users to get confused. It behaves *exactly* the same as it does in the previous sections but you would absolutely be forgiven for expecting a different behaviour. *Especially* because `--group` with the rest of uv *does* do something different.

f) `uv pip install -r a/pyproject.toml --group b/pyproject.toml:mygroup` is again just two independent requests (install `a/pyproject.toml`'s dependencies, and `b/pyproject.toml`'s `mygroup`).

g) `uv pip install -r pyproject.toml --group mygroup` is exactly like the previous case but *incidentally* the two requests refer to the same file. What the user wanted to happen is almost certainly happening, but they are likely getting "lucky" here that they're requesting something simple.

h) `uv pip install -r a/pyproject.toml --group mygroup` is again exactly the same but the user is likely to get surprised and upset as this invocation actually sources two different files (install `a/pyproject.toml`'s dependencies, and `pyproject.toml`'s `mygroup`)! I would expect most people to assume the `--group` flag here is covering all applicable requirements/sourcetrees/editables, but no, it continues to be a totally independent reference to a file with a hardcoded relative path.

------

Fixes https://github.com/astral-sh/uv/issues/8590
Fixes https://github.com/astral-sh/uv/issues/8969

---

_@Gankra reviewed on 2025-02-21 15:35_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/operations.rs`:77 on 2025-02-21 15:35_

I started doing a cleanup pass to compute a `BTreeMap<PathBuf, Vec<GroupName>>` that would be used by subsequent steps but it was just more code and value threading for no real benefit over the current impl that just does a `filter(group.path == path)` later.

---

_Comment by @henryiii on 2025-02-23 14:25_

FYI, pip support was merged.

---

_@zanieb reviewed on 2025-02-24 20:05_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:05_

Are these conflicts present in the `pip` implementation?

---

_@zanieb reviewed on 2025-02-24 20:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:07_

(it doesn't really look like it?)

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:08_

I thought `-r` was exclusive to our CLI (I didn't check the others though).

---

_@Gankra reviewed on 2025-02-24 20:08_

---

_@zanieb reviewed on 2025-02-24 20:12_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:12_

Yeah `-r` is exclusive to us, but I'd expect `pip install -e . --group foo` and `pip install anyio --group foo` to work unless they explicitly choose for it not to upstream.

---

_@zanieb reviewed on 2025-02-24 20:12_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:12_

(Whether `-r` should have the same semantics as these other options, I'm not sure)


---

_@zanieb reviewed on 2025-02-24 20:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:14_

One problem is that `uv pip install -r pyproject.toml --extra foo` is the "correct" use of `--extra` but `uv pip install -r pyproject.toml --group foo` would be invalid? Even more confusing, if we allow it, `uv pip install -r foo/pyproject.toml --group bar` would install `bar` from `./pyproject.toml` instead of `foo/pyproject.toml`.

---

_@Gankra reviewed on 2025-02-24 20:20_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:20_

Yeah that's part of why I opted to disallow it. The upstream pip semantics are *really* confusing to pair with anything else.

---

_@zanieb reviewed on 2025-02-24 20:22_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1592 on 2025-02-24 20:22_

I think it's important that we're compatible with pip on whether `--group` can be used with `<package>` and `--editable` though, which introduces all the same complexities.

---

_Comment by @Gankra on 2025-02-27 04:53_

I implemented/pushed the following changes, because I needed to wrap my head around them in code/tests to think them through semantically:

* totally unrestrict `--group` from all other sources in `uv-pip-install`
  * semantically i still have it that `--group` just installs that group in a pyproject.toml (still hardcoded ./pyproject.toml default for now, see below). All the following cases are accepted:
     * "unrelated": `uv pip install somepackage --group bar` (essentially sugar for two separate `uv pip install` invocations as it is in upstream `pip install`)
     * "nice": `uv pip install -r pyproject.toml --group bar` (installs both the package's deps and its group bar) (but this is the user of the cli potentially getting "lucky")
     * "confusing": `uv pip install -r foo/project.toml --group bar` (installs `foo/pyproject.toml`'s deps and `./pyproject.toml:bar`)
  * implementation-wise this was a bit hairy as we have to kind of detect if we're doing `--group` or `--only-group` by looking at all the other inputs, but it ends up making sense (really it's always `--only-group-but-additive-only`... it's just an artifact of the internal source_tree stuff and wanting to dedupe files that we need to do anything complex).
* enable the same semantics for `uv-pip-compile`
  * ...this one I "implemented" and then I went to test it and realized that the semantics for pip-install are absolute nonsense here, i think? i think the old-old-old impl from last month where the --group just applies to every selected source is what you want here..? I'm not totally sure, I don't have a good intuition of what uses of this would look like... but I get the impression that `uv pip compile --group pyproject.toml:foo` is essentially gibberish? Or am I wrong?
* did not add project discovery in lieu of hardcoding `pyproject.toml` -- I was *going* to do this but then I realized that nowhere in the pip interface do we ever seem to invoke project discovery? Which was surprising. This seems like a big semantic departure if we start doing that? At very least needs to be discussed more.
  

---

_Comment by @Gankra on 2025-02-27 15:59_

Zanie convinced me that pip-compile is fine actually, so I just loosened the CLI restriction to make it accepted.

I'm just adding more tests now.

---

_Renamed from "add pip-compatible `--group` flag to `uv pip install`" to "add pip-compatible `--group` flag to `uv pip install` and `uv pip compile`" by @Gankra on 2025-02-27 21:29_

---

_Comment by @zanieb on 2025-03-03 20:21_

Can you share the current state of this?  It looks like you updated the PR summary to cover the behavior we discussed?

---

_Comment by @Gankra on 2025-03-04 03:46_

I believe the PR is in the final state I'd like. Possibly it could have Even More tests but I'm not sure it would add much. (Just rebased)

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9064 on 2025-03-04 20:46_

I have a personal preference (which is not matched by all of the test suite) to prefix test cases with the command e.g., `pip_install_dependency_group` so you can just use name matching for running a subset of tests.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1026 on 2025-03-04 20:46_

```suggestion
    /// Install the specified dependency group from a `pyproject.toml`.
```

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1028 on 2025-03-04 20:47_

```suggestion
    /// If no path is provided, the`pyproject.toml` in the working directory is used.
```

---

_Review comment by @zanieb on `crates/uv-normalize/src/group_name.rs`:87 on 2025-03-04 20:47_

nit:
```suggestion
/// The pip-compatible variant of a [`GroupName`]
```

---

_Review comment by @zanieb on `crates/uv-normalize/src/group_name.rs`:94 on 2025-03-04 20:49_

It seems better to use `Option<PathBuf>` here so

1. We can distinguish between the explicit `pyproject.toml` and the inferred one, e.g., for error messages
2. We can do more advanced project discovery later

---

_Review comment by @zanieb on `crates/uv-normalize/src/group_name.rs`:111 on 2025-03-04 20:51_

This isn't particularly consistent with our styling.

I might say...

> The `--group` path is required to end in `pyproject.toml` for compatibility with `pip`; got: {path}`

---

_Review comment by @zanieb on `crates/uv-normalize/src/group_name.rs`:128 on 2025-03-04 20:51_

nit: I'd err towards moving this out of the if/else?

---

_Review comment by @zanieb on `crates/uv-normalize/src/group_name.rs`:150 on 2025-03-04 20:54_

Another reason that `Option` may be helpful here? Though I think it's fine either way, it'd be confusing to show a different thing than the user provided in subsequent messaging.

---

_Review comment by @zanieb on `crates/uv-requirements/src/source_tree.rs`:132 on 2025-03-04 20:56_

Should this use `{name}` for consistency with elsewhere? Should we use `path.user_display()` to show relative paths to the working directory?

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1131 on 2025-03-04 20:58_

I find this description confusing. Isn't this just

> Include the following dependency groups.


---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:15311 on 2025-03-04 21:01_

It might be nice to have a comment before these test cases about what you're covering? Like

> // Compile the project dependencies and a dependency group

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-04 21:02_

This could be separate, but... should we be emitting annotations for this? e.g.
```suggestion
    iniconfig==2.0.0
        # via group (pyproject.toml:bar)
    typing-extensions==4.10.0
        # via project (pyproject.toml)
```

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-04 21:04_

As an aside, these would be so helpful for reading these test cases :D

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:15988 on 2025-03-04 21:06_

I think this test is missing coverage for `uv pip compile subdir/pyproject.toml --group bar` and `uv pip compile pyproject.toml --group subdir/pyproject.toml:bar`

We also probably want test coverage for `uv pip compile subdir/pyproject.toml --group bar` where `bar` is present in `subdir/pyproject.toml` but not `pyproject.toml` — that's a good case for us to have a special-cased error message.

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9274 on 2025-03-04 21:09_

I would probably omit the hyphen here

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9274 on 2025-03-04 21:09_

(I might also say "was not found in", though I don't really have a strong preference there)

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1600 on 2025-03-04 21:12_

Does this respect `--project`? It seems important that it does.

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:250 on 2025-03-04 21:13_

Any concern about these additional hidden clones? 

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9211 on 2025-03-04 21:15_

There's not test coverage for `pip install . --group bar`, `pip install -e . --group bar`, or `pip install subdir --group bar`, right?

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:241 on 2025-03-04 21:15_

I'm pretty sure this makes sense, but it might be relevant for Charlie to take a look at before merging.

---

_@zanieb reviewed on 2025-03-04 21:20_

This looks pretty good. I have mostly minor comments, though the `--project` bit might be blocking?

We should add this to the documentation at

- https://docs.astral.sh/uv/pip/compile/
- https://docs.astral.sh/uv/pip/packages/

We may want to mark `--group` on `uv pip compile` as "in preview" until support lands in `pip-tools`. See https://github.com/jazzband/pip-tools/issues/2062. It looks like they're just considering using `::` instead of `:` as a separator? Moving this to preview would involve a runtime warning and a note in the doc.

---

_@zanieb reviewed on 2025-03-04 21:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1600 on 2025-03-04 21:24_

We should also probably call out that `-r .../pyproject.toml` and `<path>` do not affect this?

---

_@Gankra reviewed on 2025-03-06 15:16_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_install.rs`:9064 on 2025-03-06 15:16_

Ah heh, I had actively noticed the opposite convention (and the module name can be matched on if you want this). But I'm fine with moving to your preference (I don't really have one myself).

---

_@Gankra reviewed on 2025-03-06 15:30_

---

_Review comment by @Gankra on `crates/uv-normalize/src/group_name.rs`:128 on 2025-03-06 15:30_

I actually agreed with this but when I first wrote it I realized it actually makes the code longer and less clear to do so.

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9064 on 2025-03-06 15:47_

Me against the world :D

How do you match on a module name in nextest? Perhaps I'm just doing it wrong.

---

_@zanieb reviewed on 2025-03-06 15:47_

---

_@Gankra reviewed on 2025-03-06 15:50_

---

_Review comment by @Gankra on `crates/uv-requirements/src/source_tree.rs`:132 on 2025-03-06 15:50_

Could you rephrase that first part? Not sure what you mean.

---

_@Gankra reviewed on 2025-03-06 15:51_

---

_Review comment by @Gankra on `crates/uv-settings/src/settings.rs`:1131 on 2025-03-06 15:51_

Oh good catch, copy-paste error.

---

_@Gankra reviewed on 2025-03-06 15:53_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-06 15:53_

...interesting I'll look if that's easy or not.

---

_@Gankra reviewed on 2025-03-06 15:58_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1600 on 2025-03-06 15:58_

From the docs on `--project`:

> Run the command within the given project directory.
>
> Other command-line arguments (such as relative paths) will be resolved relative
> to the current working directory.

So by "respect" here do you actually mean "doesn't get affected by"? Oh I guess the specific case of the default path is an interesting one where it might be defensible to lean on --project... hmm

---

_@Gankra reviewed on 2025-03-06 15:59_

---

_Review comment by @Gankra on `crates/uv-requirements/src/specification.rs`:250 on 2025-03-06 15:59_

None, it happens only once per `uv` invocation and it's like... 0-10 strings realistically (which if non-zero are the prelude to some file i/o).

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:250 on 2025-03-06 16:00_

My qualm is more that they're hidden from the caller than anything else, but 👍 

---

_@zanieb reviewed on 2025-03-06 16:00_

---

_@Gankra reviewed on 2025-03-06 19:39_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_install.rs`:9064 on 2025-03-06 19:39_

aiui the module name is part of the test id: `uv::it pip_compile::missing_editable_directory`

so `cargo nextest run -- pip_compile` (or `pip_compile::` if you really want to avoid false positives) Just Works for me?

---

_@Gankra reviewed on 2025-03-06 19:42_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-06 19:42_

Ok I have an implementation but worth noting here that `project` is not the literal string "project" but is the actual name of the project (we just call our test projects "project" lol). So changing that to mycoolproj we currently print:

```
   # via mycoolproj (path/to/pyproject.toml)
```

Your proposal would naively be:

```
   # via mycoolproj (path/to/pyproject.toml:bar)
```

I can do that, but lmk if you want something Different.

---

_@zanieb reviewed on 2025-03-06 19:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-06 19:53_

(ugh, we should _not_ call our test projects "project" lol)

It's not a dependency of the package, so... I'm not sure we want to say the project name.

Anyway I don't think we need to bikeshed this. Let's do something vaguely sensible and we can change it in the future? cc @charliermarsh 

Can you also cross-post whatever we land on / or prod for discussion now upstream in the `pip-tools` issue?

---

_@zanieb reviewed on 2025-03-06 19:54_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1600 on 2025-03-06 19:54_

I feel strongly that the default path should be relative to `--project`.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1600 on 2025-03-06 19:54_

(and `--directory`)

---

_@zanieb reviewed on 2025-03-06 19:54_

---

_@Gankra reviewed on 2025-03-06 20:14_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-06 20:14_

Oh geez is the exact syntax of this comment load-bearing to something.

---

_@Gankra reviewed on 2025-03-06 20:16_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-06 20:16_

Or is this just a "let's not needlessly deviate from the other tool's output, even for comments"?

---

_@zanieb reviewed on 2025-03-06 20:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9064 on 2025-03-06 20:45_

Oh interesting, that does work... that's... surprising. I wonder if it didn't used to work via insta?

Thanks! Ignore this.

---

_@Gankra reviewed on 2025-03-08 00:28_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_compile.rs`:15283 on 2025-03-08 00:28_

Left a message with them.

---

_Comment by @Gankra on 2025-03-08 00:42_

Ooookay I think that's all review feedback addressed!

---

_@zanieb reviewed on 2025-03-10 19:16_

---

_Review comment by @zanieb on `crates/uv-requirements/src/source_tree.rs`:132 on 2025-03-10 19:16_

Sorry like

```
`{name}`
```

not

```
'{name}'
```

---

_@zanieb reviewed on 2025-03-10 19:19_

---

_Review comment by @zanieb on `docs/pip/compile.md`:79 on 2025-03-10 19:19_

We should also mention `--project` here, especially for multiple groups.

---

_@zanieb reviewed on 2025-03-10 19:20_

---

_Review comment by @zanieb on `docs/pip/compile.md`:68 on 2025-03-10 19:20_

"To lock a dependency group... For example, to lock the group `foo`"?

We also use backticks for `pyproject.toml`.

---

_@zanieb reviewed on 2025-03-10 19:21_

---

_Review comment by @zanieb on `docs/pip/packages.md`:110 on 2025-03-10 19:21_

My above comments for `pip compile` apply here too.

---

_@zanieb reviewed on 2025-03-10 19:21_

---

_Review comment by @zanieb on `docs/pip/packages.md`:121 on 2025-03-10 19:21_

We need to talk about how `--group` does not apply to other sources, like `-e .` etc. here. Maybe under an `!!! note` admonition? We could track this separately, but this is an important thing to teach in the docs.

---

_@zanieb reviewed on 2025-03-10 19:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9497 on 2025-03-10 19:23_

Should we quote 'pyproject.toml' for consistency with the rest?

---

_@zanieb reviewed on 2025-03-10 19:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9510 on 2025-03-10 19:23_

--group should be wrapped in backticks

---

_@zanieb reviewed on 2025-03-10 19:24_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9526 on 2025-03-10 19:24_

Probably want "in the `pyproject.toml`"

---

_@zanieb approved on 2025-03-10 19:26_

I _think_ this is ready. It's hard to keep track of everything with the scope of the changes.

I have a few stylistic nits remaining and want to see more user-facing documentation explaining how `--group` interacts with other options and how it differs from the rest of the uv interface. We can push that into a separate pull request, but it's very important. I'm happy to help with that if you need it.

---

_@zanieb reviewed on 2025-03-10 19:28_

---

_Review comment by @zanieb on `docs/pip/compile.md`:79 on 2025-03-10 19:28_

We may want to add a !!! note admonition noting that this is not yet implemented in `pip-tools` yet and link to the issue?

---

_@Gankra reviewed on 2025-03-17 14:21_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_install.rs`:9526 on 2025-03-17 14:21_

This is actually a path we're displaying, maybe "was not found: {path}"?

---

_@zanieb reviewed on 2025-03-17 17:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9526 on 2025-03-17 17:17_

You could say "The dependency group 'bar' was not found in the project at `{}`"

It's a little weird for the working directory, maybe we'd want a `./` there?

---

_@Gankra reviewed on 2025-03-17 17:55_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_install.rs`:9526 on 2025-03-17 17:55_

If we wanna do that I'd make it a change to path.user_display() across the board (I think it's fine as is...)

---

_@zanieb reviewed on 2025-03-17 17:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9526 on 2025-03-17 17:58_

I don't think it makes sense to do it across the board.  "in the project at pyproject.toml" reads pretty weird. A little better if it's in backticks though. We can always special case that though, "in the project in the working directory" or something.

---

_Comment by @Gankra on 2025-03-17 18:00_

Rebased and addressed review in latest commit.

---

_@zanieb reviewed on 2025-03-17 18:00_

---

_Review comment by @zanieb on `crates/uv-requirements/src/source_tree.rs`:138 on 2025-03-17 18:00_

Backticks on the path?

---

_@Gankra reviewed on 2025-03-17 18:01_

---

_Review comment by @Gankra on `crates/uv-requirements/src/source_tree.rs`:138 on 2025-03-17 18:01_

Oh I assumed we purposefully avoided that in our style so the path was easy to copy/alt-click in terminals

---

_@zanieb reviewed on 2025-03-17 18:04_

---

_Review comment by @zanieb on `docs/pip/compile.md`:88 on 2025-03-17 18:04_

I would not say "This is an attempt" in public facing copy. I'd just omit that whole sentence.

I think the second bit is a separate note? I'd use `!!! note` for the first part. Then add a separate `!!! important` that says

> A `--group` flag has not yet been implemented in `pip-tools` and we may change our semantics to match the upstream behavior when it is finalized. The upstream behavior can be tracked in ...

I'd move this to the first use of `pip compile --group` above a bit so there's not two admonitions in a row.

---

_@Gankra reviewed on 2025-03-17 18:05_

---

_Review comment by @Gankra on `crates/uv-requirements/src/source_tree.rs`:138 on 2025-03-17 18:05_

(This is why I suggested the `: {path}` approach)

---

_@zanieb reviewed on 2025-03-17 18:06_

---

_Review comment by @zanieb on `docs/pip/packages.md`:134 on 2025-03-17 18:06_

I wouldn't say "This matches the behaviour that `pip install` is shipping." — it'll be stale once they ship it and is a bit superfluous here as the assumption is we match pip's behavior.

---

_Review comment by @zanieb on `docs/pip/packages.md`:131 on 2025-03-17 18:06_

You could say, "As in pip, " if you want to mention that it is done to match pip's behavior.

---

_@zanieb reviewed on 2025-03-17 18:06_

---

_Review comment by @Gankra on `docs/pip/compile.md`:88 on 2025-03-17 18:13_

heh, yeah I wanted two separate notes but got stuck on the back-to-backness. adding distance between them is a fine compromise.

---

_@Gankra reviewed on 2025-03-17 18:13_

---

_Review comment by @zanieb on `crates/uv-requirements/src/source_tree.rs`:138 on 2025-03-17 18:14_

We either do `: {path}` or use backticks.

---

_@zanieb reviewed on 2025-03-17 18:14_

---

_@Gankra reviewed on 2025-03-17 18:15_

---

_Review comment by @Gankra on `docs/pip/packages.md`:131 on 2025-03-17 18:15_

Ooh I like that. I felt the need to specify that because I don't think we would have picked that behaviour in a vacuum and I expect some people will want to suggest changing it. Maybe too defensive 😅 

---

_Comment by @Gankra on 2025-03-17 18:36_

Done for reals?

---

_Comment by @zanieb on 2025-03-17 18:43_

Yeah let's go :D

---

_Merged by @Gankra on 2025-03-17 18:44_

---

_Closed by @Gankra on 2025-03-17 18:44_

---

_Branch deleted on 2025-03-17 18:44_

---
