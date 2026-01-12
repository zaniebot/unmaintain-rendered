```yaml
number: 2270
title: Debug log stats about versions tried
type: pull_request
state: closed
author: konstin
labels:
  - performance
  - tracing
assignees: []
base: main
head: konsti/log-resolver-stats
created_at: 2024-03-07T11:34:50Z
updated_at: 2024-04-29T12:52:42Z
url: https://github.com/astral-sh/uv/pull/2270
synced_at: 2026-01-12T16:04:57Z
```

# Debug log stats about versions tried

---

_@konstin_

`debug!` how many versions we tried in the resolver, for how many versions we made (cached) network requests and how many (cached) source dist builds we performed.

There are multiple uses for this:
* It allows verifying the cause of slowness: Is it backtracking or is it IO?
* The order in which we choose packages is crucial for the performance of the resolver. A good heuristic means we try as few versions as possible. This PR debug logs how many versions we tried in total and for each package.
* Besides network requests, another possible bottleneck are source distribution builds, especially when we're discarding them later, which we also log stats for.

I'm also adding a `bio_embeddings[all]` requirements file as my main reference for a bad backtracking case.

For jupyter (`uv pip compile -v scripts/requirements/jupyter.in`):

```
      0.127670s 119ms DEBUG uv_resolver::resolver Tried 98 versions: anyio 1, argon2-cffi 1, argon2-cffi-bindings 1, arrow 1, asttokens 1, async-lru 1, attrs 1, babel 1, beautifulsoup4 1, bleach 1, certifi 1, cffi 1, charset-normalizer 1, comm 1, debugpy 1, decorator 1, defusedxml 1, executing 1, fastjsonschema 1, fqdn 1, h11 1, httpcore 1, httpx 1, idna 1, ipykernel 1, ipython 1, ipywidgets 1, isoduration 1, jedi 1, jinja2 1, json5 1, jsonpointer 1, jsonschema 1, jsonschema-specifications 1, jsonschema[format-nongpl] 1, jupyter 1, jupyter-client 1, jupyter-console 1, jupyter-core 1, jupyter-events 1, jupyter-lsp 1, jupyter-server 1, jupyter-server-terminals 1, jupyterlab 1, jupyterlab-pygments 1, jupyterlab-server 1, jupyterlab-widgets 1, markupsafe 1, matplotlib-inline 1, mistune 1, nbclient 1, nbconvert 1, nbformat 1, nest-asyncio 1, notebook 1, notebook-shim 1, overrides 1, packaging 1, pandocfilters 1, parso 1, pexpect 1, platformdirs 1, prometheus-client 1, prompt-toolkit 1, psutil 1, ptyprocess 1, pure-eval 1, pycparser 1, pygments 1, python-dateutil 1, python-json-logger 1, pyyaml 1, pyzmq 1, qtconsole 1, qtpy 1, referencing 1, requests 1, rfc3339-validator 1, rfc3986-validator 1, root 1, rpds-py 1, send2trash 1, six 1, sniffio 1, soupsieve 1, stack-data 1, terminado 1, tinycss2 1, tornado 1, traitlets 1, types-python-dateutil 1, uri-template 1, urllib3 1, wcwidth 1, webcolors 1, webencodings 1, websocket-client 1, widgetsnbextension 1
    0.128729s DEBUG uv_distribution::distribution_database 96 wheel metadata requests: anyio 1, argon2-cffi 1, argon2-cffi-bindings 1, arrow 1, asttokens 1, async-lru 1, attrs 1, babel 1, beautifulsoup4 1, bleach 1, certifi 1, cffi 1, charset-normalizer 1, comm 1, debugpy 1, decorator 1, defusedxml 1, executing 1, fastjsonschema 1, fqdn 1, h11 1, httpcore 1, httpx 1, idna 1, ipykernel 1, ipython 1, ipywidgets 1, isoduration 1, jedi 1, jinja2 1, json5 1, jsonpointer 1, jsonschema 1, jsonschema-specifications 1, jupyter 1, jupyter-client 1, jupyter-console 1, jupyter-core 1, jupyter-events 1, jupyter-lsp 1, jupyter-server 1, jupyter-server-terminals 1, jupyterlab 1, jupyterlab-pygments 1, jupyterlab-server 1, jupyterlab-widgets 1, markupsafe 1, matplotlib-inline 1, mistune 1, nbclient 1, nbconvert 1, nbformat 1, nest-asyncio 1, notebook 1, notebook-shim 1, overrides 1, packaging 1, pandocfilters 1, parso 1, pexpect 1, platformdirs 1, prometheus-client 1, prompt-toolkit 1, psutil 1, ptyprocess 1, pure-eval 1, pycparser 1, pygments 1, python-dateutil 1, python-json-logger 1, pyyaml 1, pyzmq 1, qtconsole 1, qtpy 1, referencing 1, requests 1, rfc3339-validator 1, rfc3986-validator 1, rpds-py 1, send2trash 1, six 1, sniffio 1, soupsieve 1, stack-data 1, terminado 1, tinycss2 1, tornado 1, traitlets 1, types-python-dateutil 1, uri-template 1, urllib3 1, wcwidth 1, webcolors 1, webencodings 1, websocket-client 1, widgetsnbextension 1
    0.128739s DEBUG uv_distribution::distribution_database 0 source dist metadata requests
```

For jupyter (`uv pip compile -v scripts/requirements/meine_stadt_transparent.in`):

```
      0.154532s 146ms DEBUG uv_resolver::resolver Tried 109 versions: anyascii 1, argon2-cffi 1, argon2-cffi-bindings 1, arrow 1, asgiref 1, beautifulsoup4 1, blessed 1, blinker 1, certifi 1, cffi 1, charset-normalizer 1, click 1, cryptography 1, defusedxml 1, django 1, django-allauth 1, django-anymail 1, django-anymail[mailjet] 1, django-anymail[sendgrid] 1, django-csp 1, django-decorator-include 1, django-elasticsearch-dsl 1, django-environ 1, django-filter 1, django-geojson 1, django-modelcluster 1, django-permissionedforms 1, django-picklefield 1, django-q 1, django-q-sentry 1, django-q[sentry] 1, django-settings-export 1, django-simple-history 1, django-taggit 1, django-treebeard 1, django-webpack-loader 1, django-widget-tweaks 1, djangorestframework 1, draftjs-exporter 1, elasticsearch 1, elasticsearch-dsl 1, et-xmlfile 1, flask 1, geoextract 1, geographiclib 1, geopy 1, gunicorn 1, html2text 1, html5lib 1, icalendar 1, idna 1, itsdangerous 1, jinja2 1, joblib 1, jsonfield 1, l18n 1, markupsafe 1, meine-stadt-transparent 1, minio 1, mysqlclient 1, nltk 1, numpy 1, oauthlib 1, openpyxl 1, osm2geojson 1, pillow 1, psycopg2 1, pyahocorasick 1, pycparser 1, pycryptodome 1, pyjwt 1, pyjwt[crypto] 1, pypdf2 1, python-dateutil 1, python-slugify 1, python3-openid 1, pytz 1, redis 1, regex 1, requests 1, requests-oauthlib 1, root 1, scipy 1, sentry-sdk 1, setuptools 1, shapely 1, six 1, soupsieve 1, splinter 1, sqlparse 1, tablib 1, tablib[xls] 1, tablib[xlsx] 1, telepath 1, text-unidecode 1, tqdm 1, types-python-dateutil 1, typing-extensions 1, unidecode 1, urllib3 1, wagtail 1, wand 1, wcwidth 1, webencodings 1, werkzeug 1, willow 1, xlrd 1, xlsxwriter 1, xlwt 1
    0.155655s DEBUG uv_distribution::distribution_database 99 wheel metadata requests: anyascii 1, argon2-cffi 1, argon2-cffi-bindings 1, arrow 1, asgiref 1, beautifulsoup4 1, blessed 1, blinker 1, certifi 1, cffi 1, charset-normalizer 1, click 1, cryptography 1, defusedxml 1, django 1, django-anymail 1, django-csp 1, django-decorator-include 1, django-elasticsearch-dsl 1, django-environ 1, django-filter 1, django-geojson 1, django-modelcluster 1, django-permissionedforms 1, django-picklefield 1, django-q 1, django-q-sentry 1, django-simple-history 1, django-taggit 1, django-treebeard 1, django-webpack-loader 1, django-widget-tweaks 1, djangorestframework 1, draftjs-exporter 1, elasticsearch 1, elasticsearch-dsl 1, et-xmlfile 1, flask 1, geoextract 1, geographiclib 1, geopy 1, gunicorn 1, html2text 1, html5lib 1, icalendar 1, idna 1, itsdangerous 1, jinja2 1, joblib 1, jsonfield 1, l18n 1, markupsafe 1, meine-stadt-transparent 1, minio 1, mysqlclient 1, nltk 1, numpy 1, oauthlib 1, openpyxl 1, pillow 1, psycopg2 1, pyahocorasick 1, pycparser 1, pycryptodome 1, pyjwt 1, pypdf2 1, python-dateutil 1, python-slugify 1, python3-openid 1, pytz 1, redis 1, regex 1, requests 1, requests-oauthlib 1, scipy 1, sentry-sdk 1, setuptools 1, shapely 1, six 1, soupsieve 1, splinter 1, sqlparse 1, tablib 1, telepath 1, text-unidecode 1, tqdm 1, types-python-dateutil 1, typing-extensions 1, unidecode 1, urllib3 1, wagtail 1, wand 1, wcwidth 1, webencodings 1, werkzeug 1, willow 1, xlrd 1, xlsxwriter 1, xlwt 1
    0.155673s DEBUG uv_distribution::distribution_database 3 source dist metadata requests: django-allauth 1, django-settings-export 1, osm2geojson 1
```

For bio_embeddings (`uv pip compile -v scripts/requirements/bio_embeddings.in`):

```
     13.152453s  13s  DEBUG uv_resolver::resolver Tried 101329 versions: pytest 2628, botocore 2627, blis 1314, boto3 1314, certifi 1314, charset-normalizer 1314, click 1314, conllu 1314, contourpy 1314, cycler 1314, cymem 1314, editdistance 1314, flaky 1314, flask 1314, flask-cors 1314, fonttools 1314, fsspec 1314, ftfy 1314, gevent 1314, huggingface-hub 1314, idna 1314, jinja2 1314, joblib 1314, jsonnet 1314, jsonpickle 1314, kiwisolver 1314, murmurhash 1314, networkx 1314, nltk 1314, numba 1314, numpydoc 1314, nvidia-cublas-cu12 1314, nvidia-cuda-cupti-cu12 1314, nvidia-cuda-nvrtc-cu12 1314, nvidia-cuda-runtime-cu12 1314, nvidia-cudnn-cu12 1314, nvidia-cufft-cu12 1314, nvidia-curand-cu12 1314, nvidia-cusolver-cu12 1314, nvidia-cusparse-cu12 1314, nvidia-nccl-cu12 1314, nvidia-nvtx-cu12 1314, overrides 1314, packaging 1314, parsimonious 1314, plac 1314, preshed 1314, protobuf 1314, pynndescent 1314, pyparsing 1314, python-dateutil 1314, pytorch-pretrained-bert 1314, pytorch-transformers 1314, pyyaml 1314, regex 1314, responses 1314, ruamel-yaml-clib 1314, safetensors 1314, sentencepiece 1314, six 1314, spacy 1314, sqlparse 1314, srsly 1314, sympy 1314, tenacity 1314, tensorboardx 1314, thinc 1314, threadpoolctl 1314, tokenizers 1314, typing-extensions 1314, tzdata 1314, unidecode 1314, wasabi 1314, word2number 1314, wrapt 1314, allennlp 27, torch 20, bio-embeddings[all] 10, bio-embeddings 4, biopython 4, gensim 4, h5py 4, matplotlib 3, numpy 3, pandas 3, plotly 3, ruamel-yaml 3, scikit-learn 3, scipy 3, Python 2, appdirs 2, importlib-metadata 2, jax-unirep 2, jaxlib 2, lock 2, alabaster 1, babel 1, bio-embeddings-allennlp 1, bio-embeddings-bepler 1, bio-embeddings-cpcprot 1, bio-embeddings-esm 1, bio-embeddings-plus 1, bio-embeddings-tape-proteins 1, blinker 1, docutils 1, filelock 1, greenlet 1, imagesize 1, iniconfig 1, itsdangerous 1, jmespath 1, llvmlite 1, markupsafe 1, mpmath 1, nvidia-nvjitlink-cu12 1, pillow 1, pluggy 1, pygments 1, pytz 1, requests 1, root 1, s3transfer 1, setuptools 1, smart-open 1, snowballstemmer 1, sphinx 1, sphinxcontrib-applehelp 1, sphinxcontrib-devhelp 1, sphinxcontrib-htmlhelp 1, sphinxcontrib-jsmath 1, sphinxcontrib-qthelp 1, sphinxcontrib-serializinghtml 1, tabulate 1, tqdm 1, transformers 1, umap-learn 1, urllib3 1, wcwidth 1, werkzeug 1, zope-event 1, zope-interface 1
   13.338377s DEBUG uv_distribution::distribution_database 2820 wheel metadata requests: boto3 1314, botocore 1314, allennlp 27, s3transfer 10, bio-embeddings 6, transformers 3, gensim 2, h5py 2, jmespath 2, pandas 2, pillow 2, plotly 2, pytest 2, ruamel-yaml 2, scikit-learn 2, spacy 2, urllib3 2, alabaster 1, appdirs 1, babel 1, bio-embeddings-allennlp 1, bio-embeddings-bepler 1, bio-embeddings-cpcprot 1, bio-embeddings-esm 1, bio-embeddings-plus 1, bio-embeddings-tape-proteins 1, biopython 1, blinker 1, blis 1, certifi 1, charset-normalizer 1, click 1, conllu 1, contourpy 1, cycler 1, cymem 1, docutils 1, editdistance 1, filelock 1, flaky 1, flask 1, flask-cors 1, fonttools 1, fsspec 1, ftfy 1, gevent 1, greenlet 1, huggingface-hub 1, idna 1, imagesize 1, importlib-metadata 1, iniconfig 1, itsdangerous 1, jax-unirep 1, jaxlib 1, jinja2 1, joblib 1, jsonpickle 1, kiwisolver 1, llvmlite 1, lmdb 1, markupsafe 1, matplotlib 1, ml-dtypes 1, mpmath 1, multipledispatch 1, murmurhash 1, networkx 1, nltk 1, numba 1, numpy 1, numpydoc 1, nvidia-cublas-cu12 1, nvidia-cuda-cupti-cu12 1, nvidia-cuda-nvrtc-cu12 1, nvidia-cuda-runtime-cu12 1, nvidia-cudnn-cu12 1, nvidia-cufft-cu12 1, nvidia-curand-cu12 1, nvidia-cusolver-cu12 1, nvidia-cusparse-cu12 1, nvidia-nccl-cu12 1, nvidia-nvjitlink-cu12 1, nvidia-nvtx-cu12 1, optuna 1, overrides 1, packaging 1, parsimonious 1, plac 1, pluggy 1, preshed 1, protobuf 1, pygments 1, pynndescent 1, pyparsing 1, python-dateutil 1, pytorch-pretrained-bert 1, pytorch-transformers 1, pytz 1, pyyaml 1, regex 1, requests 1, responses 1, retrying 1, ruamel-yaml-clib 1, safetensors 1, scipy 1, sentencepiece 1, setuptools 1, six 1, smart-open 1, snowballstemmer 1, sphinx 1, sphinxcontrib-applehelp 1, sphinxcontrib-devhelp 1, sphinxcontrib-htmlhelp 1, sphinxcontrib-jsmath 1, sphinxcontrib-qthelp 1, sphinxcontrib-serializinghtml 1, sqlparse 1, srsly 1, sympy 1, tabulate 1, tenacity 1, tensorboardx 1, thinc 1, threadpoolctl 1, tokenizers 1, torch 1, torchvision 1, tqdm 1, typing-extensions 1, tzdata 1, unidecode 1, wasabi 1, wcwidth 1, werkzeug 1, wrapt 1, zipp 1, zope-event 1, zope-interface 1
   13.338392s DEBUG uv_distribution::distribution_database 6 source dist metadata requests: umap-learn 2, jax 1, jsonnet 1, lock 1, word2number 1
```

---

_Label `enhancement` added by @konstin on 2024-03-07 11:34_

---

_Label `performance` added by @konstin on 2024-03-07 11:34_

---

_Label `tracing` added by @zanieb on 2024-03-07 17:42_

---

_Label `enhancement` removed by @zanieb on 2024-03-07 17:42_

---

_Review requested from @charliermarsh by @konstin on 2024-03-10 14:13_

---

_Closed by @konstin on 2024-04-17 09:08_

---

_Branch deleted on 2024-04-29 12:52_

---
