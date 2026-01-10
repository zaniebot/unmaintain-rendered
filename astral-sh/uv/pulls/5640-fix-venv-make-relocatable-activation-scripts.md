```yaml
number: 5640
title: "fix(venv): make relocatable activation scripts support ksh"
type: pull_request
state: merged
author: paveldikov
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: relocatable
created_at: 2024-07-30T22:15:33Z
updated_at: 2024-12-13T20:13:02Z
url: https://github.com/astral-sh/uv/pull/5640
synced_at: 2026-01-10T11:59:59Z
```

# fix(venv): make relocatable activation scripts support ksh

---

_Pull request opened by @paveldikov on 2024-07-30 22:15_

It transpires that detecting the directory a script was sourced from is non-trivial across `bash`, `ksh` and `zsh`.

The previous version was a one-liner and supported `bash` and `zsh` but not `ksh`.

It is possible to keep the one-liner and add `ksh` support, but that is mutually-exclusive with `zsh`.

Therefore, the only way to square this circle is to add an `if` block. A silver lining here is that although longer, the script is probably easier to follow as there is less code-golfing going on.

---

© 2024 Morgan Stanley.

THIS SOFTWARE IS CONTRIBUTED SUBJECT TO THE TERMS OF THE APACHE LICENSE V.2.0 AND THE MIT LICENSE.

THIS SOFTWARE IS LICENSED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE AND ANY WARRANTY OF NON-INFRINGEMENT, ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. THIS SOFTWARE MAY BE REDISTRIBUTED TO OTHERS ONLY BY EFFECTIVELY USING THIS OR ANOTHER EQUIVALENT DISCLAIMER IN ADDITION TO ANY OTHER REQUIRED LICENSE TERMS.

---

_Comment by @zanieb on 2024-07-30 22:18_

Can you note why ksh is important to support in particular? There are _a lot_ of shells out there. Are we going to support all of them?

---

_Comment by @paveldikov on 2024-07-30 22:36_

> Can you note why ksh is important to support in particular? There are _a lot_ of shells out there. Are we going to support all of them?

I agree that trying to support *all* of them is futile.

The Korn shell in particular is very much entrenched in a lot of enterprise environments at least.

Not sure about zsh -- I recall ohmyzsh being trendy some years ago... never used it much myself. I personally do not object to excluding it, but others might?

---

_Comment by @charliermarsh on 2024-07-30 22:37_

I think Zsh is very important, I believe it's the default on macOS.

---

_Comment by @charliermarsh on 2024-07-30 22:38_

I'm fine with this as it's _only_ used for `--relocatable`, right?

---

_Comment by @paveldikov on 2024-07-30 22:43_

> I'm fine with this as it's _only_ used for `--relocatable`, right?

Yes. I will actually go back and drop the sourced'ness checks for all except bash (which is what currently exists). I am testing them right now and are really brittle stuff -- would not want them to impact real users.

---

_Converted to draft by @paveldikov on 2024-07-30 23:21_

---

_Comment by @paveldikov on 2024-07-30 23:39_

Phew, that was not fun. Tested (by hand) across bash, ksh and zsh. So far so good. zsh in particular is where the `> /dev/null` trick comes from -- TIL it has a 'loud' cd command.

---

_Marked ready for review by @paveldikov on 2024-07-30 23:40_

---

_Comment by @zanieb on 2024-07-30 23:51_

I guess my qualm with ksh (and curiosity as to why it is important) is that it's not included in our existing `uv-shell::Shell` variants. I'd like to have a solid list of shells we support, rather than support for some here and others there. 

I presume it's present in enterprise because of red hat?

---

_Comment by @paveldikov on 2024-07-31 00:58_

> I guess my qualm with ksh (and curiosity as to why it is important) is that it's not included in our existing uv-shell::Shell variants. I'd like to have a solid list of shells we support, rather than support for some here and others there.

That is a fair concern. I had a brief look at the module and it doesn't look terribly complicated (save for `configuration_files()` which requires a bit of research because I haven't got a clue as to how it's used). How would you like me/us to approach this -- same PR or track separately?

> I presume it's present in enterprise because of red hat?

The correlation is rather uncanny indeed. Although I would not be surprised if that the affinity for `ksh` predates widespread RHEL (or even Linux!) adoption.

---

_Comment by @paveldikov on 2024-07-31 01:18_

Ok @zanieb it turns out it was a fairly trivial addition considering there's already a pattern of supporting multiple POSIX shells with largely the same code.

---

_@charliermarsh approved on 2024-07-31 16:17_

---

_Label `compatibility` added by @charliermarsh on 2024-07-31 16:17_

---

_Comment by @charliermarsh on 2024-07-31 16:18_

Thank you. This seems reasonable to me, especially now that we added Ksh to our shell enum.

---

_Merged by @charliermarsh on 2024-07-31 16:18_

---

_Closed by @charliermarsh on 2024-07-31 16:18_

---

_Label `bug` added by @charliermarsh on 2024-07-31 16:18_

---

_Comment by @jsirois on 2024-12-13 20:13_

Late to this, but, FWIW, ksh is the default shell on AIX which I think a fair number of IBM customers still use. So, not silicon-valley, but certainly banks and the like.

---
