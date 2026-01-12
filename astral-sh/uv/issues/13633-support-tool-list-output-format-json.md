```yaml
number: 13633
title: "Support `tool list --output-format=json`"
type: issue
state: open
author: InSyncWithFoo
labels:
  - enhancement
assignees: []
created_at: 2025-05-24T00:28:13Z
updated_at: 2025-07-09T05:29:26Z
url: https://github.com/astral-sh/uv/issues/13633
synced_at: 2026-01-12T16:01:33Z
```

# Support `tool list --output-format=json`

---

_@InSyncWithFoo_

(@gaborbernat [asked for this on Discord](https://discord.com/channels/1039017663004942429/1207998321562619954/1375609411023798373). I'm also interested myself. Part of #411 and #3199.)

Example output:

```json
{
    "name": "ty",
    "version": "0.0.1a6",
    "directory": "/home/foobar/.local/share/uv/tools/ty",
    "executables": [
        { "name": "ty", "path": "/home/foobar/.local/bin/ty" }
    ]
}
```

---

_Label `enhancement` added by @InSyncWithFoo on 2025-05-24 00:28_

---

_Comment by @gaborbernat on 2025-05-24 05:59_

The only information that is required for me is name, version and executable names, so paths/directory are not requried for me personally.

---

_Comment by @samypr100 on 2025-05-29 02:42_

It might be worthwhile to include the python details (e.g. home, version). I find myself moving some tools between python versions at times. Right now I just do something like `cat $(uv tool dir)/**/pyvenv.cfg` to find this info 😆 

---

_Comment by @GideonBear on 2025-07-09 05:25_

Ref #13695, can this list injected packages as well? AFAICT that would have to be added to the receipt on installation.

Edit: Excuse me, I should've checked the PR first. I will test if this works as I expect.

---
