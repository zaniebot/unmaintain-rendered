---
number: 6358
title: uv run not respecting extra index preferences in cargo build vs pip install in a conda env
type: issue
state: closed
author: abazabaaa
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-08-21T17:18:11Z
updated_at: 2024-08-22T01:00:23Z
url: https://github.com/astral-sh/uv/issues/6358
synced_at: 2026-01-10T01:23:59Z
---

# uv run not respecting extra index preferences in cargo build vs pip install in a conda env

---

_Issue opened by @abazabaaa on 2024-08-21 17:18_

Hi,

I believe this to be a bug.

When creating a micromamba env (micromamba -n test2 python=3.11) and then installing uv with pip I am able to run the following and get a successful installation:
`uv pip install openeye-toolkits`
uv pip install openeye-toolkits
Resolved 1 package in 1.04s
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...                                                                                                                   warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 20.55s
 + openeye-toolkits==2024.1.1
 
 When I create the python script (this time using the cargo install of uv, ~/.cargo/bin/uv) below:
`uv add --script openeyetest.py "openeye-toolkits" "numpy"`
`# /// script`
`# requires-python = ">=3.12"`
`# dependencies = [`
`#     "openeye-toolkits",`
`#     "numpy",`
`# ]`
`# ///`

`from openeye import oechem, oegraphsim`
`import numpy as np`

`# Create an OEBitVector object from a molecule fingerprint`
`mol = oechem.OEGraphMol()`
`oechem.OEParseSmiles(mol,'c1ccccc1')`
`fp = oegraphsim.OEFingerPrint()`
`oegraphsim.OEMakeMACCS166FP(fp, mol)`


`bitv = [0]*fp.GetSize()`


`# bitv = np.array([0]*fp.GetSize())`
`bit = fp.FirstBit()`
`while bit != -1:`
    `bitv[bit]=1`
    `bit=fp.NextBit(bit)`

`packed = np.packbits(bitv)`
`unpacked = np.unpackbits(packed)`
`print(packed)`
`print(unpacked)`

I then do:
`uv run openeyetest.py`
Reading inline script metadata from: openeyetest.py
  × No solution found when resolving script dependencies:
  ╰─▶ Because there are no versions of openeye-toolkits and you require openeye-toolkits, we can conclude that your requirements are unsatisfiable.
  
My ~/.uv/uv.toml is as follows (truncated paths and removed token):
[pip]
index-url = "xxxx.com/simple/"
extra-index-url = ["https://token:xxxxxxxx@magpie.eyesopen.com/simple", "https://download.pytorch.org/whl/cu121"]

I have additionally tried various versions of python in the header of openeyetest.py (python==3.11.9 ; which works with the openeye-toolkits). I have exported the path to uv.toml so that it was set (export UV_CONFIG_FILE=/xxx/uv.toml)

uv --version
uv 0.3.0

Other than this, things seem to be working ok.




---

_Comment by @charliermarsh on 2024-08-21 17:21_

Yeah this looks wrong. We should be respecting the configuration there.

---

_Label `configuration` added by @charliermarsh on 2024-08-21 17:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-21 17:21_

---

_Comment by @charliermarsh on 2024-08-21 17:21_

Just clarifying, it _does not_ work even with `UV_CONFIG_FILE=...`?

---

_Comment by @abazabaaa on 2024-08-21 17:24_

Correct, is does not seem to make a difference.

export UV_CONFIG_FILE=/xxxxxx/uv.toml
`uv run openeyetest.py`
Reading inline script metadata from: openeyetest.py
  × No solution found when resolving script dependencies:
  ╰─▶ Because there are no versions of openeye-toolkits and you require openeye-toolkits, we can conclude that your requirements are unsatisfiable.
  
 I can see it correctly set in the help.
  `uv --help`
  ...
Global options:
  -q, --quiet                      Do not print any output
  -v, --verbose...                 Use verbose output
      --color <COLOR_CHOICE>       Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls                 Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
      --offline                    Disable network access
      --no-progress                Hide all progress outputs
      --config-file <CONFIG_FILE>  The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=/xxxxxv/uv.toml]
      --no-config                  Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) [env: UV_NO_CONFIG=]
  -h, --help                       Display the concise help for this command
  -V, --version                    Display the uv version

Use `uv help` for more details.

---

_Comment by @charliermarsh on 2024-08-21 17:24_

Ok thanks.

---

_Label `bug` added by @charliermarsh on 2024-08-21 17:28_

---

_Comment by @abazabaaa on 2024-08-21 17:45_

Just to further confirm this with an internal pypi server which I have in my uv.toml as my index-url:
[pip]
index-url = "xxxx.com/simple/"
extra-index-url = ["https://token:[xxxxxxxx@magpie.eyesopen.com](mailto:xxxxxxxx@magpie.eyesopen.com)/simple", "https://download.pytorch.org/whl/cu121"]

I was unable to run pydingertest.py  after adding "pydinger" with `uv add --script pydingertest.py "pydinger"`
`uv run pydingertest.py`
Reading inline script metadata from: pydingertest.py
  × No solution found when resolving script dependencies:
  ╰─▶ Because pydinger was not found in the package registry and you require pydinger, we can conclude that your requirements are unsatisfiable.
  
 It was successful in the micromamba env with the same version of uv (0.3).
 
`micromamba activate test2`
`uv pip install pydinger`
Resolved 23 packages in 14.19s
   Built pydinger==1.0.3
   Built ldclient==2023.2.2
Prepared 8 packages in 6.45s
░░░░░░░░░░░░░░░░░░░░ [0/22] Installing wheels...                                                                                                                  warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 22 packages in 31.93s
 + annotated-types==0.7.0
 + certifi==2024.7.4
 + charset-normalizer==3.3.2
 + deprecated==1.2.10
 + dill==0.3.8
 + idna==3.7
 + ldclient==2023.2.2
 + multiprocess==0.70.16
 + numpy==2.1.0
 + pandas==2.2.2
 + pydantic==2.8.2
 + pydantic-core==2.20.1
 + pydinger==1.0.3
 + python-dateutil==2.9.0.post0
 + pytz==2024.1
 + requests==2.32.3
 + six==1.16.0
 + tqdm==4.66.5
 + typing-extensions==4.12.2
 + tzdata==2024.1
 + urllib3==2.2.2
 + wrapt==1.16.0

So it seems that our internal pypi server is not getting searched on the cargo version.

---

_Comment by @charliermarsh on 2024-08-21 17:50_

Oh sorry -- can you change:

```toml
[pip]
index-url = "xxxx.com/simple/"
extra-index-url = ["https://token:[xxxxxxxx@magpie.eyesopen.com](mailto:xxxxxxxx@magpie.eyesopen.com)/simple", "https://download.pytorch.org/whl/cu121"]
```

To:

```
index-url = "xxxx.com/simple/"
extra-index-url = ["https://token:[xxxxxxxx@magpie.eyesopen.com](mailto:xxxxxxxx@magpie.eyesopen.com)/simple", "https://download.pytorch.org/whl/cu121"]
```

Settings under `[pip]` only apply to `uv pip`, but settings at the top-level apply everywhere.


---

_Comment by @abazabaaa on 2024-08-21 17:56_

Fixed!!

So just to be clear, I can also have a [pip] so that my uv pip works the same?

---

_Comment by @charliermarsh on 2024-08-21 17:58_

If you write it without `[pip]`, it will _also_ affect `uv pip`. It's just that if you write it with `[pip]`, it _doesn't_ affect `uv run` and other APIs. Does that make sense?

---

_Comment by @charliermarsh on 2024-08-21 17:58_

In other words, you only need the version without `[pip]`.

(We kept the `[pip]` table for backwards compatibility.)

---

_Comment by @abazabaaa on 2024-08-21 17:59_

Yes, it makes sense. 

---

_Closed by @charliermarsh on 2024-08-22 01:00_

---
