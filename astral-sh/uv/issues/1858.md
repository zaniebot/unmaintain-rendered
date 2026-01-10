```yaml
number: 1858
title: Unpacking source distribution fails when there is a symlink in the archive
type: issue
state: closed
author: Janarthanam
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-02-22T08:55:49Z
updated_at: 2024-02-24T03:22:15Z
url: https://github.com/astral-sh/uv/issues/1858
synced_at: 2026-01-10T05:40:32Z
```

# Unpacking source distribution fails when there is a symlink in the archive

---

_Issue opened by @Janarthanam on 2024-02-22 08:55_

my docker build with uv fails with this error:

11 [stage-0  7/14] RUN uv pip install -r requirements.txt
11 7.443 error: Failed to download and build: pgpdump==1.5
11 7.443   Caused by: Failed to extract source distribution
11 7.443   Caused by: failed to query metadata of file `/root/.cache/uv/.tmpFE7b3l/pgpdump-1.5/README`
11 7.443   Caused by: No such file or directory (os error 2)



### Dockerfile 

ENV VIRTUAL_ENV /usr/local/
RUN --mount=type=cache,target=/root/.cache/pip pip install uv
RUN uv pip install -r requirements.txt

### uv version: 0.1.7


### requirements.txt 
```
fastapi~=0.109
brotlipy==0.7.0
cachetools==5.3.0
cognitojwt==1.4.1
ecdsa==0.18.0
google-auth==2.16.0
pyasn1==0.4.8
pyasn1-modules==0.2.8
python-jose==3.3.0
rsa==4.9
six==1.16.0
pip~=21.2.4
attrs~=22.1.0
wheel~=0.37.0
cryptography~=39.0.0
Pillow~=9.3.0
tornado~=6.2
Jinja2~=3.1.2
pytest~=7.2.0
MarkupSafe~=2.1.1
Werkzeug~=2.2.2
click~=8.1.3
blinker~=1.5
itsdangerous~=2.1.2
setuptools~=58.0.4
cffi~=1.15.1
certifi~=2022.12.7
urllib3~=1.26.13
requests~=2.28.1
aiohttp~=3.8.3
pycparser~=2.21
decorator~=5.1.1
packaging~=21.3
pyparsing~=3.0.9
zipp~=3.10.0
future~=0.18.2
grpcio~=1.58
ipython~=8.7.0
idna~=3.4
protobuf==4.21.9
defusedxml~=0.7.1
toml~=0.10.2
py~=1.11.0
lxml~=4.9.1
parso~=0.8.3
jedi~=0.18.2
multidict~=6.0.3
wcwidth~=0.2.5
pluggy~=1.0.0
exceptiongroup~=1.0.4
iniconfig~=1.1.1
pexpect~=4.8.0
numpy~=1.20
yarl~=1.8.2
gunicorn~=20.1.0
aiosignal~=1.3.1
frozenlist~=1.3.3
Pygments~=2.13.0
traitlets~=5.8.0
matplotlib~=3.4.3
backcall~=0.2.0
pickleshare~=0.7.5
ptyprocess~=0.7.0
asttokens~=2.2.1
executing~=1.2.0
bcrypt~=4.0.1
backoff~=1.11.1
openai~=1.3.5
python-docx~=0.8.11
pdfminer
boto3~=1.26.35
pytesseract~=0.3.10
pdf2image~=1.16.3
Scrapy~=2.8.0
beautifulsoup4~=4.11.2
twilio==7.16.1
jmespath~=1.0.1
soupsieve~=2.4.1
keyring~=4.0
cssselect~=1.2.0
pytz~=2023.3
botocore~=1.29.118
s3transfer~=0.6.0
w3lib~=2.1.1
oauth2client~=1.3
google-api-python-client~=1.3
parsel~=1.8.1
Twisted~=22.10.0
itemadapter~=0.8.0
PyDispatcher~=2.0.7
itemloaders~=1.1.0
queuelib~=1.6.2
tldextract~=3.4.0
PyJWT~=2.6.0
Automat~=22.10.0
incremental~=22.10.0
constantly~=15.1.0
hyperlink~=21.0.0
python-dateutil~=2.8.2
cycler~=0.11.0
ipykernel~=6.20.2
kiwisolver~=1.4.4
filelock~=3.12.0
PyPDF2~=3.0.1
google~=3.0.0
plotly~=5.17.0
pandas~=2.1.1
scikit-learn~=1.3.1
langchain~=0.0.312
redis~=5.0.1
starlette-context~=0.3.6
starlette~=0.36
json_stream~=2.3.2
pydantic~=1.10.2
spacy~=3.7.2
uvicorn~=0.27
en_core_web_sm@https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.7.1/en_core_web_sm-3.7.1.tar.gz
```





---

_Renamed from "UV run panics while querying metadata" to "UV run errors while querying metadata" by @konstin on 2024-02-22 10:15_

---

_Renamed from "UV run errors while querying metadata" to "Unpacking source distribution fails when there is a symlink in the archive" by @konstin on 2024-02-22 10:17_

---

_Label `bug` added by @konstin on 2024-02-22 10:17_

---

_Comment by @konstin on 2024-02-22 10:49_

We fail to install [pgpdump-1.5.tar.gz](https://files.pythonhosted.org/packages/0c/b8/b4a44411077c8f8ac356c8c3e8c95dac154b50f45348c317ebac2a9a85c0/pgpdump-1.5.tar.gz) because the source distribution contains a symlink. 

We have two failures we need to solve: The linux case and the windows case.

## Linux

In the linux case, we extract `README` before `README.md`, then fail because we try to query the permissions [here](https://github.com/astral-sh/uv/blob/19890feb776bf849335b1a9ac30de74c3645a4f5/crates/uv-extract/src/stream.rs#L127) which tries to resolve the symlink. Instead, we must not set permissions for symlinks (https://linux.die.net/man/7/symlink):

> On Linux, the permissions of a symbolic link are not used in any operations; the permissions are always 0777 (read, write, and execute for all user categories), and can't be changed. 

## Windows

We get a different error on windows:

```
$ uv pip install pgpdump==1.5
error: Failed to download and build: pgpdump==1.5
  Caused by: Failed to extract source distribution
  Caused by: failed to unpack `\\?\C:\Users\Konstantin\AppData\Local\uv\cache\.tmpgH2jML\pgpdump-1.5\README`
  Caused by: A required privilege is not held by the client. (os error 1314) when symlinking README.md to \\?\C:\Users\Konstantin\AppData\Local\uv\cache\.tmpgH2jML\pgpdump-1.5\README
```

Windows, by default, does not allow creating symlinks. The explorer fails to unpack the archive when downloaded manually (no symlink permissions). Cloning the repo, git creates `README` as a file with `README.md` as content. pip uses `tarfile`'s `extract_all`, which copies `README.md` to `README` on windows (https://github.com/pypa/pip/blob/b647ed5782e1fc5627e5e18a036130fea0b413e6/src/pip/_vendor/distlib/util.py#L1310), while preserving the symlink on linux. We should do the same as pip [here](https://github.com/astral-sh/uv/blob/19890feb776bf849335b1a9ac30de74c3645a4f5/crates/uv-extract/src/stream.rs#L114).

---

_Label `good first issue` added by @konstin on 2024-02-22 10:50_

---

_Comment by @samypr100 on 2024-02-23 03:57_

Note for windows, as an alternative its possible to create [Hardlinks (files) and Junctions (directories)](https://learn.microsoft.com/en-us/windows/win32/fileio/hard-links-and-junctions) without requiring admin privs unlike symlinks which require SeCreateSymbolicLinkPrivilege. So it may be possible to leverage built in rust [hard link](https://doc.rust-lang.org/std/fs/fn.hard_link.html) at least. Not sure if junctions has support yet in std.

---

_Comment by @samypr100 on 2024-02-23 04:30_

Note, this issue also seems to affect cache cleaning (assuming wheel build worked)

```
error: Failed to clear cache at: Z:\Users\samypr100\AppData\Local\uv\cache
  Caused by: failed to remove directory `\\?\Z:\Users\samypr100\AppData\Local\uv\cache\built-wheels-v0\pypi\pgpdump\1.5\yjtRJjNhVHkKwN5Qz13ND\pgpdump-1.5.tar.gz\README`
  Caused by: The directory name is invalid. (os error 267)
error: process didn't exit successfully: `target\debug\uv.exe cache clean` (exit code: 2)
 ```

---

_Comment by @charliermarsh on 2024-02-23 04:34_

Thanks. Will fix tomorrow if no one else gets to it.

---

_Comment by @charliermarsh on 2024-02-23 04:35_

@samypr100 - BTW we already use junctions internally for some things :)

---

_Comment by @matt-the-bat on 2024-02-23 13:05_

I'm not sure if it's related, but installing `uv` fails when inside a venv with `--symlinks`, but ok without.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-23 18:57_

---

_Comment by @charliermarsh on 2024-02-23 20:21_

Thankfully it seems that wheels are not permitted to contain symlinks.

---

_Closed by @charliermarsh on 2024-02-24 03:22_

---
