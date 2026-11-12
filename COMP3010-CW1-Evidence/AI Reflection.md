# COMP3010 CW1 – Evidence Portfolio

Analysed file: `COMP3010CW1.pcap`, SHA256 `aa8bd9280b943db2e729b921de58d5fd37c4f8c76d1aa9996e9e5b44bcf7a7a3`.
The pcap and exported executables are not included because they contain live malware; hashes are given instead.
All times are UTC unless stated otherwise.

## Evidence index

| Fig | File | What it shows | Filter / location | Packet(s) | Supports finding |
|---|---|---|---|---|---|
| 1 | 01-host-identification/fig01-capture-file-properties.png | pcap hash; capture runs 6 min 10 s on 2017-06-27. Times shown are local (BST, UTC+1): 14:38 = 13:38 UTC | Statistics → Capture File Properties | – | Scope, date, time normalisation |
| 2 | 01-host-identification/fig02-inbound-traffic-to-192.168.1.96.png | First inbound traffic: 119.28.70.207:80 → 192.168.1.96:49184 (the gerv.gun download flagged by both IDSs) | `ip.dst == 192.168.1.96` | 2–30 | Infected host IP |
| 3 | 01-host-identification/fig03-packet6-source-mac-dell.png | Host sending the first GET has MAC 00:15:c5:de:c7:3b (Dell) | `ip.addr == 192.168.1.96 && http` | 6 | Infected host MAC |
| 4 | 01-host-identification/fig04-dhcp-hostname-flashgordon-pc.png | DHCP Inform from the same MAC gives hostname FlashGordon-PC | `ip.addr == 192.168.1.96 && dhcp` | 213 | Infected hostname |
| 5 | 02-infection-vector/fig05-http-get-requests-payload-timeline.png | Four downloads: gerv.gun 13:38:32, trow.exe 13:43:52, wp.exe 13:43:54, t64.bin 13:43:58 | `http.request.method == "GET"` | 6, 313, 667, 866 | Infection time and payload chain |

## AI usage evidence (supports AI reflection appendix)

| Fig | File | What it shows |
|---|---|---|
| A1 | 06-ai-usage/figA1-dolphin-mistral-outdated-answer.png | Local dolphin-mistral model gives an outdated answer (UK PM); model rejected as unreliable |
| A2 | 06-ai-usage/figA2-qwen2.5-3b-pcap-guidance.png | Prompt and output from local qwen2.5:3b used for methodology guidance |
| A3 | 06-ai-usage/figA3-opencode-free-models-list.png | Free models available through opencode |
| A4a/b | 06-ai-usage/figA4a-host-memory-usage.png, figA4b-host-memory-usage.png | Host RAM near capacity (17.8 of 20 GB in use) while running local models: a hardware constraint |
| A5 | 06-ai-usage/figA5-vm-cpu-allocation-lscpu.png | Analysis VM allocated only 2 vCPUs |
| A6 | 06-ai-usage/figA6-host-cpu-task-manager.png | Host CPU: Ryzen 7 5700U, 8 cores / 16 threads |

## Indicators of compromise

| Type | Value | Note |
|---|---|---|
| Host | 192.168.1.96 / 00:15:c5:de:c7:3b / FlashGordon-PC | Infected system |
| URL | matied.com/gerv.gun (119.28.70.207) | First-stage download; PE with fake extension |
| URL | lounge-haarstudio.nl/oud/trow.exe (145.131.10.21) | Second-stage |
| URL | vantagepointtechnologies.com/wp.exe (143.95.151.192) | Second-stage |
| URL | 59.106.164.230/img/t64.bin | 3 MB encoded payload, no MZ header |
| Domain | centler.at | POST check-ins at 13:43:50 |
| SHA256 | 0931537889c35226d00ed26962ecacb140521394279eb2ade7e9d2afcf1a7272 | gerv.gun |
| SHA256 | 94a0a09ee6a21526ac34d41eabf4ba603e9a30c26e6a1dc072ff45749dfb1fe1 | trow.exe |
| SHA256 | 79d503165d32176842fe386d96c04fb70f6ce1c8a485837957849297e625ea48 | wp.exe |
| SHA256 | 59727ea201d94d471bdbb353b1281cca3c5bebf5305b140a3c017e52a37be702 | Candidate initial .js (from zip in Japanese-language invoice malspam). OSINT: myonlinesecurity.co.uk via malware-traffic-analysis.net. Not in pcap |
| SHA256 | 64ae13592c5d8b289be81ebb50e054cef49b0bdf50ea92dd44ed611dd274150f | Second candidate .js from the same malspam campaign. Which one infected the host cannot be determined from the pcap |
