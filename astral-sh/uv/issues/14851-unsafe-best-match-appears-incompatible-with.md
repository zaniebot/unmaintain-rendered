```yaml
number: 14851
title: unsafe-best-match appears incompatible with github-dependencies and to ignore if-necessary-or-explicit
type: issue
state: open
author: SoundDesignerToBe
labels:
  - question
assignees: []
created_at: 2025-07-23T17:39:56Z
updated_at: 2025-08-05T08:19:31Z
url: https://github.com/astral-sh/uv/issues/14851
synced_at: 2026-01-12T16:01:57Z
```

# unsafe-best-match appears incompatible with github-dependencies and to ignore if-necessary-or-explicit

---

_@SoundDesignerToBe_

### Question

Hello,

I am very confused about what the `--index-strategy=unsafe-best-match` flag really does and why it fails to install my packages:


I install python packages in a sandboxed environment in CI pipelines where I ~~built uv from source~~ installed uv with `pip3 install`.

I recently added the `--index-strategy="unsafe-best-match"` flag to resolve an issue. 

Later, another private package from a private GitLab listed a github dependency in its pyproject.toml such as
```toml
# pyproject.toml
dependencies = [
  "some_package@git+https://github.com/author/some_package"
]
```
This led to an error when attempting to install the package:
```shell
error: Package `some_package` attempted to resolve via URL: git+https://github.com/author/some_package. URL dependencies must be expressed as direct requirements or constraints. Consider adding `some_package @ git+https://github.com/author/some_package` to your dependencies or constraints file.
```
I thought this might occur due to some sandbox restrictions but today I faced another issue and realized this is reproducible on my local host, but only if I use the `--index-strategy="unsafe-best-match"` flag. 

So... 'some_package' is not maintained on PYPI, but there are dev tags on the github repo. I cloned the repo, built the dev tag wheel and pushed it manually to my gitlab package registry -- the registry of the package that lists "some_package" as dependency. 
I changed the dependency in the pyproject.toml to the explicit dev tag instead of the github url (such as `"some_package==1.2.3a4"`). Then I gave a new stable tag to and built a new wheel of my package and uploaded it to its package registry on GitLab.

If I try to install my package now with plain `uv pip install` (only the index and insecure host are defined in uv.toml, no other settings), then the installation works fine. 
If I add the `--index-stratety=unsafe-best-match` flag again, it fails and will try to install the previous  stable version of my package leading to the error quoted above w.r.t URL dependencies. 
I used `uv cache clean` multiple times, but that had no impact. 

So I can only hope to solve my previous issue leading to the use of that flag in another way but I read the documentation and cannot conclude why this fails. 

It appeared to me as if the "unsafe-best-match" overwrote the `--prerelease=if-necessary-or-explicit` default value, so I set it explicitly, but that had no impact, either.

Adding "--prerelease=allow" installs the latest version of my package and its dependency, but then I get prereleases of any further dependency which I do not want for obvious reasons.

So why does the "unsafe-best-strategy" behave so oddly? Is it a bug or did I miss something in the docs? 

Specs:
- Fedora Workstation 42
- `uv venv --python 3.12 ...`
- `uv --version`: 0.7.12 

### Platform

Linux 6.15.6-200.fc42.x86_64 x86_64 GNU/Linux

### Version

uv 0.7.12

---

_Label `question` added by @SoundDesignerToBe on 2025-07-23 17:39_

---

_Comment by @SoundDesignerToBe on 2025-07-24 08:22_

Update:
Even after using `uv cache clean my_package` in the install instruction of the sandbox and removing the index strategy flag, it continued to fail to install and I had to go back to using pip there to make it work.  It tried to install it in a cached environment (cached flatpak build). I expected it to try to pull a newer version if it cannot install the old one, but it kept running into the URL dependency error. Replacing `uv pip` with `pip` again, it now works and it pulls the new package.

---
