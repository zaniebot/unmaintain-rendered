```yaml
number: 11837
title: "uv fails with \"thread '<unknown>' has overflowed its stack\" on Windows"
type: issue
state: closed
author: ricrogz
labels:
  - bug
  - windows
  - needs-mre
assignees: []
created_at: 2025-02-27T18:14:19Z
updated_at: 2025-04-16T14:44:57Z
url: https://github.com/astral-sh/uv/issues/11837
synced_at: 2026-01-12T16:00:47Z
```

# uv fails with "thread '<unknown>' has overflowed its stack" on Windows

---

_@ricrogz_

### Summary

I'm hitting the same issue using `uv` 0.5.24 and also with 0.6.30.

The problem triggers when using `uv` to install whls into an existing venv directory, running the following command:
```
uv.exe pip install \
     --find-links [path to whls] \
     -r requirements.txt \
     -r more_requirements.txt \
     -p [path to python.exe in existing venv]
```

The requirements file contains about 300 Python packages. All packages have been pre-downloaded, install correctly and work as expected when using `pip` for the installation instead of `uv`.

Before the error message, `uv` prints this:
```
Using Python 3.11.4 environment at: C:\builds\local_build\internal
Resolved 276 packages in 25ms
⠼ Preparing packages... (3/270)
thread '<unknown>' has overflowed its stack
```

### Platform

Windows x64

### Version

0.5.24, 0.6.3

### Python version

3.11.4

---

_Label `bug` added by @ricrogz on 2025-02-27 18:14_

---

_Comment by @konstin on 2025-02-27 18:16_

Just to confirm, this is a release `uv.exe`, not a debug build? (We have known cases of stack overflows in debug builds, due to the lack compiler optimizations making for really large types)

---

_Comment by @ricrogz on 2025-02-27 18:24_

Yes, I downloaded them from the Releases section here on the github repo.

---

_Label `windows` added by @konstin on 2025-02-27 18:26_

---

_Comment by @konstin on 2025-02-27 18:28_

Can you shared a minimized set of requirements that causes the stack overflow? I haven't seen that happening before, so otherwise we can't find out where that happened

---

_Label `needs-mre` added by @konstin on 2025-02-27 18:30_

---

_Comment by @ricrogz on 2025-02-28 16:23_

This is a chunk of the requirements file that I'm trying to install when the error triggers: [requirements.txt](https://github.com/user-attachments/files/19030345/requirements.txt)

I've been playing with it a little, and I can reproduce the error when I install it based on pre-build wheels using
```
uv.exe pip install \
     --no-deps \
     --find-links [path to whls] \
     -r requirements.txt \
     -p [path to python.exe in existing venv]
```

But if I omit the `--find-links` argument and allow `uv` to download the packages, it works correctly. I don't think any of the prebuilt wheels is broken: as I said, they both install and perform correctly when installed using `pip`.

---

_Comment by @notatallshaw on 2025-02-28 17:09_

FWIW I can't reproduce trying to match as close as I can to your steps:


<summary>

requirements.txt:

<details>

```
aiohttp==3.8.3
aiosignal==1.2.0
apache_beam==2.50.0
apipkg==3.0.1
appnope==0.1.0
ase==3.23.0
asn1crypto==0.24.0
asteval==0.9.29
astor==0.7.1
asttokens==2.4.1
async-generator==1.10
async-timeout==4.0.2
atomicwrites==1.3.0
attrs==24.2.0
ax-platform==0.3.4
backcall==0.2.0
backoff==1.10.0
backports-abc==0.5
backports.shutil-get-terminal-size==1.0.0
bayesian-optimization==1.4.2
bcrypt==3.2.0
biopython==1.83
bleach==2.0.0
botorch==0.9.2
captum==0.7.0
certifi==2019.9.11
chardet==3.0.4
charset-normalizer==2.1.1
cloudpickle==2.2.1
Click==7.0
colorama==0.4.6
coloredlogs==15.0.1
comm==0.1.4
coverage==7.4.1 
cryptography==3.4.7
cycler==0.10.0
decorator==4.3.1
defusedxml==0.6.0
descriptastorus==2.6.1
diskcache==5.6.3
distro==1.3.0
dnspython==2.6.1
docutils==0.19
ecdsa==0.13
einops==0.8.0
elementpath==2.2.1
emmet-core==0.64.4
entrypoints==0.3.0
execnet==1.8.1
executing==2.0.1
e3nn==0.5.1
fastavro==1.9.0
fasteners==0.19
filelock==3.12.0
flatbuffers==23.5.26
fonttools==4.29.1
forestci==0.6
funcsigs==1.0.0
gemmi==0.7.0
googledrivedownloader==0.4
gpytorch==1.11
h5py==3.9.0
html5lib==1.1
httplib2==0.22.0
humanfriendly==10.0
hypothesis==6.115.6
hypothesis-bio==0.1
idna==2.6
imageio==2.8.0
importlib_metadata==6.6.0
ImmuneBuilder==1.2
inflect==4.1.0
iniconfig==1.0.1
ipykernel==5.3.4
ipython==8.21.0
ipython-genutils==0.2.0
ipywidgets==8.1.0
isodate==0.6.0
jaxtyping==0.2.21
jedi==0.19.1
Jinja2==2.11.2
joblib==1.2.0
js2py==0.74
json5==0.9.5
jsonschema==3.2.0
jupyter-client==6.1.7
jupyter-core==4.7.0
jupyterlab==2.2.9
jupyterlab-pygments==0.1.2
jupyterlab-server==1.2.0
jupyterlab_widgets==3.0.8
latexcodec==2.0.1
linear_operator==0.5.1
llvmlite==0.41.0
lmfit==1.1.0
Mako==1.1.3
matminer==0.9.2
matplotlib-inline==0.1.6
mistune==0.8.4
mock==2.0.0
monty==2023.8.8
more-itertools==9.0.0
mp-api==0.34.2
msgpack==1.0.5
multipledispatch==1.0.0
nbclient==0.5.1
nbconvert==6.0.7
nbformat==5.0.8
nest-asyncio==1.4.3
networkx==3.4.2
notebook==6.1.5
numba==0.58.0
numexpr==2.8.4
numpy==1.24.2
objsize==0.6.1
onnx==1.17.0
opt_einsum==3.3.0
opt_einsum_fx==0.1.4
orjson==3.9.10
packaging==23.2
palettable==3.1.1
pandas==1.5.3
pandas-flavor==0.2.0
paramiko==3.4.0
parso==0.8.3
path.py==8.1.2
pathlib2==2.2.1
pbr==5.4.4
pexpect==4.8.0
pickleshare==0.7.5
Pint==0.11
plotly==5.16.1
pluggy==1.3.0
ply==3.11
prometheus-client==0.9.0
prompt-toolkit==3.0.43
PROPKA==3.5.1
proto_plus==1.22.3
psutil==5.9.6
ptyprocess==0.5.2
pure-eval==0.2.2
py==1.11.0
pyasn1==0.4.4
pybind11==2.11.1
pybtex==0.24.0
pydantic==1.10.12
pydot==1.4.2
Pygments==2.9.0
pymatgen==2023.8.10
pymongo==4.8.0
PyNaCl==1.5.0
pynvml==8.0.4
pyparsing==2.4.7
pyreadline3==3.4.1
pyro_api==0.1.2
pyro_ppl==1.8.6
pytest==6.2.5
pytest-cov==2.10.1
pytest-forked==1.3.0
pytest-xdist==2.2.1
python-dateutil==2.8.1
python-editor==1.0.4
pytz==2022.7.1
pywin32==305
qtconsole==5.3.1
qtpy==2.0.1
rdflib==5.0.0
reciprocalspaceship==1.0.3
regex==2023.10.3
requests==2.31.0
ruamel.yaml==0.17.32
ruamel.yaml.clib==0.2.7
scikit-learn==1.3.2
scipy==1.14.0
seaborn==0.12.2
Send2Trash==1.5.0
shap==0.42.1
singledispatch==3.4.0.3
six==1.16.0
skl2onnx==1.17.0
slicer==0.0.7
sortedcontainers==2.1.0
spglib==2.0.2
stack-data==0.6.3
supervisor==4.2.0
sympy==1.13.1
tabulate==0.9.0
tenacity==8.2.3
terminado==0.9.1
testpath==0.4.4
threadpoolctl==2.1.0
tifffile==2020.10.1
toml==0.10.1
torch==2.0.1
TPOT==0.12.1
tqdm==4.66.5
traitlets==5.0.5
typeguard==2.13.3
typing_extensions==4.7.1
tzdata==2023.3
tzlocal==5.2
uncertainties==3.1.7
update-checker==0.18.0
urllib3==2.0.4
wcwidth==0.1.7
webencodings==0.5.1
widgetsnbextension==4.0.8
xarray==0.16.2
xgboost==1.7.4
XlsxWriter==1.3.9
xmlschema==1.6.1
zipp==0.6.0
zstandard==0.22.0
```
</details>

</summary>


Steps:

1. `uv venv myproj -p 3.10`
2. `myproj\Scripts\activate`
3. `uv pip install pip`
4. `pip wheel "aizynthfinder @ git+https://github.com/MolecularAI/aizynthfinder.git@v4.0.0" -w wheels --no-deps`
5. `pip wheel -r .\requirements.txt -w wheels --no-deps`
6. `uv pip uninstall pip`
7. `deactivate`
8. `uv pip install "aizynthfinder==4.0.0" --no-deps --find-links .\wheels\ -r .\requirements.txt -p .\myproj\Scripts\python.exe --offline`

It takes a long time preparing the package data (~30 seconds), but I assume that's because it need to unzip every wheel.

---

_Comment by @ricrogz on 2025-02-28 17:56_

@notatallshaw fails for me every time.


---

_Comment by @ricrogz on 2025-03-04 16:37_

> Just to confirm, this is a release `uv.exe`, not a debug build? (We have known cases of stack overflows in debug builds, due to the lack compiler optimizations making for really large types)

I'm not a rust developer, but this statement about missing compiler optimizations causing stack overflows sounds quite suspicious and certainly not reassuring to me. Can you elaborate a bit more about what these optimizations are, and how they are related to stack overflows? Would it be possible for me to create a debug build in which to trace the issue?

Another thing I'm thinking about (and might not even be related at all) is whether there might be other factors involved in this issue, e.g., whether it is relevant that I'm running this on a virtualized cloud VM, or the number of cores available and parallelization might be a factor.

Finally, is there any way to find out what's going on my machine and not on yours? 

---

_Comment by @konstin on 2025-03-04 16:49_

uv uses async code extensively, which includes a lot of nested futures. Each `async fn` has the size of its own locals plus the locals of the largest `async fn` it awaits, recursively. These get very large, while building with optimizations changes the layouts of the futures to much more reasonable  sizes. On the other hand, windows has a default stack size of 1MB for the main thread, much less than other platform. We've recently fixed this by not using the real main thread at all, but always starting a new thread with a more reasonable stack size (https://github.com/astral-sh/uv/pull/10479).

To debug uv, you can clone the repository and build with `cargo build --profile profiling`. This will write a build with both optimizations (like in production) and debug symbols (for backtraces) to `target/profiling/uv`. You can run this binary in a debugger and get a backtrace after the stack overflow.

---

_Comment by @ricrogz on 2025-03-04 17:23_

Thanks for your explanation, @konstin.

What you said makes sense to me. Although I wouldn't have called the manipulation of the stack sizes an "optimization". It was the use of that word that scared me :)

I'll get back to this thread if I can figure out something relevant.

---

_Comment by @ricrogz on 2025-03-04 21:35_

Well, I didn't get as far as building `uv` locally and debugging. Just playing with the `RUST_MIN_STACK` env variable solved my issue: setting it to 3Mb (3145728 bytes) allowed the command to succeed, with the threshold being somewhere between 2.5Mb and 2.75Mb.

So, it seems that, for whatever reason, the wheel files I'm using seem to trigger a higher memory usage than what would be expected.

---

_Comment by @notatallshaw on 2025-04-16 14:43_

This should now be fixed with https://github.com/astral-sh/uv/pull/12839 after finding a more reproducible example in https://github.com/astral-sh/uv/issues/12769

Feel free to report back in next release if it's still an issue. 

---

_Closed by @notatallshaw on 2025-04-16 14:43_

---
