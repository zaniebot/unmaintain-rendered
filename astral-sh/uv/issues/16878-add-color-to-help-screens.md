---
number: 16878
title: Add color to help screens
type: issue
state: open
author: joatmor
labels:
  - question
assignees: []
created_at: 2025-11-28T00:04:55Z
updated_at: 2025-12-01T13:42:54Z
url: https://github.com/astral-sh/uv/issues/16878
synced_at: 2026-01-10T01:26:11Z
---

# Add color to help screens

---

_Issue opened by @joatmor on 2025-11-28 00:04_

### Summary

It would be nice to have color help screens. The main screen is colored, but the help screens are not. I use bat to add color to man pages and find it to be very ergonomic. I run fish and created the following function which works great:

function man -d 'color man pages'
  command man $argv | bat -l man -p
end

The uv help screens are not shipped as man pages, At least not yet. Therefore, this hack cannot be used.



### Example

Bat is a very capable tool and can be used to colorize the uv help screens.

If I were a programmer, I would implement it myself. 

To get around that, I created the following fish function:

function uvh -d 'color uv help screens'
  command uv help $argv | bat -l man -p
end

 

---

_Label `enhancement` added by @joatmor on 2025-11-28 00:04_

---

_Comment by @konstin on 2025-11-28 08:22_

What system are you using, and what help screens are you referring to? `uv help run` is colored for me.

---

_Comment by @joatmor on 2025-11-28 11:47_

Arch Linux 6.17.9
Gnome 49.2
Ghostty 1.2.3
Fish 4.2.1
Uv 0.9.13
Rustup 1.28.2

If I run ‘uv’ or ‘uv help’ I get color, but ‘uv help run’ is not colored. 
I have since found that ‘rustup default stable’  is no longer colored.
Maybe the issue lies with rust?
  

> On Nov 28, 2025, at 3:22 AM, konsti ***@***.***> wrote:
> 
> 
> konstin
>  left a comment 
> (astral-sh/uv#16878)
>  <https://github.com/astral-sh/uv/issues/16878#issuecomment-3588345881>
> What system are you using, and what help screens are you referring to? uv help run is colored for me.
> 
> —
> Reply to this email directly, view it on GitHub <https://github.com/astral-sh/uv/issues/16878#issuecomment-3588345881>, or unsubscribe <https://github.com/notifications/unsubscribe-auth/ACBMAAZ25ZGMVWIEIA75TSL37AA4ZAVCNFSM6AAAAACNNNHC2CVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTKOBYGM2DKOBYGE>.
> You are receiving this because you authored the thread.
> 



---

_Comment by @konstin on 2025-11-28 13:59_

Both are colored for me on Ubuntu 24.04 with bash in the gnome terminal, and we haven't received other reports of this problem, did you maybe set a setting such as `NO_COLOR`? `bat` is also written in Rust FWIW.

---

_Label `enhancement` removed by @konstin on 2025-11-28 13:59_

---

_Label `question` added by @konstin on 2025-11-28 13:59_

---

_Comment by @joatmor on 2025-11-28 14:52_

I have not created any config files, and I do not see any shipping with the Arch Linux package.
Where would I create a config file to set the variable(s) in question?
I can create a uv.toml file if I know what to include.  

On 11/28/25 8:59 AM, konsti ***@***.***> wrote:
> *konstin* left a comment (astral-sh/uv#16878) <https://github.com/ 
> astral-sh/uv/issues/16878#issuecomment-3589448669>
> 
> Both are colored for me on Ubuntu 24.04 with bash in the gnome terminal, 
> and we haven't received other reports of this problem, did you maybe set 
> a setting such as |NO_COLOR|?
> 
> —
> Reply to this email directly, view it on GitHub <https://github.com/ 
> astral-sh/uv/issues/16878#issuecomment-3589448669>, or unsubscribe 
> <https://github.com/notifications/unsubscribe-auth/ 
> ACBMAA3VLLHHK5KWU4B4ZQD37BIMHAVCNFSM6AAAAACNNNHC2CVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTKOBZGQ2DQNRWHE>.
> You are receiving this because you authored the thread.Message ID: 
> ***@***.***>
> 
> 


---

_Comment by @weihenglim on 2025-12-01 05:01_

FWIW I can also reproduce this behaviour on Windows 11 24H2 on both CMD and PowerShell.
`uv help` and `uv run --help` produce colored outputs but `uv help run` isn't colored.

**UPDATE:**
Did some more testing and `uv help run --no-pager` does produce colored output, so it might be a paging related issue.



---

_Comment by @joatmor on 2025-12-01 13:42_

I have since noticed this issue with other utilities that are built with rust. Thank you for a great utility. As I learn Python I will be using UV for sure. 

> On Dec 1, 2025, at 12:01 AM, Lim Wei Heng ***@***.***> wrote:
> 
> 
> weihenglim
>  left a comment 
> (astral-sh/uv#16878)
>  <https://github.com/astral-sh/uv/issues/16878#issuecomment-3594546247>
> FWIW I can also reproduce this behaviour on Windows 11 24H2 on both CMD and PowerShell.
> uv help and uv run --help produce colored outputs but uv help run isn't colored.
> 
> —
> Reply to this email directly, view it on GitHub <https://github.com/astral-sh/uv/issues/16878#issuecomment-3594546247>, or unsubscribe <https://github.com/notifications/unsubscribe-auth/ACBMAA6A2TJJEAN4IWHMQFT37PDTVAVCNFSM6AAAAACNNNHC2CVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTKOJUGU2DMMRUG4>.
> You are receiving this because you authored the thread.
> 



---
