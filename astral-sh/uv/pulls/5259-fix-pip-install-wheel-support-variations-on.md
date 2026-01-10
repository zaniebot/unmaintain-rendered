```yaml
number: 5259
title: "fix(pip.install.wheel): support variations on `pythonw.exe`"
type: pull_request
state: merged
author: paveldikov
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: fix/5256
created_at: 2024-07-21T15:53:36Z
updated_at: 2024-07-22T15:06:31Z
url: https://github.com/astral-sh/uv/pull/5259
synced_at: 2026-01-10T13:42:52Z
```

# fix(pip.install.wheel): support variations on `pythonw.exe`

---

_Pull request opened by @paveldikov on 2024-07-21 15:53_

Fixes: #5256

## Summary

Emulate `pip`'s behaviour and find `pythonw` executable by doing an `s/python/pythonw/g` style transformation, as opposed to assuming a constant `pythonw.exe` path.

See #5256 for more detail e.g. why this is a useful behaviour to emulate.

## Test Plan

Added this scenario to `test_script_executable`.

---
© 2024 Morgan Stanley.

THIS SOFTWARE IS CONTRIBUTED SUBJECT TO THE TERMS OF THE APACHE LICENSE V.2.0 AND THE MIT LICENSE.

THIS SOFTWARE IS LICENSED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE AND ANY WARRANTY OF NON-INFRINGEMENT, ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. THIS SOFTWARE MAY BE REDISTRIBUTED TO OTHERS ONLY BY EFFECTIVELY USING THIS OR ANOTHER EQUIVALENT DISCLAIMER IN ADDITION TO ANY OTHER REQUIRED LICENSE TERMS.

---

_Comment by @zanieb on 2024-07-21 16:02_

Thanks for contributing! @charliermarsh will own review of this one.

---

_Assigned to @charliermarsh by @zanieb on 2024-07-21 16:02_

---

_@samypr100 reviewed on 2024-07-21 22:32_

---

_Review comment by @samypr100 on `crates/install-wheel-rs/src/wheel.rs`:242 on 2024-07-21 22:32_

Might be useful to include the pip reference to [_get_alternate_executable](https://github.com/pypa/pip/blob/76e82a43f8fb04695e834810df64f2d9a2ff6020/src/pip/_vendor/distlib/scripts.py#L121-L126)

e.g.

 ```rust
/// Returns a [`PathBuf`] to `python[w].exe` for script execution.
///
/// <https://github.com/pypa/pip/blob/76e82a43f8fb04695e834810df64f2d9a2ff6020/src/pip/_vendor/distlib/scripts.py#L121-L126>
 ```

---

_@paveldikov reviewed on 2024-07-21 22:47_

---

_Review comment by @paveldikov on `crates/install-wheel-rs/src/wheel.rs`:242 on 2024-07-21 22:47_

Added.

---

_Review requested from @samypr100 by @paveldikov on 2024-07-21 22:47_

---

_Comment by @charliermarsh on 2024-07-21 23:04_

Do you mind confirming for me that your contribution is licensed under both the Apache _and_ MIT license? The project is dual-licensed so inbound contributions must be covered by both, and then consumers of the project can consume under the terms of either.

---

_Comment by @paveldikov on 2024-07-22 13:34_

@charliermarsh -- that is OK (I have reviewed this internally and amended the PR footer)

---

_Comment by @charliermarsh on 2024-07-22 14:14_

@paveldikov -- Thank you, I will get this merged today.

---

_@charliermarsh approved on 2024-07-22 14:27_

---

_Label `bug` added by @charliermarsh on 2024-07-22 14:27_

---

_Label `compatibility` added by @charliermarsh on 2024-07-22 14:27_

---

_Merged by @charliermarsh on 2024-07-22 14:28_

---

_Closed by @charliermarsh on 2024-07-22 14:28_

---

_Comment by @charliermarsh on 2024-07-22 14:28_

Thanks!

---

_Branch deleted on 2024-07-22 15:06_

---
