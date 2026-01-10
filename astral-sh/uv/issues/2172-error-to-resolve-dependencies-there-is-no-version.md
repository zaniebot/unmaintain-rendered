---
number: 2172
title: "Error to resolve dependencies: There is no version"
type: issue
state: closed
author: yabirgb
labels:
  - bug
assignees: []
created_at: 2024-03-04T20:28:32Z
updated_at: 2024-03-05T08:18:23Z
url: https://github.com/astral-sh/uv/issues/2172
synced_at: 2026-01-10T01:23:13Z
---

# Error to resolve dependencies: There is no version

---

_Issue opened by @yabirgb on 2024-03-04 20:28_

I'm trying to add support for uv in the rotki's CI. PR here https://github.com/rotki/rotki/pull/7499 and I get the following error

```
(rotki-main) ➜  rotki git:(gotta-go-fast) ✗ uv pip install -e .         
   Built file:///Users/yabirgb/work/rotki                                                                                                                                                                                                                                 Built 1 editable in 33.54s
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of miniupnpc==2.0.2 and rotkehlchen==1.32.2.dev72+g6c929c10b depends on miniupnpc==2.0.2, we can conclude that rotkehlchen==1.32.2.dev72+g6c929c10b cannot be used.
      And because you require rotkehlchen==1.32.2.dev72+g6c929c10b, we can conclude that the requirements are unsatisfiable.
```

The package exists and pip can resolve it https://pypi.org/project/miniupnpc/2.0.2/ . I'm using

```
(rotki-main) ➜  rotki git:(gotta-go-fast) ✗ uv --version
uv 0.1.14 (c525fdf2b 2024-03-04)
```

Also I tried it earlier when uv was released and it could solve the dependencies so I believe it is a bug that was introduced later.

```
(rotki-main) ➜  rotki git:(gotta-go-fast) ✗ uv pip install --reinstall -e .
   Built file:///Users/yabirgb/work/rotki                                                                                                                                                                                                                                 Built 1 editable in 33.86s
Resolved 99 packages in 1.07s
   Built maxminddb==2.5.2                                                                                                                                                                                                                                                 Downloaded 18 packages in 3.76s
Installed 99 packages in 138ms
 - aiohttp==3.9.1
 + aiohttp==3.9.3
 - aiosignal==1.3.1
 + aiosignal==1.3.1
 - anyio==4.2.0
 + anyio==4.3.0
 - asn1crypto==1.5.1
 + asn1crypto==1.5.1
 - attrs==23.2.0
 + attrs==23.2.0
 - backoff==2.2.1
 + backoff==2.2.1
 - base58==2.1.1
 + base58==2.1.1
 - base58check==1.0.2
 + base58check==1.0.2
 - bases==0.3.0
 + bases==0.3.0
 - beautifulsoup4==4.12.3
 + beautifulsoup4==4.12.3
 - bech32==1.2.0
 + bech32==1.2.0
 - bip-utils==2.9.1
 + bip-utils==2.9.1
 - bitarray==2.9.2
 + bitarray==2.9.2
 - blinker==1.7.0
 + blinker==1.7.0
 - cbor2==5.5.1
 + cbor2==5.6.2
 - certifi==2023.11.17
 + certifi==2024.2.2
 - cffi==1.16.0
 + cffi==1.16.0
 - charset-normalizer==3.3.2
 + charset-normalizer==3.3.2
 - click==8.1.7
 + click==8.1.7
 - coincurve==19.0.1
 + coincurve==19.0.1
 - content-hash==2.0.0
 + content-hash==2.0.0
 - crcmod==1.7
 + crcmod==1.7
 - cryptography==42.0.5
 + cryptography==42.0.5
 - cytoolz==0.12.2
 + cytoolz==0.12.3
 - ecdsa==0.18.0
 + ecdsa==0.18.0
 - ed25519-blake2b==1.4.1
 + ed25519-blake2b==1.4.1
 - eth-abi==4.2.1
 + eth-abi==5.0.0
 - eth-account==0.10.0
 + eth-account==0.11.0
 - eth-hash==0.5.2
 + eth-hash==0.7.0
 - eth-keyfile==0.7.0
 + eth-keyfile==0.8.0
 - eth-keys==0.4.0
 + eth-keys==0.5.0
 - eth-rlp==1.0.0
 + eth-rlp==1.0.1
 - eth-typing==3.5.2
 + eth-typing==4.0.0
 - eth-utils==2.3.1
 + eth-utils==2.3.1
 - filetype==1.2.0
 + filetype==1.2.0
 - flask==3.0.2
 + flask==3.0.2
 - flask-cors==4.0.0
 + flask-cors==4.0.0
 - frozenlist==1.4.1
 + frozenlist==1.4.1
 - gevent==24.2.1
 + gevent==24.2.1
 - gevent-websocket==0.10.1
 + gevent-websocket==0.10.1
 - gql==3.5.0
 + gql==3.5.0
 - graphql-core==3.2.3
 + graphql-core==3.2.3
 - greenlet==3.0.3
 + greenlet==3.0.3
 - hexbytes==0.3.1
 + hexbytes==0.3.1
 - idna==3.6
 + idna==3.6
 - itsdangerous==2.1.2
 + itsdangerous==2.1.2
 - jinja2==3.1.2
 + jinja2==3.1.3
 - jsonschema==4.20.0
 + jsonschema==4.21.1
 - jsonschema-specifications==2023.12.1
 + jsonschema-specifications==2023.12.1
 - lru-dict==1.2.0
 + lru-dict==1.2.0
 - markupsafe==2.1.3
 + markupsafe==2.1.5
 - marshmallow==3.21.0
 + marshmallow==3.21.0
 - maxminddb==2.5.2
 + maxminddb==2.5.2
 - miniupnpc==2.0.2
 + miniupnpc==2.0.2
 - more-itertools==10.1.0
 + more-itertools==10.1.0
 - multidict==6.0.4
 + multidict==6.0.5
 - multiformats==0.3.1.post4
 + multiformats==0.3.1.post4
 - multiformats-config==0.3.1
 + multiformats-config==0.3.1
 - packaging==23.2
 + packaging==23.2
 - parsimonious==0.9.0
 + parsimonious==0.9.0
 - polyleven==0.8
 + polyleven==0.8
 - protobuf==4.25.1
 + protobuf==4.25.3
 - py-bip39-bindings==0.1.11
 + py-bip39-bindings==0.1.11
 - py-ed25519-zebra-bindings==1.0.1
 + py-ed25519-zebra-bindings==1.0.1
 - py-machineid==0.5.1
 + py-machineid==0.5.1
 - py-sr25519-bindings==0.2.0
 + py-sr25519-bindings==0.2.0
 - pycparser==2.17
 + pycparser==2.17
 - pycryptodome==3.19.1
 + pycryptodome==3.20.0
 - pycryptodomex==3.19.1
 + pycryptodomex==3.20.0
 - pynacl==1.5.0
 + pynacl==1.5.0
 - pyunormalize==15.1.0
 + pyunormalize==15.1.0
 - referencing==0.32.1
 + referencing==0.33.0
 - regex==2023.12.25
 + regex==2023.12.25
 - requests==2.31.0
 + requests==2.31.0
 - requests-toolbelt==1.0.0
 + requests-toolbelt==1.0.0
 - rlp==4.0.0
 + rlp==4.0.0
 - rotkehlchen==1.32.2.dev70+g678cfb606.d20240304 (from file:///Users/yabirgb/work/rotki)
 + rotkehlchen==1.32.2.dev72+g6c929c10b.d20240304 (from file:///Users/yabirgb/work/rotki)
 - rotki-pysqlcipher3==2024.1.2
 + rotki-pysqlcipher3==2024.1.2
 - rpds-py==0.16.2
 + rpds-py==0.18.0
 - scalecodec==1.2.8
 + scalecodec==1.2.8
 - setuptools==69.0.3
 + setuptools==69.1.1
 - six==1.16.0
 + six==1.16.0
 - sniffio==1.3.0
 + sniffio==1.3.1
 - soupsieve==2.5
 + soupsieve==2.5
 - substrate-interface==1.7.7
 + substrate-interface==1.7.7
 - toolz==0.12.0
 + toolz==0.12.1
 - typing-extensions==4.9.0
 + typing-extensions==4.10.0
 - typing-validation==1.1.0
 + typing-validation==1.2.10.post4
 - urllib3==2.2.1
 + urllib3==2.2.1
 - web3==6.15.1
 + web3==6.15.1
 - webargs==8.4.0
 + webargs==8.4.0
 - websocket-client==1.7.0
 + websocket-client==1.7.0
 - websockets==12.0
 + websockets==12.0
 - werkzeug==3.0.1
 + werkzeug==3.0.1
 - wsaccel==0.6.6
 + wsaccel==0.6.6
 - xxhash==3.4.1
 + xxhash==3.4.1
 - yarl==1.9.4
 + yarl==1.9.4
 - zope-event==5.0
 + zope-event==5.0
 - zope-interface==6.1
 + zope-interface==6.2
(rotki-main) ➜  rotki git:(gotta-go-fast) ✗ uv --version                   
uv 0.1.3
(rotki-main) ➜  rotki git:(gotta-go-fast) ✗
```

The requirements file can be found in the linked PR https://github.com/rotki/rotki/blob/678cfb6061be0a1298470ef983d6f0476e64522b/requirements.txt#L21




---

_Label `bug` added by @charliermarsh on 2024-03-04 20:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-04 20:32_

---

_Comment by @charliermarsh on 2024-03-04 20:32_

Think I see the issue. We're missing a platform gate somewhere, so it's assuming you want to use a URL dependency for `miniupnpc`.

---

_Referenced in [astral-sh/uv#2176](../../astral-sh/uv/pulls/2176.md) on 2024-03-04 21:57_

---

_Closed by @charliermarsh on 2024-03-04 22:09_

---

_Comment by @yabirgb on 2024-03-05 08:18_

Thanks @charliermarsh !!

---
