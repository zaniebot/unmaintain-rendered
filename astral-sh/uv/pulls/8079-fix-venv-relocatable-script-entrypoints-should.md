```yaml
number: 8079
title: "fix(venv.relocatable): script entrypoints should work if symlinked to"
type: pull_request
state: merged
author: paveldikov
labels:
  - bug
assignees: []
merged: true
base: main
head: relocatable
created_at: 2024-10-10T09:48:12Z
updated_at: 2024-10-10T12:01:06Z
url: https://github.com/astral-sh/uv/pull/8079
synced_at: 2026-01-12T16:08:09Z
```

# fix(venv.relocatable): script entrypoints should work if symlinked to

---

_@paveldikov_

Fixes: #8058

## Test Plan

Integration test (but only for Unix, because symlinks on Windows require admin privs. Plus, they are not really all that idiomatic on Windows)

---
© 2024 Morgan Stanley.

THIS SOFTWARE IS CONTRIBUTED SUBJECT TO THE TERMS OF THE APACHE LICENSE V.2.0.

THIS SOFTWARE IS LICENSED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE AND ANY WARRANTY OF NON-INFRINGEMENT, ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. THIS SOFTWARE MAY BE REDISTRIBUTED TO OTHERS ONLY BY EFFECTIVELY USING THIS OR ANOTHER EQUIVALENT DISCLAIMER IN ADDITION TO ANY OTHER REQUIRED LICENSE TERMS.


---

_Comment by @charliermarsh on 2024-10-10 09:52_

This seems reasonable to me based on your prior comments. What other testing would you want to see prior to merging?

---

_Comment by @paveldikov on 2024-10-10 09:53_

> This seems reasonable to me based on your prior comments. What other testing would you want to see prior to merging?

It's MacOS that I worry about primarily, but I expect the automated test suite to cover that?

---

_Comment by @charliermarsh on 2024-10-10 09:53_

We do have a concept of "system tests" that test across a variety of operating systems... You could add a test to `scripts/check_system_python.py` to get coverage there.

---

_Comment by @charliermarsh on 2024-10-10 09:53_

Do you have any concerns about other POSIX systems, e.g., rare Linux distros etc.?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-10 09:53_

---

_Label `bug` added by @charliermarsh on 2024-10-10 09:53_

---

_Comment by @paveldikov on 2024-10-10 10:41_

> We do have a concept of "system tests" that test across a variety of operating systems... You could add a test to scripts/check_system_python.py to get coverage there.

Turns out past-me did in fact write an integration test to ensure that the entrypoint works. So I just added another check in the same test case to ensure that the entrypoint keeps working if symlinked to (in an arbitrary directory with no `python` inside)

> Do you have any concerns about other POSIX systems, e.g., rare Linux distros etc.?

Yes, which is why I used `realpath` instead of `readlink -f`. From my current research it seems that `realpath` is ubiquitous enough (thanks to MacOS 12's recent EOL) but I could certainly do with a second check.

Right now the hardest part is that I am developing from my home PC which is Windows :-( so I am fully reliant on the CI run + ad hoc experimentation in a separate Unix environment (which, OTOH, has no Rust)

---

_Comment by @paveldikov on 2024-10-10 11:58_

Ok, tests are green and my research w.r.t. `realpath` availability suggests that:
* RedHat and friends: available from el7+. <=el6 and below are EOL anyway.
* MacOS: available from 13+. <=12 are EOL.
* Debian and derivatives appear to have had `realpath` for decades.
* FreeBSD appears to have had `realpath` for decades.
* Busybox/Alpine appears to have `realpath`.
* Cygwin/MINGW has `realpath`.

Also of note that GNU `coreutils` has had `realpath` since 2011. So realistically all recent GNU/Linux (yes, the 'GNU' part is important here), beyond perhaps a few relict enterprise near-EOL versions, should have `realpath`.

`readlink -f` appears to remain non-portable.

For the record, I am still not 100% dead certain about compatibility, but I am inclined to make the risk calculation that:
* relocatable venvs are a fairly recent feature, therefore not likely to be used together with EOL operating systems
* common off-the-shelf OSs appear to ship `realpath` in their currently-supported versions
* less common OSs are far less end-user-centric, so we'd have all the more reason to expect `coreutils` installed.

---

_Merged by @charliermarsh on 2024-10-10 12:00_

---

_Closed by @charliermarsh on 2024-10-10 12:00_

---

_Comment by @charliermarsh on 2024-10-10 12:01_

Okay thanks for the thorough research here. Appreciate it!

---
