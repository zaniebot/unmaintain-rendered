```yaml
number: 13934
title: "feat: upgrade dependencies in pyproject.toml"
type: pull_request
state: open
author: reneleonhardt
labels: []
assignees: []
draft: true
base: main
head: feat/upgrade-pyproject-toml
created_at: 2025-06-09T20:35:56Z
updated_at: 2025-11-21T08:55:04Z
url: https://github.com/astral-sh/uv/pull/13934
synced_at: 2026-01-10T05:58:11Z
```

# feat: upgrade dependencies in pyproject.toml

---

_Pull request opened by @reneleonhardt on 2025-06-09 20:35_

## Summary
Provide `uv upgrade` to upgrade dependency constraints in pyproject.toml without changing/syncing uv.lock.

Closes #6794

## Test Plan
- Lots of manual testing 😅
- Unit tests

## Features
- `--dry-run` to check the diff manually before overwriting pyproject.toml
- Searches like the original find_dependency() method:
    * `[project.dependencies]` (prod)
    * `[project.optional-dependencies]` (optional)
    * `[dependency-groups]` (groups)
    * `[tool.uv.dev-dependencies]` (dev)
- `--types=prod,dev,optional,groups` searches given sections only
- `--allow=1,2,3,4` filters version digits (1=major, 2=minor, 3=patch, 4=build number)
- Supports the new [tool.uv.dependency-groups](https://github.com/astral-sh/uv/pull/13735) `requires-python` per group
    1. user-override (`--python=>=3.12`)
    2. group (`[tool.uv.dependency-groups]`)
    3. toml (`[project.requires-python]`)
- Common operators are covered
- Downgrades if needed (current constraint doesn't include latest, i.e. `>1000`)
- `--recursive` searches skip hidden directories
- Fetches latest versions in parallel
- Caches latest versions for 10 minutes to speed up subsequents runs
    * `--refresh` (cold cache) in this repo: `Upgraded 199/860 dependencies in 37 files` in 2 seconds
    * `--no-refresh` (hot cache) in this repo: `Upgraded 199/860 dependencies in 37 files` in 1 second
    * `--no-cache` (no cache) in this repo: `Upgraded 199/860 dependencies in 37 files` in 7 seconds
- Uses as much uv internals / code as possible
- Stays true to the original formatting except inside the constraints itself
    * `ollama<0.5, >=0.4` becomes `ollama>=0.4,<0.6` (uv sorts versions and removes inner space)
    * `; python_version >= \"3.11\"` becomes ` ; python_full_version >= '3.11'` (internal code)

Added for demonstration purposes:
```toml
[tool.uv.dependency-groups]
dev = {requires-python = ">3.10"}
```
<img width="997" alt="uv upgrade" src="https://github.com/user-attachments/assets/53edb518-ba2d-4f83-9fb0-b87fbd193c34" />



---

_Comment by @zanieb on 2025-06-09 21:25_

Thanks for the pull request!

We're still in the process of designing this feature and are consequently very unlikely to accept this pull request as-is — but hopefully it can be informative for our design and implementation!

cc @konstin 

---

_Comment by @konstin on 2025-06-10 09:50_

One major thing we need to handle in our design is when the latest version of two packages is incompatible, so we can't upgrade both or either to the latest version, another is to handle markers such as `python_version` which influence resolution, and dependency group requires-python (https://github.com/astral-sh/uv/pull/13735). We also need to design the configuration for how version ranges are updated.

---

_Converted to draft by @konstin on 2025-06-10 09:51_

---

_Comment by @reneleonhardt on 2025-06-10 15:07_

Example run in astral-sh/uv:
```shell
uv upgrade --recursive
```

```diff
diff --git a/ecosystem/github-wikidata-bot/pyproject.toml b/ecosystem/github-wikidata-bot/pyproject.toml
index eb430f513..7fceea56d 100644
--- a/ecosystem/github-wikidata-bot/pyproject.toml
+++ b/ecosystem/github-wikidata-bot/pyproject.toml
@@ -12,17 +12,17 @@ dependencies = [
     "CacheControl[filecache]>=0.14,<0.15",
     "mwparserfromhell >=0.6.4,<0.7",
     "pydantic >=2.7.1",
-    "pywikibot >=9.1.2,<10",
+    "pywikibot>=9.1.2,<11",
     "sentry-sdk >=2.0.1,<3",
     "yarl >=1.9,<2",
 ]
 
 [tool.uv]
 dev-dependencies = [
-    "httpx >=0.27.0,<0.28",
+    "httpx>=0.27.0,<0.29",
     "pytest >=8.0.0,<9",
-    "ruff >=0.5.0,<0.6",
-    "tqdm >=4.66.4,<4.67",
+    "ruff>=0.5.0,<0.12",
+    "tqdm>=4.66.4,<4.68",
     "types-requests >=2.31.0.20240406,<3",
 ]
 
diff --git a/ecosystem/home-assistant-core/pyproject.toml b/ecosystem/home-assistant-core/pyproject.toml
index 971f321d3..2e149da1a 100644
--- a/ecosystem/home-assistant-core/pyproject.toml
+++ b/ecosystem/home-assistant-core/pyproject.toml
@@ -23,53 +23,53 @@ classifiers = [
 ]
 requires-python = ">=3.12.0"
 dependencies    = [
-    "aiodns==3.2.0",
-    "aiohttp==3.9.5",
-    "aiohttp_cors==0.7.0",
-    "aiohttp-fast-url-dispatcher==0.3.0",
-    "aiohttp-fast-zlib==0.1.0",
-    "aiozoneinfo==0.2.0",
-    "astral==2.2",
-    "async-interrupt==1.1.1",
-    "attrs==23.2.0",
+    "aiodns==3.4.0",
+    "aiohttp==3.12.12",
+    "aiohttp-cors==0.8.1",
+    "aiohttp-fast-url-dispatcher==0.3.1",
+    "aiohttp-fast-zlib==0.3.0",
+    "aiozoneinfo==0.2.3",
+    "astral==3.2",
+    "async-interrupt==1.2.2",
+    "attrs==25.3.0",
     "atomicwrites-homeassistant==1.4.1",
-    "awesomeversion==24.2.0",
-    "bcrypt==4.1.2",
+    "awesomeversion==25.5.0",
+    "bcrypt==4.3.0",
     "certifi>=2021.5.30",
-    "ciso8601==2.3.1",
-    "fnv-hash-fast==0.5.0",
+    "ciso8601==2.3.2",
+    "fnv-hash-fast==1.5.0",
     # hass-nabucasa is imported by helpers which don't depend on the cloud
     # integration
-    "hass-nabucasa==0.81.1",
+    "hass-nabucasa==0.88.1",
     # When bumping httpx, please check the version pins of
     # httpcore, anyio, and h11 in gen_requirements_all
-    "httpx==0.27.0",
-    "home-assistant-bluetooth==1.12.1",
+    "httpx==0.28.1",
+    "home-assistant-bluetooth==1.13.1",
     "ifaddr==0.2.0",
-    "Jinja2==3.1.4",
+    "jinja2==3.1.6",
     "lru-dict==1.3.0",
-    "PyJWT==2.8.0",
+    "pyjwt==2.10.1",
     # PyJWT has loose dependency. We want the latest one.
-    "cryptography==42.0.8",
-    "Pillow==10.3.0",
-    "pyOpenSSL==24.1.0",
-    "orjson==3.9.15",
+    "cryptography==45.0.4",
+    "pillow==11.2.1",
+    "pyopenssl==25.1.0",
+    "orjson==3.10.18",
     "packaging>=23.1",
     "pip>=21.3.1",
     "psutil-home-assistant==0.0.1",
     "python-slugify==8.0.4",
-    "PyYAML==6.0.1",
-    "requests==2.32.3",
-    "SQLAlchemy==2.0.31",
+    "pyyaml==6.0.2",
+    "requests==2.32.4",
+    "sqlalchemy==2.0.41",
     "typing-extensions>=4.12.2,<5.0",
-    "ulid-transform==0.9.0",
+    "ulid-transform==1.4.0",
     # Constrain urllib3 to ensure we deal with CVE-2020-26137 and CVE-2021-33503
     # Temporary setting an upper bound, to prevent compat issues with urllib3>=2
     # https://github.com/home-assistant/core/issues/97248
-    "urllib3>=1.26.5,<2",
-    "voluptuous==0.13.1",
+    "urllib3>=1.26.5,<3",
+    "voluptuous==0.15.2",
     "voluptuous-serialize==2.6.0",
-    "yarl==1.9.4",
+    "yarl==1.20.1",
 ]
 
 [project.urls]
diff --git a/ecosystem/saleor/pyproject.toml b/ecosystem/saleor/pyproject.toml
index 307c54e29..013dbeb35 100644
--- a/ecosystem/saleor/pyproject.toml
+++ b/ecosystem/saleor/pyproject.toml
@@ -13,62 +13,62 @@ requires-python = ">=3.12"
 dependencies = [
   "chevron-blue>=0.2.1,<0.3.0",
   "hatchling>=1.20.0,<2.0.0",
-  "msgspec>=0.18.4,<0.19.0",
+  "msgspec>=0.18.4,<0.20.0",
   "pypiserver>=2.0.1,<3.0.0",
   "pyyaml>=6.0.1,<7.0.0",
-  "setuptools>=71.1.0,<72.0.0",
-  "twine>=4.0.2,<5.0.0",
-  "watchfiles>=0.21.0,<0.22.0",
-  "adyen>=4.0.0,<5.0.0",
+  "setuptools>=71.1.0,<80.10.0",
+  "twine>=4.0.2,<6.2.0",
+  "watchfiles>=0.21.0,<1.1.0",
+  "adyen>=4.0.0,<13.5.0",
   "authlib>=1.3.1,<2.0.0",
-  "rx>=1.6.3,<2.0.0",
-  "aniso8601>=7.0.0,<8.0.0",
+  "rx>=1.6.3,<3.3.0",
+  "aniso8601>=7.0.0,<10.1.0",
   "asgiref>=3.7.2,<4.0.0",
   "azure-common>=1.1.28,<2.0.0",
   "azure-storage-blob>=12.12.0,<13.0.0",
   "azure-storage-common>=2.1.0,<3.0.0",
-  "babel>=2.8,<2.15",
+  "babel>=2.8,<2.18",
   "boto3>=1.28,<2.0",
   "botocore>=1.34,<2.0",
-  "braintree>=4.2,<4.30",
+  "braintree>=4.2,<4.37",
   "celery[redis]>=4.4.5,<6.0.0",
-  "cryptography>=42.0.5,<43.0.0",
-  "dj-database-url>=2,<3",
+  "cryptography>=42.0.5,<45.1.0",
+  "dj-database-url>=2,<4",
   "dj-email-url>=1,<2",
   "django-cache-url>=3.1.2,<4.0.0",
   "django-celery-beat>=2.2.1,<3.0.0",
   "django-countries>=7.2,<8.0",
-  "django-filter>=23.1,<24.0",
+  "django-filter>=23.1,<25.2",
   "django-measurement>=3.0,<4.0",
   "django-mptt>=0,<1",
-  "django-phonenumber-field>=4,<8",
+  "django-phonenumber-field>=4,<9",
   "django-prices>=2.3,<3.0",
   "django-storages[google]>=1.11,<2.0",
-  "django-stubs-ext>=4.2.1,<5.0.0",
-  "django[bcrypt]>=4.2,<5.0",
+  "django-stubs-ext>=4.2.1,<5.3.0",
+  "django[bcrypt]>=4.2,<5.3",
   "draftjs-sanitizer>=1.0.0,<2.0.0",
-  "faker>=26.0.0,<27.0",
+  "faker>=26.0.0,<37.4",
   "google-cloud-pubsub>=1.7,<3.0",
-  "google-cloud-storage>=2.0.0,<3.0.0",
+  "google-cloud-storage>=2.0.0,<3.2.0",
   "google-i18n-address>=3.1.0,<4.0.0",
-  "graphene<3.0",
-  "graphql-core>=2.3.2,<3.0.0",
-  "graphql-relay>=2.0.1,<3.0.0",
-  "gunicorn>=22.0.0,<23.0.0",
+  "graphene<3.5",
+  "graphql-core>=2.3.2,<3.3.0",
+  "graphql-relay>=2.0.1,<3.3.0",
+  "gunicorn>=22.0.0,<23.1.0",
   "html-to-draftjs>=1.0.1,<2.0.0",
-  "html2text>=2024.2.26,<2025.0.0",
+  "html2text>=2024.2.26,<2025.5.0",
   "jaeger-client>=4.5.0,<5.0.0",
-  "lxml>=4.9.3,<5.0.0",
+  "lxml>=4.9.3,<5.5.0",
   "markdown>=3.1.1,<4.0.0",
   "measurement>=3.2.2,<4.0.0",
   "micawber>=0.5.5,<0.6.0",
   "oauthlib>=3.1,<4.0",
   "opentracing>=2.3.0,<3.0.0",
-  "petl==1.7.15",
-  "phonenumberslite>=8.12.25,<9.0.0",
-  "pillow>=10.3.0,<11.0.0",
+  "petl==1.7.16",
+  "phonenumberslite>=8.12.25,<9.1.0",
+  "pillow>=10.3.0,<11.3.0",
   "pillow-avif-plugin>=1.3.1,<2.0.0",
-  "posuto>=2024.7.0,<2025.0.0",
+  "posuto>=2024.7.0,<2025.7.0",
   "prices>=1.0,<2.0",
   "promise>=2.3,<3.0",
   "psycopg[binary]>=3.1.8,<4.0.0",
@@ -76,22 +76,22 @@ dependencies = [
   "pyjwt>=2.9.0,<3.0.0",
   "python-dateutil>=2.8.2,<3.0.0",
   "python-http-client>=3.3.7,<4.0.0",
-  "python-json-logger>=0.1.11,<2.1.0",
+  "python-json-logger>=0.1.11,<3.4.0",
   "python-magic>=0.4.27,<0.5.0 ; sys_platform != 'win32'",
   "python-magic-bin>=0.4.14,<0.5.0 ; sys_platform == 'win32'",
   "pytimeparse>=1.1.8,<2.0.0",
-  "pytz>=2024.1,<2025.0",
+  "pytz>=2024.1,<2025.3",
   "razorpay>=1.2,<2.0",
-  "redis>=5.0.1,<6.0.0",
+  "redis>=5.0.1,<6.3.0",
   "requests>=2.32,<3.0",
-  "requests-hardened==1.0.0b3",
+  "requests-hardened==1.0.0",
   "semantic-version>=2.10.0,<3.0.0",
   "sendgrid>=6.7.1,<7.0.0",
   "sentry-sdk>=2.12,<3.0",
-  "stripe>=3.0.0,<4.0.0",
+  "stripe>=3.0.0,<12.3.0",
   "text-unidecode>=1.2,<2.0",
-  "urllib3>=1.26.19,<2.0.0",
-  "uvicorn[standard]>=0.23.1,<0.24.0",
+  "urllib3>=1.26.19,<2.5.0",
+  "uvicorn[standard]>=0.23.1,<0.35.0",
 ]
 
 [tool.deptry]
diff --git a/ecosystem/transformers/pyproject.toml b/ecosystem/transformers/pyproject.toml
index f3139b46b..52b9a5166 100644
--- a/ecosystem/transformers/pyproject.toml
+++ b/ecosystem/transformers/pyproject.toml
@@ -28,7 +28,7 @@ dependencies = [
     "pyyaml>=5.1",
     "regex!=2019.12.17",
     "requests",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "safetensors>=0.4.1",
     "tqdm>=4.27",
 ]
@@ -44,33 +44,33 @@ ja = [
     "unidic>=1.0.2",
     "sudachipy>=0.6.6",
     "sudachidict_core>=20220729",
-    "rhoknp>=1.1.0,<1.3.1"
+    "rhoknp>=1.1.0,<1.8.0"
 ]
 sklearn = ["scikit-learn"]
 tf = [
-    "tensorflow>=2.6,<2.16",
+    "tensorflow>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1"
 ]
 tf-cpu = [
-    "tensorflow-cpu>=2.6,<2.16",
+    "tensorflow-cpu>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1"
 ]
 torch = ["torch", "accelerate>=0.21.0"]
 accelerate = ["accelerate>=0.21.0"]
 retrieval = ["faiss-cpu", "datasets!=2.5.0"]
 flax = [
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
-    "flax>=0.4.1,<=0.7.0",
-    "optax>=0.0.8,<=0.1.4"
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
+    "flax>=0.4.1,<=0.9.0",
+    "optax>=0.0.8,<=0.2.4"
 ]
-tokenizers = ["tokenizers>=0.14,<0.19"]
+tokenizers = ["tokenizers>=0.14,<0.22"]
 ftfy = ["ftfy"]
 onnxruntime = ["onnxruntime>=1.4.0", "onnxruntime-tools>=1.4.2"]
 onnx = [
@@ -79,8 +79,8 @@ onnx = [
     "onnxruntime>=1.4.0",
     "onnxruntime-tools>=1.4.2"
 ]
-modelcreation = ["cookiecutter==1.7.3"]
-sagemaker = ["sagemaker==2.226.1"] # sagemaker>=2.31.0 # TODO(konsti)
+modelcreation = ["cookiecutter==2.6.0"]
+sagemaker = ["sagemaker==2.246.0"] # sagemaker>=2.31.0 # TODO(konsti)
 deepspeed = ["deepspeed>=0.9.3", "accelerate>=0.21.0"]
 optuna = ["optuna"]
 ray = ["ray[tune]>=2.7.0"]
@@ -108,27 +108,27 @@ vision = ["Pillow>=10.0.1,<=15.0"]
 timm = ["timm"]
 torch-vision = ["torchvision", "Pillow>=10.0.1,<=15.0"]
 # natten = ["natten>=0.14.6,<0.15.0"] # TODO(konsti)
-codecarbon = ["codecarbon==1.2.0"]
+codecarbon = ["codecarbon==3.0.2"]
 video = ["decord==0.6.0", "av"]
 sentencepiece = ["sentencepiece>=0.1.91,!=0.1.92", "protobuf"]
 
 deepspeed-testing = [
     "deepspeed>=0.9.3",
     "accelerate>=0.21.0",
-    "pytest>=7.2.0,<8.0.0",
+    "pytest>=7.2.0,<8.5.0",
     "pytest-xdist",
     "timeout-decorator",
     "parameterized",
     "psutil",
     "datasets!=2.5.0",
-    "dill<0.3.5",
+    "dill<0.5.0",
     "evaluate>=0.2.0",
     "pytest-timeout",
-    "ruff==0.1.5",
-    "sacrebleu>=1.4.12,<2.0.0",
+    "ruff==0.11.13",
+    "sacrebleu>=1.4.12,<2.6.0",
     "rouge-score!=0.0.7,!=0.0.8,!=0.1,!=0.1.1",
     "nltk",
-    "GitPython<3.1.19",
+    "gitpython<3.2.0",
     "hf-doc-builder>=0.3.0",
     "protobuf",
     "sacremoses",
@@ -138,7 +138,7 @@ deepspeed-testing = [
     "pydantic",
     "faiss-cpu",
     "datasets!=2.5.0",
-    "cookiecutter==1.7.3",
+    "cookiecutter==2.6.0",
     "optuna",
     "sentencepiece>=0.1.91,!=0.1.92",
     "protobuf"
@@ -146,26 +146,26 @@ deepspeed-testing = [
 quality = [
     "datasets!=2.5.0",
     "isort>=5.5.4",
-    "ruff==0.1.5",
-    "GitPython<3.1.19",
+    "ruff==0.11.13",
+    "gitpython<3.2.0",
     "hf-doc-builder>=0.3.0",
-    "urllib3<2.0.0"
+    "urllib3<2.5.0"
 ]
 all = [
-    "tensorflow>=2.6,<2.16",
+    "tensorflow>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1",
     "torch",
     "accelerate>=0.21.0",
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
-    "flax>=0.4.1,<=0.7.0",
-    "optax>=0.0.8,<=0.1.4",
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
+    "flax>=0.4.1,<=0.9.0",
+    "optax>=0.0.8,<=0.2.4",
     "sentencepiece>=0.1.91,!=0.1.92",
     "protobuf",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "torchaudio",
     "librosa",
     "pyctcdecode>=0.4.0",
@@ -178,27 +178,27 @@ all = [
     "timm",
     "torchvision",
     "Pillow>=10.0.1,<=15.0",
-    "codecarbon==1.2.0",
+    "codecarbon==3.0.2",
     "accelerate>=0.21.0",
     "decord==0.6.0",
     "av"
 ]
 docs_specific = ["hf-doc-builder"]
 docs = [
-    "tensorflow>=2.6,<2.16",
+    "tensorflow>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1",
     "torch",
     "accelerate>=0.21.0",
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
-    "flax>=0.4.1,<=0.7.0",
-    "optax>=0.0.8,<=0.1.4",
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
+    "flax>=0.4.1,<=0.9.0",
+    "optax>=0.0.8,<=0.2.4",
     "sentencepiece>=0.1.91,!=0.1.92",
     "protobuf",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "torchaudio",
     "librosa",
     "pyctcdecode>=0.4.0",
@@ -211,7 +211,7 @@ docs = [
     "timm",
     "torchvision",
     "Pillow>=10.0.1,<=15.0",
-    "codecarbon==1.2.0",
+    "codecarbon==3.0.2",
     "accelerate>=0.21.0",
     "decord==0.6.0",
     "av",
@@ -228,7 +228,7 @@ torchhub = [
     "requests",
     "sentencepiece>=0.1.91,!=0.1.92",
     "torch",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "tqdm>=4.27"
 ]
 agents = [
@@ -243,27 +243,27 @@ agents = [
 
 [dependency-groups]
 dev = [
-    "GitPython<3.1.19",
+    "gitpython<3.2.0",
     "Pillow>=10.0.1,<=15.0",
     "accelerate>=0.21.0",
     "av",
     "beautifulsoup4",
-    "codecarbon==1.2.0",
-    "codecarbon==1.2.0",
-    "cookiecutter==1.7.3",
+    "codecarbon==3.0.2",
+    "codecarbon==3.0.2",
+    "cookiecutter==2.6.0",
     "datasets!=2.5.0",
     "decord==0.6.0",
-    "dill<0.3.5",
+    "dill<0.5.0",
     "evaluate>=0.2.0",
     "faiss-cpu",
-    "flax>=0.4.1,<=0.7.0",
+    "flax>=0.4.1,<=0.9.0",
     "fugashi>=1.0",
     "hf-doc-builder",
     "hf-doc-builder>=0.3.0",
     "ipadic>=1.0.0,<2.0",
     "isort>=5.5.4",
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
     "kenlm",
     "keras-nlp>=0.3.1",
     "librosa",
@@ -271,7 +271,7 @@ dev = [
     "onnxconverter-common",
     "onnxruntime-tools>=1.4.2",
     "onnxruntime>=1.4.0",
-    "optax>=0.0.8,<=0.1.4",
+    "optax>=0.0.8,<=0.2.4",
     "optuna",
     "parameterized",
     "phonemizer",
@@ -281,13 +281,13 @@ dev = [
     "pydantic",
     "pytest-timeout",
     "pytest-xdist",
-    "pytest>=7.2.0,<8.0.0",
+    "pytest>=7.2.0,<8.5.0",
     "ray[tune]>=2.7.0",
-    "rhoknp>=1.1.0,<1.3.1",
+    "rhoknp>=1.1.0,<1.8.0",
     "rjieba",
     "rouge-score!=0.0.7,!=0.0.8,!=0.1,!=0.1.1",
-    "ruff==0.1.5",
-    "sacrebleu>=1.4.12,<2.0.0",
+    "ruff==0.11.13",
+    "sacrebleu>=1.4.12,<2.6.0",
     "sacremoses",
     "scikit-learn",
     "sentencepiece>=0.1.91,!=0.1.92",
@@ -295,18 +295,18 @@ dev = [
     "sudachidict_core>=20220729",
     "sudachipy>=0.6.6",
     "tensorboard",
-    "tensorflow-text<2.16",
-    "tensorflow>=2.6,<2.16",
+    "tensorflow-text<2.20",
+    "tensorflow>=2.6,<2.20",
     "tf2onnx",
     "timeout-decorator",
     "timm",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "torch",
     "torchaudio",
     "torchvision",
     "unidic>=1.0.2",
     "unidic_lite>=1.0.7",
-    "urllib3<2.0.0"
+    "urllib3<2.5.0"
 ]
 
 [build-system]
diff --git a/ecosystem/warehouse/pyproject.toml b/ecosystem/warehouse/pyproject.toml
index 62c4e3c7f..67e3abfc0 100644
--- a/ecosystem/warehouse/pyproject.toml
+++ b/ecosystem/warehouse/pyproject.toml
@@ -11,15 +11,15 @@ dependencies = [
   "Babel",
   "bcrypt",
   "boto3",
-  "celery[sqs]>=5.2.2,<5.3.2",
+  "celery[sqs]>=5.2.2,<5.6.0",
   "celery-redbeat",
   "certifi",
   "click",
   "cryptography",
   "datadog>=0.19.0",
   "disposable-email-domains",
-  "elasticsearch>=7.0.0,<7.11.0",
-  "elasticsearch_dsl>=7.0.0,<8.0.0",
+  "elasticsearch>=7.0.0,<9.1.0",
+  "elasticsearch-dsl>=7.0.0,<8.19.0",
   "first",
   "forcediphttpsadapter",
   "github-reserved-names>=1.0.0",
@@ -30,7 +30,7 @@ dependencies = [
   "humanize",
   "itsdangerous",
   "Jinja2>=2.8",
-  "kombu[sqs]<5.3.2", # https://github.com/jazzband/pip-tools/issues/1577
+  "kombu[sqs]<5.6.0", # https://github.com/jazzband/pip-tools/issues/1577
   "limits",
   "linehaul",
   "lxml",
@@ -63,7 +63,7 @@ dependencies = [
   "readme-renderer[md]>=36.0",
   "requests",
   "requests-aws4auth",
-  "redis>=2.8.0,<6.0.0",
+  "redis>=2.8.0,<6.3.0",
   "rfc3986",
   "sentry-sdk",
   "setuptools",
@@ -74,7 +74,7 @@ dependencies = [
   "transaction",
   "trove-classifiers",
   "ua-parser",
-  "urllib3<2", # See https://github.com/pypi/warehouse/issues/14671,
+  "urllib3<3", # See https://github.com/pypi/warehouse/issues/14671,
   "webauthn>=1.0.0,<3.0.0",
   "whitenoise",
   "WTForms[email]>=2.0.0",
@@ -84,15 +84,15 @@ dependencies = [
 
 [project.optional-dependencies]
 deploy = [
-  "gunicorn==22.0.0",
-  "ddtrace==2.8.5"
+  "gunicorn==23.0.0",
+  "ddtrace==3.9.1"
 ]
 
 [tool.uv]
 dev-dependencies = [
   "Sphinx",
   "asyncudp>=0.7",
-  "black==24.4.2",
+  "black==25.1.0",
   "cairosvg",
   "celery-types",
   "coverage",
@@ -119,7 +119,7 @@ dev-dependencies = [
   "pretend",
   "pyramid_debugtoolbar>=2.5",
   "pytest-icdiff",
-  "pytest-postgresql>=3.1.3,<7.0.0",
+  "pytest-postgresql>=3.1.3,<7.1.0",
   "pytest-randomly",
   "pytest-socket",
   "pytest>=3.0.0",
@@ -142,7 +142,7 @@ dev-dependencies = [
   "types-python-slugify",
   "types-pytz",
   "types-redis",
-  "types-requests==2.31.0.6", # See https://github.com/pypi/warehouse/issues/14671
+  "types-requests==2.32.0.20250602", # See https://github.com/pypi/warehouse/issues/14671
   "types-setuptools",
   "types-stripe",
   "types-zxcvbn",
diff --git a/scripts/requirements/transformers/pyproject.toml b/scripts/requirements/transformers/pyproject.toml
index e247c2829..694383661 100644
--- a/scripts/requirements/transformers/pyproject.toml
+++ b/scripts/requirements/transformers/pyproject.toml
@@ -15,7 +15,7 @@ dependencies = [
     "pyyaml>=5.1",
     "regex!=2019.12.17",
     "requests",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "safetensors>=0.4.1",
     "tqdm>=4.27",
 ]
@@ -28,33 +28,33 @@ ja = [
     "unidic>=1.0.2",
     "sudachipy>=0.6.6",
     "sudachidict_core>=20220729",
-    "rhoknp>=1.1.0,<1.3.1"
+    "rhoknp>=1.1.0,<1.8.0"
 ]
 sklearn = ["scikit-learn"]
 tf = [
-    "tensorflow>=2.6,<2.16",
+    "tensorflow>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1"
 ]
 tf-cpu = [
-    "tensorflow-cpu>=2.6,<2.16",
+    "tensorflow-cpu>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1"
 ]
 torch = ["torch", "accelerate>=0.21.0"]
 accelerate = ["accelerate>=0.21.0"]
 retrieval = ["faiss-cpu", "datasets!=2.5.0"]
 flax = [
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
-    "flax>=0.4.1,<=0.7.0",
-    "optax>=0.0.8,<=0.1.4"
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
+    "flax>=0.4.1,<=0.9.0",
+    "optax>=0.0.8,<=0.2.4"
 ]
-tokenizers = ["tokenizers>=0.14,<0.19"]
+tokenizers = ["tokenizers>=0.14,<0.22"]
 ftfy = ["ftfy"]
 onnxruntime = ["onnxruntime>=1.4.0", "onnxruntime-tools>=1.4.2"]
 onnx = [
@@ -63,8 +63,8 @@ onnx = [
     "onnxruntime>=1.4.0",
     "onnxruntime-tools>=1.4.2"
 ]
-modelcreation = ["cookiecutter==1.7.3"]
-sagemaker = ["sagemaker==2.226.1"] # sagemaker>=2.31.0 # TODO(konsti)
+modelcreation = ["cookiecutter==2.6.0"]
+sagemaker = ["sagemaker==2.246.0"] # sagemaker>=2.31.0 # TODO(konsti)
 deepspeed = ["deepspeed>=0.9.3", "accelerate>=0.21.0"]
 optuna = ["optuna"]
 ray = ["ray[tune]>=2.7.0"]
@@ -92,27 +92,27 @@ vision = ["Pillow>=10.0.1,<=15.0"]
 timm = ["timm"]
 torch-vision = ["torchvision", "Pillow>=10.0.1,<=15.0"]
 # natten = ["natten>=0.14.6,<0.15.0"] # TODO(konsti)
-codecarbon = ["codecarbon==1.2.0"]
-video = ["decord==0.6.0", "av==9.2.0"]
+codecarbon = ["codecarbon==3.0.2"]
+video = ["decord==0.6.0", "av==14.4.0"]
 sentencepiece = ["sentencepiece>=0.1.91,!=0.1.92", "protobuf"]
 
 deepspeed-testing = [
     "deepspeed>=0.9.3",
     "accelerate>=0.21.0",
-    "pytest>=7.2.0,<8.0.0",
+    "pytest>=7.2.0,<8.5.0",
     "pytest-xdist",
     "timeout-decorator",
     "parameterized",
     "psutil",
     "datasets!=2.5.0",
-    "dill<0.3.5",
+    "dill<0.5.0",
     "evaluate>=0.2.0",
     "pytest-timeout",
-    "ruff==0.1.5",
-    "sacrebleu>=1.4.12,<2.0.0",
+    "ruff==0.11.13",
+    "sacrebleu>=1.4.12,<2.6.0",
     "rouge-score!=0.0.7,!=0.0.8,!=0.1,!=0.1.1",
     "nltk",
-    "GitPython<3.1.19",
+    "gitpython<3.2.0",
     "hf-doc-builder>=0.3.0",
     "protobuf",
     "sacremoses",
@@ -122,7 +122,7 @@ deepspeed-testing = [
     "pydantic",
     "faiss-cpu",
     "datasets!=2.5.0",
-    "cookiecutter==1.7.3",
+    "cookiecutter==2.6.0",
     "optuna",
     "sentencepiece>=0.1.91,!=0.1.92",
     "protobuf"
@@ -130,26 +130,26 @@ deepspeed-testing = [
 quality = [
     "datasets!=2.5.0",
     "isort>=5.5.4",
-    "ruff==0.1.5",
-    "GitPython<3.1.19",
+    "ruff==0.11.13",
+    "gitpython<3.2.0",
     "hf-doc-builder>=0.3.0",
-    "urllib3<2.0.0"
+    "urllib3<2.5.0"
 ]
 all = [
-    "tensorflow>=2.6,<2.16",
+    "tensorflow>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1",
     "torch",
     "accelerate>=0.21.0",
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
-    "flax>=0.4.1,<=0.7.0",
-    "optax>=0.0.8,<=0.1.4",
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
+    "flax>=0.4.1,<=0.9.0",
+    "optax>=0.0.8,<=0.2.4",
     "sentencepiece>=0.1.91,!=0.1.92",
     "protobuf",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "torchaudio",
     "librosa",
     "pyctcdecode>=0.4.0",
@@ -162,27 +162,27 @@ all = [
     "timm",
     "torchvision",
     "Pillow>=10.0.1,<=15.0",
-    "codecarbon==1.2.0",
+    "codecarbon==3.0.2",
     "accelerate>=0.21.0",
     "decord==0.6.0",
-    "av==9.2.0"
+    "av==14.4.0"
 ]
 docs_specific = ["hf-doc-builder"]
 docs = [
-    "tensorflow>=2.6,<2.16",
+    "tensorflow>=2.6,<2.20",
     "onnxconverter-common",
     "tf2onnx",
-    "tensorflow-text<2.16",
+    "tensorflow-text<2.20",
     "keras-nlp>=0.3.1",
     "torch",
     "accelerate>=0.21.0",
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
-    "flax>=0.4.1,<=0.7.0",
-    "optax>=0.0.8,<=0.1.4",
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
+    "flax>=0.4.1,<=0.9.0",
+    "optax>=0.0.8,<=0.2.4",
     "sentencepiece>=0.1.91,!=0.1.92",
     "protobuf",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "torchaudio",
     "librosa",
     "pyctcdecode>=0.4.0",
@@ -195,10 +195,10 @@ docs = [
     "timm",
     "torchvision",
     "Pillow>=10.0.1,<=15.0",
-    "codecarbon==1.2.0",
+    "codecarbon==3.0.2",
     "accelerate>=0.21.0",
     "decord==0.6.0",
-    "av==9.2.0",
+    "av==14.4.0",
     "hf-doc-builder"
 ]
 torchhub = [
@@ -212,7 +212,7 @@ torchhub = [
     "requests",
     "sentencepiece>=0.1.91,!=0.1.92",
     "torch",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "tqdm>=4.27"
 ]
 agents = [
@@ -226,27 +226,27 @@ agents = [
 ]
 
 dev-dependencies = [
-    "GitPython<3.1.19",
+    "gitpython<3.2.0",
     "Pillow>=10.0.1,<=15.0",
     "accelerate>=0.21.0",
-    "av==9.2.0",
+    "av==14.4.0",
     "beautifulsoup4",
-    "codecarbon==1.2.0",
-    "codecarbon==1.2.0",
-    "cookiecutter==1.7.3",
+    "codecarbon==3.0.2",
+    "codecarbon==3.0.2",
+    "cookiecutter==2.6.0",
     "datasets!=2.5.0",
     "decord==0.6.0",
-    "dill<0.3.5",
+    "dill<0.5.0",
     "evaluate>=0.2.0",
     "faiss-cpu",
-    "flax>=0.4.1,<=0.7.0",
+    "flax>=0.4.1,<=0.9.0",
     "fugashi>=1.0",
     "hf-doc-builder",
     "hf-doc-builder>=0.3.0",
     "ipadic>=1.0.0,<2.0",
     "isort>=5.5.4",
-    "jax>=0.4.1,<=0.4.13",
-    "jaxlib>=0.4.1,<=0.4.13",
+    "jax>=0.4.1,<=0.4.30",
+    "jaxlib>=0.4.1,<=0.4.30",
     "kenlm",
     "keras-nlp>=0.3.1",
     "librosa",
@@ -254,7 +254,7 @@ dev-dependencies = [
     "onnxconverter-common",
     "onnxruntime-tools>=1.4.2",
     "onnxruntime>=1.4.0",
-    "optax>=0.0.8,<=0.1.4",
+    "optax>=0.0.8,<=0.2.4",
     "optuna",
     "parameterized",
     "phonemizer",
@@ -264,13 +264,13 @@ dev-dependencies = [
     "pydantic",
     "pytest-timeout",
     "pytest-xdist",
-    "pytest>=7.2.0,<8.0.0",
+    "pytest>=7.2.0,<8.5.0",
     "ray[tune]>=2.7.0",
-    "rhoknp>=1.1.0,<1.3.1",
+    "rhoknp>=1.1.0,<1.8.0",
     "rjieba",
     "rouge-score!=0.0.7,!=0.0.8,!=0.1,!=0.1.1",
-    "ruff==0.1.5",
-    "sacrebleu>=1.4.12,<2.0.0",
+    "ruff==0.11.13",
+    "sacrebleu>=1.4.12,<2.6.0",
     "sacremoses",
     "scikit-learn",
     "sentencepiece>=0.1.91,!=0.1.92",
@@ -278,18 +278,18 @@ dev-dependencies = [
     "sudachidict_core>=20220729",
     "sudachipy>=0.6.6",
     "tensorboard",
-    "tensorflow-text<2.16",
-    "tensorflow>=2.6,<2.16",
+    "tensorflow-text<2.20",
+    "tensorflow>=2.6,<2.20",
     "tf2onnx",
     "timeout-decorator",
     "timm",
-    "tokenizers>=0.14,<0.19",
+    "tokenizers>=0.14,<0.22",
     "torch",
     "torchaudio",
     "torchvision",
     "unidic>=1.0.2",
     "unidic_lite>=1.0.7",
-    "urllib3<2.0.0"
+    "urllib3<2.5.0"
 ]
 
 [build-system]
diff --git a/scripts/workspaces/albatross-project-in-excluded/packages/seeds/pyproject.toml b/scripts/workspaces/albatross-project-in-excluded/packages/seeds/pyproject.toml
index 71d0272ed..b4d3904d9 100644
--- a/scripts/workspaces/albatross-project-in-excluded/packages/seeds/pyproject.toml
+++ b/scripts/workspaces/albatross-project-in-excluded/packages/seeds/pyproject.toml
@@ -2,7 +2,7 @@
 name = "seeds"
 version = "1.0.0"
 requires-python = ">=3.12"
-dependencies = ["idna==3.6"]
+dependencies = ["idna==3.10"]
 
 [build-system]
 requires = ["hatchling"]
diff --git a/scripts/workspaces/albatross-root-workspace/packages/seeds/pyproject.toml b/scripts/workspaces/albatross-root-workspace/packages/seeds/pyproject.toml
index f63b97736..1022fab1b 100644
--- a/scripts/workspaces/albatross-root-workspace/packages/seeds/pyproject.toml
+++ b/scripts/workspaces/albatross-root-workspace/packages/seeds/pyproject.toml
@@ -2,7 +2,7 @@
 name = "seeds"
 version = "1.0.0"
 requires-python = ">=3.12"
-dependencies = ["idna==3.6"]
+dependencies = ["idna==3.10"]
 
 [build-system]
 requires = ["hatchling"]
diff --git a/scripts/workspaces/albatross-virtual-workspace/packages/seeds/pyproject.toml b/scripts/workspaces/albatross-virtual-workspace/packages/seeds/pyproject.toml
index f63b97736..1022fab1b 100644
--- a/scripts/workspaces/albatross-virtual-workspace/packages/seeds/pyproject.toml
+++ b/scripts/workspaces/albatross-virtual-workspace/packages/seeds/pyproject.toml
@@ -2,7 +2,7 @@
 name = "seeds"
 version = "1.0.0"
 requires-python = ">=3.12"
-dependencies = ["idna==3.6"]
+dependencies = ["idna==3.10"]
 
 [build-system]
 requires = ["hatchling"]
```

---

_Comment by @reneleonhardt on 2025-06-10 18:55_

```shell
uv upgrade --recursive --types=prod,dev,extra,group
```
<img width="1077" alt="uv upgrade" src="https://github.com/user-attachments/assets/276a5b25-2ba3-43af-baca-852b992cfa5f" />


---

_Comment by @chirizxc on 2025-08-31 13:44_

Hi, maybe make this PR code as a separate package? like https://github.com/nyudenkov/pysentry?

---

_Comment by @Alan-Chen99 on 2025-11-02 18:07_

This seems to only upgrade upper bounds and not lower bounds; Is this the intended behavior?

---

_Comment by @reneleonhardt on 2025-11-05 14:24_

> This seems to only upgrade upper bounds and not lower bounds; Is this the intended behavior?

In general, yes: It extends the previous upper bound to include a higher version.

* Your app depends on `numpy > 0, < 2`
* uv upgrade bumps this to `numpy > 0, < 3` # to include 2.x
* If your CI breaks, or otherwise necessary or recommended in the release notes, you migrate your code to adapt to the new version
* If the migrated code would not be compatible to the lower bound anymore, you bump the lower bound manually to `numpy >= 2, < 3` to deny 1.x

---
