---
name: oai-ran
description: 'Work with the OpenAirInterface 5G/LTE Radio Access Network (OAI RAN) repository openairinterface5g. Use when cloning, analyzing, building, configuring, or navigating the OAI RAN codebase. Use for PHY layer, L2 stack (MAC/RLC/PDCP/RRC), L3 interfaces (NGAP/NAS), radio drivers, CU/DU splits, O-RAN, or deployment questions about gNB/eNB/UE softmodem.'
argument-hint: 'e.g. "explain PHY structure", "how to build gNB", "trace PDU session setup", "understand CU/DU split"'
---

# OAI RAN — Agent Skill

## Overview

This skill covers the **OpenAirInterface Radio Access Network** codebase — a single monolithic Git repository implementing a full 3GPP-compliant gNB/eNB/UE stack.

**Repository:** `openairinterface5g`  
**GitLab:** `https://gitlab.eurecom.fr/oai/openairinterface5g`  
**Local path:** `/home/lourens/Documents/OAI/OAI_STANDARD_SETUP/OAI_RAN_code/openairinterface5g/`  
**License:** OAI Public License V1.1  
**Language:** C (core), C++ (interfaces), Python (tooling)

Full reference: [OAI_RAN_REPOSITORIES.md](../OAI_RAN_REPOSITORIES.md)

---

## Repository Map

| Directory | Layer | Role |
|---|---|---|
| `openair1/` | L1 — PHY | NR/LTE physical layer: PUSCH/PDSCH/PUCCH/PRACH, LDPC, Polar, link-level scheduler |
| `openair2/LAYER2/` | L2 — MAC/RLC/PDCP/SDAP | MAC scheduler (NR/LTE), RLC (AM/UM/TM), PDCP, SDAP |
| `openair2/RRC/` | L2/L3 — RRC | Radio Resource Control: cell config, UE connection, handover |
| `openair2/F1AP/` | Split — F1 | CU-DU split (TS 38.473) |
| `openair2/E1AP/` | Split — E1 | CU-CP / CU-UP split (TS 38.463) |
| `openair2/E2AP/` | O-RAN — E2 | RIC–gNB E2 interface (O-RAN WG3) |
| `openair2/GNB_APP/` | Config | gNB application init, configuration parsing |
| `openair3/NGAP/` | L3 — N2 | gNB–AMF N2 interface (TS 38.413, SCTP) |
| `openair3/NAS/` | L3 — NAS | Non-Access Stratum (5GS MM / EPS MM) |
| `openair3/ocp-gtpu/` | L3 — N3 | GTPv1-U user-plane tunnelling |
| `radio/` | HAL | Hardware abstraction: USRP, RFsim, FHI 7.2, IRIS, BLADERF |
| `nfapi/` | MAC-PHY | nFAPI: MAC–PHY split (SCF FAPI spec) |
| `executables/` | Entry points | `nr-softmodem`, `lte-softmodem`, `nr-uesoftmodem` |
| `doc/` | Docs | FEATURE_SET.md, SW_archi.md, BUILD.md, RUNMODEM.md |
| `docker/` | Containers | Dockerfiles for gNB, eNB, UE, build images |
| `cmake_targets/` | Build | CMake build system entry points |

---

## Key Facts

- **Current version:** 2026.w12 (branch `develop`)
- **Supported modes:** SA (Standalone), NSA (Non-Standalone), CU/DU split, O-RAN
- **Supported hardware:** USRP B/X/N series, RFsimulator, FHI 7.2, BLADERF, LMSSDR
- **Frequency ranges:** FR1 (sub-6 GHz), FR2 (mmWave)
- **3GPP releases:** LTE Rel-10/12, NR Rel-15/16/17
- **Submodule:** `cmake_targets/tools/openair-cmake` (build tooling)

---

## Common Procedures

### Build the gNB (nr-softmodem)

```bash
cd openairinterface5g
source oaienv
cd cmake_targets
./build_oai -I              # install dependencies (first time only)
./build_oai --gNB -w USRP   # build gNB with USRP support
./build_oai --gNB -w SIMU   # build gNB with RF simulator
```

### Run with RF Simulator (no hardware)

```bash
# Terminal 1 — gNB
cd cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf \
  --rfsim --sa

# Terminal 2 — UE
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 \
  --rfsim --sa --uicc0.imsi 001010000000001
```

### Check Version

```bash
python3 -c "
import subprocess, os
repo = '/home/lourens/Documents/OAI/OAI_STANDARD_SETUP/OAI_RAN_code/openairinterface5g'
env = dict(os.environ, GIT_DIR=f'{repo}/.git', GIT_WORK_TREE=repo)
tag = subprocess.check_output(['git','describe','--tags','--always'], env=env, text=True).strip()
commit = subprocess.check_output(['git','log','-1','--format=%h %ai'], env=env, text=True).strip()
print(f'Version: {tag}\nCommit:  {commit}')
"
```

---

## 3GPP Interface Reference

| Interface | Between | Protocol | Directory |
|---|---|---|---|
| Uu | UE ↔ gNB | NR air interface | `openair1/`, `openair2/LAYER2/` |
| N1 | UE ↔ AMF (via gNB) | 5G NAS | `openair3/NAS/` |
| N2 | gNB ↔ AMF | NGAP (SCTP) | `openair3/NGAP/` |
| N3 | gNB ↔ UPF | GTPv1-U (UDP) | `openair3/ocp-gtpu/` |
| F1-C/U | CU ↔ DU | F1AP (SCTP) + GTP | `openair2/F1AP/` |
| E1 | CU-CP ↔ CU-UP | E1AP (SCTP) | `openair2/E1AP/` |
| E2 | gNB ↔ Near-RT RIC | E2AP (SCTP) | `openair2/E2AP/` |
| Xn | gNB ↔ gNB | XnAP (SCTP) | `openair2/XNAP/` |
| nFAPI | gNB-MAC ↔ PHY | FAPI over UDP | `nfapi/` |
