```yaml
number: 12317
title: Publish wasm API to npm
type: pull_request
state: merged
author: mattrunyon
labels: []
assignees: []
merged: true
base: main
head: npm-publish
created_at: 2024-07-14T07:23:20Z
updated_at: 2024-07-18T18:33:10Z
url: https://github.com/astral-sh/ruff/pull/12317
synced_at: 2026-01-12T15:55:40Z
```

# Publish wasm API to npm

---

_@mattrunyon_

## Summary

Will need to add an actions secret `NPM_TOKEN` that is an npm token w/ publish rights to the `@astral-sh` org. It seems right now the "best" option for is a classic automation token which never expires (npm scoped tokens expire which would cause this to randomly fail after it expires).

Fixes #1385

Added `ruff_wasm` to the pyproject version_files.

Added a build and publish wasm job with cargo-dist. I am not familiar with this toolchain, but I regenerated the publish action. Note this does not add the wasm build to the GH release artifacts.

This will publish `@astral-sh/ruff-wasm-{target}` where target is web, bundler, or nodejs

## Test Plan

I test published this in my fork with a few modifications
- workflow_dispatch for the build/publish action so it could be run standalone
- Published under my npm scope
- Changed repo URL to my fork so npm provenance works
- Bumped the version for the 2nd successful publish to check the LICENSE file copied properly

Workflows: https://github.com/mattrunyon/ruff/actions/workflows/publish-wasm.yml
npm: https://www.npmjs.com/package/@mattrunyon/ruff-api



---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:2 on 2024-07-15 06:16_

I would prefer to keep this named `ruff_wasm`. We may have other API crates in the future that aren't wasm specific.

---

_Review comment by @MichaReiser on `crates/ruff_wasm/README.md`:5 on 2024-07-15 06:17_

We should add a very big warning saying that this API is experimental and that it can change at any time.

---

_@MichaReiser reviewed on 2024-07-15 06:19_

Wow this is neat. 

I prefer to keep the old `ruff_wasm` crate name. `ruff_api` feels to generic as a crate name. 

I see that this package now only publishes the `web` bindings. I wonder if we need multiple packages similar to what [BiomeJS](https://github.com/biomejs/biome/blob/main/packages/%40biomejs/js-api/package.json) does (they actually have an even more complicated setup) where they have a `web`, `node` (possibly deno) and `bundler` package. 

---

_@mattrunyon reviewed on 2024-07-15 06:23_

---

_Review comment by @mattrunyon on `crates/ruff_wasm/Cargo.toml`:2 on 2024-07-15 06:23_

Would you still want to publish as `@astral-sh/ruff-api`? The crate name is used in the generated `package.json`, but there can be a CI step to rewrite that to use the preferred name for publishing

---

_@MichaReiser reviewed on 2024-07-15 06:29_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:2 on 2024-07-15 06:29_

Possibly. It depends on what we want to do about the different targets. 

---

_@mattrunyon reviewed on 2024-07-15 06:58_

---

_Review comment by @mattrunyon on `crates/ruff_wasm/Cargo.toml`:2 on 2024-07-15 06:58_

It looks like wasm-pack creates a different wasm file depending on the target (not just the JS glue code)

Each of those wasm files is ~7.5MB. So publishing all under 1 package would mean a developer needs to download ~30MB. While that's not massive in terms of node_modules size, it's still pretty big and would be mostly unused

So it might be better to upload separate packages per target. Also simplifies the CI since it could in theory just use a matrix w/ the targets and publish as `@astral-sh/ruff-api-{target}`

As an aside, projects shipping Rust native binaries on npm (such as [swc](https://www.npmjs.com/package/@swc/core) often publish a single entry package w/ optional dependencies. This takes advantage of npm optional dependencies failing to install if the dependency package.json specifies a specific architecture requirement.

The same can't be used here since web/node is not something you can specify with npm, but just pointing it out that it seems other projects tend to go towards multiple packages. Although the binaries for swc are 50MB and there's like 10 architectures, so in that case they definitely don't want to push 450MB of unusable code to every user.

---

_@MichaReiser reviewed on 2024-07-15 07:15_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:2 on 2024-07-15 07:15_

That makes sense and also roughly matches what Biome does (one `js-api` package that has optional dependencies on `wasm-node` and `wasm-web`). I think it's okay to make it the users responsibility to install the optional dependencies (which we don't need for now). 

I'm leaning towards naming the packages `ruff-wasm-node`, and `ruff-wasm-web`. That would allow us to create a `ruff-api` package long term that wraps the glue code with a proper API. But I don't mind much either way.

---

_@mattrunyon reviewed on 2024-07-15 07:34_

---

_Review comment by @mattrunyon on `crates/ruff_wasm/Cargo.toml`:2 on 2024-07-15 07:34_

Oh neat I learned something new from looking at their package.

Npm used to not install `peerDependencies` by default. They changed it in v7 to install them by default. Looks like now there's a `peerDependenciesMeta` field which can mark as optional so it doesn't install those by default on v7+

---

_Comment by @mattrunyon on 2024-07-15 07:51_

I've updated the warning and also adjusted the naming w/ a CI matrix job to publish 3 wasm-pack targets. I haven't used deno, but looks like it doesn't use package.json so it would need its own slightly different job probably

Test run on my fork here: https://github.com/mattrunyon/ruff/actions/runs/9935706011
Example should be published in a few minutes to https://www.npmjs.com/package/@mattrunyon/ruff-wasm-web

---

_@dhruvmanila reviewed on 2024-07-15 07:58_

---

_Review comment by @dhruvmanila on `crates/ruff_wasm/README.md`:3 on 2024-07-15 07:58_

```suggestion
> [!WARNING]
>
> This API is experimental and may change at any time
```

I'd suggest this instead of using a header:

<img width="729" alt="Screenshot 2024-07-15 at 13 27 30" src="https://github.com/user-attachments/assets/97155bfe-d70d-49c7-95a9-ab47b090070b">


---

_@mattrunyon reviewed on 2024-07-15 07:59_

---

_Review comment by @mattrunyon on `crates/ruff_wasm/README.md`:3 on 2024-07-15 07:59_

I tried this and it didn't seem to work on npm, only on github
![image](https://github.com/user-attachments/assets/b476c42f-6709-4a02-8fc1-65e8fc011b2a)


---

_Review comment by @dhruvmanila on `crates/ruff_wasm/README.md`:3 on 2024-07-15 08:02_

Oh, I didn't realize this was going to be rendered on NPM. It's fine then to either use a header or block quote:

```
> **⚠️ WARNING: This API is experimental and may change at any time**
```
> **⚠️ WARNING: This API is experimental and may change at any time**

---

_@dhruvmanila reviewed on 2024-07-15 08:02_

---

_Comment by @MichaReiser on 2024-07-15 08:24_

Nice thank you. We now need @charliermarsh to add the NPM token.

---

_Assigned to @charliermarsh by @MichaReiser on 2024-07-15 08:25_

---

_Comment by @charliermarsh on 2024-07-16 14:57_

Thanks for the ping -- I'll get to this today.

---

_Comment by @charliermarsh on 2024-07-16 15:55_

@MichaReiser -- I added `NPM_TOKEN` to the repo with read/write access on `@astral-sh`.

---

_@MichaReiser reviewed on 2024-07-16 16:04_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:1 on 2024-07-16 16:04_

We need to add the `Cargo.toml` to 


https://github.com/astral-sh/ruff/blob/7a7c601d5ed294a3c868b5e83f757105e0a189b8/pyproject.toml#L105-L112

or it won't get automatically updated when doing the next release.

---

_Comment by @MichaReiser on 2024-07-16 16:05_

Thanks @charliermarsh . Stupid question. How can I dry-run the workflow? And is the trigger for the npm workflow in the correct release step? 

---

_Comment by @mattrunyon on 2024-07-16 16:10_

I saw something about a `tag=dry-run` in some other workflows. If that actually runs the publish jobs, then could add a check to instead run `npm publish --dry-run` if that's the case. It will just print out what would have happened, but doesn't actually check the validity of the token (it will warn if there was no token)

---

_Comment by @mattrunyon on 2024-07-16 16:12_

Nevermind, looks like dry-run probably doesn't publish based on this line in `publish.yml`

```
publishing: ${{ inputs.tag && inputs.tag != 'dry-run' }}
```

---

_Comment by @MichaReiser on 2024-07-16 16:15_

Hmm okay. Let's see if @charliermarsh has an idea on how to test it or we'll test on release day :laughing: 

---

_Comment by @mattrunyon on 2024-07-16 16:21_

If anybody w/ more knowledge on the release system wants to adjust this in the future to separate build and publish, you would basically run everything except the `setup-node` and `publish` steps and upload the resulting `crates/ruff_wasm/pkg` to an artifact. Then download the artifacts, run `setup-node` and then `npm publish` for each.

Could also run `setup-node` in both build and publish and then run `npm pack crates/ruff_wasm/pkg --pack-destination some_optional_dir_if_not_current_dir` to create a tarball. Then upload the resulting `<name>-<version>.tgz`. Then `npm publish <name>-<version.tgz` those

---

_Review comment by @MichaReiser on `.github/workflows/publish-wasm.yml`:36 on 2024-07-16 16:24_

Most our workflows use [`taiki-e/install-action`](https://github.com/taiki-e/install-action) for fast installation of cargo dependencies 

---

_@MichaReiser approved on 2024-07-16 16:25_

---

_@mattrunyon reviewed on 2024-07-16 16:27_

---

_Review comment by @mattrunyon on `.github/workflows/publish-wasm.yml`:36 on 2024-07-16 16:27_

I stole this setup from the playground workflow, so might want to update both at some point

---

_Comment by @charliermarsh on 2024-07-16 16:42_

@MichaReiser - You should be able to release with `dry-run` as the tag (it should be the pre-populated default).

The check in GitHub Actions is `${{ inputs.plan != '' && !fromJson(inputs.plan).announcement_tag_is_implicit }}`. You can see an example in `build-docker.yml`.

---

_Comment by @mattrunyon on 2024-07-16 17:01_

There is currently nothing handling dry-run in the action and build/publish are in the same workflow. So fair warning if the action runs at all it will try to publish. I mostly wasn't sure about how the different release steps worked w/ dry-run and I thought the publish jobs were skipped entirely w/ dry-run

---

_Comment by @charliermarsh on 2024-07-16 17:07_

@mattrunyon - I will check...

---

_Comment by @charliermarsh on 2024-07-16 17:10_

Confirmed that publish jobs do not run with `dry-run`.

---

_Comment by @charliermarsh on 2024-07-16 17:11_

I think I would make just make the new job `workflow_dispatch` too and then skip the publish step if `${{ inputs.plan == '' || fromJson(inputs.plan).announcement_tag_is_implicit }}`, so we can test the rest manually without publishing.

---

_Comment by @charliermarsh on 2024-07-16 20:15_

LGTM! I’ll let @MichaReiser merge.

---

_Comment by @mattrunyon on 2024-07-16 20:18_

Here's a run from my fork: https://github.com/mattrunyon/ruff/actions/runs/9963353823

If there's no token, the 2nd or 3rd to last line of the dry-run publish output will say something like "warning: You need to login to npm"

Due to how Github actions work, there's no way to dry-run this until it merges (since there's no workflow w/ the same name already on main)

---

_Merged by @MichaReiser on 2024-07-17 06:50_

---

_Closed by @MichaReiser on 2024-07-17 06:50_

---

_Comment by @MichaReiser on 2024-07-17 06:57_

Thanks again @mattrunyon  for this amazing contribution. The dry run was completed successfully. 

https://github.com/astral-sh/ruff/actions/runs/9969546556/job/27546697354

---

_Branch deleted on 2024-07-17 07:00_

---

_Comment by @MichaReiser on 2024-07-17 08:20_

Yes, the packages should be published once we do the next release. 

---

_Comment by @dhruvmanila on 2024-07-18 18:32_

The first version of the packages are up!
* Node.js: https://www.npmjs.com/package/@astral-sh/ruff-wasm-nodejs
* Bundler: https://www.npmjs.com/package/@astral-sh/ruff-wasm-bundler
* Web: https://www.npmjs.com/package/@astral-sh/ruff-wasm-web

---
