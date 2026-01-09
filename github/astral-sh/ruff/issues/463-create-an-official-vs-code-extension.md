---
number: 463
title: Create an official VS Code extension
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-10-24T21:39:11Z
updated_at: 2022-11-10T04:10:40Z
url: https://github.com/astral-sh/ruff/issues/463
synced_at: 2026-01-07T13:12:14-06:00
---

# Create an official VS Code extension

---

_Issue opened by @charliermarsh on 2022-10-24 21:39_

Related: #271.

---

_Label `enhancement` added by @charliermarsh on 2022-10-24 21:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-24 21:39_

---

_Comment by @charliermarsh on 2022-10-24 21:39_

\cc @Seamooo (I'm going to build this on top of `ruffd`)

---

_Comment by @Seamooo on 2022-10-25 13:16_

feel free to spam me with issues to get this working

---

_Label `enhancement` removed by @charliermarsh on 2022-10-26 01:55_

---

_Added to milestone `Editor integrations` by @charliermarsh on 2022-10-26 01:55_

---

_Comment by @charliermarsh on 2022-10-26 15:45_

@Seamooo - Will do!


---

_Comment by @charliermarsh on 2022-10-28 13:08_

@Seamooo - Is there any risk in modifying `ruffd` to run on-edit, in addition to on-save? In my testing, it seems fast and responsive enough even on very large Python files.

---

_Comment by @charliermarsh on 2022-10-28 13:09_

(I've made these changes locally and will upstream them.)

---

_Comment by @Seamooo on 2022-10-29 09:04_

If it's fast enough, that's exciting. I didn't want to add it initially as there was a decent amount of effort put into making partial edits very fast. Adding diagnostic generation on-edit requires the whole file, losing this speed up. I had thought of adding a debounced call on-edit but if it's fast enough that even that isn't necessary I'm happy to see it upstreamed

---

_Comment by @Seamooo on 2022-10-29 09:41_

At a later date ideally operating on lines changed and the ast/cst diff would be preferrable such that the whole file isn't required

---

_Comment by @charliermarsh on 2022-11-07 18:14_

@Seamooo - I've been working on this on and off. The main thing I'm running into is that I think if we want the extension to be self-contained (i.e., install the extension from the VS Code Marketplace and have it "just work"), then we have to write it in Python (see the [extension template](https://code.visualstudio.com/api/advanced-topics/python-extension-template)). Those templates use `pygls` as the language server.

If, instead, we want to use `ruffd`, then we'll need users to install `ruffd` separately. (I might be wrong about this.)

Another option: we publish `ruffd` to PyPI, similar to `ruff`? Then we might be able to publish a Python extension that uses `ruffd` as the language server. At the very least, users could `pip install ruffd` instead of `cargo install ruffd`. Perhaps we should even consider bundling `ruffd` into `ruff`?

[This post](https://www.osohq.com/post/building-vs-code-extension-with-rust-wasm-typescript) on the Polar extension gets at some of these issues.


---

_Comment by @charliermarsh on 2022-11-07 18:19_

So I think the options are as follows:

- Use their extension template, implement a Ruff extension that doesn't use `ruffd`.
- Write a VS Code extension that _does_ use `ruffd`, but require users to install `ruffd` separately (via `pip`).
- Write a VS Code extension that _does_ use `ruffd`, and find some way to distribute it as a Python extension that bundles both `ruff` and `ruffd`.

Those are roughly in order by how much work they would require (and, I think, how powerful they would be). Perhaps I'll start with (1), then we can do (2) or (3)?


---

_Comment by @tiangolo on 2022-11-07 21:49_

I understand the Pylance (VS Code Python) are currently splitting their support for linters in independent but connected extensions. I understand that's the case for the Flake8 and Isort support. I suspect for black as well.

I don't know if they are bundling a version of those packages in the new extensions. But up to now, that the support has been just part of Pylance, it asks to install Flake8 and mypy in the current environment on the first automatic run of the linter, it shows a button prompting to install, that when clicked runs the `pip install` command on the terminal.

And when running the thing to "format code", as I have it configured with Black, it shows the prompt asking to install Black.

Just my two cents of the behavior I've seen.

But also, that's probably changing in some way with the new separated extensions, not sure.

But anyway, maybe worth checking how they are doing that to copy the behavior, maybe.

---

_Comment by @charliermarsh on 2022-11-07 21:55_

Oh interesting, that's really helpful to hear. (I almost exclusively use PyCharm so I'm not that familiar with extension norms.) Will look into this.


---

_Comment by @fannheyward on 2022-11-08 02:05_

@charliermarsh you can bundle `ruffd` in your extensions, many VSCode extensions did as this.

---

_Comment by @charliermarsh on 2022-11-08 02:07_

@fannheyward - Oh cool. Can I include the Rust directly? Or does it need to be compiled into a binary? Or installable as Python? (Also, if there are any examples that come to mind, would be much appreciated.)

---

_Comment by @fannheyward on 2022-11-08 02:11_

@charliermarsh only `ruffd` binary. `rust-analyzer` does in this way.

This's rust-analyzer, download the extension and change it to `zip` type to unzip, you'll see `rust-analyzer` bin in server.

https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

Here's how rust-analyzer build and publish extension with GitHub Actions https://github.com/rust-lang/rust-analyzer/blob/master/.github/workflows/release.yaml



---

_Comment by @charliermarsh on 2022-11-08 02:16_

Thanks tons.

---

_Comment by @charliermarsh on 2022-11-08 02:18_

This makes a lot of sense, it just requires that we build for each platform (which is fine).

---

_Referenced in [astral-sh/ruff#655](../../astral-sh/ruff/pulls/655.md) on 2022-11-08 04:52_

---

_Comment by @charliermarsh on 2022-11-08 04:53_

For now, I did the easier thing and published a VS Code extension based on Microsoft's Python template: https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff. I'd like to move that codebase over to `ruffd` internally.


---

_Comment by @charliermarsh on 2022-11-08 04:55_

The extension is implemented in https://github.com/charliermarsh/vscode-ruff.

\ht to @harupy for some of the scaffolding.


---

_Comment by @charliermarsh on 2022-11-08 04:57_

I'll enable autofix and code actions within that extension shortly. For now, it's just diagnostics.

---

_Comment by @charliermarsh on 2022-11-08 04:57_

If you can, please give it a whirl! Would also appreciate reviews for the extension and stars on the repo if you're so inclined 🙏 

---

_Comment by @tiangolo on 2022-11-09 13:17_

Awesome! 🚀 

Right now it expects `ruff` to be in the environment. And if it is not, it logs an error in the Ruff-specific output, but there's no UI telling the user there was an error with Ruff, or asking the user to install Ruff in the current environment.

Not sure how Pylance does that, that shows a dialog asking the user to install, e.g. `mypy` and when clicking "OK" it runs the pip command to install it in the current environment, I think that would help.

---

_Comment by @charliermarsh on 2022-11-09 14:50_

Oh interesting! My expectation from the template had been that if Ruff wasn't in your environment, it'd fall back to a bundled version. Let me play around with.

---

_Comment by @charliermarsh on 2022-11-09 22:20_

(Having trouble with this because VS Code seems to omit the Ruff binary from the VSIX package, so the "bundled" version isn't working as expected.)

---

_Comment by @charliermarsh on 2022-11-09 22:24_

(Looking at the rust-analyzer workflow file for guidance.)

---

_Comment by @charliermarsh on 2022-11-09 22:37_

There's a `bundled/libs/bin/**` in the `.vscodeignore` from the extension template 🤦 

But I may still need to create separate builds for each platform? Since it's bundling a binary?


---

_Comment by @charliermarsh on 2022-11-10 04:10_

The Ruff VS Code extension now comes bundled with the native `ruff` binary: https://github.com/charliermarsh/vscode-ruff/pull/10. So it should work out-of-the-box with no additional installation step.

---

_Closed by @charliermarsh on 2022-11-10 04:10_

---
