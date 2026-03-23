---
name: oai-ran-splits
description: 'Expert skill group for OAI RAN functional split interfaces: F1AP/E1AP (CU/DU split), E2AP (O-RAN Near-RT RIC interface), and nFAPI (MAC-PHY split). Use agents in this group when the question concerns inter-node protocol interfaces, distributed gNB architecture, or RAN Intelligent Controller integration.'
---

# OAI RAN — Split Interface Skills

This skill group covers the **functional split interfaces** within the OAI RAN — the protocols that allow gNB functions to be distributed across separate nodes.

## Functional Splits Overview

```
           Near-RT RIC
               │ E2 (E2AP)     ← oran-e2.agent.md
               │
┌──────────────┼──────────────────────────────────┐
│  CU-CP       │ (NGAP/XnAP upstream)              │
│  ┌──────┐    │                                   │
│  │ RRC  │    │                                   │
│  │ PDCP │    │  E1 ← cu-du.agent.md              │
│  └──┬───┘    │                                   │
│     │  CU-UP (PDCP/SDAP)                         │
└─────┼─────────────────────────────────────────── ┘
      │ F1-C / F1-U ← cu-du.agent.md
┌─────┼──────────────────────────────────────────── ┐
│ DU  │                                              │
│  ┌──┴──┐                                           │
│  │ RLC │                                           │
│  │ MAC │ ← nFAPI ← nfapi.agent.md                 │
│  └──┬──┘                                           │
│     │ nFAPI                                        │
│  ┌──┴──┐                                           │
│  │ PHY │                                           │
└─────────────────────────────────────────────────── ┘
      │ 7.2 (FHI) ← radio.agent.md
   RU / Radio
```

## Agent Directory

| Agent | Interface | Spec |
|---|---|---|
| `cu-du.agent.md` | F1AP (CU-DU) + E1AP (CU-CP/CU-UP) | TS 38.473, TS 38.463 |
| `oran-e2.agent.md` | E2AP (gNB↔Near-RT RIC) | O-RAN WG3, E2SM-KPM, E2SM-RC |
| `nfapi.agent.md` | nFAPI (MAC↔PHY FAPI split) | SCF 222.10 |

For the radio-side of 7.2 fronthaul, use `oai-ran-layers/radio.agent.md`.
