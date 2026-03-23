---
name: oai-ran-layers
description: 'Expert skill group covering individual protocol layer deep-dives in the OAI RAN stack: PHY (openair1), MAC, RLC/PDCP/SDAP (openair2/LAYER2), RRC (openair2/RRC), NGAP/NAS (openair3), and radio hardware (radio/). Use agents in this group when the question is clearly scoped to one protocol layer.'
---

# OAI RAN — Layer Expert Skills

This skill group contains specialised agents for deep-dives into individual protocol layers within `openairinterface5g`.

## Protocol Stack Overview

```
┌─────────────────────────────────────────────────────┐
│                   Application                        │
├─────────────────────────────────────────────────────┤
│   NAS     (openair3/NAS/)         [ngap-nas agent]   │
│   NGAP    (openair3/NGAP/)        [ngap-nas agent]   │
│   GTP-U   (openair3/ocp-gtpu/)    [ngap-nas agent]   │
├─────────────────────────────────────────────────────┤
│   RRC     (openair2/RRC/)         [rrc agent]        │
├─────────────────────────────────────────────────────┤
│   SDAP    (openair2/SDAP/)        [rlc-pdcp agent]   │
│   PDCP    (openair2/LAYER2/nr_pdcp/) [rlc-pdcp agent]│
│   RLC     (openair2/LAYER2/nr_rlc/)  [rlc-pdcp agent]│
├─────────────────────────────────────────────────────┤
│   MAC     (openair2/LAYER2/NR_MAC_gNB/) [mac agent]  │
├─────────────────────────────────────────────────────┤
│   PHY     (openair1/PHY/, SCHED_NR/)    [phy agent]  │
├─────────────────────────────────────────────────────┤
│   Radio HAL (radio/)              [radio agent]      │
└─────────────────────────────────────────────────────┘
```

## Agent Directory

| Agent | Scope |
|---|---|
| `phy.agent.md` | PHY layer: waveform, coding, LDPC/Polar, channel estimation |
| `mac.agent.md` | MAC scheduler, HARQ, RACH, resource allocation |
| `rlc-pdcp.agent.md` | RLC AM/UM, PDCP security/compression, SDAP QoS |
| `rrc.agent.md` | RRC state machine, cell config, bearer setup, handover |
| `ngap-nas.agent.md` | N2 (NGAP), NAS, GTPv1-U tunnelling |
| `radio.agent.md` | USRP, RFsimulator, FHI 7.2, hardware abstraction |

For cross-layer or multi-layer questions, use the **RAN Search** agent (`oai-ran/ran-search.agent.md`).
