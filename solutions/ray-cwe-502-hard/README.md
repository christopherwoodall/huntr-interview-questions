# Ray 2.9.1 Deserialization Demo - CWE-502
Report: https://huntr.com/bounties/ce6cb61c-fce1-406f-80e2-f3b7cdddbd6f
Resources:
   - https://docs.ray.io/en/latest/ray-overview/installation.html
   - https://github.com/ray-project/ray


## Overview
...


## Commands

### Terminal 1 - Start the Service
```bash
docker build -t ray-cwe-demo:2.9.1 --no-cache .
docker run -it --network=host --shm-size=4.43gb ray-cwe-demo:2.9.1
```

### Terminal 2 - `netcat` Listener
```bash
nc -lvp 4321
```

### Terminal 3 - Attack
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python exploit.py
```
